# 기존 글 수정 가이드

> 작성일: 2026-04-17
> 대상: lbin91.github.io — 기존 12개 글 정비

---

## 1. 수정이 필요한 이유

ProCrewz iOS Engineer 포지션 지원을 앞두고, 블로그가 곧 포트폴리오 역할을 합니다. 현재 글들에는 다음 문제가 있습니다:

| 문제 | 영향 | 우선순위 |
|------|------|----------|
| About 페이지가 Lorem Ipsum | 포트폴리오로서 신뢰도 제로 | 🔴 긴급 |
| 카테고리 불일치 (`server` vs `Home Lab` vs `development`) | 분류 체계 엉킴 | 🟡 중간 |
| 시리즈 연결 약함 (nanobot 1편→2편) | 독자 이탈 | 🟡 중간 |
| 글 내용 시의성 (AI 모델 스펙 변화) | 정보 부정확 가능 | 🟢 점진적 |
| SEO 메타 누락 (og:image, excerpt) | 검색 노출 약화 | 🟡 중간 |

---

## 2. 글별 수정 항목

### 🔴 Tier 1: 즉시 수정 (지원 전 필수)

#### 2.1 About 페이지 — 전면 재작성

**파일**: `_pages/about.md`

현재: Lorem Ipsum 더미 텍스트
수정: 실제 소개글 (01-blog-theme-change.md의 3.3 About 페이지 재작성 참고)

#### 2.2 _config.yml — 작성자 정보 정정

**파일**: `_config.yml`

| 항목 | 현재값 | 수정값 |
|------|--------|--------|
| `title` | `MM` | `Bongjin Lee` |
| `description` | Jekyll 기본 텍스트 | 블로그 설명 문장 |
| `author.bio` | 키워드 나열 | 한 줄 카피 |
| `author.links` | 빈 URL 4개 | GitHub 실제 링크만 |
| `twitter_username` | `username` | 제거 또는 실제값 |
| `github_username` | `username` | `Lbin91` |

---

### 🟡 Tier 2: 카테고리 정정

#### 기존 글 카테고리 현황 및 변경안

| # | 파일명 | 현재 카테고리 | 변경 제안 | 사유 |
|---|--------|-------------|-----------|------|
| 1 | `2026-01-29-mac-mini-server-setup-with-pm2.md` | `server` | `server` | 유지 |
| 2 | `2026-02-02-git-bare-repository-pattern.md` | `development` | `devtools` | 통일 |
| 3 | `2026-02-12-claude-skill-guide.md` | `ai` | `devtools` | Claude 도구 관련 |
| 4 | `2026-02-13-ssh-claude-code-tmux.md` | `server` | `server` | 유지 |
| 5 | `2026-02-15-nanobot-setup-part1.md` | `server` | `server` | 유지 |
| 6 | `2026-02-16-nanobot-setup-part2.md` | `server` | `server` | 유지 |
| 7 | `2026-02-18-rag-cost-effective-llm-models.md` | `ai` | `ai` | 유지 |
| 8 | `2026-02-19-home-lab-ai-agent-control-center.md` | `Home Lab, AI` | `server` | 카테고리 통일 |
| 9 | `2026-04-16-claude-vs-chatgpt-developer-migration.md` | `ai` | `devtools` | AI 도구 비교 |
| 10 | `2026-04-17-open-source-ai-counterattack.md` | `ai` | `ai` | 유지 |
| 11 | `2026-04-18-glm-5-1-threatens-opus.md` | `ai` | `ai` | 유지 |
| 12 | `2026-04-19-meta-react-talent-exodus-to-expo.md` | `tech` | `devtools` | 개발 도구 생태계 |

---

### 🟡 Tier 2: 시리즈 연결 강화

#### Nanobot 시리즈 (2편)

**파일**: `2026-02-15-nanobot-setup-part1.md`, `2026-02-16-nanobot-setup-part2.md`

**추가할 내용:**
- 1편 하단에 "다음 글: [2편 링크]" 추가
- 2편 상단에 "이전 글: [1편 링크]" 추가
- front matter에 `series: "nanobot-setup"` 필드 추가

**template:**
```markdown
---
# 기존 front matter에 추가
series: "nanobot-setup"
---

<!-- 글 하단에 추가 -->
---
> 📖 **Nanobot 설치기 시리즈**
> - 1편: 메인 서버를 지켜줄 AI 비서 찾기 (현재 글)
> - 2편: [Config의 늪과 PM2 무중단 배포 →](/server/nanobot-setup-part2/)
```

#### AI 시리즈 (오픈소스 AI 3부작)

**파일**: 
- `2026-04-17-open-source-ai-counterattack.md` (1부)
- `2026-04-18-glm-5-1-threatens-opus.md` (2부)

**추가할 내용:**
- 1부 하단에 "다음 글: GLM-5.1 심층 분석 →" 추가
- 2부 상단에 "이전 글: 오픈소스 AI의 반격 →" 추가
- front matter에 `series: "opensource-ai-2026"` 추가

---

### 🟡 Tier 2: SEO 메타 보강

모든 글에 다음 front matter 필드 추가:

```yaml
excerpt: "이 글의 핵심 요약 한 문장 (검색결과에 표시됨)"
header:
  teaser: "/assets/images/teaser-default.png"  # 기본 티저 이미지
```

#### 글별 추천 excerpt

| 글 | 추천 excerpt |
|----|-------------|
| 맥미니 PM2 | `iOS 개발자가 백엔드 초보의 시선으로 맥미니 홈서버에 PM2를 도입하며 겪은 시행착오` |
| Git Bare Repository | `AI 시대에 git worktree + bare repository로 동시 다작업 환경 구축하기` |
| Claude Skill | `프롬프트 복붙의 시대는 끝났다. Claude Skill 시스템으로 반복 워크플로우를 자동화하는 법` |
| SSH Claude Code tmux | `맥미니 서버에서 SSH로 Claude Code 인증을 유지하는 방법, tmux 활용법 정리` |
| Nanobot 1편 | `홈 서버에 설치하기엔 불안했던 OpenClaw. 더 안전한 AI 비서 대안을 찾아 나선 이야기` |
| Nanobot 2편 | `Nanobot config 파일의 늪에 빠지고, PM2로 무중단 배포까지 달성한 과정` |
| RAG 가성비 LLM | `RAG로 돈 버는 법. 실제 서비스에 적합한 가성비 LLM 모델 총정리` |
| AI 관제탑 | `텔레그램, 슬랙, 디스코드를 직접 비교한 AI 에이전트 관제 센터 선택 가이드` |
| Claude vs ChatGPT | `Claude에서 ChatGPT로 갈아타는 개발자들이 늘고 있다. 두 도구를 실전에서 비교해봤다` |
| 오픈소스 AI 반격 | `GLM-5.1, Qwen, Gemma 등 오픈소스 AI 모델이 폐쇄형 모델을 위협하는 이유` |
| GLM-5.1 | `744B 파라미터 MIT 라이선스. Z.ai의 GLM-5.1이 Opus를 위협하는 이유` |
| Meta→Expo | `Meta React 핵심 인재들이 Expo로 이동하는 이유와 AI Agent 시대의 시사점` |

---

### 🟢 Tier 3: 점진적 개선 (이후 진행)

- 글 내용 업데이트 (AI 모델 스펙 변경 반영)
- 코드 블록 하이라이팅 개선
- 이미지/다이어그램 추가
- 관련 글 링크 (cross-reference) 보강

---

## 3. 실행 순서

```
1차 (지원 전): About 페이지 재작성 + _config.yml 정정
2차 (테마 변경과 병행): 카테고리 정정 + 시리즈 연결 + SEO 메타
3차 (이후): 글 내용 업데이트 + 이미지 추가
```

---

## 4. 글별 상세 수정 체크리스트

- [ ] `_pages/about.md` — 전면 재작성
- [ ] `_config.yml` — title, description, bio, 링크 정정
- [ ] `git-bare-repository-pattern.md` — 카테고리 `development` → `devtools`
- [ ] `claude-skill-guide.md` — 카테고리 `ai` → `devtools`
- [ ] `home-lab-ai-agent-control-center.md` — 카테고리 `Home Lab, AI` → `server`
- [ ] `claude-vs-chatgpt-developer-migration.md` — 카테고리 `ai` → `devtools`
- [ ] `meta-react-talent-exodus-to-expo.md` — 카테고리 `tech` → `devtools`
- [ ] Nanobot 1편 — 시리즈 네비게이션 추가
- [ ] Nanobot 2편 — 시리즈 네비게이션 추가
- [ ] 오픈소스 AI 반격 — 시리즈 네비게이션 추가
- [ ] GLM-5.1 — 시리즈 네비게이션 추가
- [ ] 전체 글 — excerpt 추가
