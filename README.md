# 2026 CUAI Datathon

CUAI 동아리 데이터톤 프로젝트 저장소입니다.

---

## 목차

1. [Git이란?](#1-git이란)
2. [초기 설정](#2-초기-설정)
3. [프로젝트 시작하기](#3-프로젝트-시작하기)
4. [Python 환경설정 (venv)](#4-python-환경설정-venv)
5. [브랜치 전략](#5-브랜치-전략)
6. [기본 작업 흐름](#6-기본-작업-흐름)
7. [커밋 메시지 규칙](#7-커밋-메시지-규칙)
8. [Pull Request (PR) 만들기](#8-pull-request-pr-만들기)
9. [자주 쓰는 명령어 모음](#9-자주-쓰는-명령어-모음)
10. [문제 상황 해결](#10-문제-상황-해결)

---

## 1. Git이란?

Git은 **여러 사람이 같은 코드를 동시에 작업**할 수 있게 해주는 버전 관리 도구입니다.

쉽게 말하면:
- Google Docs에서 여러 사람이 동시에 문서를 편집하는 것처럼, 코드를 여러 사람이 함께 작업할 수 있습니다.
- 모든 변경 이력이 저장되어, 언제든지 이전 상태로 되돌릴 수 있습니다.
- **GitHub**는 Git으로 관리하는 코드를 인터넷에 저장하고 공유하는 서비스입니다.

### 핵심 개념 한눈에 보기

```
내 컴퓨터 (Local)                     GitHub (Remote)
┌──────────────────────┐              ┌──────────────────┐
│  작업 폴더            │   git push   │                  │
│  (Working Directory) │ ──────────►  │  원격 저장소      │
│         │            │              │  (Remote Repo)   │
│    git add           │   git pull   │                  │
│         ▼            │ ◄──────────  │                  │
│  스테이징 영역        │              └──────────────────┘
│  (Staging Area)      │
│         │            │
│   git commit         │
│         ▼            │
│  로컬 저장소          │
│  (Local Repo)        │
└──────────────────────┘
```

| 용어 | 설명 |
|------|------|
| **Repository (저장소)** | 프로젝트의 모든 파일과 변경 이력이 저장되는 공간 |
| **Commit (커밋)** | 변경사항을 저장하는 하나의 단위 (게임의 세이브 포인트와 비슷) |
| **Branch (브랜치)** | 독립적으로 작업할 수 있는 갈래. 다른 사람의 작업에 영향을 주지 않음 |
| **Push (푸시)** | 내 컴퓨터의 커밋을 GitHub에 올리는 것 |
| **Pull (풀)** | GitHub의 최신 코드를 내 컴퓨터로 가져오는 것 |
| **Pull Request (PR)** | 내 브랜치의 작업을 다른 브랜치에 합쳐달라고 요청하는 것 |

---

## 2. 초기 설정

### 2-1. Git 설치

- [Git 다운로드 (Windows)](https://git-scm.com/download/win)에서 설치 파일을 다운받아 설치합니다.
- 설치 시 모든 옵션은 기본값으로 두고 Next를 눌러 진행하면 됩니다.

### 2-2. Git 사용자 정보 등록

터미널(Git Bash, VS Code 터미널 등)을 열고 아래 명령어를 입력합니다.

```bash
git config --global user.name "본인이름"
git config --global user.email "본인GitHub이메일@example.com"
```

> 이 정보는 커밋할 때 "누가 이 변경을 했는지" 기록하는 데 사용됩니다.

### 2-3. GitHub 계정 연동

GitHub에 push할 때 인증이 필요합니다. 아래 중 하나를 설정해주세요.

**방법 A: VS Code에서 자동 로그인 (가장 쉬움)**
1. VS Code 좌측 하단의 사람 아이콘 클릭
2. "Sign in with GitHub" 선택
3. 브라우저에서 GitHub 로그인 후 승인

**방법 B: GitHub CLI 사용**
```bash
# GitHub CLI 설치 후
gh auth login
# 안내에 따라 로그인 진행
```

---

## 3. 프로젝트 시작하기

### 3-1. 저장소 복제 (처음 한 번만)

```bash
git clone https://github.com/brian-223134/datathon.git
cd datathon
```

> `git clone`은 GitHub에 있는 프로젝트를 내 컴퓨터에 복사하는 명령어입니다.

### 3-2. 내 브랜치 만들기 (처음 한 번만)

```bash
# develop 브랜치로 이동
git checkout develop

# 내 이름으로 브랜치 생성 및 이동
git checkout -b 본인이름
# 예: git checkout -b jiyoung
```

---

## 4. Python 환경설정 (venv)

이 프로젝트는 Python **가상환경(venv)**을 사용합니다. 가상환경을 사용하면 **프로젝트마다 독립적인 패키지 환경**을 만들 수 있어서, 다른 프로젝트와 패키지가 충돌하지 않습니다.

> 비유하자면: 각 프로젝트마다 별도의 "도구 상자"를 두는 것과 같습니다. A 프로젝트에서 쓰는 도구와 B 프로젝트에서 쓰는 도구가 섞이지 않습니다.

### 4-1. Python 설치 확인

먼저 Python이 설치되어 있는지 확인합니다.

```bash
python --version
```

`Python 3.10` 이상이 출력되면 됩니다. 설치되어 있지 않다면 [Python 공식 사이트](https://www.python.org/downloads/)에서 다운로드합니다.

> **설치 시 주의**: 설치 화면에서 **"Add Python to PATH"** 체크박스를 반드시 체크해주세요!

### 4-2. 가상환경 만들기 (처음 한 번만)

프로젝트 폴더(`datathon/`) 안에서 아래 명령어를 실행합니다.

```bash
python -m venv .venv
```

> 이 명령어는 `.venv`라는 폴더를 만들고, 그 안에 독립적인 Python 환경을 세팅합니다.
> `.venv` 폴더는 `.gitignore`에 포함되어 있어서 GitHub에는 올라가지 않습니다.

### 4-3. 가상환경 활성화

**가상환경을 활성화**해야 그 안에 패키지를 설치하고 사용할 수 있습니다. 터미널 종류에 따라 명령어가 다릅니다.

**VS Code 터미널 (PowerShell) — 가장 많이 사용:**
```powershell
.venv\Scripts\Activate.ps1
```

> PowerShell에서 "스크립트를 실행할 수 없습니다" 오류가 나면 아래 명령어를 **먼저** 실행하세요:
> ```powershell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```
> 이 명령어는 한 번만 실행하면 됩니다.

**VS Code 터미널 (Git Bash):**
```bash
source .venv/Scripts/activate
```

**CMD (명령 프롬프트):**
```cmd
.venv\Scripts\activate
```

### 활성화 확인

활성화에 성공하면 터미널 프롬프트 앞에 `(.venv)`가 표시됩니다:

```
(.venv) PS C:\...\datathon>     ← 이렇게 (.venv)가 보이면 성공!
```

### 4-4. 패키지 설치

가상환경이 활성화된 상태에서 아래 명령어를 실행합니다.

```bash
pip install -r requirements.txt
```

> `requirements.txt` 파일에 이 프로젝트에 필요한 모든 패키지 목록이 적혀 있습니다.
> 위 명령어 한 줄이면 모든 패키지가 자동으로 설치됩니다.

설치되는 주요 패키지:

| 패키지 | 용도 |
|--------|------|
| `pandas` | 데이터 분석 (표 형태 데이터 다루기) |
| `numpy` | 수치 계산 |
| `matplotlib` | 그래프/시각화 |
| `seaborn` | 통계 시각화 (matplotlib 기반) |
| `scikit-learn` | 머신러닝 모델 |
| `ipykernel` | VS Code에서 Jupyter Notebook 실행 |

### 4-5. VS Code에서 Jupyter Notebook 커널 연결

패키지 설치 후, VS Code에서 `.ipynb` 파일을 열었을 때 **커널을 선택**해야 합니다.

1. `.ipynb` 파일을 엽니다 (예: `datathon_base_notebook.ipynb`)
2. 우측 상단의 **"커널 선택"** (또는 "Select Kernel") 버튼을 클릭합니다
3. **"Python Environments..."** 를 선택합니다
4. 목록에서 **`.venv (Python 3.x.x)`** 를 선택합니다

> 커널 목록에 `.venv`가 안 보이면, VS Code를 껐다가 다시 열어보세요.

### 4-6. 가상환경 비활성화

작업이 끝나면 아래 명령어로 비활성화할 수 있습니다 (필수는 아닙니다):

```bash
deactivate
```

### 4-7. 새 패키지를 추가한 경우

작업 중 새로운 패키지를 설치했다면, 다른 팀원과 공유하기 위해 `requirements.txt`를 업데이트해야 합니다.

```bash
# 새 패키지 설치 (예시)
pip install xgboost lightgbm

# requirements.txt 업데이트
pip freeze > requirements.txt

# 변경된 requirements.txt를 커밋
git add requirements.txt
git commit -m "chore: requirements.txt 업데이트 (xgboost, lightgbm 추가)"
git push origin 본인이름
```

> **중요**: 새 패키지를 설치한 후 `pip freeze > requirements.txt`를 잊지 마세요!
> 이걸 해야 다른 팀원도 같은 환경을 만들 수 있습니다.

### 4-8. 전체 순서 요약 (처음 세팅할 때)

```bash
# 1. 저장소 복제
git clone https://github.com/brian-223134/datathon.git
cd datathon

# 2. 가상환경 생성
python -m venv .venv

# 3. 가상환경 활성화 (PowerShell 기준)
.venv\Scripts\Activate.ps1

# 4. 패키지 설치
pip install -r requirements.txt

# 5. 끝! 이제 노트북을 열고 작업하면 됩니다
```

### 매번 작업을 시작할 때

```bash
# 1. 가상환경 활성화 (매번 터미널을 새로 열 때마다)
.venv\Scripts\Activate.ps1

# 2. 혹시 다른 팀원이 새 패키지를 추가했을 수 있으니, 동기화
pip install -r requirements.txt

# 3. 작업 시작!
```

---

## 5. 브랜치 전략

우리 프로젝트는 **3단계 브랜치 구조**를 사용합니다.

```
main (최종 결과물)
 │
 └── develop (개발 통합)
      │
      ├── cjkim (개인 작업)
      ├── jiyoung (개인 작업)
      └── minsoo (개인 작업)
```

| 브랜치 | 역할 | 누가 관리? |
|--------|------|-----------|
| `main` | 최종 완성된 코드만 올라감 | 팀장 |
| `develop` | 각자의 작업을 모아서 합치는 곳 | 팀 전체 |
| `본인이름` | 각자 자유롭게 작업하는 공간 | 본인 |

### 규칙

- **`main` 브랜치에 직접 push 금지!** 반드시 PR을 통해서만 merge합니다.
- 각자 **본인 이름 브랜치**에서 작업하고, 완료되면 `develop`으로 PR을 보냅니다.
- `develop`에서 충분히 테스트된 코드만 `main`에 merge합니다.

---

## 6. 기본 작업 흐름

매일 작업할 때 아래 순서를 따라주세요.

### Step 1: 최신 코드 가져오기

```bash
# 내 브랜치에서 시작
git checkout 본인이름

# develop의 최신 코드를 내 브랜치에 반영
git pull origin develop
```

> 다른 사람이 작업한 내용이 있을 수 있으므로, **작업 시작 전에 항상 pull**을 해주세요.

### Step 2: 코드 작업하기

자유롭게 파일을 수정하거나 추가합니다.

### Step 3: 변경사항 확인하기

```bash
# 어떤 파일이 변경되었는지 확인
git status
```

출력 예시:
```
Changes not staged for commit:
        modified:   datathon_base_notebook.ipynb    ← 수정된 파일 (빨간색)

Untracked files:
        my_analysis.py                               ← 새로 만든 파일 (빨간색)
```

### Step 4: 변경사항 스테이징 (커밋 준비)

```bash
# 특정 파일만 추가
git add datathon_base_notebook.ipynb

# 또는 변경된 파일 모두 추가
git add .
```

> `git add`는 "이 파일들을 다음 커밋에 포함시키겠다"는 의미입니다.

### Step 5: 커밋하기

```bash
git commit -m "feat: EDA 시각화 코드 추가"
```

> 커밋 메시지 작성 규칙은 [7. 커밋 메시지 규칙](#7-커밋-메시지-규칙)을 참고하세요.

### Step 6: GitHub에 올리기

```bash
git push origin 본인이름
# 예: git push origin jiyoung
```

### Step 7: PR 만들기 (작업 단위가 완료되었을 때)

GitHub 웹사이트에서 Pull Request를 만듭니다. ([8번 섹션](#8-pull-request-pr-만들기) 참고)

---

## 7. 커밋 메시지 규칙

### 형식

```
타입: 변경 내용 요약
```

### 타입 종류

| 타입 | 사용 상황 | 예시 |
|------|----------|------|
| `feat` | 새로운 기능/코드 추가 | `feat: 결측치 처리 함수 추가` |
| `fix` | 버그 수정 | `fix: 데이터 로딩 오류 수정` |
| `modify` | 기존 코드 수정/개선 | `modify: 시각화 그래프 색상 변경` |
| `docs` | 문서 수정 (README 등) | `docs: README 작성` |
| `refactor` | 코드 구조 변경 (기능 변화 없음) | `refactor: 전처리 코드 함수화` |
| `chore` | 기타 잡일 (설정, 파일 정리 등) | `chore: 불필요한 파일 삭제` |

### 좋은 예시 vs 나쁜 예시

```
# 좋은 예시
feat: 랜덤 포레스트 모델 학습 코드 추가
fix: train/test 분리 시 데이터 누수 문제 해결
modify: feature engineering 파이프라인 개선

# 나쁜 예시
수정함               ← 타입이 없음
feat: 수정           ← 무엇을 했는지 알 수 없음
update               ← 타입도 없고 내용도 불명확
```

---

## 8. Pull Request (PR) 만들기

PR은 "내 작업을 develop 브랜치에 합쳐주세요"라고 요청하는 것입니다.

### 순서

1. GitHub 저장소 페이지에 접속합니다.
2. 상단의 **"Pull requests"** 탭을 클릭합니다.
3. **"New pull request"** 버튼을 클릭합니다.
4. **base**를 `develop`, **compare**를 `본인이름` 브랜치로 선택합니다.
5. 제목과 설명을 작성합니다.
6. **"Create pull request"** 버튼을 클릭합니다.

### PR 제목 작성 규칙

커밋 메시지와 동일한 형식을 사용합니다.

```
feat: EDA 및 기초 시각화 코드 추가
```

### PR 설명 템플릿

```markdown
## 작업 내용
- 어떤 작업을 했는지 간단히 설명

## 변경 사항
- 변경된 파일이나 추가된 기능 목록

## 참고 사항
- 리뷰어가 알아야 할 사항 (선택)
```

---

## 9. 자주 쓰는 명령어 모음

### 기본 명령어

| 명령어 | 설명 |
|--------|------|
| `git status` | 현재 상태 확인 (수정된 파일, 스테이징 여부 등) |
| `git add 파일명` | 특정 파일을 스테이징 |
| `git add .` | 모든 변경 파일을 스테이징 |
| `git commit -m "메시지"` | 스테이징된 파일을 커밋 |
| `git push origin 브랜치명` | GitHub에 업로드 |
| `git pull origin 브랜치명` | GitHub에서 최신 코드 가져오기 |

### 브랜치 관련

| 명령어 | 설명 |
|--------|------|
| `git branch` | 현재 브랜치 목록 보기 (현재 브랜치에 * 표시) |
| `git checkout 브랜치명` | 해당 브랜치로 이동 |
| `git checkout -b 브랜치명` | 새 브랜치 생성 + 이동 |

### 확인/조회

| 명령어 | 설명 |
|--------|------|
| `git log --oneline` | 커밋 이력 간단히 보기 |
| `git diff` | 수정한 내용 확인 |

---

## 10. 문제 상황 해결

### "내가 수정한 파일이 충돌(conflict)났어요"

다른 사람과 같은 파일의 같은 부분을 수정하면 충돌이 발생합니다.

```bash
# pull 했는데 충돌이 난 경우
git pull origin develop
# CONFLICT 메시지가 뜸
```

충돌이 발생한 파일을 열면 아래와 같은 표시가 보입니다:

```
<<<<<<< HEAD
내가 작성한 코드
=======
다른 사람이 작성한 코드
>>>>>>> develop
```

**해결 방법:**
1. `<<<`, `===`, `>>>` 표시를 모두 지우고
2. 원하는 코드만 남긴 뒤
3. 저장하고 커밋합니다.

```bash
git add .
git commit -m "fix: merge conflict 해결"
git push origin 본인이름
```

> 충돌이 어렵게 느껴지면 팀원에게 도움을 요청하세요!

### "아직 커밋 안 한 수정사항을 되돌리고 싶어요"

```bash
# 특정 파일 되돌리기
git checkout -- 파일명

# 모든 수정사항 되돌리기 (주의: 수정한 내용이 사라집니다!)
git checkout -- .
```

### "커밋 메시지를 잘못 썼어요"

```bash
# 직전 커밋 메시지 수정 (아직 push 하기 전에만!)
git commit --amend -m "올바른 메시지"
```

### "어떤 브랜치에 있는지 모르겠어요"

```bash
git branch
# * 표시가 현재 브랜치입니다
```

---

## 프로젝트 구조

```
datathon/
├── .venv/                        # Python 가상환경 (Git에 포함되지 않음)
├── csv/                          # 데이터 파일 (Git에 포함되지 않음)
├── datathon_base_notebook.ipynb  # 기본 노트북
├── requirements.txt              # Python 패키지 목록
├── .gitignore                    # Git에서 제외할 파일 목록
└── README.md                     # 이 문서
```

> `.gitignore`에 의해 `.csv` 파일과 `.venv/` 폴더는 GitHub에 올라가지 않습니다. 데이터 파일은 별도로 공유합니다.
