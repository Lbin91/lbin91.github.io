# AGENTS.md — Blog Writing Guide

## Blog Overview

- **Platform**: Jekyll (Minimal Mistakes theme)
- **URL**: lbin91.github.io
- **Language**: Korean
- **Author**: Bongjin Lee (jay_bongss), iOS Developer
- **Topics**: AI, LLM, developer tools, home lab, tech trends

---

## Voice & Tone (MANDATORY)

### Reference File

**`_data/voice.yml`** — Threads(@jay_bongss) 게시글에서 추출한 화자 어투 분석.

**블로그 글을 작성하거나 수정할 때 반드시 이 파일을 먼저 읽고 적용한다.**

### Workflow

```
1. _data/voice.yml 읽기
2. sentence_endings (어미 패턴) 확인 → 해라체(~다, ~한다) 중심
3. emotion_patterns (감정 표현) 확인 → 솔직하게, 숨기지 않기
4. blog_rules (적용 규칙) 확인 → 정중체 금지, 괄호 메타코멘트 활용
5. transformation_examples (변환 예시) 참고 → Before/After 대조
6. 글 작성
```

### Core Rules

| Rule | Detail |
|------|--------|
| **종결어미** | 해라체(~다, ~한다) 중심. ~습니다, ~입니다 금지 |
| **감정** | 솔직하게. 불만·기쁨·당황을 숨기지 않는다 |
| **유머** | 자조적 유머 + 괄호 메타코멘트 적극 활용. 예: "(아님)" |
| **도입부** | 짧은 감상 한 줄로 시작 |
| **어휘** | 신조어/줄임말은 뉘앙스만 차용, 원형에 가깝게 정제 |
| **구조** | H2 섹션, 리스트, 코드블록 등 기존 구조는 유지. 목소리만 교체 |

### PERSONA.md Integration

`PERSONA.md`의 Writing Style도 참고하되, voice.yml의 규칙이 **우선**이다.

- PERSONA.md: "Professional yet approachable" → 해라체로 구현
- PERSONA.md: "Honest about learning curve" → 감정 솔직 표현으로 구현
- PERSONA.md: "Practical application focus" → 일상 경험담 + 실전 팁으로 구현

---

## Post Structure

### Front Matter

```yaml
---
title: "제목"
date: YYYY-MM-DDTHH:mm:ss+09:00
categories:
  - category
tags:
  - tag1
  - tag2
---
```

- `layout`은 명시하지 않는다. `_config.yml` defaults에서 `single`이 자동 적용된다.
- `categories`와 `tags`는 기존 포스트에서 사용된 것과 일관성 있게 유지한다.

### Filename

```
_posts/YYYY-MM-DD-slug.md
```

한글 슬러그도 허용된다. 예: `2026-04-20-구독-끝났다.md`

---

## Blog Categories

| Category | Usage |
|----------|-------|
| `ai` | AI/LLM 관련 글 |
| `development` | 개발 기술, 도구, 워크플로우 |
| `tech` | 기술 트렌드, 산업 동향 |
| `Home Lab` | 홈랩 구축, 서버 관리 |

---

## Commit Convention

- **Language**: Korean (한국어)
- **Format**: `type: 설명`
- **Types**: `feat`, `fix`, `refactor`, `docs`, `chore`, `글`
- **`글` type**: 블로그 포스트 내용 수정 시 사용
