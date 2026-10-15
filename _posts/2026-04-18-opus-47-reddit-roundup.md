---
title: "Opus 4.7 나왔는데 사용자 반응이 심상치 않다"
date: 2026-04-18T14:00:00+09:00
excerpt: "Anthropic의 Claude Opus 4.7 발표 직후 사용자 커뮤니티에 쏟아진 반응을 바탕으로 성능, 비용, 거절 이슈를 정리했다."
categories:
  - ai
tags:
  - Claude
  - Opus 4.7
  - Anthropic
  - 사용자 반응
  - Claude Design
  - AI 코딩
  - 개발자 도구
---

Opus 4.7이 나왔는데 축제 분위기는커녕 장례식 분위기다.

Anthropic은 "역대 최강의 Opus 모델"이라고 온갖 찬사를 쏟아내며 Claude Opus 4.7을 발표했다. 발표 자료만 보면 장기 작업 처리, 지시 준수, 비전 성능까지 환상적인 전방위 업그레이드처럼 보인다. 그런데 사용자 반응은 정반대다. "더 좋아졌다"는 환호는 실종되고, "왜 이렇게 퇴보했냐", "불편해서 못 써먹겠다"는 아우성이 레딧을 뒤덮었다.

커뮤니티 반응만 보고 모델을 성급히 깎아내리는 건 조심해야 하지만, 이번만큼은 불만의 목소리가 너무나 일관적이다. 도대체 무엇이 개발자들을 이렇게 뿔나게 만들었는지 짚어본다.

## 공식 발표: "가장 강력한 Opus"

[공식 계정이 올린 발표 글](https://www.reddit.com/r/ClaudeAI/comments/1sn57af/introducing_claude_opus_47_our_most_capable_opus/)은 3,271개 추천과 788개 댓글을 기록했다. Anthropic이 강조한 포인트는 크게 두 가지였다.

> "더 엄격하게 장기 작업을 처리하고, 지시를 더 정확하게 따르며, 결과를 보고하기 전에 스스로 검증한다. 비전 능력도 대폭 향상되어 이미지를 기존보다 3배 이상 높은 해상도로 볼 수 있다."

요약하면 **강화된 추론·지시 준수 능력**과 **3배 향상된 비전 해상도**다. 여기에 Claude Design이라는 신규 디자인 도구도 함께 공개됐다.

문제는 첫 반응부터 축포가 아니라 깊은 한숨이었다는 점이다. 새 모델 발표인데 사용자들은 성능보다 한도부터 떠올린다(다소 씁쓸한 현실이다).

> "WHAT?! 이 포스트 읽기만 했는데도 월간 한도에 도달했어요 😭"
> — 댓글 작성자, **2,408개 추천**

성능보다 먼저 사용량 제한이 떠오른다는 건 이미 사용자 신뢰가 바닥을 쳤다는 방증이다. 나 역시 Claude 쓰다가 한도 경고 팝업이 뜨면 손가락이 굳어버린다(프롬프트를 아끼게 된다).

## "심각한 퇴보다"라는 반발이 더 크게 터졌다

발표 다음 날, [한 사용자가 올린 반론 글](https://www.reddit.com/r/ClaudeAI/comments/1snhfzd/claude_opus_47_is_a_serious_regression_not_an/)은 **2,891개 추천**과 **714개 댓글**을 기록했다. 공식 발표 글보다 추천 수가 더 높았다. 제목도 직설적이다. **"Claude Opus 4.7은 심각한 퇴보다. 업그레이드가 아니다."**

가장 공감을 많이 얻은 댓글은 대체로 비슷한 감각을 공유한다. "수치상 개선"보다 "실사용 품질 저하"를 먼저 느꼈다는 이야기다. 벤치마크가 아무리 좋아도, 실제 작업에서 멍청해지면 끝이다.

> "처음으로 동의한다. 이 모델은 4.6보다 못하다. 왜인지 설명할 수 없는데, 그냥 더 멍청해 보인다. 지시를 잘 안 따른다. 무슨 일이 있었던 거지?"
> — 댓글 작성자, **753개 추천**

> "오늘 4.7로 물리학 프로젝트를 계속 작업했는데, 모든 작업에서 완전히 실패했다. 솔직히 Sonnet 4.0이 선택된 줄 알았다."
> — 댓글 작성자, **294개 추천**

특히 일부 사용자는 원인을 **적응형 추론(Adaptive Reasoning)**에서 찾았다. 모델이 매 요청마다 추론 강도를 자체 판단하다 보니, 복잡한 작업에서도 지나치게 가볍게 응답하는 것 아니냐는 지적이다. 유용한 기능이긴 하지만, 사용자가 제어할 수 없으면 답답함이 먼저 밀려온다.

> "적응형 추론 때문이라고 본다. 모델이 스스로 추론하지 않거나 낮은 effort로 처리하도록 선택하고 있다. Extended 선택 옵션이 있으면 해결될 것이다. 가끔 간단한 프롬프트에서는 Extended Thinking을 켜야 정상 작동한다."
> — 댓글 작성자, **278개 추천**

400개 이상 댓글이 쌓인 뒤 auto-mod가 남긴 자동 요약도 냉정했다. **"평결이 내려졌고, 평가는 박하다."** 발표 직후의 커뮤니티 정서를 이보다 간단히 설명하기도 어렵다. 자동 요약 봇마저 싸늘하면 진짜 민망한 대목이다.

## 사용자가 가장 거칠게 반응한 지점은 "과도한 거절"이었다

[제목부터 요지를 드러낸 글](https://www.reddit.com/r/ClaudeAI/comments/1snuu1o/opus_47_with_literally_anything/)도 있었다. 1,530개 추천을 받은 이 글은 Opus 4.7의 **과도한 거절(over-refusal)** 문제를 정면으로 다뤘다.

> "유튜브 상원 청문회 영상에 대해 물어봤더니, 아동용 콘텐츠에는 코멘트하지 않는다고 하더라. 영상 속 네 사람 스크린샷을 보냈는데도."
> — 댓글 작성자, **107개 추천**

이 반응은 단순 해프닝으로 보이지 않았다. 한 댓글 작성자는 공식 발표에 적힌 문장을 근거로, Anthropic이 다음 세대 모델용 안전장치를 Opus 4.7에서 먼저 테스트하고 있다고 해석했다.

> "궁금한 분들을 위해: 공식 발표에서 Mythos 출시 전에 안전장치(safeguards)를 테스트하고 있다고 명시했다. 즉, Opus 4.7에서 과도하게 보수적인 필터링이 적용된 것은 의도적인 거다."
> — 댓글 작성자, **54개 추천**

해석이 맞다면 사용자 입장에서는 화가 치밀 수밖에 없다. 비싼 돈 내고 쓰는 상용 플래그십 모델에서 차세대 검열 정책의 마루타 노릇을 하고 있다는 뜻이기 때문이다. 유료 베타 테스터가 되고 싶은 사람은 아무도 없다.

## 50% 인상과 컨텍스트 퇴보의 이중고

[비용 문제를 정면으로 짚은 글](https://www.reddit.com/r/ClaudeAI/comments/1sn8ovi/opus_47_is_50_more_expensive_with_context/)은 653개 추천을 받았다. 여기서 반복해서 제기된 문제는 세 가지다.

1. **토크나이저 변경**으로 인해 Opus 4.7은 4.6 대비 1.35배, 타사 모델 대비 2배의 토큰을 소모
2. **컨텍스트 유지력 저하** — 더 비싼데 더 자주 맥락을 잃음
3. 이전 버전(Opus 4.6)도 이미 "게을러지고 둔해졌다"는 불만이 누적

> "입력 토큰 인상은 컨텍스트 품질이 개선된다면 괜찮을 것이다. 하지만 더 비싼 가격에 컨텍스트를 더 자주 잃는 건 최악의 조합이다."
> — 댓글 작성자, **202개 추천**

이 지점이 중요한 이유는 단순히 "가격이 비싸다"는 징징댐이 아니기 때문이다. 사용자들이 분노하는 본질은 **비용 상승이 품질 개선으로 이어지지 않는다**는 배신감이다. 비싸졌어도 밥값을 제대로 하면 궁시렁거리면서도 쓴다. 근데 비싸졌는데 더 자주 뻗고 멍청해지면 탈출 러시가 시작된다.

한 사용자는 더 넓은 시각에서 이런 질문도 던졌다.

> "LLM 가격이 너무 낮고 AI 기업들이 적자라는 이야기를 자주 본다. Uber가 초기처럼 말이다. 그렇다면 결국 가격이 올라가는 게 정상일 수도 있지 않나?"
> — 댓글 작성자, **71개 추천**

이 질문 자체는 타당하다. 다만 커뮤니티 분위기는 "가격 인상 자체"보다 "품질이 엉망인 상태에서의 기습 인상"에 훨씬 더 격렬히 반응했다. 돈을 더 내는 건 참아도, 멍청해진 모델에 호구 잡히는 건 못 참는다는 거다.

## Claude Design 공개에도 분위기는 반전되지 않았다

같은 날 발표된 [Claude Design](https://www.reddit.com/r/ClaudeAI/comments/1so3k1y/introducing_claude_design_by_anthropic_labs/)은 1,414개 추천을 받았다. Opus 4.7의 비전 능력을 앞세워 프로토타입, 슬라이드, 디자인 시스템을 생성하는 대화형 디자인 도구다.

반응은 극명하게 갈렸다.

> "Figma 입장에선 끔찍하겠네..."
> — 댓글 작성자, **203개 추천**

> "Claude, 빌드해... Claude..."
> — 댓글 작성자, **295개 추천**

새 기능 자체는 흥미롭다는 평가가 있었지만, 댓글 흐름은 다시 본론으로 돌아갔다. 많은 사용자가 디자인 도구 같은 곁가지보다 기존 모델의 안정성부터 수습하라고 쏘아붙였다. 집안 기둥이 흔들리는데 새 인테리어 들여놓는 꼴이다.

> "난 Claude Opus 4.7 extended thinking을 원한다..."
> — 댓글 작성자, **186개 추천**

auto-mod의 요약도 비슷했다. **"커뮤니티의 압도적 반응은 '됐고'."** 새 제품 발표가 불만을 덮지 못했다는 뜻이다. 이 정도면 Anthropic 발표팀도 속이 시커멓게 탔을 거다.

## 발표 전후로 기대와 피로감이 동시에 누적됐다

사실 Opus 4.7에 대한 기대감은 발표 전부터 하늘을 찔렀다. [The Information 보도를 인용한 글](https://www.reddit.com/r/ClaudeAI/comments/1slhkt8/the_information_anthropic_preps_opus_47_model/)은 "이번 주 내 출시 가능" 소식으로 636개 추천을 받았다. 하지만 발표 직후 [영구 rate limit 인상 발표](https://www.reddit.com/r/ClaudeAI/comments/1snc532/permanent_increase_in_rate_limits/)가 621개 추천을 받은 걸 보면, 관심의 방향은 곧바로 실사용 문제로 옮겨갔다.

[Opus 4.7 관련 논의](https://www.reddit.com/r/openclaw/comments/1sopqkz/opus_47_released_but_its_still_behind_preregression/)도 이어졌다. "출시됐지만 리그레션 이전 Opus에는 못 미친다"는 평가가 대표적이었다. 특정 커뮤니티만의 불평이라기보다, 에이전트 사용자 전반의 체감 성능이 떨어졌다는 뜻이다.

## 그래서 Opus 4.7, 써야 할까?

커뮤니티 반응을 한 표로 요약하면 이렇다.

| 측면 | 평가 |
|------|------|
| 비전 능력 | ✅ 실제로 3배 해상도 향상 체감 |
| 코딩·추론 | ⚠️ 4.6과 비슷하거나 약간 하락, 적응형 추론이 불안정 |
| 지시 준수 | ❌ 과도한 거절로 인해 체감 악화 |
| 토큰 비용 | ❌ 토크나이저 변경으로 35~50% 비용 증가 |
| 컨텍스트 | ⚠️ 긴 대화에서 더 자주 맥락 상실 |

핵심은 두 가지다. **적응형 추론의 널뛰기**와 **과도한 안전 필터링의 벽**이다. 기저 모델의 능력이 아무리 좋아졌더라도, 사용자가 체감하는 레이어에서 버그와 거절이 터지면 평가는 곤두박질칠 수밖에 없다.

여기에 토큰 가격 상승과 컨텍스트 이슈까지 겹쳤으니 민심이 흉흉할 만하다. "더 비싸졌고, 더 자주 멈추며, 더 자주 맥락을 까먹는다"는 구체적인 불만들이다. 기분 탓이라고 퉁치기엔 너무 뼈아프다.

특히 Anthropic이 "Mythos를 위한 안전장치 테스트"를 대놓고 언급한 대목은 씁쓸하다. Opus 4.7이 다음 세대 모델을 위한 시험용 프로토타입 취급을 받고 있다는 인상을 지우기 어렵기 때문이다. 월 \$100~200씩 내면서 마루타가 되고 싶은 개발자는 없다.

[Opus 4.6 시절의 토큰 폭탄과 서드파티 차단 논란](/ai/2026/04/16/claude-vs-chatgpt-developer-migration.html)에 이어, Opus 4.7마저 삐걱대고 있다. [오픈소스 대안으로의 전환 흐름](/ai/2026/04/18/open-source-ai-counterattack.html)이 괜히 불붙는 게 아니다.

나 역시 새 모델이 나오면 일단 결제부터 누르고 보는 얼리어답터지만, 이번 Opus 4.7만큼은 선뜻 메인으로 올리기 망설여진다. 모델이 아무리 이론상 똑똑하면 뭐하나. 내가 원할 때 묵묵히 코드를 짜주는 안정성이 없으면 개발자에겐 계륵일 뿐이다.

---

**참고 자료:**
- [Introducing Claude Opus 4.7 (공식)](https://www.reddit.com/r/ClaudeAI/comments/1sn57af/introducing_claude_opus_47_our_most_capable_opus/)
- [Claude Opus 4.7 is a serious regression](https://www.reddit.com/r/ClaudeAI/comments/1snhfzd/claude_opus_47_is_a_serious_regression_not_an/)
- [Opus 4.7 with literally anything](https://www.reddit.com/r/ClaudeAI/comments/1snuu1o/opus_47_with_literally_anything/)
- [Opus 4.7 is 50% more expensive](https://www.reddit.com/r/ClaudeAI/comments/1sn8ovi/opus_47_is_50_more_expensive_with_context/)
- [Introducing Claude Design](https://www.reddit.com/r/ClaudeAI/comments/1so3k1y/introducing_claude_design_by_anthropic_labs/)
- [Anthropic Preps Opus 4.7 - The Information](https://www.reddit.com/r/ClaudeAI/comments/1slhkt8/the_information_anthropic_preps_opus_47_model/)
- [Opus 4.7 released, still behind pre-regression Opus](https://www.reddit.com/r/openclaw/comments/1sopqkz/opus_47_released_but_its_still_behind_preregression/)
