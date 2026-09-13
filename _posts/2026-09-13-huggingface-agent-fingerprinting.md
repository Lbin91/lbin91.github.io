---
title: "로컬에서 모델 돌리는데 무슨 전화를 하나 — huggingface_hub의 조용한 지문 수집"
date: 2026-09-13 09:45:00 +0900
categories: ai
tags: [llm, localllm, huggingface, privacy, telemetry]
slug: huggingface-agent-fingerprinting
---

로컬 LLM을 쓰는 이유는 사람마다 다르지만, 그중 하나는 분명 프라이버시다. 클라우드 API에 프롬프트를 보내고 싶지 않아서, 내 파일과 내 코드가 내 방식으로 처리되길 바라서 로컬을 택한다. 그런데 r/localllm에 어제 올라온 글이 이 전제에 균열을 만들었다. huggingface_hub 라이브러리가 어떤 AI 코딩 도구를 쓰는지 조용히 식별해서 텔레메트리로 보내고 있다는 것.

## 시작은 허가받지 않은 연결 하나

발견자는 로컬에서 TTS, ASR, 비전 모델을 돌리는 프로젝트를 운영 중이었다. 아웃바운드 트래픽을 통제하려고 파이썬 수준의 방화벽을 만들면서 감사를 돌다가, 예상 못한 HTTPS 연결을 발견한다. huggingface.co로 향하는 연결이었다.

호출 경로를 추적해보니 이랬다. WhisperModel("base.en") → faster_whisper → huggingface_hub.snapshot_download → huggingface.co. 모델명을 넘기면 이미 캐시에 있어도 업데이트 확인을 위해 허브를 찾는다. 여기까지는 그럴 수 있다. 문제는 그 요청에 함께 실려 가는 정보였다.

HF 캐시 디렉터리에는 낯선 파일이 하나 있었다. ~/.cache/huggingface/.agent_harnesses.json. 열어보니 6KB짜리 JSON인데, Claude Code, Cursor, Copilot, Gemini CLI, Devin, Cline, Goose, Codex 등 AI 코딩 에이전트 26종의 레지스트리가 담겨 있었다. 각 항목은 해당 에이전트가 실행 중 세팅하는 환경변수 시그니처를 적어두고 있다.

```json
{
  "standardEnvVars": ["AI_AGENT", "AGENT"],
  "harnesses": {
    "cursor": {
      "prettyLabel": "Cursor",
      "envVars": {"CURSOR_TRACE_ID": "*"}
    },
    "claude-code": {
      "prettyLabel": "Claude Code",
      "envVars": {"CLAUDECODE": "*", "CLAUDE_CODE": "*"}
    },
    "github-copilot": {
      "prettyLabel": "GitHub Copilot",
      "envVars": {"COPILOT_MODEL": "*", "COPILOT_GITHUB_TOKEN": "*"}
    }
  }
}
```

## 어떻게 동작하나

huggingface_hub 패키지 안에는 _detect_agent.py라는 모듈이 있다. 흐름은 이렇다.

먼저 {HF_ENDPOINT}/api/agent-harnesses에서 에이전트 레지스트리를 받아와 24시간 주기로 캐시를 갱신한다. 그리고 허브 API를 호출할 때마다 detect_agent()가 내 환경변수를 레지스트리와 대조한다. Cursor를 쓰고 있다면 CURSOR_TRACE_ID가, Claude Code라면 CLAUDECODE가 환경에 남아 있을 테니, 그걸로 어떤 도구가 세션을 주도하는지 알아낸다. 결과는 요청 헤더에 실려 가고, 이 데이터는 HF의 공개 에이전트 사용량 데이터셋을 먹인다.

즉 faster-whisper든 transformers든, huggingface_hub를 거치는 라이브러리라면 모델 다운로드 한 번, 토크나이저 로딩 한 번에도 내 도구 체인이 보고된다. "내가" 부르는 게 아니라 도구가 조용히 보내는 것이다.

막는 방법은 몇 가지다.

```bash
export HF_HUB_OFFLINE=1
export TRANSFORMERS_OFFLINE=1
export HF_HUB_DISABLE_TELEMETRY=1
```

앞의 두 변수는 네트워크 호출 자체를 차단한다. 세 번째는 텔레메트리를 끄지만 에이전트 감지 헤더까지 커버하는지는 불확실하다고 한다. 더 근본적인 방법은 모델명 대신 로컬 경로를 넘기는 것이다. WhisperModel("/path/to/local/model/")처럼 디렉터리를 직접 지정하면 대부분의 HF 기반 라이브러리는 허브를 아예 거치지 않는다. 캐시 파일 자체는 자격증명도 API 키도 없고 그냥 레지스트리라 지워도 무해하다.

## 왜 주목할까

글쓴이도 명확히 하고 있다. 악의적이라고 보지는 않는다. HF 입장에서는 에이전트 생태계 채택률을 집계하는 비즈니스 인텔리전스일 뿐이고, 레지스트리는 공개된 곳에서 PR로 관리된다. 숨겨진 건 아니다. 다만 pip install 한 번으로 들어오는 라이브러리가 옵트인 프롬프트 없이 내 개발 도구 사용 현황을 수집한다는 사실을 아는 사용자는 거의 없다.

문제의 핵심은 정보의 종류가 아니라 동의다. 모델 가중치도 내 데이터도 아니지만, "어떤 도구를 언제 쓰는가"라는 메타데이터가 묻지도 따지지도 않고 집계되어 공개 데이터셋이 된다. 프라이버시를 이유로 로컬을 택한 사람에게 이건 꽤 아픈 지점이다. 로컬 모델과 나 사이에 있는 라이브러리 계층이 나 대신 이야기하고 있었으니까.

이 일은 더 넓은 교훈으로도 이어진다. 로컬 LLM이라는 선택이 끝점만 바꾼 게 아니라면, 그 사이의 모든 계층을 한 번쯤 들여다봐야 한다. 발견자가 사용한 방식, 즉 socket.connect와 getaddrinfo를 감싸는 파이썬 수준 방화벽을 사이트 훅으로 걸어 허용 목록 밖의 연결을 차단하는 접근은 남겨둘 만하다. 허브 연결 하나를 추적하다가 이걸 찾아냈으니, 네트워크 레벨에서 직접 보는 게 결국 가장 확실한 감사 방법이다.

## 정리

로컬 LLM은 프라이버시를 위한 선택이지만, 그 아래 깔린 라이브러리가 상황을 복잡하게 만든다. HF_HUB_OFFLINE=1과 로컬 경로 사용이라는 두 가지 습관만 들여도 이번 사례는 피할 수 있다. 도구 체인 지문 수집이 악성은 아니라지만, 알고 선택하는 것과 모르고 노출되는 것은 다르다. 다음에 로컬 모델을 띄울 때, 한 번쯤 `ls ~/.cache/huggingface/`를 확인해보자. 생각지 못한 파일이 놓여 있을 수도 있다.
