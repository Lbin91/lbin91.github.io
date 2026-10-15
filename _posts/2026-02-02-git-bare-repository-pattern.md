---
title: "아직도 git checkout 하나? AI 시대의 로컬 개발 환경: Bare Repository 패턴"
date: 2026-02-02T16:00:00+09:00
excerpt: "AI 시대에 git worktree + bare repository로 동시 다작업 환경 구축하기"
categories:
  - devtools
tags:
  - Git
  - Worktree
  - Bare Repository
  - AI
  - Claude Code
  - Cursor
  - 생산성
---

하루에도 수십 번씩 git stash와 checkout을 반복하다 보면 진짜 멘탈이 나간다.

개발자라면 누구나 한 번쯤(아니 매일) 겪는 끔찍한 상황이 있다.

`feature/a` 브랜치에서 한창 삘받아서 코드를 짜고 있는데, 슬랙으로 불길한 알림이 뜬다. "급한 핫픽스 건 하나만 먼저 봐주세요!"

1. 하던 작업을 `git stash`로 꾸역꾸역 밀어 넣는다.
2. `git checkout hotfix`로 브랜치를 갈아탄다.
3. `npm install`이나 패키지 설치를 다시 돌린다(재수 없으면 라이브러리 버전 충돌 터진다).
4. IDE가 미친 듯이 파일 변경 감지하고 인덱싱하느라 팬이 굉음을 내며 버벅인다.
5. 핫픽스 고치고 다시 원래 브랜치로 돌아와서 `git stash pop`을 한다(충돌 나면 혈압 급상승).

이 빌어먹을 **"문맥 전환(Context Switching)"** 비용은 생각보다 진짜 크다. 집중력 와르르 깨지는 건 기본이고, 빌드 캐시 싹 날아가고 IDE 뻗어버리는 물리적 시간 낭비까지 당하면 영혼이 탈탈 털린다.

오늘은 이 구시대적인 checkout 노가다에서 벗어나, **물리적으로 완벽하게 격리된 병렬 작업 환경**을 구축하는 Git Worktree와, 이를 가장 깔끔하게 관리하는 **Bare Repository 패턴**을 정리해 본다.

특히 Cursor나 Claude Code 같은 AI 코딩 에이전트를 달고 사는 개발자라면, 이 세팅은 선택이 아니라 **생존 필수템**이다.

---

## 1. checkout vs worktree: 책상 정리냐, 옆 책상이냐

개념부터 확실하게 잡고 가자.

- **Checkout 방식**: 좁은 책상 하나(단일 폴더)에서 서류(코드)만 계속 갈아치우는 방식이다. A 작업 하다가 B 작업 하려면 책상 위를 싹 쓸어서 서랍에 쑤셔 박아야 한다.
- **Worktree 방식**: 사무실에 책상을 여러 개 깔아두는 방식이다. A 작업용 책상, B 작업용 책상이 물리적으로 따로 있다. 그냥 몸만 쓱 이동하면 끝이다.

`git worktree`를 사용하면 이런 아름다운 구조가 가능해진다:

```
/MyProject
    /feature-login    (로그인 기능 개발 중)
    /fix-crash        (크래시 수정 중)
    /refactor-ui      (UI 개선 중)
```

서버 A를 띄워놓은 상태에서 옆 폴더의 서버 B를 동시에 띄워 비교할 수도 있고, 빌드 아티팩트나 캐시가 섞여서 스트레스받을 일도 전혀 없다.

---

## 2. 왜 하필 "Bare Repository"인가?

"그냥 `git worktree` 쓰면 되지, 왜 굳이 Bare Repository라는 생소한 개념까지 끌고 와야 하나?" 싶을 수 있다.

일반적인 방식으로 worktree를 처음 만들면 보통 메인 프로젝트 폴더 하위에 대충 만들게 된다.

### ❌ 좋지 않은 구조 (Nested)

```
MyProject/
    .git/
    src/
    .worktrees/
        feature-A/
```

이 구조는 써보면 재앙 그 자체다:

- `.gitignore` 관리가 꼬인다 (메인 프로젝트가 worktree 폴더를 일반 파일이나 하위 모듈로 인식해 버린다)
- IDE가 중복된 파일을 싹 다 인덱싱해서 파일 검색할 때마다 같은 파일이 줄줄이 뜬다
- 상위 폴더의 린트나 환경 설정(ESLint, tsconfig 등)이 하위 폴더에 의도치 않게 간섭한다

그래서 우리는 **"부모 루트 폴더"**를 하나 만들고, 그 안에 **".git 저장소(영혼)"**와 **"작업 폴더(육체)"**들을 나란히(Sibling) 배치하는 전략을 쓴다.

이것이 바로 **Bare Repository 패턴**이다.

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

백문이 불여일타다. 터미널을 열고 직접 세팅해 보자.

### Step 1. 워크스페이스 생성 및 Bare Clone

일반적인 clone 대신 `--bare` 옵션을 쓴다.

```bash
mkdir MyService-Workspace
cd MyService-Workspace

# .bare라는 이름의 폴더로 Git 데이터만 받아온다
git clone --bare git@github.com:username/repo.git .bare
```

### Step 2. .bare 설정 변경 (★중요)

Bare 저장소는 태생이 서버용이라 기본적으로 원격 브랜치를 fetch하지 않도록 설정되어 있다. 로컬에서 편하게 작업하려면 이 설정을 반드시 바꿔줘야 한다(이거 안 하고 왜 원격 브랜치 안 가져오냐고 울부짖는 사람 여럿 봤다).

```bash
cd .bare
git config remote.origin.fetch "+refs/heads/*:refs/remotes/origin/*"
cd ..
```

### Step 3. 메인 브랜치 생성

이제 텅 빈 공간에 기준이 될 main 폴더를 만든다.

```bash
# .bare를 git directory로 지정하여 worktree 생성
git --git-dir=.bare worktree add main main
```

이제 `main` 폴더에 들어가서 평소처럼 개발하면 된다!

---

## 4. AI 에이전트 시대의 필수 생존 전략

이 글을 쓰게 된 진짜 이유는 사실 여기에 있다.

최근 **AI 에이전트를 활용한 개발(Agentic Workflow)**이 대세로 자리 잡고 있다. 그런데 옛날처럼 checkout 방식으로 브랜치를 와리가리하면, 터미널에 상주하는 AI 에이전트는 십중팔구 "멘붕"에 빠진다.

AI가 코드를 싹 분석하고 수정 계획까지 세워놨는데, 사용자가 뒤에서 몰래 브랜치를 휙 바꿔버리면 파일 내용이 통째로 뒤바뀐다. 엉뚱한 코드에 패치를 때려 박거나, 컨텍스트가 심각하게 오염되는 대형 사고가 터진다.

**Bare Repository 패턴을 쓰면 어떻게 될까?**

- **터미널 1** (feature-A 폴더): "Claude야, 로그인 API 리팩토링해라." → 던져두고 딴짓
- **터미널 2** (feature-B 폴더): "Cursor야, 메인 페이지 UI 고쳐봐라." → 던져두고 딴짓
- **터미널 3** (main 폴더): 나는 여유롭게 코드 리뷰 및 머지 진행

각 에이전트는 자신에게 할당된 폴더(Worktree) 안에 얌전히 갇혀 있기 때문에, 서로의 작업을 침범할 일도 없고, 내가 다른 짓을 한다고 해서 꼬이지도 않는다.

**완벽한 병렬 위임(Parallel Delegation)**이 가능해지는 셈이다.

---

## 5. 마무리하며

물론 단점도 있다. 폴더마다 `node_modules`나 빌드 캐시가 생기니 디스크 용량을 야금야금 갉아먹는다.

하지만 요즘 SSD 용량 가격과 내 멘탈 보존 비용(그리고 개발 생산성)을 저울질해 보면, 이 정도는 기꺼이 지불할 만한 가치가 있다.

### 한편으로는...

솔직히 말하면 요즘 AI 에이전트 도구들은 무서울 정도로 똑똑해졌다.

Claude Code나 Cursor 같은 도구들은 이미 병렬 작업을 염두에 두고 설계되어 있어서, 단일 저장소에서도 브랜치 전환을 제법 잘 처리한다. 컨텍스트 관리도 예전보다 훨씬 안정적이고, 파일 변경 감지도 똑똑하게 대응한다.

그래서 **"무조건 무지성으로 Bare Repository만 써야 한다"**고 억지 부릴 생각은 없다.

프로젝트 규모가 작거나 브랜치 전환이 잦지 않다면 기존 방식으로도 충분하다. 다만 여러 피처를 동시에 쳐내야 하거나, AI 에이전트를 둘셋씩 띄워놓고 병렬로 작업을 맡기는 헤비 유저라면 Bare Repository 패턴이 주는 해방감은 차원이 다르다.

결국 본인의 워크플로우에 맞게 선택하면 된다. 이 글이 그 선택지를 하나 더 넓혀줬다면 그걸로 대만족이다.

> **"관리는 밖(Root)에서, 실행은 안(Worktree)에서."**

이 원칙 딱 하나만 기억하자. 지긋지긋한 checkout 지옥에서 벗어나고 싶을 때, 이 글이 작은 구원이 되길 바란다.

---

### 💡 Tip: Alias 등록하기

매번 저 긴 명령어를 치다간 손가락 관절부터 나간다. `.zshrc`에 alias를 박아두자:

```bash
alias gwt='git --git-dir=.bare worktree'
```

이제 `gwt add feature-x feature-x` 딱 한 줄이면 독립된 새 작업 공간이 뚝딱 만들어진다!
