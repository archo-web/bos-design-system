# Figma → 코드 컴포넌트 상세 가이드

> **출처**: Figma 파일 `jmK75D3yVgpYh0wHAlsAwy` · 라이브러리 `BOS4.0 design system`
> **추출 일자**: 2026-05-27 (Figma Plugin API `variantGroupProperties` + `componentPropertyDefinitions`)
> **범위**: 113 COMPONENT_SET (47 페이지) · variant ≈ 2,300+ · code prop ≈ 250+
> **사용처**: Vue 코드 구현 시 prop 명세·variant 매트릭스 1차 참고. 사용 규칙은 [`03-component-usage.md`](03-component-usage.md), 토큰은 [`bos4-design-tokens.css`](../tokens/bos4-design-tokens.css) 참조.

---

## 📑 인덱스

| 카테고리 | 컴포넌트 세트 | variant 합계 |
|---|---|---:|
| [패턴](#-패턴) | 1 | 20 |
| [액션 — Alerts / Banner / Badges](#1-alerts) | 3 | 39 |
| [Accordion](#2-accordion) | 1 | 10 |
| [Buttons + Button group](#3-buttons) | 4 | 524 |
| [Bottom navigation](#4-bottom-navigation) | 3 | 13 |
| [Breadcrumbs](#5-breadcrumbs) | 2 | 9 |
| [Cards](#6-cards) | 1 | 16 |
| [Chat bubble](#7-chat-bubble) | 3 | 23 |
| [Carousel](#8-carousel) | 3 | 7 |
| [Drawer](#9-drawer) | 3 | 20 |
| [Dropdowns](#10-dropdowns) | 4 | 75 |
| [Forms](#11-forms) | 17 | 489 |
| [Floating label inputs](#12-floating-label-inputs) | 1 | 84 |
| [Datepicker](#13-datepicker) | 4 | 16 |
| [Timepicker](#14-timepicker) | 3 | 94 |
| [Autocomplete](#15-autocomplete) | 3 | 26 |
| [Range slider](#16-range-slider) | 4 | 7 |
| [Toggle](#17-toggle) | 5 | 26 |
| [Checkbox & radio](#18-checkbox--radio) | 5 | 81 |
| [Gallery](#19-gallery) | 1 | 11 |
| [Icon shape](#20-icon-shape) | 1 | 84 |
| [List](#21-list) | 2 | 37 |
| [Navbar + Megamenu + Mobile menu](#22-navbar) | 6 | 107 |
| [Nav Tabs](#23-nav-tabs) | 2 | 20 |
| [Modals](#24-modals) | 3 | 21 |
| [Popovers](#25-popovers) | 2 | 17 |
| [Pagination](#26-pagination) | 1 | 9 |
| [Progress Bars](#27-progress-bars) | 2 | 26 |
| [Rating](#28-rating) | 1 | 3 |
| [Sidebar](#29-sidebar) | 3 | 14 |
| [Spinners](#30-spinners) | 2 | 14 |
| [Speed Dial](#31-speed-dial) | 5 | 51 |
| [Skeleton](#32-skeleton) | 1 | 6 |
| [Stepper](#33-stepper) | 3 | 57 |
| [Tables](#34-tables) | 1 | 3 |
| [Timelines](#35-timelines) | 3 | 27 |
| [KBD](#36-kbd) | 1 | 49 |
| [Tooltips](#37-tooltips) | 1 | 4 |
| [Toasts](#38-toasts) | 1 | 9 |

---

## ▒ 패턴

### Pattern (20 variant)

배경 장식 패턴.

| Property | Values |
|---|---|
| **Type** | Lines · Rounded Lines · Diamond right · Vertical lines · Dots · Lines modern · Squares · Modern · oblique lines · Diamond left · Creative · comb · tech · tech-square · tech-squares · technology · big-comb · lines-oblique · technology-top · comb-linear |

---

## 1. Alerts

### Alert (20 variant)

페이지 내 영구/세미-영구 안내. 색상으로 의미 전달, Type 으로 무게 조정.

| Property | Values |
|---|---|
| **Type** | Complex · Default · Small · Border top |
| **Color** | Success · Info · Warning · Default · Danger |

**Component Props** (9): Show alert heading · Show left icon · Right icon style · Show right icon · Body text · Show body text · Heading text · Left icon style · Text

**규칙**
- ARIA role: `success`/`info` → `status` · `warning`/`danger` → `alert`
- `Complex` 만 제목+CTA, 나머지는 한 줄 안내
- 에러 메시지 황금 공식: **무엇이 + 왜 + 어떻게**

#### 🎯 UX 의도
- **위치 의도**: 페이지 인라인 — 사용자가 해당 영역을 보는 중에 등장 (Toast 와 구분)
- **지속 시간**: 영구 (사용자가 dismiss 또는 새로고침까지)
- **시선 우선순위**: Danger(빨강) > Warning(주황) > Success(초록) > Info(파랑) > Default(회색)
- **반복 표시 금지**: 같은 메시지를 반복하지 않음 (한 번 보여주고 닫힌 후엔 다른 이벤트가 생길 때까지 안 나타남)

#### ✅ 권장 (Do)

```vue
<!-- 폼 결과 — 인라인 -->
<BAlert color="success" dismissible>
  ⓘ 거래가 성공적으로 등록되었습니다. <RouterLink to="/list">목록 보기</RouterLink>
</BAlert>

<!-- 즉시 조치 필요 — Complex + Danger + CTA -->
<BAlert color="danger" type="complex" title="결제 실패">
  카드 잔액이 부족합니다. 다른 결제 수단을 선택해 주세요.
  <template #actions>
    <BButton color="danger" size="sm">다시 시도</BButton>
    <BButton color="ghost" size="sm">결제 수단 변경</BButton>
  </template>
</BAlert>

<!-- 페이지 안내 — Default (중립) -->
<BAlert color="default">
  본 화면은 K-IFRS 1109호 기준 EIR 상각 결과입니다.
</BAlert>
```

#### ❌ 지양 (Don't)

```vue
<!-- ❌ Toast 처럼 사용 (자동 사라짐) — 영구 안내가 의도 -->
<BAlert color="success" :auto-dismiss="3000" />  <!-- → BToast 사용 -->

<!-- ❌ Modal 처럼 차단 (overlay) — Alert 은 인라인 -->
<BAlert style="position: fixed; inset: 0;" />  <!-- → BModal 사용 -->

<!-- ❌ 한 페이지에 Alert 5개 이상 — 시선 분산 -->
<BAlert color="info">...</BAlert>
<BAlert color="warning">...</BAlert>
<BAlert color="danger">...</BAlert>
<!-- → 우선순위 따라 하나만, 또는 그룹화 -->

<!-- ❌ 닫혔다 다시 열림 — 사용자가 의도적으로 닫은 알림은 그 세션에서 다시 안 보임 -->
<BAlert v-if="hasError" />  <!-- → 사용자 dismiss 후엔 hasError 조건 외에 별도 ref 로 추적 -->

<!-- ❌ Danger 로 긍정 메시지 -->
<BAlert color="danger">결제 완료</BAlert>  <!-- → success -->
```

#### 📐 Alert vs Toast vs Modal vs Banner 결정 트리

```
사용자가 즉시 응답해야 하는가?
├─ YES → Modal (차단형)
└─ NO  → 메시지가 영구한가?
         ├─ YES → 사이트 전체 공지인가?
         │       ├─ YES → Banner (최상단)
         │       └─ NO  → Alert (페이지 인라인)
         └─ NO  → Toast (3-5초 자동 사라짐)
```

### Banner (14 variant)

사이트 최상단 영구 공지. 모바일/태블릿/데스크톱 별 레이아웃.

| Property | Values |
|---|---|
| **Type** | Default · Logo + Button · Newsletter · Icon & link · Heading & description |
| **Breakpoint** | Mobile · Tablet · Desktop |

### Logo (5 variant)

로고 단독 표시 (Header/Footer/Card 안).

| Property | Values |
|---|---|
| **Size** | xs · sm · base · lg · XXS |

**Component Props** (1): Show text — 텍스트 숨김 시 마크 단독.

---

## 2. Accordion

### Accordion (10 variant)

펼침/접힘 콘텐츠. FAQ 표준 패턴.

| Property | Values |
|---|---|
| **Style** | Default · Flush · Separate Cards · Multi-level · With sub-header |
| **Dark Version** | False |
| **Icon** | True |
| **Breakpoint** | Desktop · Mobile |

**Component Props** (3): Show left icon · Show right icon · Left icon

**규칙**: 키보드 Tab + Enter/Space 토글, ARIA `aria-expanded`, 접근성 핵심.

---

## 3. Buttons

### Button (500 variant) ★

가장 큰 컴포넌트 세트. 페이지 메인 액션의 단일 진실.

| Property | Values |
|---|---|
| **Color** | Brand · Secondary · Tertiary · Success · Danger · Warning · Dark · Ghost |
| **Size** | xs · sm · base · l · xl |
| **State** | Initial · Hover · Focus · Disabled |
| **Icon only** | False · True |
| **Outline** | False · True |
| **Logo inside** | False · True |

**Component Props** (8): Show text · Button text · Show left icon · Left icon style · Right icon style · Show right icon · Icon Style · Logo

**규칙**
- 한 화면 brand 1차 CTA 는 **1개만**
- Icon-only 단독 버튼 → `aria-label` 필수
- 위험 액션 → `Danger` + (무게 줄이려면) `Outline`
- `:focus-visible` 보존, `outline: none` 금지

#### 🎯 UX 의도
- **시선 흐름**: 사용자 시선은 brand 색상으로 먼저 이동 → 페이지에 brand 1개일 때만 의도가 살아남
- **상태 피드백**: Hover(8% 톤 변화) → Focus(2px 보더 + 4px ring) → Active(눌림 4px shadow) — 3단계 즉시 피드백
- **타격 영역**: 모바일 최소 44×44px 터치 — `size="base"` 이상 권장
- **로딩 인지**: `loading` prop 활성 시 자동 disabled + spinner — 사용자가 두 번 누르는 실수 방지

#### ✅ 권장 (Do)

```vue
<!-- 페이지 메인 액션 — 명확한 동사 + brand -->
<BButton color="brand" size="base" @click="save">저장</BButton>

<!-- 보조 액션 — secondary (취소 등) -->
<BButton color="secondary" @click="cancel">취소</BButton>

<!-- 위험 액션 — danger, 무게 줄이려면 outline -->
<BButton color="danger" outline @click="confirmDelete">삭제</BButton>

<!-- Icon-only — aria-label 필수 -->
<BButton color="ghost" icon-only aria-label="편집">
  <template #icon><Pencil :size="16" /></template>
</BButton>

<!-- 로딩 처리 — 자동 disabled -->
<BButton :loading="isSaving" @click="save">
  {{ isSaving ? '저장 중...' : '저장' }}
</BButton>
```

#### ❌ 지양 (Don't)

```vue
<!-- ❌ brand 1차 CTA 2개 — 어디를 눌러야 할지 혼란 -->
<BButton color="brand">저장</BButton>
<BButton color="brand">전송</BButton>

<!-- ❌ 빨강 Confirm — 취소·삭제와 혼동 (긍정 액션에 danger 금지) -->
<BButton color="danger">확인</BButton>

<!-- ❌ outline: none — 키보드 사용자 차단 -->
<BButton style="outline: none">저장</BButton>

<!-- ❌ icon-only + aria-label 누락 -->
<BButton icon-only><Trash2 /></BButton>

<!-- ❌ 모호한 라벨 — "확인" 만으로는 동작 알 수 없음 -->
<BButton color="danger">확인</BButton>  <!-- → "삭제" 로 -->

<!-- ❌ div 에 onClick — 키보드 접근·스크린리더 안 됨 -->
<div @click="submit">제출</div>  <!-- → <BButton> 사용 -->
```

#### 📐 라벨 가이드 (06-ux-writing.md 정합)
- 2-4자 동사 (예: "저장", "삭제", "추가", "전송")
- 파괴적 액션은 **실제 동작 명시** ("삭제" ✓ / "확인" ✗)
- 진행 상태 명시 ("저장 중..." 같은 로딩 라벨)

### Download App Button (4 variant)

| Property | Values |
|---|---|
| **Device** | Google Play · AppStore |
| **Type** | Light · Dark |

### Group button (8 variant)

연속된 버튼 그룹 (필터, 토글).

| Property | Values |
|---|---|
| **Type** | Default |
| **Size** | xs · sm · base · l |
| **Color** | Gray · White |

### Group button examples (12 variant)

| Property | Values |
|---|---|
| **Type** | Default · 2 buttons · Icon button + Info · Button + Icon · Only icons · With dropdown button · With badge · Numbers · Start CTA button · Vertical · Vertical with icons · Plus & minus |

---

## 4. Bottom navigation

### Bottom navigation (Buttons) (2 variant)

각 항목 — 아이콘 + 텍스트.

| Property | Values |
|---|---|
| **Style** | Default |
| **Status** | Initial · Hover |

### Bottom navigation (5 variant) — 모바일 표준

| Property | Values |
|---|---|
| **Style** | Bordered · Default · Action button · Pagination · Segment controls |

### Bottom navigation (Desktop) (6 variant) — 데스크톱 호환

| Property | Values |
|---|---|
| **Style** | Call · Media Player |
| **Breakpoint** | Mobile · Tablet · Desktop |

---

## 5. Breadcrumbs

### Breadcrumb (6 variant)

페이지 위계 경로. Detail Page 최상단 표준.

| Property | Values |
|---|---|
| **Type** | Default · Background · With dropdown · With group buttons · Only buttons · With badge |

### Breadcrumb item (3 variant)

| Property | Values |
|---|---|
| **Type** | Default |
| **Status** | Initial · Hover · Active |

**Component Props** (6): Left icon · Show left icon · Separator icon · Show separator icon · Show text · Text

**규칙**: 마지막 항목은 현재 페이지 — 링크 없이 `aria-current="page"`. nav 컨테이너에 `aria-label`.

---

## 6. Cards

### Card (16 variant)

> 정보 단위 묶음. 응답형 카드.
> 데이터를 다양한 형식·맥락(블로그·앱·프로필 등)으로 표현.

| Property | Values |
|---|---|
| **Type** | Default · With Image · Split · Centered with full image · User profile · Login form · E-commerce · With full width tabs · With nav tabs · Pricing card · Testimonial card · Cars with list · Crypto · Grid card |
| **Mobile Version** | False · True |

**Component Props** (3): Heading · Show Heading · Paragraph

**규칙**
- 시멘틱 HTML — `<article>` (독립) / `<section>` (구획)
- 카드 전체 클릭 가능 → `<a>` 로 감싸기. 단 내부 액션 버튼 있으면 금지
- Hover — border-color + shadow 변화로 충분. `transform: scale` 금지

#### 🎯 UX 의도
- **정보 단위 구분**: 한 카드 = 한 가지 의미 (한 거래 / 한 사용자 / 한 차주)
- **그리드 정렬**: 카드 높이 통일 (`align-items: stretch`) — 시각 정렬 깨짐 방지
- **클릭 영역 명확**: 전체 클릭 + 내부 액션 동시 사용 금지 (HTML invalid — `<a>` 안 `<button>` 불가)
- **이미지 비율 일관**: 카드 그룹 안에서 16:9 또는 4:3 통일

#### ✅ 권장 (Do)

```vue
<!-- 데이터 카드 — 전체 클릭 -->
<BCard variant="default" :href="`/deal/${id}`">
  <BCardHeader title="과천2단지 재건축 PF" subtitle="2026.04.20 등록" />
  <p>약정금액 ₩345억 · IRR 8.5%</p>
</BCard>

<!-- 폼 + 액션 — 내부 버튼 있어 href 없음 -->
<BCard variant="login-form">
  <BCardHeader title="로그인" />
  <BFormField label="이메일"><BInput type="email" /></BFormField>
  <BFormField label="비밀번호"><BInput type="password" /></BFormField>
  <BButton color="brand" style="width: 100%;">로그인</BButton>
</BCard>

<!-- KPI 위젯 -->
<BCard variant="grid">
  <BStat label="총 자산" value="₩1,234,567,890" delta="+2.4%" trend="up" />
</BCard>
```

#### ❌ 지양 (Don't)

```vue
<!-- ❌ 카드 전체 링크 + 내부 버튼 — HTML invalid -->
<BCard href="/detail">
  <p>...</p>
  <BButton>편집</BButton>  <!-- ← <a> 안 <button> 금지 -->
</BCard>

<!-- ❌ transform: scale on hover — 인접 카드 가림 -->
<BCard style="transition: transform; transform: scale(1.05) on hover" />
<!-- → border-color + box-shadow 변화 -->

<!-- ❌ 카드 안에 카드 — 시각 위계 혼란 -->
<BCard><BCard>...</BCard></BCard>

<!-- ❌ 카드 한 줄에 여러 의미 -->
<BCard>
  <h3>사용자 정보</h3>... <h3>설정</h3>... <h3>알림</h3>...
</BCard>  <!-- → 카드 분리 또는 Tabs -->
```

---

## 7. Chat bubble

### Chat bubble (12 variant)

채팅 메시지 단위.

| Property | Values |
|---|---|
| **Type** | Text · File · Audio · Calendar · Images · URL · Type10 · Type11 · Type12 · Type7 · Type8 · Type9 |
| **Direction** | Left · Right |

### Bubble (4 variant) — wrapper

| Property | Values |
|---|---|
| **Orientation** | Right · Left |
| **Clean** | False · True |

**Component Props** (6): Icon · Show icon · Status · Sender name · Time · Show time

### Bubble content (7 variant)

| Property | Values |
|---|---|
| **Type** | Text · File · Audio · Calendar · Image & text · Grid images · Link |

**규칙**: Avatar 좌측 — 수신, 우측 — 발신. Status 표시(전송됨/읽음/실패) 시간 옆에 미니 아이콘.

---

## 8. Carousel

### Carousel (4 variant)

| Property | Values |
|---|---|
| **Type** | Default · Default with controls & indicators · With cards · Cards & bottom controls |

### Carousel control (1 variant) — 좌/우 화살표
### Carousel indicator (2 variant) — 페이지 점

| Property | Values |
|---|---|
| **Type** | Dot |
| **Status** | Initial · Hover/Active |

**규칙**: `prefers-reduced-motion` 존중. 자동재생은 hover/focus 시 일시정지. ARIA `role="region"` + `aria-roledescription="carousel"`.

---

## 9. Drawer

### Drawer (8 variant)

화면 가장자리 슬라이드 패널.

| Property | Values |
|---|---|
| **Type** | Default · Type3 · With forms · Advanced · With alert · With list · Bottom · Swipeable |

### Drawer heading (8 variant)

| Property | Values |
|---|---|
| **Type** | Heading · Logo · Logo & icons · Logo & text · With user group · With breadcrumb · With icon · With avatar |

**Component Props** (7): Drawer heading · Heading icon · Show heading left icon · Text · Show left icon · Helper text · Show helper text

### Drawer footer (4 variant)

| Property | Values |
|---|---|
| **Type** | Two buttons · Buttons & links · 3 buttons · Type4 |
| **Breakpoint** | Desktop · Mobile |

**규칙**: Modal 과 동일한 focus trap · ESC 닫기 · `aria-modal` 적용. 모달보다 큰 콘텐츠 / 다단계 수정에 적합.

---

## 10. Dropdowns

### Dropdown menu (16 variant)

| Property | Values |
|---|---|
| **Type** | Default · User profile · with separator · With radio input · With toggle · Checkbox · Users · With scroll · Heading & Button · User selection · Text & illustration · With forms · Grid · Language select · Menu · With number inputs |

### Dropdown list item (48 variant = 16 × 3)

| Property | Values |
|---|---|
| **Type** | Secondary text · Default · Two icons · Left form · Right form · With avatar & form · Only form · With badge · User select · Boxed · Icon shapes · With flag · Flag & checkbox · Form & dot · Form & icons · Checkbox & icon |
| **State** | Initial · Hover · Disabled |

**Component Props** (5): Left icon · Show left icon · Text · Secondary text · Right icon

### Dropdown header (5 variant)

| Property | Values |
|---|---|
| **Type** | Text & helper · With avatar · Form · Selector · Big avatar |

### Dropdown (ready to use examples) (6 variant) — 트리거 + 메뉴 조합

| Property | Values |
|---|---|
| **Type** | Button · Ghost button · Icon only button · Type4 · Avatar & name · Link |

**규칙**: 필수 키보드 — `↓` `↑` `Home` `End` `Enter` `Space` `Esc` + 알파벳 typeahead. `role="menu"`.

#### 🎯 UX 의도
- **Dropdown vs Select 결정**: 액션 메뉴(각 항목이 다른 동작) → Dropdown · 폼 값 선택 → Select
- **위치 자동 조정**: 트리거 근처에 충분한 공간 없으면 자동 flip (`bottom-start` → `top-start`)
- **위험 액션 격리**: "삭제" 같은 항목은 구분선(divider) 아래 + danger 색
- **카테고리 그룹화**: 항목 7개 이상이면 헤더로 그룹 분할

#### ✅ 권장 (Do)

```vue
<!-- 행 액션 메뉴 — 표준 -->
<BDropdown placement="bottom-end" menu-type="default">
  <template #trigger>
    <BButton icon-only size="sm" aria-label="작업">
      <MoreVertical :size="14" />
    </BButton>
  </template>
  <BDropdownItem @click="view">상세 보기</BDropdownItem>
  <BDropdownItem @click="edit">수정</BDropdownItem>
  <BDropdownItem @click="duplicate">복제</BDropdownItem>
  <BDropdownItem divider />
  <BDropdownItem danger @click="del">삭제</BDropdownItem>
</BDropdown>

<!-- 사용자 메뉴 -->
<BDropdown placement="bottom-end" menu-type="user-profile">
  <template #trigger>
    <BAvatar src="..." initials="홍" />
  </template>
  <BDropdownHeader type="with-avatar" title="홍길동" subtitle="hong@noaats.com" />
  <BDropdownItem><User :size="14" /> 내 프로필</BDropdownItem>
  <BDropdownItem><Settings :size="14" /> 설정</BDropdownItem>
  <BDropdownItem divider />
  <BDropdownItem danger><LogOut :size="14" /> 로그아웃</BDropdownItem>
</BDropdown>

<!-- 다중 선택 (체크박스 메뉴) -->
<BDropdown menu-type="checkbox" placement="bottom-start">
  <template #trigger>
    <BButton color="ghost"><Filter :size="14" /> 컬럼 ({{ selectedCols.length }})</BButton>
  </template>
  <BDropdownItem v-for="c in cols" :key="c.key">
    <BCheckbox v-model="c.visible" :label="c.label" />
  </BDropdownItem>
</BDropdown>
```

#### ❌ 지양 (Don't)

```vue
<!-- ❌ 폼 입력에 Dropdown — Select 사용 -->
<BDropdown>
  <BDropdownItem v-for="c in countries" @click="form.country = c">{{ c }}</BDropdownItem>
</BDropdown>
<!-- → BSelect v-model="form.country" :options="countries" -->

<!-- ❌ 항목 30개 + 스크롤 없음 -->
<BDropdown>
  <BDropdownItem v-for="item in 30Items" />
</BDropdown>
<!-- → menu-type="with-scroll" + 검색 입력 -->

<!-- ❌ 위험 액션이 일반 항목 사이에 -->
<BDropdownItem>수정</BDropdownItem>
<BDropdownItem danger>삭제</BDropdownItem>  <!-- ← 구분선 없이 인접 — 실수 클릭 위험 -->
<BDropdownItem>복제</BDropdownItem>
<!-- → BDropdownItem divider + danger 항목을 맨 아래로 -->

<!-- ❌ 외부 클릭 닫기 누락 (직접 구현 시) -->
<div v-if="open" class="my-dropdown">...</div>
<!-- → BDropdown 또는 useClickOutside 사용 -->
```

---

## 11. Forms

폼 카테고리는 17개 서브 컴포넌트로 가장 풍부함.

### Input form (7 variant)

| Property | Values |
|---|---|
| **Type** | Default · Inline · With Button · 2 inputs · Range inputs · Stacked · 2 buttons |

### Select form (7 variant)

| Property | Values |
|---|---|
| **Type** | Default · Inline · With Buttons · 2 inputs · Range inputs · 3 inputs · Left button |

### Search form (8 variant)

| Property | Values |
|---|---|
| **Type** | Default · Inline · With buttons · With spaced buttons · Inner button · Three buttons · With button (spaced) · With button (no-space) |

### Number form (13 variant)

| Property | Values |
|---|---|
| **Type** | Default · Inline · With buttons · With spaced buttons · Inner button · Three buttons · Stacked placeholder · With progress bar · Stacked inputs · Stacked full · Range · Ghost input · One button |

### Phone form (8 variant)

| Property | Values |
|---|---|
| **Type** | Default · Inline · With spaced buttons · Inner button · Three buttons · Stacked with button · Select & Button · One button |

### Copy to clipboard form (10 variant)

| Property | Values |
|---|---|
| **Type** | Default · Inline · With spaced buttons · Inner button · Three buttons · Select & Button · Default with button · Add-on · Two buttons · Large input |

### Input field (140 variant) ★

| Property | Values |
|---|---|
| **Type** | Default · Add-on icon · Add-on text · Inner button · Stacked placeholder |
| **Size** | sm · base · lg · xl |
| **State** | Initial · Focus/Typing · Filled · Disabled · Read-only · Success · Danger |

**Component Props** (12): Left icon · Right icon · Placeholder text · Show left icon · Show right icon · Filled text · Show filled text · Typing text · Show placeholder · Show typing text · Add-on text · Top placeholder

**State ↔ ARIA**:
- Disabled → `disabled`
- Read-only → `readonly`
- Success → `aria-invalid="false"`
- Danger → `aria-invalid="true"` + `aria-describedby`

#### 🎯 UX 의도
- **즉시 검증**: blur 시점에 검증 표시 (typing 중 빨강 깜빡임 금지)
- **에러 위치**: 필드 바로 아래 — 시선이 다음 액션으로 가기 전에 발견
- **자동완성 인지**: 브라우저 native autocomplete 활용 (이름·이메일·주소 등) — `autocomplete` 속성 명시
- **단위·통화 보조**: 금액은 `addon-text` 로 `₩` 또는 `원` 좌측/우측에 — placeholder만으로 단위 추측 금지

#### ✅ 권장 (Do)

```vue
<!-- 기본 — 라벨 명시적 연결 -->
<BFormField label="이메일" :error="errors.email">
  <BInput
    v-model="form.email"
    type="email"
    autocomplete="email"
    placeholder="name@company.com"
  />
</BFormField>

<!-- 통화 입력 — Add-on text -->
<BFormField label="투자금액">
  <BInput
    v-model="form.amount"
    type="number"
    variant="addon-text"
    addon-text="₩"
  />
</BFormField>

<!-- 검색 — Inner button -->
<BInput
  v-model="query"
  type="search"
  variant="inner-button"
  placeholder="ISIN 또는 종목명"
>
  <template #inner-button>
    <BButton size="sm" @click="search">검색</BButton>
  </template>
</BInput>

<!-- 에러 — 무엇이 + 왜 + 어떻게 -->
<BFormField
  label="이메일"
  :error="'이메일 형식이 올바르지 않습니다. @ 기호를 포함해 주세요.'"
>
  <BInput v-model="form.email" :error="true" type="email" />
</BFormField>
```

#### ❌ 지양 (Don't)

```vue
<!-- ❌ placeholder만으로 라벨 대체 — 입력 시작하면 안내 사라짐 -->
<BInput placeholder="이메일을 입력하세요" />  <!-- → BFormField label 사용 -->

<!-- ❌ 사용자 책망형 에러 메시지 -->
<BFormField error="잘못 입력했습니다" />  <!-- → 형식·이유 설명 -->

<!-- ❌ typing 중 빨강 표시 — 사용자가 입력 시작하자마자 빨강 → 불쾌 -->
<BInput v-model="email" :error="!validateOnType(email)" />  <!-- → blur 시점에만 -->

<!-- ❌ 비밀번호에 type="text" — 보안 + 모바일 키보드 -->
<BInput type="text" placeholder="비밀번호" />  <!-- → type="password" -->

<!-- ❌ 단위 placeholder — 입력 시작하면 사라짐 -->
<BInput placeholder="₩ 금액 입력" />  <!-- → variant="addon-text" addon-text="₩" -->

<!-- ❌ 폼 그룹 외에 검증 표시 (예: alert 으로 전체 폼 에러 나열) -->
<BAlert>이메일 형식 오류, 비밀번호 8자 이상...</BAlert>  <!-- → 각 필드 옆에 inline -->
```

#### 📐 폼 레이아웃 가이드
- **수직 정렬** — 데스크톱 1 열, 모바일 1 열 통일 (zigzag 시선 방지)
- **연관 필드 그룹** — 이름(성·이름) 같은 짝은 가로 배치, 나머지는 수직
- **필수/선택 표기** — 필수에 `*` 라벨 + `required` 속성. "선택" 라벨은 명시적으로
- **자동 포커스** — 폼 열렸을 때 첫 입력에 자동 포커스 (`autofocus` 또는 `ref().focus()`)

### File upload input (2 variant)

| Property | Values |
|---|---|
| **Type** | Default · Advanced |

### File upload input (10 variant) — state matrix

| Property | Values |
|---|---|
| **Type** | Advanced · With CTAs |
| **Size** | base |
| **State** | Disabled · Success · Danger · False · Hover |

### Tag input (24 variant)

| Property | Values |
|---|---|
| **Type** | Default |
| **Size** | sm · base · lg · xl |
| **State** | Initial · Focus · Filled · Disabled · Success · Danger |

**Component Props** (6): Show placeholder · Placeholder text · Right icon style · Show right icon · Typing text · Show typing text

### OTP Inputs (28 variant) ★ T5 Step-up MFA 필수

| Property | Values |
|---|---|
| **Type** | Default |
| **Size** | sm · base · lg · xl |
| **State** | Initial · Focus · Filled · Disabled · Read-only · Success · Danger |

### OTP form examples (4 variant)

| Property | Values |
|---|---|
| **Type** | Separator spaced · Spaced · Default · Separator |

### Textarea input (10 variant)

| Property | Values |
|---|---|
| **Type** | Default · Chatroom · Buttons inside · Buttons outside |
| **State** | Initial · Focus · Filled |

### Rich text editor input (9 variant)

| Property | Values |
|---|---|
| **Type** | Buttons inside · Buttons outside · Default |
| **State** | Initial · Focus · Filled |

### Textarea input ready to use examples (3 variant)

| Property | Values |
|---|---|
| **Type** | Default · Inline · With button |

### Label (3 variant) · Helper (5 variant)

| 컴포넌트 | Type |
|---|---|
| Label | Default · Required & icon · Two texts |
| Helper | Default · Tags · Left & Right text · Progress bar · features |

**규칙**: 모든 폼 필드에 Label htmlFor 연결 필수. placeholder만으로 라벨 대체 금지.

---

## 12. Floating label inputs

### Floating label (84 variant)

| Property | Values |
|---|---|
| **Type** | Default · With background · Bordered |
| **Size** | sm · base · lg · xl |
| **State** | Initial · Focus · Filled · Disabled · Read only · Success · Danger |

---

## 13. Datepicker

### Datepicker cell (8 variant)

| Property | Values |
|---|---|
| **Type** | Day |
| **State** | Initial · Hover · Active · Active start · Active between · Active end · Disabled · Outlined |

### Datepicker controls (1 variant) — 헤더 좌우 화살표 + 월 선택
### Datepicker dropdown menu (5 variant)

| Property | Values |
|---|---|
| **Type** | Default · Range · Year picker · Month · Day/Year/Month |

### Datepicker (ready to use examples) (2 variant)

| Property | Values |
|---|---|
| **Type** | Default · Range |

**규칙**: 한국어 포맷 `2026.04.20` 또는 `2026년 4월 20일`. 입력 위주는 네이티브 input 권장 (모바일 OS 키보드 우수).

---

## 14. Timepicker

### Timepicker input (84 variant) — Input field 와 동일 매트릭스

| Property | Values |
|---|---|
| **Type** | Default · Left add-on · Right add-on |
| **Size** | sm · base · lg · xl |
| **State** | Initial · Focus · Filled · Disabled · Read-only · Success · Danger |

### Timepicker form (6 variant)

| Property | Values |
|---|---|
| **Type** | Default · Inline · With Dropdown · With button · With select · With input |

### Timepicker dropdown menus (4 variant)

| Property | Values |
|---|---|
| **Type** | Default · Timepicker dropdown menus · Advanced with buttons · Advanced with slider |

---

## 15. Autocomplete

### Autocomplete input (ready to use examples) (6 variant)

| Property | Values |
|---|---|
| **Type** | Default · User · Highlighted text · Advanced with save · Advanced · Categories |

### Autocomplete dropdown menu (5 variant)

| Property | Values |
|---|---|
| **Type** | Default · CTA · With save option · Advanced · Buttons & categories |

### Autocomplete list item (15 variant)

| Property | Values |
|---|---|
| **Type** | Default · With avatar & form · Bordered · Heading & description · With secondary text |
| **State** | Initial · Hover · Disabled |

**규칙**: 옵션 20+ 일 때 Autocomplete (Search-driven combobox). 옵션 < 20 → Select.

---

## 16. Range slider

### Range Slider examples (2 variant)

| Property | Values |
|---|---|
| **Type** | Default · Range |

### Range Slider bars (2 variant)

| Property | Values |
|---|---|
| **Type** | Range · Default |
| **Size** | base |

### Range Slider thumb (2 variant)

| Property | Values |
|---|---|
| **Type** | Default |
| **State** | Active · Initial |

### Range Slider helper (1 variant)

**규칙**: 키보드 — `←` `→` 1단위 · `Shift + ← →` 10단위 · `Home`·`End` 양끝. 모바일 thumb 최소 44×44 터치 영역. prefix/unit 표시 가능 (예: `₩`, `%`).

---

## 17. Toggle

### Toggle switches (12 variant)

| Property | Values |
|---|---|
| **Type** | Default |
| **Size** | base · lg |
| **Status** | Initial · Focus · Disabled |
| **Checked** | False · True |

### Toggle inputs (8 variant)

| Property | Values |
|---|---|
| **Type** | Default · Left & Right label · Left & Right icons · Advanced · Simple card · Logo & text · Advanced with icon · Only icons |

### Toggle group (3 variant)

| Property | Values |
|---|---|
| **Type** | Default · Integration · Settings |

### Label (toggle) (2 variant)
### Helper (toggle) (1 variant)

**규칙**: **즉시 반영되는** on/off 만 사용. 저장 버튼 필요 시 Checkbox. 라벨은 토글 좌측.

---

## 18. Checkbox & radio

### Checkbox input (12 variant)

| Property | Values |
|---|---|
| **Type** | Checkbox input · Radio input |
| **Size** | base · Default |
| **Status** | Initial · Focus · Disabled |
| **Checked** | False · True |

### Checkbox/Radio form (54 variant)
### Checkbox/Radio group (10 variant)

| Property | Values |
|---|---|
| **Type** | Default · Cards · Card + Icon · Big cards grid · Small cards grid · Only icons · Advanced · With numbers · With avatar · With description |

### Label (4 variant) · Helper (1 variant)

**규칙**: Indeterminate state 지원 (`aria-checked="mixed"`). 옵션 < 5 → Radio, 5-20 → Select, 20+ → Autocomplete.

---

## 19. Gallery

### Gallery (11 variant)

| Property | Values |
|---|---|
| **Type** | Grid 3 columns · Grid 2 columns · Featured image · Carousel · With filters · Masonry |
| **Breakpoint** | Mobile · Tablet · Desktop |

---

## 20. Icon shape

### Icon Shape (84 variant) — Size 6 × Color 7 × Type 2

| Property | Values |
|---|---|
| **Size** | xs · sm · base · lg · xl · 2xl |
| **Color** | White · Dark · Brand · Red · Gray · Green · Yellow |
| **Type** | Circle · Square |

**용도**: 아이콘을 시각적으로 강조하는 컨테이너. Empty state, Error state, Onboarding card 등.

---

## 21. List

### List item (33 variant)

| Property | Values |
|---|---|
| **Type** | Default · Image & number · With number · with badge · With icons · Card · With right icon · Image & buttons · Image & checkbox · Transaction · Avatar & text + badge |
| **State** | Initial · Hover/Active · Disabled |

**Component Props** (12)

### List (4 variant)

| Property | Values |
|---|---|
| **Type** | Divider · Card · Default · CTA |

---

## 22. Navbar

### Nav link (52 variant)

| Property | Values |
|---|---|
| **Type** | Default · Flag & icon · Two icons · Icon top · Double text · Heading & helper · With icon shape · Bordered · Right icon · Big Icons · Top icon & cta · Logo & text · 2 icons |
| **State** | Initial · Hover · Active · Disabled |

### Navbar (15 variant)

| Property | Values |
|---|---|
| **Type** | Default · With icons · With search input · With links · Centered logo |
| **Breakpoint** | Mobile · Tablet · Desktop |

### Navbar content (27 variant)

| Property | Values |
|---|---|
| **Type** | With icons · With search input · Default · With links · Centered logo · Only links · With dropdowns and social icons · Links & Input · Text |
| **Breakpoint** | Mobile · Tablet · Desktop |

### Megamenu (full-width) (3 variant)
### Megamenu (fixed-width) (8 variant)
### Mobile menu (2 variant)

---

## 23. Nav Tabs

### Nav tabs (12 variant) ★ 03-component-usage.md §8 정합

| Property | Values |
|---|---|
| **Type** | Default · Pills · Button · Button group · Border bottom · Vertical with list item · Vertical pills |
| **State** | Initial |
| **Mobile** | False · True |

### Nav item (8 variant)

| Property | Values |
|---|---|
| **Type** | Default · Pill · Border bottom · User select |
| **State** | Initial · Hover/Active |

**규칙**: `role="tablist"` · 각 탭 `aria-selected` · 각 패널 `aria-labelledby`. Roving tabindex.

#### 🎯 UX 의도
- **Tabs vs 분리 페이지**: 한 컨텍스트 안 콘텐츠 그룹 → Tabs · 의미 다른 페이지 → 사이드바
- **첫 탭 의미**: 첫 진입 시 가장 자주 사용하는 탭 (또는 마지막 방문 탭 메모리)
- **변경 비파괴**: 탭 전환 시 폼 데이터 손실 금지 (탭별 state 보존)
- **수직 vs 수평**: 탭 수 5+ 일 때 vertical, 3-5 일 때 horizontal

#### ✅ 권장 (Do)

```vue
<!-- 페이지 내 콘텐츠 그룹 -->
<BTabs v-model="activeTab" variant="default">
  <template #tabs>
    <BTab name="overview" label="개요" />
    <BTab name="members"  label="멤버" />
    <BTab name="settings" label="설정" />
  </template>
  <div v-if="activeTab === 'overview'">...</div>
  <div v-if="activeTab === 'members'">...</div>
</BTabs>

<!-- 설정 페이지 — 항목 많을 때 vertical -->
<BTabs v-model="settings" variant="vertical-list">
  <template #tabs>
    <BTab name="profile"      label="프로필" />
    <BTab name="security"     label="보안" />
    <BTab name="notification" label="알림" />
    <BTab name="billing"      label="결제" />
    <BTab name="team"         label="팀" />
  </template>
  <!-- ... -->
</BTabs>

<!-- 짧은 카테고리 토글 — button-group -->
<BTabs v-model="period" variant="button-group">
  <template #tabs>
    <BTab name="MTD" label="MTD" />
    <BTab name="QTD" label="QTD" />
    <BTab name="YTD" label="YTD" />
  </template>
</BTabs>
```

#### ❌ 지양 (Don't)

```vue
<!-- ❌ 탭에 의미가 다른 페이지 — 사이드바 사용 -->
<BTabs>
  <BTab name="users" label="사용자 관리" />
  <BTab name="reports" label="리포트 생성" />  <!-- 별도 페이지여야 함 -->
</BTabs>

<!-- ❌ 탭 전환 시 폼 리셋 — 사용자 입력 손실 -->
<BTabs v-model="tab" @change="form = {}" />  <!-- → state 보존 -->

<!-- ❌ 탭 라벨이 길어 줄바꿈 -->
<BTab label="이번 달 거래 내역 (월간 보고서 포함)" />  <!-- → "월간" -->

<!-- ❌ 모바일에 가로 탭 10개 — 가로 스크롤 -->
<BTabs variant="default">10 BTab</BTabs>  <!-- → vertical 또는 drawer -->

<!-- ❌ 활성 탭 표시 색만 — 색약 사용자 차단 -->
<BTab :class="{ 'text-blue': active }" />  <!-- → 보더 + 굵기 + 색 다중 단서 -->
```

---

## 24. Modals

### Modal (10 variant) ★ OTP — T5 Step-up MFA 핵심

| Type | 권장 size | 용도 |
|---|---|---|
| Info | sm | 단순 안내 |
| Popup | sm | 확인/취소 |
| Sign In | base | 로그인 폼 |
| Create product | base | 폼 모달 |
| With radio inputs | sm | 옵션 선택 |
| With timeline | lg | 단계별 마법사 |
| With progress bar | sm | 업로드/처리 |
| With list | base | 항목 선택 |
| **OTP** | sm | 인증번호 (★ T5) |
| Share | base | 공유 옵션 |

### Modal header (7 variant)

| Property | Values |
|---|---|
| **Type** | Default · Icon + Heading & helper text · Breadcrumb · Avatar group · With Logo · Logo & icons · Heading & supporting text |

### Modal footer (4 variant)

| Property | Values |
|---|---|
| **Type** | Buttons · Buttons & links · 3 buttons · Pagination |

**규칙**: `role="dialog"` · `aria-modal="true"` · `aria-labelledby` 필수. ESC 닫기 · focus trap · 첫 focusable 포커스 · 닫을 때 트리거로 복귀. 미저장 폼 모달은 backdrop 클릭 차단.

#### 🎯 UX 의도
- **차단의 의도**: 사용자에게 명시적 응답을 요구. 다른 작업 불가
- **크기 선택**: 콘텐츠 양에 맞게 — sm(480) 단순 확인, base(720) 폼, lg(960) 마법사
- **footer 액션 순서**: 좌측 보조(취소·뒤로) · 우측 1차(저장·확인). 화면 시선이 우하단으로 흐르도록
- **닫힘 경로**: ESC · 좌상단 X · backdrop 클릭 (단 미저장 폼은 confirm 또는 차단)
- **이중 모달 금지**: 모달 위에 모달 — 사용자 길 잃음

#### ✅ 권장 (Do)

```vue
<!-- 단순 안내 — Info, sm -->
<BModal v-model="open" type="info" size="sm" title="저장 완료">
  거래가 성공적으로 등록되었습니다.
  <template #footer>
    <BButton @click="open = false">확인</BButton>
  </template>
</BModal>

<!-- 확인/취소 — Popup, sm -->
<BModal v-model="open" type="popup" size="sm" title="삭제하시겠습니까?" persistent>
  삭제된 거래는 30일간 보관되며, 이후 영구 삭제됩니다.
  <template #footer>
    <BButton color="secondary" @click="open = false">취소</BButton>
    <BButton color="danger" @click="confirmDelete">삭제</BButton>
  </template>
</BModal>

<!-- 폼 입력 — base + 미저장 보호 -->
<BModal v-model="open" type="create-product" size="base" title="새 거래 등록" persistent>
  <BFormField label="ISIN" :error="errors.isin">
    <BInput v-model="form.isin" placeholder="KR1035010001" />
  </BFormField>
  <!-- ... -->
  <template #footer>
    <BButton color="secondary" @click="onCancel">취소</BButton>
    <BButton :disabled="!isValid" :loading="saving" @click="save">등록</BButton>
  </template>
</BModal>

<!-- ★ OTP — T5 Step-up MFA -->
<BModal v-model="open" type="otp" size="sm" title="인증번호 입력" persistent>
  등록된 인증 앱의 6자리 코드를 입력해 주세요.
  <BFormField label="인증번호">
    <BInput v-model="otp" type="tel" maxlength="6" inputmode="numeric" autofocus />
  </BFormField>
  <template #footer>
    <BButton color="secondary" @click="close">취소</BButton>
    <BButton :disabled="otp.length !== 6" :loading="verifying" @click="verify">
      인증
    </BButton>
  </template>
</BModal>
```

#### ❌ 지양 (Don't)

```vue
<!-- ❌ 단순 알림에 Modal — Toast 면 충분 -->
<BModal v-model="open">파일이 업로드되었습니다.</BModal>  <!-- → BToast -->

<!-- ❌ 모달 위 모달 — 사용자 path 혼란 -->
<BModal v-model="open1">
  ...
  <BModal v-model="open2">...</BModal>  <!-- → Drawer or Wizard step -->
</BModal>

<!-- ❌ backdrop 클릭으로 미저장 폼 닫힘 (persistent 누락) -->
<BModal v-model="open" title="새 거래 등록">
  <BInput v-model="form.isin" />  <!-- 데이터 입력 중인데 실수 클릭으로 닫힘 -->
</BModal>

<!-- ❌ ESC 비활성화 — 키보드 사용자 차단 -->
<BModal v-model="open" :close-on-escape="false">  <!-- 정당한 이유 있을 때만 -->

<!-- ❌ footer 액션 순서 반대 (1차 좌측) -->
<template #footer>
  <BButton color="brand">저장</BButton>  <!-- ← 시선 흐름과 반대 -->
  <BButton color="secondary">취소</BButton>
</template>

<!-- ❌ 모달 크기 고정 픽셀 (반응형 깨짐) -->
<BModal style="width: 800px;">  <!-- → size="base" -->
```

#### 📐 키보드·접근성 핵심
- **ESC** → 닫기 (persistent 모달은 confirm 다이얼로그 또는 무시)
- **Tab** → 모달 내부에서만 순환 (focus trap)
- **Shift+Tab** → 첫 요소에서 마지막 요소로 (역방향 trap)
- **열릴 때** → 첫 focusable 요소 자동 포커스 (보통 첫 input 또는 1차 버튼)
- **닫힐 때** → 모달 열기 전 트리거 요소로 복귀

---

## 25. Popovers

### Popover (8 variant)

| Property | Values |
|---|---|
| **Type** | Default · Vertical items |
| **Position** | Left · Right · Top · Bottom |

### Popover content (9 variant)

| Property | Values |
|---|---|
| **Type** | Text · User profile · Company profile · Description & image · Progress bar · Password strength · Paragraphs · Illustration & CTA · User details |

---

## 26. Pagination

### Pagination (9 variant)

| Type | 용도 |
|---|---|
| Two buttons | 이전/다음만 |
| Default | 페이지 번호 (가장 보편) |
| Text buttons | 텍스트 페이지 버튼 |
| Buttons & helper text | 카운트 + 페이지 번호 |
| **With dropdown** | per-page 선택 + 페이지 번호 (관리 페이지 표준) |
| With input | 페이지 번호 직접 입력 |
| Simple with input | 입력만 |
| Arrows | 화살표만 |
| Dropdown & buttons | 조합 |

**규칙**: `<nav aria-label="페이지네이션">` · `aria-current="page"` · URL 동기화 (`?page=3`) 필수.

#### 🎯 UX 의도
- **위치 일관**: 표 하단 우측 (좌측은 per-page + 카운트, 우측은 페이지 번호)
- **건너뛰기**: 1, 2, 3, ..., 마지막 — 처음과 끝 항상 표시
- **현재 페이지 강조**: 색 + 굵기 + `aria-current` (다중 단서)
- **모바일 단순화**: 모바일은 "이전·다음 + 1/124" 형태

#### ✅ 권장 (Do)

```vue
<!-- 관리 페이지 표준 (with-dropdown) -->
<div class="row" style="justify-content: space-between;">
  <div class="row" style="gap: 12px;">
    <BSelect
      v-model="perPage"
      :options="[
        { label: '10개씩', value: 10 },
        { label: '20개씩', value: 20 },
        { label: '50개씩', value: 50 },
        { label: '100개씩', value: 100 },
      ]"
      size="sm"
    />
    <span class="muted">전체 1,234건</span>
  </div>
  <BPagination v-model="page" :total="1234" :per-page="perPage" show-prev-next />
</div>

<!-- 모바일 — 컴팩트 -->
<BPagination v-model="page" :total="1234" :per-page="20" simple />
```

#### ❌ 지양 (Don't)

```vue
<!-- ❌ 무한 스크롤만 — 사용자가 위치 잃음 -->
<div @scroll="loadMore" />
<!-- → 페이지네이션 또는 "더보기" 버튼 -->

<!-- ❌ URL 동기화 누락 — 새로고침 시 1페이지로 -->
<BPagination v-model="page" />  <!-- → router.push({ query: { page } }) -->

<!-- ❌ 페이지 번호만 10개 (시작·끝 없음) -->
1 2 3 4 5 6 7 8 9 10  <!-- → 1 ... 5 6 [7] 8 9 ... 124 -->

<!-- ❌ 페이지 진입 시 항상 1페이지로 강제 -->
onMounted(() => page.value = 1)  <!-- → URL ?page= 유지 -->
```

---

## 27. Progress Bars

### Progress Bars (18 variant) — Color 6 × Type 3

| Property | Values |
|---|---|
| **Color** | Primary · Gray · Dark · Success · Danger · Warning |
| **Type** | Top label · Side label · Bottom label |

### Bars (8 variant) — bar 자체

| Property | Values |
|---|---|
| **Size** | base · lg |
| **Value** | 100 · 75 · 50 · 25 |

---

## 28. Rating

### Rating (3 variant)

| Property | Values |
|---|---|
| **Type** | Default · With badge · Radio input |

---

## 29. Sidebar

### Sidebar (Free component) (4 variant)

| Property | Values |
|---|---|
| **Type** | Default · With alert · Double sidebar |
| **Contracted** | False · True |

### Navigation list item (7 variant)

| Property | Values |
|---|---|
| **Type** | With icon · With badge |
| **State** | Initial · Hover/Active · Disabled · Collapsed |

### Sidebar header (3 variant)

| Property | Values |
|---|---|
| **Type** | Heading · Logo · User select |

---

## 30. Spinners

### Spinner (10 variant)

| Property | Values |
|---|---|
| **Type** | Default |
| **Size** | large · xs · small · base · medium |
| **Track** | True · False |

### Interactive-spinner (4 variant)

| Property | Values |
|---|---|
| **State** | loader 1 · loader 2 · loader 3 · loader 4 |

**규칙**: 로딩 ≤ 1초 → Spinner · 1초+ → Skeleton.

---

## 31. Speed Dial

### Speed dial (ready to use examples) (16 variant)

| Property | Values |
|---|---|
| **Type** | Default · Rounded · Menu · Article dropdown menu |
| **Position** | Top · Bottom · Right · Left |

### Speed dial (navigation link) (25 variant)

| Property | Values |
|---|---|
| **Type** | Default · Text & icon · Outside text · Icon & text |
| **State** | Initial · Hover · Disabled |
| **Rounded** | False · True |
| **Horizontal mode** | False · True |

### Speed dial (menus) (3 variant) · Interactive (5+2)

---

## 32. Skeleton

### Skeleton (6 variant)

| Property | Values |
|---|---|
| **Type** | Card + Image · Image + Text · Text · List · Simple text · Widget |

**규칙**: 300ms 지연 시작 — 깜빡임 방지. 표 로딩 `aria-busy="true"`.

---

## 33. Stepper

### Stepper navigation (12 variant)

| Property | Values |
|---|---|
| **Type** | Default · Only icons · Vertical · Icons & text · With dots · Cards |
| **Breakpoint** | Mobile · Desktop |

### Stepper nav link (35 variant) ★ Maker-Checker 5단계 시각화

| Property | Values |
|---|---|
| **Type** | Default · Icon & text · Icon shape · Icon shape & text · Number & text · Card alert · Dot |
| **State** | Initial · Active · Completed · Error · Disabled |

### Stepper examples (10 variant)

**규칙**: 3-5단계가 적정. 10+ 는 분할 권장. ★ Maker-Checker 거부 시각화 — Error state.

#### 🎯 UX 의도
- **진행 인지**: 사용자가 "어디까지 왔고 얼마나 남았는지" 한눈에
- **돌아갈 수 있음 표시**: 완료한 단계는 클릭 가능 (수정), 미래 단계는 비활성
- **에러 상태**: ★ Maker-Checker 거부 시 해당 step `state: 'error'` + 사유 노출
- **방향성**: 좌→우 또는 위→아래 (RTL 언어는 우→좌)

#### ✅ 권장 (Do)

```vue
<!-- 회원가입 4단계 -->
<BStepper
  type="number"
  orientation="horizontal"
  :current="step"
  :steps="[
    { label: '계정 정보', description: '이메일·비밀번호' },
    { label: '본인 인증', description: '휴대폰 OTP' },
    { label: '약관 동의', description: '서비스·개인정보' },
    { label: '완료', description: '환영 메시지' },
  ]"
/>

<!-- ★ Maker-Checker 5단계 — 거부 시 error state -->
<BStepper
  type="icon"
  :current="3"
  :steps="[
    { label: 'Maker 작성', icon: FileText, state: 'completed' },
    { label: 'Reviewer 검토', icon: Eye, state: 'completed' },
    { label: 'Checker 결재', icon: AlertCircle, state: 'error', description: 'CCO 반려 — 사유: 한도 초과' },
    { label: 'Step-up MFA', icon: Lock, state: 'disabled' },
    { label: '확정', icon: Check, state: 'disabled' },
  ]"
/>

<!-- 컴팩트 — dot type (긴 프로세스, 시각 단순화) -->
<BStepper type="dot" :current="2" :steps="[
  { label: '주문' }, { label: '검증' }, { label: '발주' },
  { label: '확정' }, { label: '결제' }, { label: '결산' },
]" />
```

#### ❌ 지양 (Don't)

```vue
<!-- ❌ 10+ 단계 — 사용자 압도 -->
<BStepper :steps="14단계" />  <!-- → 큰 단계로 묶기 (1.회원가입 → 1-1, 1-2 하위) -->

<!-- ❌ 단계 라벨 길음 -->
<BStepper :steps="[{ label: '본인 명의 휴대폰으로 OTP 인증' }]" />
<!-- → "본인 인증" + description 으로 -->

<!-- ❌ Stepper 만으로 진행 (이전/다음 버튼 없음) — 키보드/터치 불편 -->
<BStepper :current="step" />
<!-- → 하단에 [이전] [다음] 버튼 함께 -->

<!-- ❌ Error state 시 사유 없음 — 사용자 다음 액션 모름 -->
<BStepper :steps="[{ state: 'error' }]" />
<!-- → description 에 사유 + 해결 방법 -->
```

---

## 34. Tables

### Table (free-component) (3 variant)

| Property | Values |
|---|---|
| **Type** | Striped columns · Striped rows · With table caption |
| **Breakpoint** | Desktop |

**규칙**
- 시멘틱 HTML — `<table>` 사용 (`<div>` 그리드 금지)
- `<caption>` (sr-only 허용) · `<th scope="col">`
- 정렬: `aria-sort="ascending|descending|none"`
- 로딩 — `aria-busy="true"`
- 3가지 데이터 상태 필수 — **Empty · Loading(Skeleton 300ms 지연) · Error**

#### 🎯 UX 의도
- **밀도와 가독성 trade-off** — `compact` 는 50+ 행, `default` 는 10-50 행, `cards` 는 모바일
- **숫자 우측 정렬** — 금액·이율은 `align: right` (자릿수 시각적 정렬). 음수는 빨강, 양수는 일반/초록
- **첫 열 sticky** — 가로 스크롤 시 식별 컬럼(ISIN, 종목명) 유지
- **행 액션** — 더보기 메뉴(⋮) 우선. 아이콘 2-3개는 호버 시 표시 (visibility hidden → opacity 1)
- **빈 상태의 의미** — "결과 없음" 보다 다음 액션 제안 ("필터 초기화" "새 거래 등록")

#### ✅ 권장 (Do)

```vue
<!-- 기본 — 거래 목록 -->
<BTable
  :columns="[
    { key: 'isin',   label: 'ISIN',   sortable: true, width: '140px' },
    { key: 'name',   label: '종목명', sortable: true },
    { key: 'amount', label: '금액',   sortable: true, align: 'right' },
    { key: 'status', label: '상태',   align: 'center', width: '100px' },
    { key: 'action', label: '',       align: 'right', width: '60px' },
  ]"
  :rows="rows"
  selectable
  sticky-header
  :aria-busy="loading"
>
  <!-- 음수는 빨강, 양수는 초록 -->
  <template #cell-amount="{ value }">
    <span :class="value < 0 ? 'text-danger' : ''">
      ₩{{ value.toLocaleString() }}
    </span>
  </template>

  <!-- 행 액션 — Dropdown (더보기) -->
  <template #cell-action="{ row }">
    <BDropdown placement="bottom-end">
      <template #trigger>
        <BButton color="ghost" icon-only size="sm" aria-label="작업">
          <MoreVertical :size="14" />
        </BButton>
      </template>
      <BDropdownItem @click="edit(row)">수정</BDropdownItem>
      <BDropdownItem danger @click="del(row)">삭제</BDropdownItem>
    </BDropdown>
  </template>
</BTable>

<!-- 3가지 데이터 상태 처리 -->
<BTable v-if="rows.length" :rows="rows" :columns="columns" />
<BEmptyState v-else-if="!loading && !error" title="거래 내역이 없습니다">
  <template #icon><BIconShape color="brand" size="xl"><Package /></BIconShape></template>
  <p>새 거래를 등록해 보세요.</p>
  <template #action>
    <BButton color="brand" @click="openNew">+ 새 거래 등록</BButton>
  </template>
</BEmptyState>
<BSkeleton v-else-if="loading" variant="list" :count="5" />
<BAlert v-else color="danger" title="불러오기 실패">
  네트워크 오류. <BButton size="sm" @click="reload">다시 시도</BButton>
</BAlert>
```

#### ❌ 지양 (Don't)

```vue
<!-- ❌ <div> 그리드 — 정렬·키보드·스크린리더 모두 깨짐 -->
<div class="grid grid-cols-4">
  <div>ISIN</div><div>종목명</div><div>금액</div><div>상태</div>
  <div v-for="r in rows">...</div>
</div>  <!-- → <BTable> 사용 -->

<!-- ❌ 정렬 누락 — 금액·날짜 컬럼인데 정렬 불가 -->
<BTable :columns="[{ key: 'amount', label: '금액' }]" />
<!-- → sortable: true -->

<!-- ❌ 모바일에 가로 스크롤만 — 사용자 잃음 -->
<BTable style="overflow-x: auto;" />  <!-- → cards variant 또는 첫 열 sticky -->

<!-- ❌ 빈 상태에 "결과 없음" 만 — 다음 액션 없음 -->
<div v-if="!rows.length">결과 없음</div>
<!-- → BEmptyState + CTA -->

<!-- ❌ 행 더블클릭으로만 상세 진입 — 모바일·터치 사용자 차단 -->
<tr @dblclick="goDetail(row)" />  <!-- → 단일 클릭 또는 명시적 버튼 -->

<!-- ❌ Loading spinner 만 표시 (전체 화면 가림) -->
<BSpinner v-if="loading" size="xl" style="position: fixed; inset: 0;" />
<!-- → Skeleton 으로 형태 미리 보여주기 (300ms 지연 시작) -->

<!-- ❌ 정렬 헤더에 클릭 가능 인지 없음 -->
<th>금액</th>  <!-- → 정렬 가능 시 화살표 아이콘 + cursor:pointer -->
```

#### 📐 페이지네이션 패턴 (Table footer 표준)

```
좌측: per-page Select [10▾]  ⌐  전체 1,234건
우측: ‹ 이전  1 2 3 ... 124  다음 ›
```

URL 동기화 필수 — `?page=3&perPage=10` 으로 새로고침/공유 시 위치 유지.

---

## 35. Timelines

### Timeline dots (3 variant)

| Property | Values |
|---|---|
| **Type** | Elipse · Icon · Avatar |

### Timeline (12 variant)

| Property | Values |
|---|---|
| **Type** | Default · Icons & Card · Type3 · Stacked cards · Horizontal · Horizontal advanced · Type10 · Type11 · Type12 · Type7 · Type8 · Type9 |
| **Breakpoint** | Desktop · Mobile |

### Timeline Item (12 variant)

| Property | Values |
|---|---|
| **Type** | Text & Button · As card · User actions · Stacked cards · Horizontal · Horizontal small |
| **Breakpoint** | Desktop · Mobile |

---

## 36. KBD

### KBD (49 variant)

키보드 단축키 표시 — `<kbd>` 시멘틱. Ctrl + K 조합은 `+` 구분자.

| Property | Values |
|---|---|
| **Keyboard** | Ctrl · Shift · Tab · Caps Lock · Esc · Spacebar · Enter · Q-M (개별 알파벳 26종) · F1-F12 · Arrow up/down/left/right |
| **Dark mode** | False |

---

## 37. Tooltips

### Tooltip (4 variant)

| Property | Values |
|---|---|
| **Type** | Default |
| **Position** | Top · Bottom · Left · Right |

**규칙**: 텍스트만 — 액션 버튼 넣지 말 것. 호버 200-500ms 지연 후 표시, 떠나면 즉시 제거. 키보드 focus 시 표시.

#### 🎯 UX 의도
- **보조 정보만**: 본문에 없는 추가 설명 (예: 아이콘 의미, 약어 풀이, 단축키)
- **포인터 안내**: 작은 아이콘 버튼의 의미를 텍스트로 보강 (`aria-label` + 시각 확인 가능)
- **지연 시간**: 200-500ms — 마우스가 우연히 지나갈 때 안 뜨도록

#### ✅ 권장 (Do)

```vue
<!-- 아이콘 버튼의 의미 명확화 -->
<BTooltip text="필터 초기화">
  <BButton icon-only size="sm" aria-label="필터 초기화">
    <RefreshCw :size="14" />
  </BButton>
</BTooltip>

<!-- 약어 풀이 -->
<BTooltip text="Internal Rate of Return — 내부수익률">
  <span class="abbr">IRR</span>
</BTooltip>

<!-- 단축키 안내 -->
<BTooltip>
  <template #default>
    검색
    <BKbd keyboard="Ctrl" /> +
    <BKbd keyboard="K" />
  </template>
  <BButton icon-only>
    <Search :size="16" />
  </BButton>
</BTooltip>
```

#### ❌ 지양 (Don't)

```vue
<!-- ❌ 액션 버튼을 툴팁 안에 — 클릭 영역 사라짐 -->
<BTooltip>
  <BButton>저장</BButton>  <!-- 마우스 떠나면 사라져서 클릭 불가 -->
</BTooltip>  <!-- → BPopover 사용 -->

<!-- ❌ 본문에 있는 정보를 다시 툴팁으로 — 중복 -->
<BTooltip text="저장">
  <BButton>저장</BButton>  <!-- ← 라벨이 이미 "저장" -->
</BTooltip>

<!-- ❌ 너무 긴 텍스트 -->
<BTooltip text="이 기능은 K-IFRS 1109호 기준 EIR 상각을 자동 계산하며, 회계 패턴 매핑 후 4 GAAP Saga 트랜잭션으로 처리됩니다. 자세한 내용은 ...">
  <BButton>?</BButton>  <!-- → BPopover content="paragraphs" -->
</BTooltip>

<!-- ❌ 모바일에서만 표시 — touch device 는 호버 없음 -->
<BTooltip text="..." class="md:hidden" />  <!-- → 본문에 명시 -->
```

---

## 38. Toasts

### Toast (9 variant)

| Type | 용도 |
|---|---|
| Avatar & button | 좌측 아바타 + 우측 버튼 |
| Icon shape & text | Icon shape + 텍스트 (기본) |
| Icon shape & buttons | Icon shape + 액션 버튼 |
| With header | 제목 + 본문 |
| Icon & text | 단순 아이콘 + 텍스트 |
| Warning | 경고 (자동 사라짐 금지) |
| With illustration | 일러스트 포함 |
| Error | 오류 (자동 사라짐 금지) |
| With progress bar | 진행률 표시 |

**규칙**: 위치 우하단 (모바일은 상단 허용). 자동 사라짐 3-5초. **Error·Warning 은 자동 사라짐 금지**. 동시 최대 3개, 그 이상 큐잉.

#### 🎯 UX 의도
- **순간 피드백**: 작업 완료를 0.3초 안에 알림 — 사용자가 다음 액션에 집중하면서 인지
- **방해 최소**: 위치 우하단 — 본문을 가리지 않음. 자동 fade-out
- **중요 메시지 보호**: Error · Warning · 진행률 표시는 자동 사라짐 금지 (수동 닫기)
- **누적 방지**: 같은 메시지 중복 표시 시 카운트로 합침 ("저장됨 × 3")

#### ✅ 권장 (Do)

```js
// 성공 — 자동 사라짐 OK
toast.success('거래가 등록되었습니다.')

// 실패 — 자동 사라짐 X (사용자 인지 보장)
toast.error('거래 등록에 실패했습니다.', { duration: 0 })

// 진행률 — duration 0 + 진행 콜백
const t = toast.show({
  type: 'with-progress-bar',
  title: '파일 업로드 중',
  duration: 0,
})
uploadFile().on('progress', p => t.setProgress(p))

// 액션 포함 — Undo 패턴
toast.show({
  type: 'icon-shape-buttons',
  title: '거래가 삭제되었습니다.',
  actions: [{ label: '실행 취소', onClick: () => restore() }],
  duration: 5000,
})
```

#### ❌ 지양 (Don't)

```js
// ❌ Modal 자리에 Toast — 사용자 응답 필요한 작업
toast.warning('정말 삭제하시겠습니까?')  // → BModal 사용

// ❌ Error 자동 사라짐 — 놓치면 끝
toast.error('결제 실패', { duration: 3000 })  // → duration: 0

// ❌ 영구 안내에 Toast — 페이지 결과
toast.info('이 페이지는 K-IFRS 기준입니다.')  // → BAlert

// ❌ 토스트 5개 동시 — 시선 분산
items.forEach(item => toast.success(`${item} 처리됨`))  // → 합쳐서 "5건 처리됨"

// ❌ 좌상단 위치 — 시선 흐름 역행 (좌→우, 위→아래)
toast.show({ position: 'top-left' })  // → bottom-right
```

#### 📐 ARIA · 실시간 알림
- `role="status"` — 일반 알림 (정중하게, 현재 작업 끊지 않음)
- `role="alert"` — Error · Warning (스크린리더 즉시 발화, 사용자 흐름 중단)
- `aria-live="polite"` (status) / `aria-live="assertive"` (alert)

---

## 코드 측 매핑 — Vue 컴포넌트 일람

> 본 가이드의 Figma 38 컴포넌트 ↔ Vue 코드(`B*.vue`) 1:1 매핑 표는 **각 사용처(consumer) 리포의 09 디자인시스템 디렉토리**에 동기화되어 있습니다.
>
> - 예: NOAATS NSTAR — `docs/public/09_design_system/_figma-component-guide.md` (부록)
>
> bos-design-system 자체는 Vue/React 구현체에 비의존적인 **단일 명세(specification)** 만 제공합니다.
> 사용처 리포에서 본 문서를 참조할 때는 코드 경로를 사용처 기준으로 보정하세요.

---

## 부록 A — 가이드 사용 방법

1. **Vue 컴포넌트 신규 작성 시**: 위 인덱스 → 해당 컴포넌트 섹션 → `Property × Values` 표 보고 prop enum 정의
2. **기존 코드에 variant 추가 시**: 코드 props에 Figma value 그대로 명명(예: `with-secondary-text` → `with-secondary`)
3. **디자이너와 협업 시**: Figma 라이브러리 패널의 컴포넌트 인스턴스 → 우측 패널 Properties 탭 → 본 문서와 비교 검증

## 부록 B — Figma 측 변경 시 동기화 절차

```
1. Figma 라이브러리 수정 (디자이너)
   ↓
2. mcp__5b70b77d-...__use_figma 로 variantGroupProperties + componentPropertyDefinitions 재추출
   ↓
3. 본 문서 갱신 (또는 자동 재생성 스크립트 추가)
   ↓
4. 영향받은 Vue 컴포넌트 types.ts · vue 파일 prop enum 동기화
   ↓
5. 03-component-usage.md 사용 규칙도 함께 갱신
```

---

## 부록 C — 전사 UX 권장/지양 요약

각 컴포넌트별 자세한 가이드는 본문 참조. 본 표는 빠른 자가 점검용.

### ✅ 항상 권장 (Do)

| 영역 | 가이드 | 출처 |
|---|---|---|
| **접근성** | 모든 `<input>` 에 `<label htmlFor>` 명시적 연결 | accessibility.md §1 |
| | Icon-only 버튼은 `aria-label` 필수 | §3 Buttons |
| | `:focus-visible` 보존 — `outline: none` 금지 | §1 Foundations |
| | Modal `role="dialog"` + `aria-modal="true"` + ESC + focus trap | §24 Modal |
| | Tabs 키보드 — ← → (수평) ↑ ↓ (수직) Home End | §23 Nav Tabs |
| | Color 만으로 정보 전달 금지 — 아이콘·텍스트 병행 | accessibility.md |
| **시멘틱 HTML** | `<button>` · `<a>` · `<table>` · `<nav>` 정확히 — `<div onClick>` 금지 | §34 Table |
| | Card 시멘틱 — `<article>` 독립 · `<section>` 구획 | §6 Card |
| | Pagination — `<nav aria-label="페이지네이션">` | §26 |
| **상태 처리** | 인터랙티브 요소 4상태 — Initial · Hover · Focus · Disabled | §3 Buttons |
| | 데이터 화면 3상태 — Empty · Loading(Skeleton 300ms 지연) · Error | §34 Table |
| | 로딩 ≤ 1초 → Spinner · 1초+ → Skeleton | §32 Skeleton |
| **메시지** | 에러 황금 공식 — **무엇이 + 왜 + 어떻게** | §11 Input field |
| | 액션 버튼 라벨 — 동사 2-4자 ("저장", "삭제") | §3 Button |
| | 위험 액션 — 실제 동작 명시 ("삭제" ✓ / "확인" ✗) | §3 Button |
| **위계** | 한 화면 brand 1차 CTA — **1개만** | §3 Button |
| | 메인 액션 우하단 · 보조 좌측 (시선 흐름 우하단) | §24 Modal footer |
| **반응형** | 모바일 터치 영역 — 최소 44×44px | §16 Range slider |
| | Table 모바일 — 첫 컬럼 sticky 또는 Card list 자동 전환 | §34 Table |
| **상태 유지** | URL 동기화 — page · perPage · sort 등 query string | §26 Pagination |
| | 폼 데이터 보존 — Tabs · Modal 전환 시 손실 금지 | §23 Tabs |

### ❌ 항상 지양 (Don't)

| 영역 | 패턴 | 대체안 |
|---|---|---|
| **접근성** | `outline: none` (키보드 사용자 차단) | `:focus-visible` 보존 |
| | placeholder 만으로 라벨 대체 | `<BFormField label>` |
| | 아이콘 단독 + `aria-label` 누락 | aria-label 추가 |
| | `<div @click>` 인터랙티브 | `<BButton>` 또는 `<a>` |
| **컴포넌트 오용** | Toast 자리에 Modal (단순 알림) | BToast |
| | Modal 자리에 Toast (응답 필요) | BModal |
| | Alert 에 자동 사라짐 (영구 안내가 의도) | BToast |
| | Banner 를 페이지 인라인 (사이트 공지가 의도) | BAlert |
| | Dropdown 자리에 Select (폼 값 선택) | BSelect |
| | Select 자리에 Dropdown (액션 메뉴) | BDropdown |
| | Tooltip 안에 액션 버튼 (호버 떠나면 사라짐) | BPopover |
| **시각** | `transform: scale` on hover (인접 요소 가림) | border + shadow 변화 |
| | 빨강 Confirm (긍정 액션에 Danger) | Brand 1차 |
| | brand 1차 CTA 2개+ | 1개만, 보조는 secondary |
| | 한 화면 Alert 5개+ | 우선순위 따라 1-2개 |
| | 같은 hue 활성+호버 (다크모드 묻힘) | 흰 텍스트 또는 좌측 indicator |
| **메시지** | 사용자 책망형 ("잘못 입력했습니다") | "형식이 올바르지 않습니다" |
| | typing 중 빨강 표시 (입력 시작하자마자) | blur 시점에만 |
| | 너무 긴 Tooltip 텍스트 | Popover (paragraphs) |
| | 모호한 라벨 ("확인", "OK") | 실제 동작 명시 |
| **레이아웃** | `<div>` 그리드로 Table 흉내 (정렬·키보드 깨짐) | `<table>` semantic |
| | Modal 위 Modal (이중) | Drawer 또는 Wizard step |
| | 무한 스크롤만 (위치 잃음) | Pagination + "더보기" |
| **모바일** | 가로 스크롤만 (사용자 잃음) | 첫 컬럼 sticky 또는 Card list |
| | hover 전용 표시 (touch 없음) | tap 또는 항상 표시 |

---

## 부록 D — 시나리오별 컴포넌트 조합 레시피

### 시나리오 1: 거래 상세 페이지

```
Breadcrumbs (홈 > 거래 > [현재]) ★ aria-current="page"
   ↓
EntityHeader — 제목 + 상태 Badge + 액션 Button
   ↓
Tabs (변형 default) — 개요 · 분개 · 평가 · 이력
   ↓
탭 콘텐츠:
  - 개요   → BDetailKV (key-value 그리드)
  - 분개   → BTable (회계 패턴)
  - 평가   → BStat + BProgressBar
  - 이력   → BTimeline (12 type)
   ↓
Bottom action — BButton (secondary "취소" + brand "저장")
```

### 시나리오 2: 신규 등록 폼 모달

```
BModal type="create-product" size="base" persistent
   ↓
BModalHeader — 제목 + X 닫기
   ↓
BFormField × N — 라벨 + Input/Select + Helper/Error
   ↓
BModalFooter type="buttons"
   - 좌측: BButton color="secondary" "취소"
   - 우측: BButton color="brand" "등록" :loading="saving"
   ↓
저장 성공 → BToast.success("거래가 등록되었습니다") → 모달 닫기 + 목록 새로고침
저장 실패 → BAlert color="danger" 인라인 (모달 안 첫 줄)
```

### 시나리오 3: 데이터 목록 페이지

```
페이지 헤더
   ↓
필터 바: BInput(search) + BSelect(상태) + BButton color="brand" "+ 신규"
   ↓
Loading 상태 (300ms 지연 후 표시)
   BSkeleton variant="list" :count="10"
   ↓
Empty 상태 (rows.length === 0 && !loading)
   BEmptyState
     · BIconShape size="xl" + Package 아이콘
     · 제목 "거래가 없습니다"
     · CTA: BButton "+ 첫 거래 등록"
   ↓
Error 상태
   BAlert color="danger" + 재시도 버튼
   ↓
정상 상태
   BTable + 행별 BDropdown (⋮ 더보기 메뉴)
   ↓
Footer
   좌측: BSelect per-page + "전체 N건"
   우측: BPagination
```

### 시나리오 4: ★ T5 Step-up MFA + Maker-Checker 결재

```
1. 위험 액션 클릭 (예: 거래 확정)
   ↓
2. BModal type="popup" size="sm" — 확인 다이얼로그
   "이 거래를 확정하시겠습니까? Step-up 인증이 필요합니다."
   ↓
3. BModal type="otp" size="sm" persistent — OTP 인증
   BInput type="tel" maxlength=6 inputmode="numeric" autofocus
   BButton "인증" :loading
   ↓
4. BStepper type="icon" — Maker-Checker 5단계 진행
   [Maker(나, ✓)] - [Reviewer(검토 중)] - [Checker(대기)] - [MFA(대기)] - [확정(대기)]
   ↓
5. 결재 진행 — BTimeline 으로 실시간 상태
   거부 시 → BStepper Checker state="error" + description "사유: ..."
   승인 시 → 다음 단계 진행
   ↓
6. 완료 → BToast.success("거래 확정 완료") + Hash Chain ID 표시
```

---

## 변경 이력

- `2026-05-27 (v2)` — UX 의도·권장/지양 코드 예시·시나리오 레시피 추가 (10 핵심 컴포넌트 + 부록 C·D)
- `2026-05-27` — 초안. Figma 113 세트 일괄 추출 + 코드 매핑 표
