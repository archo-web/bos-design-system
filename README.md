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
├── docs/                           ← 가이드라인 문서 7개
│   ├── 00-CLAUDE.md                ← 마스터 진입점
│   ├── 01-design-tokens.md         ← 토큰 카탈로그
│   ├── 02-design-principles.md     ← 6가지 디자인 원칙
│   ├── 03-component-usage.md       ← 47개 카테고리 + 핵심 10개 가이드
│   ├── 04-layout-patterns.md       ← 5가지 페이지 패턴
│   ├── 05-interaction-patterns.md  ← 상태/인터랙션 표준
│   └── 06-ux-writing.md            ← 한국어 UX 라이팅
├── tokens/
│   └── bos4-design-tokens.css      ← 단일 진실 공급원 (489 토큰)
└── site/                           ← 인터랙티브 사이트 (GitHub Pages 배포)
    ├── index.html                  ← 홈
    ├── tokens.html                 ← 토큰 시각 카탈로그
    ├── components.html             ← 핵심 10개 컴포넌트 라이브 갤러리
    ├── patterns.html               ← 5가지 페이지 패턴
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
**Last Updated**: 2026-04-20
