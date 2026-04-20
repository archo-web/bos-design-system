# Design Tokens — BOS 4.0

> 디자인 시스템의 **단일 진실 공급원**.
> 모든 시각 결정은 이 토큰에서 시작합니다.
>
> - 코드용 CSS: [`bos4-design-tokens.css`](../tokens/bos4-design-tokens.css)
> - 시각 카탈로그: [`site/tokens.html`](../site/tokens.html)
> - Figma: `jmK75D3yVgpYh0wHAlsAwy`

---

## 토큰 시스템 구조 — 3-tier

BOS 4.0은 **3단계 토큰 계층**을 가집니다.

```
[Tier 1] Primitives  (339개)   ← 원시 값 (white, gray/900, brand/500, spacing/4)
              ↓
[Tier 2] Themes      (150개)   ← 시멘틱 매핑 (text-heading, bg-brand, ...)
   ├── Light mode             각 시멘틱 토큰이 모드별로 다른 원시 값을 참조
   └── Dark mode
              ↓
[Tier 3] Component             ← 컴포넌트는 항상 시멘틱 토큰 사용
```

**원칙**: 컴포넌트는 항상 **Tier 2 (시멘틱)** 토큰 사용. Tier 1 (원시)을 직접 참조하지 않습니다.

```css
/* ❌ Tier 1 직접 참조 — 다크 모드에서 깨짐 */
.title { color: var(--colors-gray-900); }

/* ✅ Tier 2 시멘틱 — Light/Dark 자동 대응 */
.title { color: var(--colors-text-text-heading); }
```

---

## Tier 1 — Primitives (원시 값)

### Spacing — 4px base

Tailwind 호환. 모든 간격은 4의 배수.

| 토큰 | 값 |
|---|---|
| `--spacing-0` | 0 |
| `--spacing-px` | 1px |
| `--spacing-0_5` | 2px |
| `--spacing-1` | 4px |
| `--spacing-1_5` | 6px |
| `--spacing-2` | 8px |
| `--spacing-2_5` | 10px |
| `--spacing-3` | 12px |
| `--spacing-3_5` | 14px |
| `--spacing-4` | 16px |
| `--spacing-5` | 20px |
| `--spacing-6` | 24px |
| `--spacing-8` | 32px |
| `--spacing-10` | 40px |
| `--spacing-12` | 48px |
| `--spacing-16` | 64px |
| `--spacing-20` | 80px |
| `--spacing-24` | 96px |
| `--spacing-32` | 128px |
| `--spacing-40` | 160px |
| `--spacing-48` | 192px |
| `--spacing-64` | 256px |
| `--spacing-72` | 288px |
| `--spacing-80` | 320px |
| `--spacing-96` | 384px |

### Container widths

| 토큰 | 값 | 용도 |
|---|---|---|
| `--container-xs` | 380px | Auth 카드, Toast |
| `--container-sm` | 640px | 폼 콘텐츠, Modal |
| `--container-md` | 768px | 좁은 페이지 콘텐츠 |
| `--container-lg` | 1024px | 일반 페이지 콘텐츠 |
| `--container-xl` | 1280px | 대시보드 |
| `--container-2xl` | 1440px | 풀폭 페이지 최대치 |

### Brand Color (BOS 핵심)

Brand는 기존 Bos PB와 동일한 값을 유지 (마이그레이션 호환).

| 토큰 | 값 | 비고 |
|---|---|---|
| `--colors-brand-050` | `#e9eafb` | 가장 옅음 |
| `--colors-brand-100` | `#c7cbf5` | |
| `--colors-brand-200` | `#a1aaef` | |
| `--colors-brand-300` | `#7988e9` | |
| `--colors-brand-400` | `#5a6ce4` | |
| `--colors-brand-500` | **`#3851dd`** | **메인 brand** |
| `--colors-brand-600` | `#3248d2` | Dark mode primary 액션 |
| `--colors-brand-700` | `#283dc5` | Light mode primary 액션 |
| `--colors-brand-800` | `#3b3d9e` | Hover 강조 |
| `--colors-brand-900` | `#011ba7` | 강한 강조 |
| `--colors-brand-950` | `#28207d` | Dark 배경 |

### Gray (Tailwind gray)

중립 색. 텍스트, 배경, 보더에 사용.

| 토큰 | 값 |
|---|---|
| `--colors-gray-050` | `#f9fafb` |
| `--colors-gray-100` | `#f3f4f6` |
| `--colors-gray-200` | `#e5e7eb` |
| `--colors-gray-300` | `#d1d5dc` |
| `--colors-gray-400` | `#99a1af` |
| `--colors-gray-500` | `#6a7282` |
| `--colors-gray-600` | `#4a5565` |
| `--colors-gray-700` | `#333e4f` |
| `--colors-gray-800` | `#1e2939` |
| `--colors-gray-900` | `#101828` |
| `--colors-gray-950` | `#030712` |

### Status Color

#### Rose (Danger)

| 토큰 | 값 |
|---|---|
| `--colors-rose-050` | `#fef0f2` |
| `--colors-rose-100` | `#fee2e8` |
| `--colors-rose-200` | `#fecdd6` |
| `--colors-rose-700` | `#c70036` |
| `--colors-rose-800` | `#a50036` |
| `--colors-rose-900` | `#881337` |
| `--colors-rose-950` | `#4c0519` |

#### Orange (Warning)

| 토큰 | 값 |
|---|---|
| `--colors-orange-050` | `#fff8f1` |
| `--colors-orange-100` | `#feecdc` |
| `--colors-orange-200` | `#fcd9bd` |
| `--colors-orange-500` | `#ff5a1f` |
| `--colors-orange-600` | `#d03801` |
| `--colors-orange-900` | `#771d1d` |

#### Emerald (Success)

| 토큰 | 값 |
|---|---|
| `--colors-emerald-050` | `#ecfdf5` |
| `--colors-emerald-100` | `#d0fae5` |
| `--colors-emerald-500` | `#10b981` |
| `--colors-emerald-600` | `#059669` |
| `--colors-emerald-700` | `#047857` |
| `--colors-emerald-900` | `#064e3b` |

### Border Radius

| 토큰 | 값 | 사용처 |
|---|---|---|
| `--border-radius-rounded-0` | 0 | 보더 없음 |
| `--border-radius-rounded-xs` | 4px | 체크박스 |
| `--border-radius-rounded-sm` | 6px | 작은 칩 |
| `--border-radius-rounded` | 8px | 인풋, 작은 카드 |
| `--border-radius-rounded-base` | **12px** | **기본 — 버튼/인풋** |
| `--border-radius-rounded-lg` | 16px | 큰 카드 |
| `--border-radius-rounded-xl` | 20px | 모달 |
| `--border-radius-rounded-full` | 9999px | 아바타, Pill 뱃지 |

### Shadow

| 토큰 | 값 | 사용처 |
|---|---|---|
| `--shadow-xs` | `0 1px 0.5px 0.05px rgba(29, 41, 61, 0.02)` | Button (subtle) |
| `--shadow-sm` | `0 1px 2px 0 rgba(29, 41, 61, 0.05)` | Card |
| `--shadow-md` | `0 4px 6px -1px ..., 0 2px 4px -2px ...` | Toast, Popover |
| `--shadow-lg` | `0 10px 15px -3px ..., 0 4px 6px -4px ...` | Drawer |
| `--shadow-xl` | `0 20px 25px -5px ..., 0 8px 10px -6px ...` | Modal, Auth Card |

### Typography

#### Font Family

```css
--typography-font-family-base: "Wanted Sans Variable", "Wanted Sans", "Inter", system-ui, -apple-system, sans-serif;
```

한국어는 Wanted Sans, 영문은 Inter fallback.

#### Font Size

| 토큰 | 값 | 사용처 |
|---|---|---|
| `--typography-font-size-text-xxs` | 8px | 거의 사용 안 함 |
| `--typography-font-size-text-xs` | 12px | 캡션, 헬퍼 |
| `--typography-font-size-text-sm` | **14px** | **본문 기본** |
| `--typography-font-size-text-base` | 16px | 강조 본문, 카드 제목 |
| `--typography-font-size-text-lg` | 18px | 섹션 제목 (소) |
| `--typography-font-size-text-xl` | 20px | 카드 헤딩, Auth 제목 |
| `--typography-font-size-text-2xl` | 24px | 섹션 제목 (대) |
| `--typography-font-size-text-3xl` | 30px | 페이지 제목 |
| `--typography-font-size-text-4xl` | 36px | 히어로 |

#### Font Weight

| 토큰 | 값 |
|---|---|
| `--typography-font-weight-font-normal` | 400 |
| `--typography-font-weight-font-medium` | 500 |
| `--typography-font-weight-font-semibold` | 600 |
| `--typography-font-weight-font-bold` | 700 |

#### Line Height (px)

| 토큰 | 값 |
|---|---|
| `--typography-line-height-leading-3` | 12px |
| `--typography-line-height-leading-4` | 16px |
| `--typography-line-height-leading-5` | 20px |
| `--typography-line-height-leading-6` | 24px |
| `--typography-line-height-leading-7` | 28px |
| `--typography-line-height-leading-8` | 32px |
| `--typography-line-height-leading-9` | 36px |

---

## Tier 2 — Themes (시멘틱 토큰)

같은 시멘틱 키가 Light/Dark 모드에서 다른 원시 값을 참조합니다.
컴포넌트는 **항상** 이 시멘틱 토큰을 사용합니다.

### Background

| 시멘틱 토큰 | Light | Dark |
|---|---|---|
| `bg-primary` | `white` | `gray-950` |
| `bg-primary-soft` | `white` | `gray-900` |
| `bg-primary-medium` | `white` | `gray-800` |
| `bg-secondary` | `gray-050` | `gray-950` |
| `bg-tertiary` | `gray-100` | `gray-800` |
| `bg-quaternary` | `gray-200` | `gray-700` |
| `bg-disabled` | `gray-100` | `gray-800` |
| `bg-brand` | `brand-700` (`#283dc5`) | `brand-600` (`#3248d2`) |
| `bg-brand-soft` | `brand-100` | `brand-900` |
| `bg-brand-softer` | `brand-050` | `brand-950` |
| `bg-brand-strong` | `brand-800` | `brand-700` |
| `bg-success` | `emerald-700` | `emerald-600` |
| `bg-success-soft` | `emerald-050` | `emerald-900` |
| `bg-danger` | `rose-700` | `rose-700` |
| `bg-danger-soft` | `rose-050` | `rose-950` |
| `bg-danger-strong` | `rose-800` | `rose-800` |
| `bg-warning` | `orange-500` | `orange-600` |
| `bg-warning-soft` | `orange-050` | `orange-900` |

### Text

| 시멘틱 토큰 | Light | Dark |
|---|---|---|
| `text-heading` | `gray-900` | `white` |
| `text-body` | `gray-600` | `gray-400` |
| `text-body-subtle` | `gray-500` | `gray-400` |
| `text-disabled` | `gray-400` | `gray-600` |
| `text-white` | `white` | `white` |
| `text-fg-brand` | `brand-700` | `brand-500` |
| `text-fg-brand-strong` | `brand-900` | `brand-400` |
| `text-fg-success` | `emerald-700` | `emerald-600` |
| `text-fg-danger` | `rose-700` | `rose-500` |
| `text-fg-warning` | `orange-900` | `orange-300` |

### Border

| 시멘틱 토큰 | Light | Dark |
|---|---|---|
| `border-base` | `gray-200` | `gray-800` |
| `border-base-strong` | `gray-300` | `gray-700` |
| `border-light` | `gray-100` | `gray-900` |
| `border-brand` | `brand-700` | `brand-500` |
| `border-brand-subtle` | `brand-200` | `brand-900` |
| `border-success` | `emerald-700` | `emerald-600` |
| `border-success-subtle` | `emerald-200` | `emerald-900` |
| `border-danger` | `rose-700` | `rose-600` |
| `border-danger-subtle` | `rose-200` | `rose-900` |
| `border-warning` | `orange-600` | `orange-500` |

---

## 빠른 참조 — 자주 쓰는 토큰 조합

### 일반 텍스트

```css
.body {
  font-family: var(--typography-font-family-base);
  font-size: var(--typography-font-size-text-sm);
  line-height: var(--typography-line-height-leading-5);
  color: var(--colors-text-text-body);
}
```

### Primary Button

```css
.btn-primary {
  background: var(--colors-background-bg-brand);
  color: var(--colors-text-text-white);
  border-radius: var(--border-radius-rounded-base);
  padding: var(--spacing-2_5) var(--spacing-4);
  font-size: var(--typography-font-size-text-sm);
  font-weight: var(--typography-font-weight-font-medium);
  box-shadow: var(--shadow-xs);
}
.btn-primary:hover {
  background: var(--colors-background-bg-brand-strong);
}
```

### Card

```css
.card {
  background: var(--colors-background-bg-primary);
  border: var(--border-width-border) solid var(--colors-border-border-base);
  border-radius: var(--border-radius-rounded-base);
  padding: var(--spacing-6);
  box-shadow: var(--shadow-sm);
}
```

### Input field

```css
.input {
  background: var(--colors-background-bg-secondary);
  border: var(--border-width-border) solid var(--colors-border-border-base);
  border-radius: var(--border-radius-rounded-base);
  padding: var(--spacing-2_5) var(--spacing-3);
  height: 40px;
  font-size: var(--typography-font-size-text-sm);
  color: var(--colors-text-text-heading);
}
.input:focus {
  border-color: var(--colors-border-border-brand);
  box-shadow: 0 0 0 2px var(--colors-border-border-brand-subtle);
}
```

---

## 다크 모드 적용 방법

HTML 루트에 `data-theme` 속성:

```html
<html data-theme="light">  <!-- 또는 "dark" -->
```

JavaScript로 토글:

```js
document.documentElement.setAttribute('data-theme', 'dark');
```

시멘틱 토큰을 사용한 모든 컴포넌트가 자동으로 다크 모드 색상으로 전환됩니다.

---

## 변경 이력

- `2026-04-20` — BOS 4.0 토큰 시스템 초기 배포 버전
