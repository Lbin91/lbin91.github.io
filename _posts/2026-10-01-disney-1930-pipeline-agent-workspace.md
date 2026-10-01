---
title: "실수는 스토리보드에서 고쳐라 — 디즈니 1930년 파이프라인을 에이전트 워크스페이스로 옮기다"
date: 2026-10-01 09:45:00 +0900
categories: ai
tags: [claude-code, agent, pipeline, workspace, human-in-the-loop]
slug: disney-1930-pipeline-agent-workspace
---

AI로 영상을 만드는 이야기는 대개 모델 얘기로 흘러간다. 이번 건 다르다. r/ClaudeCode에 올라온 [한 편의 글](https://www.reddit.com/r/ClaudeCode/comments/1wtra8s/i_copied_disneys_1930_animation_pipeline_into_a/)이 모델 성능을 자랑하는 대신 구조만으로 품질 문제를 푼다. 원조는 1930년대 디즈니 스튜디오다. 거기서 가져온 방식이 AI 워크스페이스에 이상하리만치 들어맞는다.

작성자인 u/VanCliefMedia는 Claude Code로 애니메이션 영상을 뽑는다. Sonnet으로 한 편씩 정성 들여 만들 수도 있고 Opus 5.5를 얹으면 두 시간 남짓에 50편까지 돌릴 수도 있다. 그런데 글의 무게중심은 어느 쪽도 아니다. 자기 말로는 "워크스페이스 구조가 다른 일에도 그대로 옮겨질 부분"이라고 한다.

## 실수는 늦게 발견할수록 비싸진다

자동화의 함정은 익숙하다. 중간에 아무도 보지 않으면 나쁜 테이크 하나가 나쁜 영상 한 편이 된다. 스크립트의 작은 실수는 렌더까지 가면 폭발해 있다. 아무것도 안 보이는 채로 자동화를 돌리면 실수는 지수적으로 불어난다는 게 작성자의 관찰이다.

그래서 문제를 앞단으로 끌어온다. 렌더된 장면에서 비트 하나를 고치는 데 한 시간이 걸린다면 텍스트 스토리보드에서는 몇 초면 끝난다. 단순한 산수인데, 이걸 파이프라인 전체의 축으로 삼는 팀은 드물다.

## 계약서가 있는 폴더

워크스페이스는 ICM이라 부르는 [폴더 구조](https://github.com/RinDig/icm-architect)로 짜여 있다.

```text
animations/
  CLAUDE.md     <- 지도: 무엇이 어디 있는지, 각 작업은 어디로 가는지
  CONTEXT.md    <- 파이프라인 전체를 한 표로
  anim.py       <- 모든 명령 (speak, transcribe, cues, stills, render)
  stages/
    01_voice/CONTEXT.md
    02_transcript/CONTEXT.md
    03_cues/CONTEXT.md
    04_scene/CONTEXT.md
    05_render/CONTEXT.md
  videos/<slug>/  <- 영상 한 편에 폴더 하나
```

각 스테이지의 CONTEXT.md가 계약서 역할을 한다. 스토리보드 단계의 실제 계약을 옮겨 오면 이렇다.

```markdown
# 03_cues: the storyboard

Reads: 02_transcript/transcript.md, 01_voice/brief.md, _shared/kit.md
Don't read: words.json (the command does)

1. 03_cues/cues.md를 쓴다. 화면이 바뀌는 지점마다 cue 하나
2. python anim.py cues <slug> 가 각 구절을 단어 타이밍에 맞춘다

Output: cues.md (원본), cues.js + cues.json (생성물. 손대지 않는다)

Person checks: 스토리보드를 읽는다. 볼 만한 비트가 다 들어갔는지,
이야기 순서가 맞는지.
```

읽을 것, 산출물, 사람의 확인. 계약은 이 셋이면 끝이다. Claude는 해당 스테이지가 필요한 파일만 읽고, 사람이 직전 산출물을 읽기 전엔 다음 단계로 넘어가지 않는다. CLAUDE.md는 지도 역할만 하므로 짧게 유지되고, 디테일은 각 스테이지의 CONTEXT.md에 나눠 둔다. 맥락은 필요한 만큼만 로드된다.

## 1930년으로 돌아가기

디즈니가 1930년 무렵 쓰던 방식이 이 구조와 맞닿아 있다. 목소리를 먼저 녹음하고, 대사를 exposure sheet에 프레임 단위로 분해하고, rough 애니메이션은 잉킹 전에 sweatbox에서 검수했다.

파이프라인을 나란히 놓으면 대응이 보인다. Voice는 말하는 채로 쓴 스크립트를 자기 목소리의 클론으로 녹음한다. Words에서 Whisper가 단어마다 시작과 끝 타이밍을 찍고, 스크립트와 소리가 어긋난 지점엔 플래그를 단다. Cues는 텍스트 스토리보드인데 각 비트가 실제 발화에 매달려 있어 타이밍이 목소리에서 나온다. Scene은 Claude가 캐릭터와 방, 소품으로 이루어진 키트의 함수들로 장면을 JS로 쓴다. 화면의 모든 것이 시간의 함수라서 타이머가 없고, 다 찍은 뒤엔 컨택트 시트를 읽어 겹침을 고친다. Render는 헤드리스 Chrome이 프레임을 밟고 ffmpeg가 목소리와 음악, 효과음을 섞는다.

작성자가 계약서에서 단연 우선으로 꼽는 줄은 Person checks다. 다음 단계가 돌기 전에 사람이 읽는 그 한 줄이, 나쁜 테이크가 나쁜 영상으로 자라는 길을 막는다.

## 왜 주목할까

에이전트 세션을 길게 키우고 컨텍스트를 통째로 밀어 넣는 접근과 정반대에 있는 설계다. 어제 다룬 100만 토큰 붕괴 사례가 컨텍스트를 모으는 쪽의 한계를 보여줬다면, 이쪽은 잘게 쪼개고 사이마다 사람을 세운다. 스테이지마다 필요한 파일만 읽으니 컨텍스트 창에 걸리는 부담도 구조적으로 작아진다.

또 하나. 이 구조에는 모델이 없다. 파일을 읽는 AI라면 누구나 같은 폴더를 걸을 수 있다는 게 작성자의 말이다. 모델 교체 주기가 몇 주 단위로 짧아진 지금, 폴더와 계약서라는 모델 중립적 자산의 값은 오히려 커진다.

## 정리

성능 경쟁이 계속되는 동안 구조는 여전히 저평가돼 있다. Reads, Output, Person checks. 이 패턴은 영상 제작이 아니어도 옮겨 붙는다. 문서 작업, 코드 리뷰, 데이터 처리 어디든 단계 사이에 사람이 읽는 줄 하나를 넣을 수 있다. 영상 한 편에 폴더 하나, 스테이지마다 계약서 한 장. 화려하진 않지만 생각보다 큰 그림이다.
