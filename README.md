# BOS 4.0 Design System

> Flowbite + Tailwind 기반 한국어 디자인 시스템. 시멘틱 토큰, 다크 모드 정식 지원, 47개 컴포넌트 카테고리.

**🌐 라이브 사이트**: `https://<your-github-username>.github.io/bos4-design-system/site/`
(GitHub Pages 설정 후 실제 URL로 교체하세요)

**🎨 Figma 파일**: [`jmK75D3yVgpYh0wHAlsAwy`](https://figma.com/design/jmK75D3yVgpYh0wHAlsAwy)

---

## ⚡ 빠른 시작

### 디자이너로서

1. **Figma 파일 열기** → 위 링크
2. **가이드라인 읽기** → [`docs/00-CLAUDE.md`](docs/00-CLAUDE.md)부터 시작
3. **컴포넌트 사용** → Figma에서 BOS 4.0 라이브러리를 Enable
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

**옵션 2 — 전체 디자인 시스템 참조**

`site/` 폴더의 HTML 파일들을 브라우저로 열면 토큰, 컴포넌트, 패턴을 라이브로 확인 가능 (외부 의존성 없음).

### Claude AI와 작업하기

이 레포의 `docs/` 폴더 파일들을 Claude Project Knowledge에 업로드하면, Claude가 이 디자인 시스템 기준으로 화면을 만들어 줍니다.

1. Claude Projects → 프로젝트 생성 또는 선택
2. Project Knowledge에 `docs/*.md` 전체 + `tokens/bos4-design-tokens.css` 업로드
3. "멤버 관리 페이지 만들어줘" 같은 요청 시 자동으로 이 디자인 시스템을 따름

---

## 📦 폴더 구조

```
bos4-design-system/
├── README.md                       ← 이 파일
├── CONTRIBUTING.md                 ← 기여 방법
├── SETUP.md                        ← GitHub 관리법 (디자이너용)
├── docs/                           ← 가이드라인 문서 8개
│   ├── 00-CLAUDE.md                ← 마스터 진입점
│   ├── 01-design-tokens.md         ← 토큰 카탈로그
│   ├── 02-design-principles.md     ← 6가지 디자인 원칙
│   ├── 03-component-usage.md       ← 47개 카테고리 + 핵심 18개 가이드
│   ├── 04-layout-patterns.md       ← 12가지 페이지 패턴
│   ├── 05-interaction-patterns.md  ← 상태/인터랙션 표준
│   ├── 06-ux-writing.md            ← 한국어 UX 라이팅
│   └── 07-accessibility.md         ← 웹 접근성 가이드 (WCAG AA)
├── tokens/
│   └── bos4-design-tokens.css      ← 단일 진실 공급원 (489 토큰)
└── site/                           ← 인터랙티브 사이트 (GitHub Pages 배포)
    ├── index.html                  ← 홈
    ├── tokens.html                 ← 토큰 시각 카탈로그
    ├── components.html             ← 핵심 18개 컴포넌트 라이브 갤러리
    ├── patterns.html               ← 12가지 페이지 패턴
    └── shared.css                  ← 사이트 공통 스타일
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

47개 카테고리, 수천 개 variant — 상세는 [`docs/03-component-usage.md`](docs/03-component-usage.md).

---

## 🔄 업데이트 흐름

디자인 시스템 변경 시:

```
1. Figma에서 변수 또는 컴포넌트 수정
2. Figma "Publish library" (수동)
3. 이 레포에서 관련 파일 업데이트
   - tokens/bos4-design-tokens.css
   - docs/*.md (해당되는 경우)
   - site/*.html (inline된 토큰 주의)
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

### 2026-05-08 — 레이아웃 패턴 5개 → 12개로 확장

**배경**: 실무에서 자주 만드는 화면 중 기존 5개 패턴으로 분류되지 않던 7가지 화면(이메일 클라이언트형 Master-Detail, 상품 카탈로그형 Card Grid, 단계별 Wizard, 카테고리별 Settings, 첫 진입 Onboarding, 빈/오류 상태)을 표준 패턴으로 추가.

**추가된 패턴 (7개)**:
- **§2 Master-Detail** — 좌측 리스트 + 우측 상세 동시 표시 (이메일·메시지·코드 리뷰)
- **§3 Card Grid** — 시각적 항목을 카드로 펼치기 (상품·갤러리·템플릿)
- **§7 Wizard / Stepper** — 단계별 긴 프로세스 (회원가입·결제·워크스페이스 만들기)
- **§9 Settings** — 카테고리별 환경설정 (즉시/저장 반영 분리)
- **§11 Onboarding** — 첫 진입 사용자 안내
- **§12 Empty / 404 / Error** — 4가지 변형 (Empty/404/403/500)

**의사결정 트리 재구성 — 다면적 3단계**:
- Step 1: 액션 동사 (본다·한다·바꾼다·진입한다)
- Step 2: 정보 구조 (행/카드/단일/폼/멀티지표)
- Step 3: 마이크로 분기 (5개 이하 / 100개 이상 / 모바일 / 첫 진입 등)
- 세 관점이 같은 패턴을 가리키는지 검증

**기타**: 반응형 브레이크포인트별 동작 표 추가, 흔한 실수 6개 → **12개**로 확장.

**적용 범위**: `docs/04-layout-patterns.md` 메인 문서 + `site/patterns.html` 시각 패턴 12개 (디자이너도 참조 가능).

**학습**: "패턴은 단순 분류가 아니라 의사결정 도구다. 트리를 다면적으로 만들면 모호한 요구사항을 더 빨리 잡아낼 수 있다."

---

### 2026-05-08 — 핵심 컴포넌트 가이드를 18개로 확장

**배경**: Figma 공식 파일(`jmK75D3yVgpYh0wHAlsAwy`)과의 1:1 대조를 통해 기존 가이드의 정합성을 점검하고, 자주 쓰는 컴포넌트 8종의 깊이 가이드를 추가.

**추가된 깊이 가이드 (8개)**:
- **§11 Tooltip** (4 variants) — Position 4종 (Top/Bottom/Left/Right)
- **§12 Nav Tabs** (12 variants) — Type 7종 × Mobile
- **§13 Toggle** (12+8+3 variants) — Switches / Inputs / Group
- **§14 Checkbox & Radio** (12+54+10 variants) — Input / Form 54 / Group 10
- **§15 Floating label inputs** (84 variants) — 3 Type × 4 Size × 7 State
- **§16 Range slider** (2+1+2+2 variants) — 단일/범위 선택
- **§17 Datepicker** (8+1+5+2 variants) — Cell 8 state + Range 선택
- **§18 Timepicker** (84+6+4 variants) — Input 84 + Form 6 + Dropdown 4

**기존 가이드 정합성 수정**:
- **Card §6** — `CTA card` 제거(Figma에 없음), `With full width tabs` / `With nav tabs` / `Cars with list` 추가, variant 수 `18 types` → `16 variants` 정정
- **Modal §5** — 동반 컴포넌트 `Modal header` (7), `Modal footer` (4) 추가
- **Dropdown §8** — 조립 단위 `Dropdown list item` (48), `Dropdown header` (5), `ready to use examples` (6) 추가

**구조 갱신**:
- Icons 섹션 자리이동: §11 → **§19**
- 핵심 컴포넌트 카운트: 10 → **18** (사이트 4개 페이지 사이드바 동기화)
- 페이지 상단에 "What's New" 배너 추가 — 변경된 모든 섹션으로 점프 링크 제공

**학습**: "기억과 추측이 아닌 Figma `componentPropertyDefinitions`를 직접 추출해서 검증하는 것이 가장 빠른 정합성 점검 방법."

---

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

**Maintained by**: BOS Design Team
**Last Updated**: 2026-05-08
