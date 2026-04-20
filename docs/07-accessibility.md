# accessibility.md — BOS 4.0 웹 접근성 가이드

> 이 문서는 BOS 4.0 시스템에서 **웹 접근성(Accessibility, a11y)** 표준을 정의합니다.
> WCAG 2.1 AA 레벨을 목표로 하며, 모든 사용자가 차별 없이 사용할 수 있는 UI를 만들기 위한 실무 가이드예요.

---

## 🎯 왜 접근성이 중요한가

**접근성은 소수를 위한 부가 기능이 아니라, 잘 만든 디자인의 기본입니다.**

- 🇰🇷 **법적 요구사항** — 한국 장애인차별금지법에 따라 웹 접근성 확보 의무 (단계적 확대)
- 👥 **더 많은 사용자** — 시각/청각/운동 장애인 + 고령자 + 일시적 장애 포함 전체 인구의 약 15%
- 🔍 **SEO & 품질** — 시멘틱 HTML은 검색엔진에도, 스크린 리더에도 더 좋음
- 🎯 **모두를 위한 UX** — 키보드 네비게이션, 명확한 대비, 논리적 구조는 모든 사용자에게 이득

BOS 4.0은 다크 모드, 시멘틱 토큰, 명확한 상태 정의 등으로 이미 접근성 기반이 탄탄해요. 이 문서는 그 위에 **개발 단계에서 지켜야 할 실무 가이드**를 제공합니다.

---

## 📐 WCAG 2.1 4대 원칙

준수 목표: **WCAG 2.1 Level AA**

| 원칙 | 의미 | BOS 4.0에서의 적용 |
|---|---|---|
| **Perceivable** (지각 가능성) | 정보가 사용자에게 인지 가능해야 | 명암 대비, 대체 텍스트, 자막 |
| **Operable** (운용 가능성) | UI 구성 요소를 다룰 수 있어야 | 키보드 접근, Focus 관리, 시간 제한 |
| **Understandable** (이해 가능성) | 정보와 UI 사용법이 이해 가능해야 | 명확한 라벨, 예측 가능한 동작 |
| **Robust** (견고성) | 다양한 보조 기술과 호환되어야 | 시멘틱 HTML, ARIA, 유효한 코드 |

---

## 1. 색상 & 대비 (Perceivable)

### 1.1 WCAG AA 대비비 기준

텍스트와 배경의 대비비는 **최소 이 기준 이상**이어야 합니다.

| 텍스트 종류 | 최소 대비비 | BOS 4.0 토큰 예시 |
|---|---|---|
| 본문 (18pt 미만 또는 14pt Bold 미만) | **4.5 : 1** | `text-heading` on `bg-primary` |
| 큰 텍스트 (18pt 이상 또는 14pt Bold 이상) | **3 : 1** | 헤딩, 큰 CTA |
| UI 컴포넌트 (보더, 아이콘) | **3 : 1** | `border-brand` on `bg-primary` |
| 장식 요소 | 제한 없음 | 배경 패턴, 구분선 |

### 1.2 BOS 4.0 시멘틱 토큰 대비비 (검증됨)

자주 쓰이는 조합의 대비비 점검 결과:

**Light 모드**
| 배경 | 텍스트 | 대비비 | 판정 |
|---|---|---|---|
| `bg-primary` (#fff) | `text-heading` (gray-900) | 16.7 : 1 | ✅ AAA |
| `bg-primary` (#fff) | `text-body` (gray-600) | 7.6 : 1 | ✅ AAA |
| `bg-primary` (#fff) | `text-body-subtle` (gray-500) | 5.7 : 1 | ✅ AA |
| `bg-primary` (#fff) | `text-fg-disabled` (gray-400) | 3.0 : 1 | ⚠️ UI만 |
| `bg-brand` (brand-700) | `text-white` | 7.8 : 1 | ✅ AAA |
| `bg-danger` (rose-700) | `text-white` | 5.9 : 1 | ✅ AA |

**Dark 모드**
| 배경 | 텍스트 | 대비비 | 판정 |
|---|---|---|---|
| `bg-primary` (gray-950) | `text-heading` (white) | 19.0 : 1 | ✅ AAA |
| `bg-primary` (gray-950) | `text-body` (gray-400) | 7.9 : 1 | ✅ AAA |
| `bg-brand` (brand-600) | `text-white` | 6.5 : 1 | ✅ AA |

> 💡 시멘틱 토큰을 올바르게 사용하면 자동으로 대비비 기준이 충족됩니다.

### 1.3 색만으로 정보 전달 금지

**색상은 유일한 정보 전달 수단이 되면 안 됩니다.** 색맹 사용자(전체의 약 8%)도 구분 가능해야 해요.

```jsx
// ❌ Bad — 빨강/초록만으로 성공/실패 구분
<span style={{ color: 'var(--colors-text-text-fg-success)' }}>완료</span>
<span style={{ color: 'var(--colors-text-text-fg-danger)' }}>실패</span>

// ✅ Good — 색 + 아이콘 + 텍스트 조합
<span>
  <CheckIcon /> 완료
</span>
<span>
  <XIcon /> 실패
</span>
```

### 1.4 다크 모드 색조 동조 함정

**경고**: 다크 배경에 같은 hue 텍스트를 올리면 색조 동조로 대비비가 급감합니다.

```css
/* ❌ Bad — 다크모드에서 brand-950 배경 + brand-500 텍스트는 3:1 미만 */
[data-theme="dark"] .nav--active {
  background: var(--colors-background-bg-brand-softer); /* brand-950 */
  color: var(--colors-text-text-fg-brand);              /* brand-500 */
}

/* ✅ Good — 흰 텍스트로 대비 확보 */
[data-theme="dark"] .nav--active {
  background: var(--colors-background-bg-brand);        /* brand-600 */
  color: var(--colors-text-text-white);                 /* 대비비 6.5:1 */
}
```

상세는 `05-interaction-patterns.md §1.3` 참조.

---

## 2. 키보드 접근성 (Operable)

### 2.1 기본 원칙

**모든 인터랙티브 요소는 키보드만으로 조작 가능해야 합니다.** 마우스 없이 작업하는 사용자(운동 장애, 파워 유저, 스크린 리더 사용자)를 위한 필수 사항이에요.

### 2.2 Tab 순서와 Focus 관리

```jsx
// ✅ Good — 시멘틱 HTML은 자동으로 Tab 순서 부여
<button onClick={handleSave}>저장</button>
<a href="/help">도움말</a>
<input type="text" />

// ❌ Bad — div/span은 기본적으로 Tab 대상이 아님
<div onClick={handleSave}>저장</div>  {/* 키보드 접근 불가 */}

// ⚠️ 꼭 div를 써야 한다면 tabIndex + 역할 + 키보드 이벤트 모두 필요
<div
  role="button"
  tabIndex={0}
  onClick={handleSave}
  onKeyDown={(e) => (e.key === 'Enter' || e.key === ' ') && handleSave()}
>
  저장
</div>
```

**원칙**: 되도록 시멘틱 HTML 사용. div에 역할 부여는 최후의 수단.

### 2.3 Focus Ring 필수 (`:focus-visible`)

키보드 사용자에게 **현재 포커스가 어디 있는지 시각적으로 명확**해야 합니다.

```css
/* ❌ Bad — focus 제거는 접근성 파괴 */
button:focus { outline: none; }

/* ✅ Good — 마우스 클릭 시엔 숨기고 키보드 포커스만 표시 */
button:focus-visible {
  outline: 2px solid var(--colors-border-border-brand);
  outline-offset: 2px;
}

/* BOS 4.0 권장 패턴 — ring 스타일 */
button:focus-visible {
  outline: none;
  box-shadow: 0 0 0 2px var(--colors-background-bg-primary),
              0 0 0 4px var(--colors-border-border-brand);
}
```

### 2.4 키보드 단축키 표준

상세는 `05-interaction-patterns.md §6` 참조. 요약:

| 키 | 동작 | 지원 필수? |
|---|---|---|
| `Tab` / `Shift+Tab` | 다음/이전 요소 이동 | ✅ 필수 |
| `Enter` | 버튼 활성화, 링크 이동, 폼 제출 | ✅ 필수 |
| `Space` | 버튼 활성화, 체크박스 토글 | ✅ 필수 |
| `Esc` | Modal/Drawer/Dropdown 닫기 | ✅ 필수 |
| `↑/↓/←/→` | 리스트/라디오/탭 내 이동 | ✅ 필수 |

### 2.5 키보드 트랩 방지

Modal이나 Drawer 같은 오버레이에서는 **Focus가 그 안에서만 순환**해야 합니다. 배경으로 새어나가면 안 돼요.

```jsx
// React 예시 — focus-trap-react 같은 라이브러리 사용 권장
import FocusTrap from 'focus-trap-react';

<FocusTrap>
  <div role="dialog" aria-modal="true">
    <h2>제목</h2>
    <button>확인</button>
    <button>취소</button>
  </div>
</FocusTrap>
```

Modal이 닫힐 때는 **원래 Focus 위치로 복귀**해야 합니다.

---

## 3. 시멘틱 HTML (Robust)

### 3.1 왜 시멘틱 HTML이 중요한가

스크린 리더는 HTML 구조를 읽어서 사용자에게 전달해요. `<div>`만 가득한 페이지는 "그냥 덩어리"로 들리고, 시멘틱 요소는 "여기는 메뉴, 여기는 주요 콘텐츠, 여기는 푸터"처럼 안내돼요.

### 3.2 시멘틱 요소 사용 가이드

```jsx
// ❌ Bad — div만 가득
<div className="header">
  <div className="nav">
    <div className="menu-item">홈</div>
  </div>
</div>
<div className="content">
  <div className="title">제목</div>
  <div className="article">...</div>
</div>

// ✅ Good — 시멘틱 요소
<header>
  <nav>
    <ul>
      <li><a href="/">홈</a></li>
    </ul>
  </nav>
</header>
<main>
  <article>
    <h1>제목</h1>
    <p>...</p>
  </article>
</main>
```

**사용 빈도 높은 시멘틱 요소**

| 요소 | 용도 |
|---|---|
| `<header>` | 페이지/섹션 헤더 |
| `<nav>` | 네비게이션 |
| `<main>` | 페이지 주요 콘텐츠 (페이지당 1개) |
| `<article>` | 독립적 콘텐츠 (뉴스, 포스트) |
| `<section>` | 주제별 섹션 |
| `<aside>` | 보조 정보 (사이드바) |
| `<footer>` | 페이지/섹션 푸터 |
| `<button>` | 동작 트리거 |
| `<a href>` | 링크 (페이지 이동) |

### 3.3 제목 구조 (Heading Hierarchy)

`<h1>` ~ `<h6>`는 **순서대로** 사용. 단계를 건너뛰지 마세요.

```jsx
// ❌ Bad — h1 다음 h3
<h1>페이지 제목</h1>
<h3>섹션 제목</h3>  {/* h2 건너뜀 */}

// ✅ Good — 순서대로
<h1>페이지 제목</h1>
  <h2>섹션 제목</h2>
    <h3>하위 제목</h3>
```

**원칙**: 페이지당 `<h1>`은 1개. 시각적 크기보다 **의미적 계층**으로 선택.

### 3.4 Button vs Link 구분

```jsx
// ❌ Bad — 페이지 이동인데 button 사용
<button onClick={() => navigate('/about')}>소개</button>

// ✅ Good — 페이지 이동은 <a> (또는 Link 컴포넌트)
<a href="/about">소개</a>
<Link to="/about">소개</Link>  // React Router

// ❌ Bad — 동작인데 <a href="#"> 사용
<a href="#" onClick={handleDelete}>삭제</a>

// ✅ Good — 동작은 <button>
<button type="button" onClick={handleDelete}>삭제</button>
```

**판단 기준**:
- URL이 바뀌면 → `<a>` (링크)
- 페이지 내 동작이면 → `<button>` (버튼)

---

## 4. ARIA 속성

### 4.1 ARIA의 기본 원칙

> **"No ARIA is better than bad ARIA."** — 잘못된 ARIA는 ARIA 없는 것보다 나쁩니다.

시멘틱 HTML로 해결되는 건 ARIA를 쓰지 마세요. ARIA는 "시멘틱으로 안 되는 걸 보완"하는 용도예요.

### 4.2 자주 쓰는 ARIA 속성

**`aria-label`** — 텍스트 없는 요소에 라벨 제공
```jsx
// 아이콘만 있는 버튼 (필수!)
<button aria-label="검색">
  <SearchIcon />
</button>

// 닫기 버튼
<button aria-label="모달 닫기" onClick={onClose}>
  ✕
</button>
```

**`aria-labelledby`** — 다른 요소를 라벨로 참조
```jsx
<h2 id="dialog-title">프로젝트 삭제</h2>
<div role="dialog" aria-labelledby="dialog-title">
  정말 삭제하시겠습니까?
</div>
```

**`aria-describedby`** — 추가 설명 연결 (에러 메시지 등)
```jsx
<label htmlFor="email">이메일</label>
<input
  id="email"
  type="email"
  aria-describedby="email-error"
  aria-invalid={hasError}
/>
{hasError && (
  <span id="email-error" role="alert">
    이메일 형식이 올바르지 않습니다
  </span>
)}
```

**`aria-expanded`** — 접기/펼치기 상태
```jsx
<button
  aria-expanded={isOpen}
  aria-controls="dropdown-menu"
  onClick={() => setIsOpen(!isOpen)}
>
  메뉴
</button>
<ul id="dropdown-menu" hidden={!isOpen}>
  <li>...</li>
</ul>
```

**`aria-current`** — 현재 위치/페이지 표시
```jsx
<nav>
  <a href="/" aria-current={pathname === '/' ? 'page' : undefined}>홈</a>
  <a href="/about" aria-current={pathname === '/about' ? 'page' : undefined}>소개</a>
</nav>
```

**`aria-busy`** — 로딩 중 상태
```jsx
<div aria-busy={isLoading}>
  {isLoading ? <Spinner /> : <DataTable />}
</div>
```

### 4.3 Live Region (동적 콘텐츠)

화면에 새로 나타나는 콘텐츠(Toast, 알림)는 스크린 리더가 감지할 수 있도록 live region으로 선언.

```jsx
// 일반 알림 (중요하지 않음) — 사용자 작업 방해 X
<div role="status" aria-live="polite">
  저장되었습니다
</div>

// 긴급 알림 (즉시 읽어야 함) — 진행 중 작업 중단 후 읽음
<div role="alert" aria-live="assertive">
  저장 실패. 다시 시도해 주세요.
</div>
```

**선택 기준**: 대부분의 Toast는 `polite`. 치명적 에러만 `assertive`.

---

## 5. 폼 접근성

### 5.1 Label 연결 필수

**모든 폼 필드는 `<label>`과 연결**되어야 합니다.

```jsx
// ❌ Bad — placeholder만 있음 (입력 시 사라짐)
<input type="email" placeholder="이메일" />

// ❌ Bad — label이 있지만 연결 안 됨
<label>이메일</label>
<input type="email" />

// ✅ Good — htmlFor로 명시적 연결
<label htmlFor="email-input">이메일</label>
<input id="email-input" type="email" />

// ✅ Good — label 안에 input 포함
<label>
  이메일
  <input type="email" />
</label>

// ✅ Good — 시각적으로 label 숨기고 싶을 때
<label htmlFor="search" className="sr-only">검색</label>
<input id="search" type="search" placeholder="검색어 입력" />
```

**`sr-only` 클래스** (screen reader only) — 시각적으로 안 보이지만 스크린 리더는 읽음:
```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

### 5.2 필수 필드 표시

```jsx
// ❌ Bad — 별표만으로 필수 표시 (스크린 리더는 "별" 읽음)
<label>이메일 *</label>
<input type="email" />

// ✅ Good — aria-required + 시각적 표시
<label htmlFor="email">
  이메일 <span aria-hidden="true" style={{ color: 'var(--colors-text-text-fg-danger)' }}>*</span>
</label>
<input id="email" type="email" required aria-required="true" />
```

### 5.3 에러 메시지 연결

```jsx
<label htmlFor="password">비밀번호</label>
<input
  id="password"
  type="password"
  aria-invalid={!!error}
  aria-describedby={error ? "password-error" : "password-hint"}
/>
<span id="password-hint">8자 이상, 영문/숫자 포함</span>
{error && (
  <span id="password-error" role="alert" style={{ color: 'var(--colors-text-text-fg-danger)' }}>
    {error}
  </span>
)}
```

**핵심**:
- `aria-invalid="true"` — 검증 실패 상태
- `aria-describedby` — 도움말과 에러 메시지 연결
- `role="alert"` — 에러 발생 시 스크린 리더 즉시 안내

### 5.4 BOS 4.0 Input field 7가지 상태와 ARIA 매핑

| BOS 4.0 State | ARIA 속성 |
|---|---|
| `Initial` | 없음 |
| `Focus/Typing` | 자동 |
| `Filled` | 없음 |
| `Disabled` | `disabled` (또는 `aria-disabled`) |
| `Read-only` | `readonly` (또는 `aria-readonly`) |
| `Success` | `aria-invalid="false"` (명시적) |
| `Danger` | `aria-invalid="true"` + `aria-describedby` |

---

## 6. 이미지 & 아이콘

### 6.1 `alt` 속성 규칙

```jsx
// 정보를 전달하는 이미지 — 의미를 서술
<img src="/chart.png" alt="2026년 1월 매출 전년 대비 23% 증가" />

// 장식용 이미지 — 빈 문자열 (스크린 리더 무시)
<img src="/decoration.svg" alt="" />

// 링크/버튼 안의 이미지 — 해당 동작을 서술
<a href="/profile">
  <img src="/user.jpg" alt="프로필 페이지로 이동" />
</a>

// ❌ Bad — 파일명을 alt로
<img src="/IMG_1234.jpg" alt="IMG_1234.jpg" />

// ❌ Bad — "이미지" 같은 무의미한 텍스트
<img src="/chart.png" alt="이미지" />
```

### 6.2 SVG 아이콘 접근성

**의미 있는 아이콘** (단독 사용):
```jsx
<button aria-label="검색">
  <svg aria-hidden="true" viewBox="0 0 24 24">...</svg>
</button>
```

**라벨과 함께 있는 아이콘**:
```jsx
<button>
  <svg aria-hidden="true" viewBox="0 0 24 24">...</svg>
  저장
</button>
```

**장식용 아이콘**:
```jsx
<h2>
  <svg aria-hidden="true" viewBox="0 0 24 24">...</svg>
  섹션 제목
</h2>
```

**원칙**: 아이콘은 기본적으로 `aria-hidden="true"`. 아이콘만 단독 사용 시에는 **부모 요소에 `aria-label`** 추가.

---

## 7. 동적 콘텐츠 & 상태 변화

### 7.1 Modal / Dialog

```jsx
<div
  role="dialog"
  aria-modal="true"
  aria-labelledby="modal-title"
  aria-describedby="modal-description"
>
  <h2 id="modal-title">프로젝트 삭제</h2>
  <p id="modal-description">
    이 작업은 되돌릴 수 없습니다.
  </p>
  <button onClick={onConfirm}>삭제</button>
  <button onClick={onCancel}>취소</button>
</div>
```

**필수 체크**:
- [ ] `role="dialog"` + `aria-modal="true"`
- [ ] `aria-labelledby` 또는 `aria-label`로 제목 연결
- [ ] 열릴 때 첫 포커서블 요소로 focus 이동
- [ ] 닫힐 때 원래 위치로 focus 복귀
- [ ] `Esc` 키로 닫기 가능
- [ ] Focus trap (배경으로 새어나가지 않음)

### 7.2 Toast / Notification

```jsx
// 정보성 알림
<div role="status" aria-live="polite">
  파일이 업로드되었습니다
</div>

// 에러 알림
<div role="alert" aria-live="assertive">
  업로드 실패. 네트워크를 확인해 주세요.
</div>
```

### 7.3 로딩 상태

```jsx
// 영역 로딩
<div aria-busy={isLoading} aria-live="polite">
  {isLoading ? (
    <>
      <Spinner />
      <span className="sr-only">불러오는 중</span>
    </>
  ) : (
    <DataTable data={data} />
  )}
</div>

// 버튼 로딩
<button disabled={isSaving} aria-busy={isSaving}>
  {isSaving ? '저장 중...' : '저장'}
</button>
```

---

## 8. 모션 & 애니메이션

### 8.1 `prefers-reduced-motion` 존중

일부 사용자는 화면 전환이나 애니메이션에 어지러움을 느낄 수 있어요. OS 설정(모션 줄이기)을 존중해야 합니다.

```css
/* 기본 애니메이션 */
.modal-enter {
  transition: transform 250ms ease-out, opacity 250ms ease-out;
}

/* 사용자가 모션 줄이기 선택 시 */
@media (prefers-reduced-motion: reduce) {
  .modal-enter {
    transition: opacity 100ms ease-out;
    /* 이동은 없애고 페이드만 */
  }

  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

### 8.2 자동 재생 금지

- 영상/소리 자동 재생 금지 (사용자 조작 후 시작)
- 5초 이상의 자동 애니메이션은 일시정지 가능해야 함
- 깜빡이는 콘텐츠 (초당 3회 이상) 금지 — 광과민성 발작 위험

---

## 9. 한국어 특수 고려사항

### 9.1 언어 선언

```html
<!-- ✅ 필수 — 스크린 리더가 한국어 발음 엔진 사용 -->
<html lang="ko">

<!-- 영문이 섞인 경우 -->
<p>이것은 <span lang="en">Design System</span>입니다</p>
```

### 9.2 한글 가독성

한글은 영문보다 **글꼴 크기를 조금 더 크게** 해야 가독성이 좋아요. BOS 4.0의 기본 본문 사이즈(`text-sm` = 14px)는 한국어에 맞춰 조정된 값이에요.

- 본문: `text-sm` (14px) 최소, 가급적 `text-base` (16px)
- 노년층 대상 서비스: `text-base` (16px) 이상
- 작은 텍스트(`text-xs` = 12px)는 캡션/헬퍼 전용

### 9.3 한국어 스크린 리더

한국에서 많이 쓰이는 스크린 리더:

- **센스리더** (한국 개발, 유료)
- **NVDA** (오픈소스, 한국어 지원)
- **VoiceOver** (macOS/iOS 내장)
- **TalkBack** (Android 내장)

**테스트 권장**: 최소한 NVDA + Chrome 조합으로 주요 페이지 점검.

### 9.4 한국 특화 권장 가이드라인

**KWCAG 2.2** (한국형 웹 콘텐츠 접근성 지침) — WCAG 2.1에 한국 상황을 반영한 가이드. 공공기관 프로젝트는 준수 필수.

- 한국웹접근성인증평가원: https://www.wa.or.kr
- 행정안전부 KWCAG 2.2: 공공기관 웹사이트 필수 기준

---

## 10. 개발자 체크리스트

새 화면을 만들거나 기존 화면을 수정할 때 **이 체크리스트로 점검**하세요.

### 🎨 시각 & 색상
- [ ] 모든 텍스트가 배경과 4.5:1 이상 대비비 (큰 텍스트는 3:1)
- [ ] 색상만으로 정보 전달하지 않음 (아이콘/텍스트 병행)
- [ ] 다크 모드에서 모든 상태가 명확히 보임 (특히 active/focus)
- [ ] Focus ring이 키보드 사용자에게 명확히 보임 (`:focus-visible`)

### ⌨️ 키보드
- [ ] 마우스 없이 모든 기능 사용 가능
- [ ] Tab 순서가 시각적 흐름과 일치
- [ ] Esc로 Modal/Dropdown/Drawer 닫기 가능
- [ ] Enter로 버튼 활성화, 폼 제출 가능
- [ ] Modal 열릴 때 focus 이동, 닫힐 때 복귀

### 🏗️ HTML 구조
- [ ] `<html lang="ko">` 설정됨
- [ ] 시멘틱 요소 사용 (`<header>`, `<main>`, `<nav>`, `<button>` 등)
- [ ] 제목 계층 순서대로 (`h1` → `h2` → `h3`)
- [ ] 페이지당 `<h1>` 하나만
- [ ] 버튼은 `<button>`, 링크는 `<a href>`로 구분

### 🏷️ 라벨 & 설명
- [ ] 모든 폼 필드에 `<label>` 연결 (`htmlFor`)
- [ ] 아이콘 단독 버튼에 `aria-label`
- [ ] 이미지에 의미 있는 `alt` (또는 장식용은 `alt=""`)
- [ ] 에러 메시지가 `aria-describedby`로 연결됨
- [ ] 필수 필드가 `aria-required` 또는 `required`

### 🔄 동적 콘텐츠
- [ ] Modal에 `role="dialog"` + `aria-modal="true"`
- [ ] Toast/알림에 `role="status"` 또는 `role="alert"`
- [ ] 로딩 상태에 `aria-busy` 또는 스크린 리더용 텍스트
- [ ] 실시간 업데이트에 `aria-live` 설정

### 🎬 모션
- [ ] `prefers-reduced-motion` 대응
- [ ] 자동 재생 없음 (영상/음성)
- [ ] 깜빡임 초당 3회 미만

### 📱 반응형 & 터치
- [ ] 터치 영역 최소 44x44px (모바일)
- [ ] 200% 확대 시에도 콘텐츠 유실 없음
- [ ] 가로/세로 방향 모두 대응

---

## 11. 테스트 도구 & 방법

### 11.1 브라우저 확장 (Chrome/Edge)

- **axe DevTools** — 자동 접근성 검사 (https://www.deque.com/axe/)
- **WAVE** — 시각적 접근성 오류 표시 (https://wave.webaim.org)
- **Lighthouse** — Chrome 내장 감사 도구 (Accessibility 섹션)

### 11.2 키보드 테스트

마우스를 전혀 쓰지 않고 페이지 전체를 돌아다녀 보세요:

1. `Tab`으로 모든 인터랙티브 요소 접근 가능한가?
2. Focus가 보이지 않거나 사라지는 곳이 있는가?
3. `Enter` / `Space`로 버튼이 동작하는가?
4. Modal에서 Esc로 닫히는가?
5. Tab 순서가 자연스러운가?

### 11.3 스크린 리더 테스트

**macOS**: VoiceOver (`Cmd + F5`로 켜기)
**Windows**: NVDA (무료 다운로드) 또는 센스리더
**iOS**: 설정 > 접근성 > VoiceOver
**Android**: 설정 > 접근성 > TalkBack

기본 조작 (VoiceOver 기준):
- `Ctrl + Option + →` : 다음 요소
- `Ctrl + Option + ←` : 이전 요소
- `Ctrl + Option + Space` : 요소 활성화

### 11.4 대비비 측정

- **WebAIM Contrast Checker**: https://webaim.org/resources/contrastchecker/
- Chrome DevTools > Elements > Styles > 색상 부분 클릭 → 대비비 자동 표시
- Figma plugin: **Stark** (Figma에서 직접 대비비 점검)

---

## 12. 자주 하는 실수 Top 10

1. **`<div onClick>`으로 버튼 만들기** → `<button>` 사용
2. **placeholder를 label 대신 사용** → `<label>` 명시적 연결
3. **색상만으로 에러 표시** → 아이콘 + 텍스트 병행
4. **Focus outline 제거 (`outline: none`)** → `:focus-visible`로 대체
5. **아이콘 버튼에 `aria-label` 누락** → 스크린 리더 사용자가 뭔지 모름
6. **이미지 `alt` 누락 또는 파일명 사용** → 의미 서술하거나 `alt=""`
7. **Modal에 role과 focus 관리 없음** → `role="dialog"` + focus trap
8. **`<div>` 만으로 페이지 구성** → 시멘틱 HTML 사용
9. **다크 모드에서 같은 hue 텍스트/배경** → 대비비 확인
10. **`<html lang>` 미설정 또는 `lang="en"`** → `lang="ko"` 설정

---

## 13. 참고 자료

- **WCAG 2.1 (W3C)**: https://www.w3.org/TR/WCAG21/
- **WCAG 2.1 한국어 번역**: https://www.w3.org/Translations/WCAG21-ko/
- **KWCAG 2.2 (한국)**: 행정안전부 공공기관 웹 접근성 지침
- **한국웹접근성인증평가원**: https://www.wa.or.kr
- **MDN Accessibility**: https://developer.mozilla.org/ko/docs/Web/Accessibility
- **A11y Project**: https://www.a11yproject.com/ (실무 체크리스트)
- **Deque University**: https://dequeuniversity.com/ (무료 학습 자료)

---

## 📝 변경 이력

- `2026-04-20` — 초기 버전 (WCAG 2.1 AA 기반 실무 가이드). BOS 4.0 시멘틱 토큰 대비비 검증, React 예시, 한국어 특수사항, 개발자 체크리스트 포함.
