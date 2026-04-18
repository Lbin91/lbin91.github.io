# 블로그 테마 변경 계획서

> 작성일: 2026-04-17
> 대상: lbin91.github.io (GitHub Pages / Jekyll)

---

## 1. 현재 상태 진단

### 현행 테마
- **Minimal Mistakes** (`mmistakes/minimal-mistakes`)
- 스킨: `default` (기본 밝은 테마)
- 레이아웃: `home` 인덱스 → 포스트 리스트
- 사이드바: 작성자 프로필 + 소셜 링크
- 네비게이션: Posts / Categories / Tags / About

### 문제점

| 항목 | 현황 | 개선 필요 |
|------|------|-----------|
| **타이틀** | `MM` (의미 불명확) | 작성자 정체성이 드러나는 타이틀 필요 |
| **스킨** | `default` (밋밋한 흰색) | 기술 블로그에 어울리는 다크/에이쿠아 톤 |
| **bio** | 장황한 키워드 나열 | 한 줄 카피로 임팩트 필요 |
| **About 페이지** | Lorem Ipsum 더미 텍스트 | 실제 소개글 필요 |
| **소셜 링크** | 모두 `https://` 빈 URL | 실제 링크 또는 제거 |
| **og:image** | 미설정 | SNS 공유 시 썸네일 없음 |
| **카테고리 구조** | `server`, `ai`, `tech` 혼재 | 체계적인 분류 체계 필요 |

---

## 2. 테마 변경 방안

### Option A: Minimal Mistakes 스킨 변경 (추천)

현재 테마를 유지하면서 스킨과 설정만 교체. 변경 리스크 최소화.

**적용 스킨: `dark` 또는 `contrast`**

```yaml
# _config.yml
minimal_mistakes_skin: "dark"  # 또는 "contrast"
```

**변경 내역:**

| 설정 | 변경 전 | 변경 후 |
|------|---------|---------|
| `title` | `MM` | `Bongjin Lee` 또는 `이봉진의 기술 블로그` |
| `description` | 기본 Jekyll 더미 텍스트 | `iOS Developer & AI-Augmented Builder. 6년차 iOS 개발자가 바라본 모바일·AI·홈랩 이야기.` |
| `minimal_mistakes_skin` | `default` | `dark` |
| `author.bio` | 키워드 나열 | `6년차 iOS Developer. AI를 개발 파트너로 쓰는 방법을 실험합니다.` |
| `author.links` | 빈 URL | GitHub 실제 링크, 나머지 제거 |
| `footer.links` | 빈 URL | GitHub만 유지 |
| `locale` | 미설정 | `ko-KR` |

### Option B: 테마 전체 교체

완전히 다른 Jekyll 테마로 갈아타는 방안. 작업량 많음.

| 후보 테마 | 특징 | 장점 | 단점 |
|-----------|------|------|------|
| **Chirpy** | 깔끔한 다크 테마, 카테고리·태그 강력 | 기술 블로그에 최적화 | 마이그레이션 공수 큼 |
| **Hydejack** | 프로페셔널, 이력서 내장 | 포트폴리오 겸용 | 유료 기능 多 |
| **TeXt** | 미니멀, 반응형 | 가볍고 빠름 | 커스텀 제한적 |

### 권장: Option A (스킨 변경)

이유:
1. **마이그레이션 리스크 제로** — 기존 12개 글의 레이아웃 호환성 보장
2. **작업 시간 1시간 이내** — `_config.yml` + About 페이지만 수정
3. **ProCrewz 지원 타이밍** — 빠른 정비가 우선

---

## 3. 상세 변경 항목

### 3.1 _config.yml 변경

```yaml
title: "Bongjin Lee"
email: bongjin.lee.dev@gmail.com
description: "iOS Developer & AI-Augmented Builder. 6년차 iOS 개발자가 바라본 모바일, AI, 홈랩 이야기."
timezone: Asia/Seoul
locale: ko-KR
minimal_mistakes_skin: dark

author:
  name: "Bongjin Lee"
  avatar: "/assets/images/profile_bear.png"
  bio: "6년차 iOS Developer. AI를 개발 파트너로 쓰는 방법을 실험합니다."
  links:
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/Lbin91"

footer:
  links:
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/Lbin91"
```

### 3.2 og:image 및 SEO

```yaml
# _config.yml에 추가
defaults:
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      comments: true
      share: true
      related: true
      og_image: "/assets/images/profile_bear.png"  # 기본 썸네일
```

### 3.3 About 페이지 재작성

```markdown
---
permalink: /about/
title: "About"
---

## 이봉진

iOS Developer · AI-Augmented Builder

6년간 3개 서비스를 기획부터 출시까지 단독 개발했습니다. 지금은 AI 코딩 도구를 개발 파트너로 활용해 새로운 가능성을 탐색 중입니다.

### 경력 하이라이트
- 엑소더스이엔티: 최애돌(Obj-C→Swift 마이그레이션), FLUV, FLIRTING, FILLIT 단독 개발
- 노마드소프트: 다수 고객사 iOS 앱 개발 (BLE, HealthKit, GPS, Firebase)
- 프리랜서(현재): AI 코딩 도구 비교 실험, 홈랩 구축, 다양한 사이드 프로젝트

### 이 블로그에서 다루는 것
- iOS/Swift 개발 실전 경험
- AI 코딩 도구 (Claude Code, Cursor, Codex) 활용기
- 홈랩 서버 구축 및 운영
- 오픈소스 AI 모델 리뷰

[이력서 보기 →](https://github.com/Lbin91)
```

### 3.4 네비게이션 수정

```yaml
# _data/navigation.yml
main:
  - title: "Posts"
    url: /posts/
  - title: "Categories"
    url: /categories/
  - title: "Tags"
    url: /tags/
  - title: "About"
    url: /about/
```
→ 현재와 동일하게 유지 (이미 적절함)

---

## 4. 실행 체크리스트

- [ ] `_config.yml` 스킨 `dark`로 변경
- [ ] `_config.yml` title, description, bio, locale 업데이트
- [ ] `_config.yml` 소셜 링크 정리 (GitHub만 남기기)
- [ ] `_pages/about.md` 실제 소개글로 교체
- [ ] 로컬 빌드 확인 (`bundle exec jekyll serve`)
- [ ] 모바일 반응형 확인
- [ ] GitHub Pages 배포 후 라이브 확인
