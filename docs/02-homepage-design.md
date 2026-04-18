# 첫 화면(홈페이지) 설계서

> 작성일: 2026-04-17
> 대상: lbin91.github.io — 방문자가 처음 보는 페이지

---

## 1. 현재 첫 화면 문제점

현재 `index.html`은 Minimal Mistakes의 기본 `home` 레이아웃을 사용합니다:
- 포스트 리스트가 시간순으로 나열만 됨
- 작성자 정체성이 즉각 드러나지 않음
- 카테고리 구분 없이 모든 글이 섞여 보임
- 방문자가 "이 블로그가 뭘 다루나?"를 파악하기 어려움

---

## 2. 첫 화면 설계 방향

### 핵심 목표
> **방문자가 3초 안에 "이 사람이 누구고, 어떤 글을 쓰는지" 파악할 수 있어야 한다.**

### 페르소나 기반 설계

이봉진님의 정체성은 크게 3가지 축입니다:

| 축 | 키워드 | 대상 독자 |
|----|--------|-----------|
| **iOS Developer** | Swift, UIKit, SwiftUI, App Store | iOS 개발자, 채용 담당자 |
| **AI-Augmented Builder** | Claude Code, Cursor, 오픈소스 AI | AI 도구 관심 개발자 |
| **Home Lab Enthusiast** | 맥미니, PM2, 자동화 | 홈랩/셀프호스팅 관심자 |

---

## 3. 홈페이지 레이아웃 설계

### 구조 (위에서 아래로)

```
┌─────────────────────────────────────────┐
│  [사이드바]  │  [메인 콘텐츠 영역]          │
│              │                            │
│  프로필 사진  │  ┌──────────────────────┐  │
│  이름         │  │ Hero Section         │  │
│  한줄 소개    │  │ "iOS Developer &     │  │
│  GitHub 링크  │  │  AI-Augmented Builder"│  │
│              │  └──────────────────────┘  │
│              │                            │
│              │  ┌──────────────────────┐  │
│              │  │ Featured Posts       │  │
│              │  │ (카테고리별 대표글)    │  │
│              │  │ ┌─────┐ ┌─────┐     │  │
│              │  │ │ AI  │ │ iOS │     │  │
│              │  │ └─────┘ └─────┘     │  │
│              │  │ ┌─────┐ ┌─────┐     │  │
│              │  │ │Home │ │Srv  │     │  │
│              │  │ │Lab  │ │er   │     │  │
│              │  │ └─────┘ └─────┘     │  │
│              │  └──────────────────────┘  │
│              │                            │
│              │  ┌──────────────────────┐  │
│              │  │ Recent Posts         │  │
│              │  │ (최신 글 5개 리스트)  │  │
│              │  └──────────────────────┘  │
│              │                            │
└─────────────────────────────────────────┘
```

---

## 4. Section별 상세 설계

### 4.1 Hero Section

첫 화면 최상단. 정체성을 한눈에 보여주는 영역.

**내용:**
- 헤드라인: `iOS Developer & AI-Augmented Builder`
- 서브카피: `6년차 iOS 개발자. AI를 개발 파트너로 쓰는 방법을 실험합니다.`
- CTA: `[GitHub ↗]` `[이력서 ↗]`

**Jekyll 구현 방식:**

`index.html`을 커스텀 레이아웃으로 변경:

```yaml
---
layout: single
author_profile: true
---

## 최근 글

{% for post in site.posts limit:5 %}
  <!-- 포스트 카드 -->
{% endfor %}

## 카테고리별 대표글

### 🤖 AI & 개발 도구
{% for post in site.categories['ai'] limit:3 %}
  <!-- AI 카테고리 글 -->
{% endfor %}

### 📱 iOS & 모바일
<!-- iOS 카테고리 글 (향후 추가 시) -->

### 🏠 Home Lab
{% for post in site.categories['server'] limit:3 %}
  <!-- server 카테고리 글 -->
{% endfor %}

### 💡 Tech
{% for post in site.categories['tech'] limit:3 %}
  <!-- tech 카테고리 글 -->
{% endfor %}
```

### 4.2 Featured Posts (카테고리별 대표글)

각 카테고리에서 가장 조회수/반응이 좋은 글 1~2개를 카드 형태로 노출.

| 카테고리 | 추천 대표글 | 이유 |
|----------|------------|------|
| **AI** | `오픈소스 AI의 반격` (4/17) | 시리즈 첫 글, 검색 유입 가능성 높음 |
| **Tech** | `Meta의 React 핵심 인재` (4/19) | 트렌드 주제, 화제성 |
| **Server** | `맥미니 홈서버, PM2로` (1/29) | 실전 팁, 구글 검색 유입 기대 |

### 4.3 Recent Posts (최신 글 리스트)

기본 5개 노출. Minimal Mistakes 기본 pagination 유지.

---

## 5. 카테고리 재구성 제안

현재 카테고리 분포:

| 카테고리 | 글 수 | 비고 |
|----------|-------|------|
| `ai` | 3개 | 최근 집중 영역 |
| `tech` | 1개 | 분류 모호 |
| `server` | 4개 | 홈랩·인프라 |
| `Home Lab` | 1개 | 카테고리 불일치 (`server` vs `Home Lab`) |

### 제안 카테고리 체계

```
ai/          → AI 모델 리뷰, AI 코딩 도구, LLM 활용
ios/         → Swift, UIKit, SwiftUI, App Store (향후 글 작성 시)
server/      → 홈랩, PM2, 자동화, 인프라
devtools/    → 개발 도구, Claude Code, Cursor, Git
```

기존 글 카테고리 정정:

| 기존 파일 | 기존 카테고리 | 변경 제안 |
|-----------|-------------|-----------|
| `claude-vs-chatgpt-developer-migration.md` | `ai` | `devtools` |
| `open-source-ai-counterattack.md` | `ai` | `ai` (유지) |
| `glm-5-1-threatens-opus.md` | `ai` | `ai` (유지) |
| `meta-react-talent-exodus-to-expo.md` | `tech` | `devtools` |
| `claude-skill-guide.md` | (확인 필요) | `devtools` |
| `ssh-claude-code-tmux.md` | (확인 필요) | `devtools` |
| `home-lab-ai-agent-control-center.md` | `Home Lab, AI` | `server` |
| 그 외 server 글 | `server` | `server` (유지) |

---

## 6. 모바일 최적화 체크

- [ ] Hero 섹션 모바일에서 한 화면에 보이는지
- [ ] 카테고리 카드 1열로 정상 노출
- [ ] 사이드바 프로필이 모바일에서 상단으로 이동 (Minimal Mistakes 기본 동작)
- [ ] 폰트 사이즈 가독성 확인

---

## 7. 실행 체크리스트

- [ ] `index.html` 커스텀 Hero Section 추가
- [ ] 카테고리별 포스트 섹션 구현
- [ ] 기존 글 front matter 카테고리 정정 (필요시)
- [ ] 로컬 빌드 후 첫 화면 확인
- [ ] 모바일/데스크탑 레이아웃 확인
- [ ] 배포
