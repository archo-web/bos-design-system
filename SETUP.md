# 디자인 시스템 관리 가이드

> 디자이너를 위한 GitHub 셋업 + 일상 관리 워크플로우.
> 터미널 명령어 없이 **GitHub Desktop 앱**으로 모든 작업이 가능합니다.

---

## 🎯 이 가이드의 목적

- 디자인 시스템을 **GitHub에 안전하게 올리기**
- Figma 변경 시 **코드 레포도 쉽게 업데이트**
- 팀원들에게 **자동으로 공개 URL** 제공 (GitHub Pages)

---

## 📋 준비물

1. **GitHub 계정** (무료) — [github.com](https://github.com)에서 가입
2. **GitHub Desktop 앱** (무료) — [desktop.github.com](https://desktop.github.com)에서 다운로드
3. **이 폴더** (`bos4-design-system/`)를 로컬에 준비

---

## 🚀 1단계: GitHub 레포 만들기 (최초 1회)

### A. 웹에서 레포 생성

1. [github.com](https://github.com) 로그인
2. 우측 상단 **+** 버튼 → **New repository**
3. 입력:
   - **Repository name**: `bos4-design-system`
   - **Description**: `BOS 4.0 Design System — 한국어 디자인 시스템`
   - **Public** 또는 **Private** 선택
     - 🔓 **Public**: 누구나 볼 수 있음. 외부 공유 쉬움. GitHub Pages 무료.
     - 🔒 **Private**: 팀 내부만. 팀원 초대 필요. GitHub Pages는 유료 플랜 필요.
     - 💡 **추천**: 사내 디자인 시스템이면 Private부터, 나중에 공개도 가능
   - **"Add a README file" 체크 해제** (우리 것이 이미 있음)
   - **"Add .gitignore" 체크 해제** (우리 것이 이미 있음)
4. **Create repository** 클릭

### B. GitHub Desktop으로 로컬 폴더 연결

1. **GitHub Desktop** 열기 → **Sign in** (GitHub 계정)
2. 상단 **File** → **Add local repository**
3. `bos4-design-system/` 폴더 선택
4. "This directory does not appear to be a Git repository" 경고 나오면:
   → **"create a repository"** 클릭
5. 다음 화면에서:
   - **Name**: `bos4-design-system`
   - **Description**: 자동 채워짐
   - **Initialize this repository with a README** 체크 해제
   - **Create repository** 클릭
6. 상단 **Publish repository** 클릭
7. GitHub에서 만든 레포와 연결 확인 → **Publish repository**

✅ **완료!** 이제 `github.com/your-username/bos4-design-system`에서 파일들이 보여요.

---

## 🌐 2단계: GitHub Pages 켜기 (라이브 사이트 공개)

`site/index.html` 같은 파일들을 **웹 URL로 접근 가능**하게 만들기.

### 설정 순서

1. GitHub 웹에서 레포 열기 → 상단 **Settings** 탭
2. 좌측 사이드바에서 **Pages**
3. **Build and deployment** 섹션:
   - **Source**: `GitHub Actions` 선택 (이미 준비된 워크플로우 사용)
4. 저장

### 첫 배포 확인

1. 레포의 **Actions** 탭 클릭
2. "Deploy Design System Site to GitHub Pages" 워크플로우 실행 중인 것 확인
3. 녹색 체크(✅)가 뜨면 완료 (보통 1-2분)
4. **Settings → Pages**에서 공개 URL 확인:
   ```
   https://your-username.github.io/bos4-design-system/
   ```
5. 위 URL 뒤에 `index.html` 붙여서 접속 가능:
   ```
   https://your-username.github.io/bos4-design-system/index.html
   ```

### README URL 업데이트

`README.md` 상단의 `라이브 사이트` 부분을 실제 URL로 바꿔주세요 (GitHub Desktop에서 수정 후 커밋).

---

## 🔄 3단계: 일상 관리 워크플로우

### 시나리오: "Figma에서 색상을 바꿨는데 레포도 업데이트해야 함"

#### Step 1 — Figma에서 변경

1. Figma에서 변수 수정
2. **Publish library** (우측 상단 Assets 패널)
3. 변경 내용 메모 (나중에 커밋 메시지에 쓸 것)

#### Step 2 — 로컬 파일 수정

**방법 A** (간단): 직접 텍스트 에디터로 수정
- VSCode, Sublime Text, 또는 메모장으로 `tokens/bos4-design-tokens.css` 열기
- 해당 변수의 값 수정
- `site/*.html` 4개 파일도 같은 방식으로 수정 (inline된 토큰)

**방법 B** (추천): Claude와 작업
- 이전 세션들처럼 Claude한테 "이 토큰 바꿔줘"라고 요청
- Claude가 모든 파일을 동기화해서 만들어 줌
- Claude가 준 파일을 로컬 `bos4-design-system/` 폴더에 덮어쓰기

#### Step 3 — GitHub Desktop에서 커밋 & 푸시

1. **GitHub Desktop** 열기
2. 좌측에 변경된 파일 목록이 자동으로 뜸
3. 하단 입력란에:
   - **Summary**: `토큰: text-fg-brand Dark mode 매핑 변경`
   - **Description** (선택): 자세한 설명
4. **Commit to main** 버튼 클릭
5. 상단 **Push origin** 버튼 클릭

#### Step 4 — 자동 배포 확인

1. GitHub 웹에서 **Actions** 탭 확인
2. 새 워크플로우 실행 중 → 1-2분 대기
3. ✅ 녹색 체크 뜨면 사이트에 반영됨
4. GitHub Pages URL 새로고침하면 변경사항 보임

---

## 💡 자주 발생하는 상황

### "토큰 바꿨는데 사이트에 반영이 안 됐어요"

확인할 것:
1. **GitHub Desktop에서 push 했나요?** → 커밋만 하고 push 안 하면 GitHub에 안 올라감
2. **Actions 탭에서 배포가 성공했나요?** → 빨간 X가 떴다면 클릭해서 에러 메시지 확인
3. **브라우저 캐시** → `Cmd + Shift + R` (강력 새로고침)

### "실수로 파일을 잘못 수정했어요"

GitHub Desktop에서:
1. 좌측 변경 파일 목록에서 해당 파일 **우클릭**
2. **Discard changes** → 마지막 커밋 상태로 되돌림

### "이미 푸시한 커밋을 되돌리고 싶어요"

GitHub Desktop에서:
1. 상단 **History** 탭
2. 되돌릴 커밋 **우클릭** → **Revert changes in commit**
3. 새 "revert" 커밋이 생김 → push

### "여러 명이 같이 작업해요"

**브랜치** 개념이 필요해요:
1. GitHub Desktop 상단 **Current Branch** → **New Branch**
2. 예: `design/dark-mode-fix`
3. 이 브랜치에서 작업 후 push
4. GitHub 웹에서 **Pull Request** 생성 → 팀 리뷰 후 merge

---

## 📁 폴더별 역할

| 폴더 | 수정 빈도 | 누가 수정? |
|---|---|---|
| `docs/` | 가끔 | 디자이너 (가이드라인 변경 시) |
| `tokens/` | 자주 | 디자이너 (Figma 변경 시 동기화) |
| `site/` | 가끔 | 디자이너 or 개발자 (데모 추가 시) |
| `.github/workflows/` | 거의 안 함 | 개발자 (자동화 수정 시) |

---

## 🆘 문제가 생겼을 때

1. **GitHub Desktop이 이상해요** → 앱 재시작, 그래도 안 되면 재설치
2. **Push가 안 돼요** → 에러 메시지 스크린샷 → 개발자 팀원에게 문의
3. **실수로 중요한 파일 삭제** → GitHub 웹의 **History** 탭에서 이전 버전 복구 가능
4. **GitHub Pages가 안 보여요** → Settings → Pages에서 배포 상태 확인

---

## 🎯 다음 단계 (선택)

시스템이 안정되면 고려할 것들:

- **Branch Protection Rule** — `main` 브랜치에 직접 push 막기 (팀 커지면)
- **Issue Templates** — 버그 리포트, 기능 요청 표준화
- **Pull Request Templates** — 변경사항 체크리스트 자동 제공
- **NPM Package 배포** — 개발자가 `npm install`로 쉽게 사용 (수준 2)

이 중에 필요해지는 게 있으면 Claude에게 "이거 추가해줘" 하시면 됩니다.

---

**기본은 이걸로 충분해요. 복잡해지면 그때 확장하세요.**
