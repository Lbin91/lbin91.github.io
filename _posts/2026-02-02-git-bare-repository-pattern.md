---
title: "아직도 git checkout 하세요? AI 시대의 로컬 개발 환경: Bare Repository 패턴"
date: 2026-02-02T16:00:00+09:00
categories:
  - development
tags:
  - Git
  - Worktree
  - Bare Repository
  - AI
  - Claude Code
  - Cursor
  - 생산성
---

개발자라면 하루에도 수십 번씩 겪는 상황이 있습니다.

`feature/a` 브랜치에서 열심히 코드를 짜고 있는데, 갑자기 긴급한 핫픽스 요청이 들어옵니다.

1. 하던 작업을 `git stash`로 꾸역꾸역 밀어 넣는다
2. `git checkout hotfix`로 브랜치를 바꾼다
3. `npm install`을 다시 돌린다 (운 없으면 node_modules 충돌)
4. IDE가 파일 변경을 감지하고 다시 인덱싱하느라 버벅인다
5. 수정 후 다시 원래 브랜치로 돌아와서 `git stash pop`을 한다

이 **"문맥 전환(Context Switching)"** 비용은 생각보다 큽니다. 단순히 집중력 문제뿐만 아니라, 빌드 캐시가 깨지고 IDE가 버벅이는 물리적인 시간 낭비도 포함되죠.

오늘은 이 구시대적인 checkout 방식에서 벗어나, **물리적으로 완벽하게 격리된 병렬 작업 환경**을 구축하는 Git Worktree와, 이를 가장 우아하게 관리하는 **Bare Repository 패턴**을 소개합니다.

특히, Cursor나 Claude Code 같은 AI 에이전트를 사용하는 분들이라면 이 세팅은 선택이 아니라 **필수**입니다.

---

## 1. checkout vs worktree: 책상 정리냐, 옆 책상이냐

가장 먼저 개념을 잡고 가야 합니다.

- **Checkout 방식**: 책상 하나(폴더 하나)에서 서류(코드)만 계속 갈아 끼우는 방식입니다. A 작업을 하다가 B 작업을 하려면 책상을 싹 치워야 합니다.
- **Worktree 방식**: 책상을 여러 개 두는 방식입니다. A 작업용 폴더, B 작업용 폴더가 물리적으로 따로 존재합니다. 몸만 이동하면 됩니다.

`git worktree`를 사용하면 다음과 같은 구조가 가능합니다:

```
/MyProject
    /feature-login    (로그인 기능 개발 중)
    /fix-crash        (크래시 수정 중)
    /refactor-ui      (UI 개선 중)
```

서버 A를 띄워놓은 상태에서 서버 B를 동시에 띄워 비교할 수도 있고, 빌드 아티팩트가 섞일 일도 없습니다.

---

## 2. 왜 하필 "Bare Repository"인가?

그냥 `git worktree`를 쓰면 되지, 왜 "Bare Repository"라는 낯선 개념을 가져와야 할까요?

일반적인 방식으로 worktree를 만들면 보통 메인 프로젝트 안에 하위 폴더로 생성하게 됩니다.

### ❌ 좋지 않은 구조 (Nested)

```
MyProject/
    .git/
    src/
    .worktrees/
        feature-A/
```

이 구조는 재앙입니다:

- `.gitignore` 관리가 꼬입니다 (메인 프로젝트가 worktree 폴더를 파일로 인식)
- IDE가 중복된 파일을 인덱싱해서 검색 결과가 지저분해집니다
- 상위 폴더의 설정(ESLint 등)이 하위 폴더에 의도치 않게 영향을 줍니다

그래서 우리는 **"부모 폴더"**를 하나 만들고, 그 안에 **".git 저장소(영혼)"**와 **"작업 폴더(육체)"**들을 나란히(Sibling) 배치하는 전략을 씁니다.

이것이 바로 **Bare Repository 패턴**입니다.

### ✅ 추천 구조 (Bare Pattern)

```
MyService-Workspace/
    .bare/          (Git 데이터만 있는 저장소)
    main/           (메인 브랜치)
    feature-auth/   (기능 A)
    fix-payment/    (기능 B)
```

---

## 3. 실전: 3분 만에 환경 구축하기

백문이 불여일타. 터미널을 열고 직접 세팅해 봅시다.

### Step 1. 워크스페이스 생성 및 Bare Clone

일반적인 clone이 아니라 `--bare` 옵션을 사용합니다.

```bash
mkdir MyService-Workspace
cd MyService-Workspace

# .bare라는 이름의 폴더로 Git 데이터만 받아옵니다
git clone --bare git@github.com:username/repo.git .bare
```

### Step 2. .bare 설정 변경 (★중요)

Bare 저장소는 기본적으로 원격 브랜치를 fetch 하지 않게 설정되어 있습니다. 로컬 작업을 위해 설정을 바꿔줍니다.

```bash
cd .bare
git config remote.origin.fetch "+refs/heads/*:refs/remotes/origin/*"
cd ..
```

### Step 3. 메인 브랜치 생성

이제 텅 빈 공간에 기준이 될 main 폴더를 만듭니다.

```bash
# .bare를 git directory로 지정하여 worktree 생성
git --git-dir=.bare worktree add main main
```

이제 `main` 폴더에 들어가서 평소처럼 개발하면 됩니다!

---

## 4. AI 에이전트 시대의 필수 생존 전략

이 글을 쓰게 된 진짜 이유는 사실 여기에 있습니다.

최근 **AI 에이전트를 활용한 개발(Agentic Workflow)**이 늘어나고 있습니다. checkout 방식으로 브랜치를 왔다 갔다 하면, 터미널에 상주하는 AI 에이전트는 "멘붕"에 빠집니다.

AI가 코드를 분석하고 수정 계획을 세웠는데, 갑자기 사용자가 브랜치를 바꿔버리면 파일 내용이 통째로 바뀌어 버립니다. 엉뚱한 코드에 패치를 적용하거나, 컨텍스트가 오염되는 사고가 발생하죠.

**Bare Repository 패턴을 쓰면?**

- **터미널 1** (feature-A 폴더): "Claude야, 로그인 API 리팩토링 해줘." → 시켜놓고 딴짓
- **터미널 2** (feature-B 폴더): "Claude야, 메인 페이지 CSS 수정해줘." → 시켜놓고 딴짓
- **터미널 3** (main 폴더): 나는 코드 리뷰 및 머지 진행

각 에이전트는 자신에게 할당된 폴더(Worktree) 안에 갇혀 있기 때문에, 서로의 작업을 침범하지도 않고, 내가 다른 일을 한다고 해서 꼬이지도 않습니다.

**완벽한 병렬 위임(Parallel Delegation)**이 가능해지는 것이죠.

---

## 5. 마무리하며

물론 단점도 있습니다. `node_modules`가 폴더마다 생기니 디스크 용량을 더 많이 차지합니다.

하지만 요즘 SSD 가격과 개발자의 생산성(특히 정신적 피로도)을 비교하면, 이건 충분히 지불할 만한 비용입니다.

### 한편으로는...

솔직히 말하면, 요즘 AI 에이전트들은 정말 똑똑해졌습니다.

Claude Code나 Cursor 같은 도구들은 이미 병렬 작업을 염두에 두고 설계되어 있어서, 단일 저장소에서도 브랜치 전환을 꽤 잘 처리합니다. 컨텍스트 관리도 예전보다 훨씬 안정적이고, 파일 변경 감지도 똑똑하게 대응하죠.

그래서 **"무조건 Bare Repository를 써야 한다"**고 주장하려는 건 아닙니다.

프로젝트 규모가 작거나, 브랜치 전환이 잦지 않다면 기존 방식으로도 충분합니다. 다만 여러 피처를 동시에 개발하거나, AI 에이전트를 여러 개 띄워서 병렬로 작업을 맡기는 환경이라면 Bare Repository 패턴이 확실한 이점을 줍니다.

결국 본인의 워크플로우에 맞게 선택하면 됩니다. 이 글이 그 선택지를 하나 더 늘려드렸다면 성공입니다.

> **"관리는 밖(Root)에서, 실행은 안(Worktree)에서."**

이 원칙만 기억하세요. checkout의 답답함에서 벗어나고 싶을 때, 이 글이 도움이 되길 바랍니다.

---

### 💡 Tip: Alias 등록하기

매번 긴 명령어를 치기 귀찮다면, `.zshrc`에 alias를 등록하세요:

```bash
alias gwt='git --git-dir=.bare worktree'
```

이제 `gwt add feature-x feature-x` 한 줄이면 새 작업 공간이 뚝딱 만들어집니다!
