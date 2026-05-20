# NSTAR 1.0 Design System

> Flowbite + Tailwind 기반 한국어 디자인 시스템. 시멘틱 토큰, 다크 모드 정식 지원, 24개 컴포넌트 라이브 가이드.

**🌐 라이브 사이트**: `https://<your-github-username>.github.io/bos4-design-system/site/components.html`
**📚 컴포넌트 라이브러리**: [components.html](site/components.html) — 24 컴포넌트 + 5 Foundations, 단일 HTML SPA (외부 의존성 0, 오프라인 동작)
**🎨 Figma 파일**: [`jmK75D3yVgpYh0wHAlsAwy`](https://figma.com/design/jmK75D3yVgpYh0wHAlsAwy)

---

## ⚡ 빠른 시작

### 디자이너로서

1. **Figma 파일 열기** → 위 링크
2. **가이드라인 읽기** → [`docs/00-CLAUDE.md`](docs/00-CLAUDE.md)부터 시작
3. **컴포넌트 사용** → Figma에서 NSTAR 1.0 라이브러리를 Enable
4. **새 화면 만들 때** → [`docs/04-layout-patterns.md`](docs/04-layout-patterns.md) 5가지 패턴 중 선택

### 개발자로서

**옵션 1 — CSS 토큰만 사용**

이 레포에서 `tokens/bos4-design-tokens.css` 파일을 프로젝트에 복사:

```css
/* 프로젝트 CSS */
@import "./bos4-design-tokens.css";

.my-button {
  background: var(--colors-background-bg-brand);
  color: var(--colors-text-text-white);
  padding: var(--spacing-2_5) var(--spacing-4);
  border-radius: var(--border-radius-rounded-base);
}
```

```html
<!-- 다크 모드 전환 -->
<html data-theme="dark"> <!-- 또는 "light" -->
```

**옵션 2 — 컴포넌트 라이브러리 라이브 뷰**

`site/components.html` 파일을 브라우저로 열면:

- 24개 컴포넌트의 인터랙티브 라이브 데모
- 각 컴포넌트의 variant 매트릭스 + API 표
- 복사 가능한 코드 스니펫
- 권장 / 지양 가이드
- 5개 Foundations (Colors / Type / Spacing / Radii & Shadows / Icons)

외부 의존성 없는 단일 HTML 파일. 오프라인에서도 동작합니다.

### Claude AI와 작업하기

이 레포의 `docs/` 폴더 파일들을 Claude Project Knowledge에 업로드하면, Claude가 이 디자인 시스템 기준으로 화면을 만들어 줍니다.

1. Claude Projects → 프로젝트 생성 또는 선택
2. Project Knowledge에 `docs/*.md` 전체 + `tokens/bos4-design-tokens.css` 업로드
3. "멤버 관리 페이지 만들어줘" 같은 요청 시 자동으로 이 디자인 시스템을 따름

---

## 📦 폴더 구조

```
bos4-design-system/
├── README.md                          ← 이 파일
├── CHANGELOG.md                       ← 버전별 변경 이력
├── CONTRIBUTING.md                    ← 기여 방법
├── SETUP.md                           ← GitHub 관리법 (디자이너용)
├── docs/                              ← 가이드라인 문서 9개
│   ├── 00-CLAUDE.md                   ← 마스터 진입점
│   ├── 01-design-tokens.md            ← 토큰 카탈로그
│   ├── 02-design-principles.md        ← 6가지 디자인 원칙
│   ├── 03-component-usage.md          ← 24개 컴포넌트 + 5 Foundations 가이드
│   ├── 04-layout-patterns.md          ← 5가지 페이지 패턴
│   ├── 05-interaction-patterns.md     ← 상태/인터랙션 표준
│   ├── 06-ux-writing.md               ← 한국어 UX 라이팅
│   ├── 07-accessibility.md            ← 웹 접근성 가이드 (WCAG AA)
│   ├── 08-dependency-guardrails.md    ← 외부 의존성 룰
│   └── icon-loader-boilerplate.md     ← Lucide 환경별 로더
├── tokens/
│   └── bos4-design-tokens.css         ← 단일 진실 공급원 (489 토큰)
└── site/                              ← 컴포넌트 라이브러리 (GitHub Pages 배포)
    └── components.html                ← NSTAR 1.0 라이브러리 (24 컴포넌트 + 5 Foundations, React SPA)
```

---

## 🎨 핵심 개념

### 3-tier 토큰 시스템

```
[Tier 1] Primitives (339개)   → brand-700: #283dc5
              ↓
[Tier 2] Themes (150개)        → bg-brand (Light: brand-700, Dark: brand-600)
              ↓
[Tier 3] Component             → <button style="background: var(--bg-brand)">
```

**핵심 원칙**: 컴포넌트는 항상 Tier 2 (시멘틱) 토큰만 사용. 다크 모드는 `data-theme` 속성으로 자동 전환.

### 속성 조합형 컴포넌트

- Button: `Color × Size × State × Outline × Icon-only` = **500 variants**
- Input field: `Type × Size × State` = **140 variants**
- Badge: `Theme × Size × Type` = **84 variants**
- Dropdown: 16 menu × 48 items × 5 headers × 6 ready-to-use = **75 variants**

라이브 데모와 코드는 [`site/components.html`](site/) 참조.

---

## 🆕 v2 라이브러리 — 추가된 15개 컴포넌트

기존 v1(10 컴포넌트)에서 v2(24 컴포넌트)로 확장되었습니다.

| 카테고리 | 신규 컴포넌트 |
|---|---|
| 액션 | Avatar, Icon shape |
| 폼 | Select, Checkbox/Radio, Toggle, **Range slider**, Datepicker, Timepicker |
| 피드백 | Toast, Drawer, Tooltip, Empty state |
| 네비/데이터 | Stepper, Breadcrumbs, Misc (KBD, Rating, Progress, Spinner, Skeleton, Carousel 등) |

각 컴포넌트의 variant 매트릭스, 라이브 데모, 코드 스니펫, 접근성 가이드는 라이브러리에서 직접 확인하세요. 자세한 변경 사항은 [`CHANGELOG.md`](CHANGELOG.md) 참조.

---

## 🔄 업데이트 흐름

디자인 시스템 변경 시:

```
1. Figma에서 변수 또는 컴포넌트 수정
2. Figma "Publish library" (수동)
3. 이 레포에서 관련 파일 업데이트
   - tokens/bos4-design-tokens.css
   - docs/*.md (해당되는 경우)
   - site/components.html (라이브러리 재생성 — 토큰 변경 시 인라인된 부분 갱신)
4. Git commit + push
5. GitHub Pages 자동 재배포
```

상세: [`docs/00-CLAUDE.md`의 "Figma 동기화 워크플로우" 섹션](docs/00-CLAUDE.md)

---

## 🙋 기여 / 문의

- 디자인 시스템 변경 제안 → [`CONTRIBUTING.md`](CONTRIBUTING.md) 참조
- 디자이너가 이 레포 관리하는 방법 → [`SETUP.md`](SETUP.md) 참조
- Figma에서 헷갈리는 부분 → 해당 Figma 파일에 코멘트
- 문서 오류 / 개선 제안 → GitHub Issue 또는 Pull Request

---

## 📝 최근 변경 이력

### 2026-05-20 — v2 컴포넌트 라이브러리 출시

**변경**:
- 컴포넌트 라이브러리 v2 출시 — 10 → 24 컴포넌트로 확장
- React 기반 단일 HTML SPA로 전환 (`components.html`)
- 5개 Foundations 페이지 추가 (Colors / Type / Spacing / Radii & Shadows / Icons)
- 신규 컴포넌트 15개 추가 — Avatar, Icon shape, Select, Checkbox/Radio, Toggle, Range slider, Datepicker, Timepicker, Toast, Drawer, Tooltip, Empty state, Stepper, Breadcrumbs, Misc
- `docs/03-component-usage.md`를 24 컴포넌트 기준으로 재작성

**제거**:
- 기존 `site/components/` 폴더의 개별 컴포넌트 페이지 10개 (v2 라이브러리로 통합)
- 기존 `bos4-app.html` SPA (v2 라이브러리가 대체)

자세한 사항은 [`CHANGELOG.md`](CHANGELOG.md) 참조.

### 2026-04-20 — Navigation Active 표준화

**문제**: 다크 모드에서 사이드바 active 메뉴의 가독성 이슈 발견

**해결**:
1. Figma 컴포넌트 `Navigation list item` Active 배경을 `bg-brand-softer`로 변경
2. 코드에서 다크 모드 override (`bg-brand` + `text-white`) 추가
3. 가이드라인 `05-interaction-patterns.md §1.3` 섹션 신설

**학습**: "다크 모드에서 같은 hue 내 채도 차이만으로 active 표시 금지"

---

## 🔗 관련 링크

- [Figma 파일](https://figma.com/design/jmK75D3yVgpYh0wHAlsAwy)
- [Wanted Sans (한국어 폰트)](https://github.com/wanteddev/wanted-sans)
- [Tailwind CSS 색상 참조](https://tailwindcss.com/docs/customizing-colors)

---

**Maintained by**: NSTAR Design Team
**Last Updated**: 2026-05-20
