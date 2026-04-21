---
title: "Opus 4.7 나왔는데 Reddit 분위기가 심상치 않다"
date: 2026-04-18T14:00:00+09:00
excerpt: "Anthropic의 Claude Opus 4.7 발표 직후 Reddit에 쏟아진 반응을 바탕으로 성능, 비용, 거절 이슈를 정리했다."
categories:
  - ai
tags:
  - Claude
  - Opus 4.7
  - Anthropic
  - Reddit
  - Claude Design
  - AI 코딩
  - 개발자 도구
---

모델은 새로 나왔는데 사람들 표정은 별로 안 좋아 보인다.

Anthropic은 Claude Opus 4.7을 "가장 뛰어난 Opus 모델"이라고 소개했다. 발표 문구만 보면 장기 작업 처리, 지시 준수, 비전 성능까지 전방위 업그레이드처럼 보인다. 그런데 Reddit 반응은 정반대에 가깝다. "더 좋아졌다"보다 "왜 더 불편해졌지?"라는 불만이 훨씬 많이 보인다.

이번 글은 r/ClaudeAI와 관련 커뮤니티에서 올라온 반응을 기준으로, Opus 4.7이 왜 기대와 달리 역풍을 맞고 있는지 정리한 기록이다. 사실 Reddit 반응만 보고 모델을 단정하면 위험한데, 이번엔 불만의 방향이 꽤 일관적이라 그냥 넘기기 어렵다.

## 공식 발표: "가장 강력한 Opus"

r/ClaudeAI에 [공식 계정](https://www.reddit.com/r/ClaudeAI/comments/1sn57af/introducing_claude_opus_47_our_most_capable_opus/)이 올린 발표 글은 3,271 upvote와 788개 댓글을 기록했다. Anthropic이 강조한 포인트는 크게 두 가지였다.

> "더 엄격하게 장기 작업을 처리하고, 지시를 더 정확하게 따르며, 결과를 보고하기 전에 스스로 검증합니다. 비전 능력도 대폭 향상되어 이미지를 기존보다 3배 이상 높은 해상도로 볼 수 있습니다."

요약하면 **강화된 추론·지시 준수 능력**과 **3배 향상된 비전 해상도**다. 여기에 Claude Design이라는 신규 디자인 도구도 함께 공개됐다.

문제는 첫 반응부터 축포가 아니라 한숨이었다는 점이다. 새 모델 발표인데 다들 성능보다 한도부터 떠올린다. 이거 좀 슬픈 그림이다.

> "WHAT?! 이 포스트 읽기만 했는데도 월간 한도에 도달했어요 😭"
> — u/Sihtric, **2,408 upvote**

성능보다 먼저 사용량 제한이 떠오른다는 건, 이미 사용자 신뢰가 꽤 깎여 있다는 뜻이다. 나도 Claude 쓸 때 한도 문구가 보이면 갑자기 말수가 줄어든다(프롬프트도 절약 모드).

## "심각한 퇴보다"라는 반발이 더 크게 터졌다

발표 다음 날, [u/drivetheory가 올린 반론 글](https://www.reddit.com/r/ClaudeAI/comments/1snhfzd/claude_opus_47_is_a_serious_regression_not_an/)은 **2,891 upvote**와 **714개 댓글**을 기록했다. 공식 발표 글보다 추천 수가 더 높았다. 제목도 직설적이다. **"Claude Opus 4.7은 심각한 퇴보지, 업그레이드가 아니다."**

가장 공감을 많이 얻은 댓글은 대체로 비슷한 감각을 공유한다. "수치상 개선"보다 "실사용 품질 저하"를 먼저 느꼈다는 이야기다. 벤치마크가 아무리 좋아도, 내 작업에서 멍청해 보이면 끝이다.

> "처음으로 동의한다. 이 모델은 4.6보다 못하다. 왜인지 설명할 수 없는데, 그냥 더 멍청해 보인다. 지시를 잘 안 따른다. 무슨 일이 있었던 거지?"
> — u/BigMagnut, **753 upvote**

> "오늘 4.7로 물리학 프로젝트를 계속 작업했는데, 모든 작업에서 완전히 실패했다. 솔직히 Sonnet 4.0이 선택된 줄 알았다."
> — u/0KBL00MER, **294 upvote**

특히 일부 사용자는 원인을 **적응형 추론(Adaptive Reasoning)**에서 찾았다. 모델이 매 요청마다 추론 강도를 자체 판단하다 보니, 복잡한 작업에서도 지나치게 가볍게 응답하는 것 아니냐는 지적이다. 똑똑한 기능이긴 한데, 사용자가 제어할 수 없으면 답답함이 먼저 온다.

> "적응형 추론 때문이라고 본다. 모델이 스스로 추론하지 않거나 낮은 effort로 처리하도록 선택하고 있다. Extended 선택 옵션이 있으면 해결될 것이다. 가끔 간단한 프롬프트에서는 Extended Thinking을 켜야 정상 작동한다."
> — u/RevolutionaryBox5411, **278 upvote**

400개 이상 댓글이 쌓인 뒤 auto-mod가 남긴 자동 요약도 냉정했다. **"평결이 내려졌고, 평가는 박하다."** 발표 직후의 커뮤니티 정서를 이보다 간단히 설명하기도 어렵다. 자동 요약까지 싸늘하면 좀 민망하다.

## 사용자가 가장 거칠게 반응한 지점은 "과도한 거절"이었다

[제목부터 요지를 드러낸 글](https://www.reddit.com/r/ClaudeAI/comments/1snuu1o/opus_47_with_literally_anything/)도 있었다. 1,530 upvote를 받은 이 글은 Opus 4.7의 **과도한 거절(over-refusal)** 문제를 정면으로 다뤘다.

> "유튜브 상원 청문회 영상에 대해 물어봤더니, 아동용 콘텐츠에는 코멘트하지 않는다고 하더라. 영상 속 네 사람 스크린샷을 보냈는데도."
> — u/jillybombs, **107 upvote**

이 반응은 단순 해프닝으로 보이지 않았다. 한 댓글 작성자는 공식 발표에 적힌 문장을 근거로, Anthropic이 다음 세대 모델용 안전장치를 Opus 4.7에서 먼저 테스트하고 있다고 해석했다.

> "궁금한 분들을 위해: 공식 발표에서 Mythos 출시 전에 안전장치(safeguards)를 테스트하고 있다고 명시했습니다. 즉, Opus 4.7에서 과도하게 보수적인 필터링이 적용된 것은 의도적인 것입니다."
> — u/daniel-sousa-me, **54 upvote**

해석이 맞다면 사용자 입장에서는 불만이 커질 수밖에 없다. 돈을 내고 쓰는 상용 모델에서 차세대 필터링 정책의 시험대 역할을 하고 있다는 인상을 받기 때문이다. 유료 베타 테스터가 되고 싶은 사람은 별로 없다.

## 50% 인상과 컨텍스트 퇴보의 이중고

[비용 문제를 정면으로 짚은 글](https://www.reddit.com/r/ClaudeAI/comments/1sn8ovi/opus_47_is_50_more_expensive_with_context/)은 653 upvote를 받았다. 여기서 반복해서 제기된 문제는 세 가지다.

1. **토크나이저 변경**으로 인해 Opus 4.7은 4.6 대비 1.35배, 타사 모델 대비 2배의 토큰을 소모
2. **컨텍스트 유지력 저하** — 더 비싼데 더 자주 맥락을 잃음
3. 이전 버전(Opus 4.6)도 이미 "게을러지고 둔해졌다"는 불만이 누적

> "입력 토큰 인상은 컨텍스트 품질이 개선된다면 괜찮을 것이다. 하지만 더 비싼 가격에 컨텍스트를 더 자주 잃는 건 최악의 조합이다."
> — u/mymir-dev, **202 upvote**

이 지점이 중요한 이유는 단순히 "가격이 비싸다"는 불평이 아니기 때문이다. 사용자들이 진짜 문제 삼는 것은 **비용 상승이 품질 개선으로 연결되지 않는다**는 체감이다. 비싸졌는데 좋아졌으면 투덜거리면서도 쓴다. 비싸졌는데 더 자주 막히면 이야기가 달라진다.

한 사용자는 더 넓은 시각에서 이런 질문도 던졌다.

> "LLM 가격이 너무 낮고 AI 기업들이 적자라는 이야기를 자주 본다. Uber가 초기처럼 말이다. 그렇다면 결국 가격이 올라가는 게 정상일 수도 있지 않나?"
> — u/CalGuy456, **71 upvote**

이 질문 자체는 타당하다. 다만 Reddit 분위기는 "가격 인상 자체"보다 "품질이 흔들리는 상태에서의 가격 인상"에 훨씬 더 민감했다. 돈 더 내는 건 참을 수 있는데, 멍해진 모델에 돈 더 내는 건 못 참는 느낌.

## Claude Design 공개에도 분위기는 반전되지 않았다

같은 날 발표된 [Claude Design](https://www.reddit.com/r/ClaudeAI/comments/1so3k1y/introducing_claude_design_by_anthropic_labs/)은 1,414 upvote를 받았다. Opus 4.7의 비전 능력을 앞세워 프로토타입, 슬라이드, 디자인 시스템을 생성하는 대화형 디자인 도구다.

반응은 극명하게 갈렸다.

> "Figma 입장에선 끔찍하겠네..."
> — u/Federal_Cupcake_304, **203 upvote**

> "Claude, 빌드해... Claude..."
> — u/Hazrd_Design, **295 upvote**

새 기능 자체는 흥미롭다는 평가가 있었지만, 댓글 흐름은 다시 본론으로 돌아갔다. 많은 사용자가 디자인 도구보다 기존 모델의 안정성 문제를 먼저 해결하라고 요구했다. 새 장난감보다 기존 도구가 제대로 도는지가 먼저다.

> "난 Claude Opus 4.7 extended thinking을 원한다..."
> — u/Regular_Eggplant_248, **186 upvote**

auto-mod의 요약도 비슷했다. **"커뮤니티의 압도적 반응은 '됐고'."** 새 제품 발표가 불만을 덮지 못했다는 뜻이다. 이 정도면 발표팀도 속이 쓰릴 듯하다.

## 발표 전후로 기대와 피로감이 동시에 누적됐다

사실 Opus 4.7에 대한 기대감은 발표 전부터 높았다. [The Information 보도를 인용한 글](https://www.reddit.com/r/ClaudeAI/comments/1slhkt8/the_information_anthropic_preps_opus_47_model/)은 "이번 주 내 출시 가능"이라는 소식으로 636 upvote를 받았다. 하지만 발표 직후 [영구 rate limit 인상 발표](https://www.reddit.com/r/ClaudeAI/comments/1snc532/permanent_increase_in_rate_limits/)가 621 upvote를 받은 걸 보면, 관심의 방향은 곧바로 사용성 문제로 이동했다.

r/openclaw에서도 [Opus 4.7 관련 논의](https://www.reddit.com/r/openclaw/comments/1sopqkz/opus_47_released_but_its_still_behind_preregression/)가 이어졌다. "출시됐지만 리그레션 이전 Opus에는 못 미친다"는 평가가 대표적이었다. 특정 서브레딧만의 불만이라기보다, 에이전트 사용자 전반의 체감 저하로 읽힌다.

## 그래서 Opus 4.7, 써야 하나?

Reddit 반응을 한 표로 정리하면 이렇다.

| 측면 | 평가 |
|------|------|
| 비전 능력 | ✅ 실제로 3배 해상도 향상 체감 |
| 코딩·추론 | ⚠️ 4.6과 비슷하거나 약간 하락, 적응형 추론이 불안정 |
| 지시 준수 | ❌ 과도한 거절로 인해 체감 악화 |
| 토큰 비용 | ❌ 토크나이저 변경으로 35~50% 비용 증가 |
| 컨텍스트 | ⚠️ 긴 대화에서 더 자주 맥락 상실 |

핵심은 두 가지다. **적응형 추론의 불안정한 노출**과 **과도한 안전 필터링**이다. 기저 모델의 능력이 실제로 개선됐더라도, 사용자가 직접 체감하는 레이어에서 품질이 무너지면 평가는 나빠질 수밖에 없다.

여기에 가격과 컨텍스트 이슈까지 겹치면서 불만은 더 커졌다. 성능 논쟁은 원래 주관적일 수 있지만, "더 비싸졌고 더 자주 막히며 더 자주 맥락을 잃는다"는 인식은 꽤 구체적이다. 이 정도면 그냥 기분 탓이라고 넘기기 어렵다.

특히 Anthropic이 공식 발표에서 "Mythos를 위한 안전장치 테스트"를 언급한 대목은 민감하다. Opus 4.7 자체보다 다음 세대 모델을 위한 정책 실험의 성격이 강하다는 인상을 주기 때문이다. 사용자들은 월 $100~200을 내고 베타 테스터 역할을 하고 싶어 하지 않는다.

[Opus 4.6 시절의 토큰 폭탄과 서드파티 차단 논란](/ai/2026/04/16/claude-vs-chatgpt-developer-migration.html)에 이어, Opus 4.7은 또 다른 불만을 양산하고 있다. [오픈소스 대안으로의 전환 흐름](/ai/2026/04/18/open-source-ai-counterattack.html)이 가속될 수밖에 없는 이유다.

정리하면, Opus 4.7은 발표 자료만 보면 진화다. 하지만 Reddit 기준 실사용 체감은 아직 퇴보에 가깝다. Anthropic이 적응형 추론 제어권과 과도한 거절 문제를 얼마나 빨리 손보느냐에 따라, 이 모델의 평가는 얼마든지 뒤집힐 수 있다.

나도 새 모델 나오면 일단 써보는 쪽인데, 이번 반응을 보면 바로 메인으로 올리긴 좀 고민된다. 모델이 똑똑한 것도 중요하지만, 내가 원하는 순간에 원하는 만큼 일을 해주는지가 더 중요하니까.

---

**참고 자료:**
- [Introducing Claude Opus 4.7 - r/ClaudeAI (공식)](https://www.reddit.com/r/ClaudeAI/comments/1sn57af/introducing_claude_opus_47_our_most_capable_opus/)
- [Claude Opus 4.7 is a serious regression - r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/comments/1snhfzd/claude_opus_47_is_a_serious_regression_not_an/)
- [Opus 4.7 with literally anything - r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/comments/1snuu1o/opus_47_with_literally_anything/)
- [Opus 4.7 is 50% more expensive - r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/comments/1sn8ovi/opus_47_is_50_more_expensive_with_context/)
- [Introducing Claude Design - r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/comments/1so3k1y/introducing_claude_design_by_anthropic_labs/)
- [Anthropic Preps Opus 4.7 - The Information - r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/comments/1slhkt8/the_information_anthropic_preps_opus_47_model/)
- [Opus 4.7 released, still behind pre-regression Opus - r/openclaw](https://www.reddit.com/r/openclaw/comments/1sopqkz/opus_47_released_but_its_still_behind_preregression/)
