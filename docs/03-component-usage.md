# 03-component-usage.md — BOS 4.0 컴포넌트 사용 가이드

> 이 문서는 BOS 4.0 디자인 시스템(`jmK75D3yVgpYh0wHAlsAwy`)의 **24개 컴포넌트와 5개 Foundations**를 다룹니다.
> 작업 시작 전 반드시 이 문서에서 컴포넌트와 variant를 결정하세요.
>
> **마지막 동기화**: 2026-05-20 (React 컴포넌트 라이브러리 v2 출시)
>
> **🌐 라이브 데모**: `components.html` — 24개 컴포넌트 + 5개 foundations의 인터랙티브 라이브 뷰, 코드 복사 가능

---

## 🗂 전체 인덱스

### Foundations (5)

| ID | 영역 | 핵심 |
|---|---|---|
| `colors` | 컬러 | 시멘틱 토큰 (Light/Dark 자동), Brand `#3851DD`, 9개 컬러 램프 |
| `type` | 타이포그래피 | Wanted Sans (한국어), Inter (영문), 12 size step |
| `spacing` | 여백 | 4px base, Tailwind 호환 (`spacing-1` ~ `spacing-96`) |
| `radii` | 모서리·그림자 | `rounded-base` 8px, `rounded-lg` 12px, shadow xs/sm/md/lg/xl |
| `icons` | 아이콘 | Lucide 라이브러리, line stroke 1.6~2.0, 16/20/24px 표준 |

### 액션 (4)

| ID | 컴포넌트 | variants | 한 줄 |
|---|---|---|---|
| `button` | Button | 500 | Color × Size × State × Outline × Icon-only |
| `badge` | Badge | 84 | Theme × Size × Type — 상태와 카운트 표시 |
| `avatar` | Avatar | — | 사용자 식별. 이미지/이니셜/icon fallback |
| `iconshape` | Icon shape | 84 | Size × Color × Type (Circle/Square) — 강조 아이콘 컨테이너 |

### 폼 (7)

| ID | 컴포넌트 | variants | 한 줄 |
|---|---|---|---|
| `input` | Input field | 140 | Type 5 × Size 4 × State 7 |
| `select` | Select | 84 | Type × Size × State — Input과 같은 시각 언어, 단일 선택 |
| `checkbox` | Checkbox/Radio | 12+ | 다중/단일 선택 + 그룹 |
| `toggle` | Toggle switch | 12 | 즉시 반영되는 on/off 스위치 |
| `slider` | Range slider | 7 | 연속 숫자 값 (단일/범위) — 가격 필터, 평점, 볼륨 |
| `datepicker` | Datepicker | — | 날짜 / 범위 선택 |
| `timepicker` | Timepicker | 84 | 시간 입력 (3-tier 매트릭스) |

### 피드백 (6)

| ID | 컴포넌트 | variants | 한 줄 |
|---|---|---|---|
| `alert` | Alert | 20 | 페이지 내 영구/세미-영구 안내 |
| `toast` | Toast | 9 | 일시적 결과 피드백 (자동 사라짐) |
| `modal` | Modal | 10 | 차단형 다이얼로그 (확인/폼 입력) |
| `drawer` | Drawer | 8 | 화면 가장자리 슬라이드 패널 |
| `tooltip` | Tooltip | 4 | 짧은 도움말 텍스트 |
| `empty` | Empty state | — | 데이터 없음 / 검색 결과 없음 / 에러 |

### 네비게이션 · 데이터 (8)

| ID | 컴포넌트 | variants | 한 줄 |
|---|---|---|---|
| `tabs` | Nav Tabs | 20 | 한 영역 안에서 콘텐츠 그룹 전환 |
| `stepper` | Stepper | 35+ | 단계 진행 안내 (회원가입, 결제) |
| `breadcrumbs` | Breadcrumbs | 6 | 경로 표시 |
| `pagination` | Pagination | 9 | 목록 페이지 분할 탐색 |
| `dropdown` | Dropdown | 75 | 액션 메뉴 (사용자 메뉴, 옵션, 필터) |
| `card` | Card | 16 | 정보 단위 묶음 |
| `table` | Table | 3 | 구조화된 데이터 표 |
| `misc` | 기타 | — | KBD, Rating, Progress bar, Spinner, Skeleton, Carousel 등 |

---

## 📋 핵심 컴포넌트 깊이 가이드

가장 자주 쓰이는 10개를 깊이 다룹니다. 나머지는 라이브러리에서 직접 확인하세요.

---

### 1. Button (500 variants)

**Variant 매트릭스**
```
Color × Size × Outline × Icon-only × State
  8   ×  5   ×   2     ×     2     ×   4
```

| 속성 | 값 |
|---|---|
| **Color** | `brand` (1순위 행동) · `secondary` · `tertiary` · `ghost` · `danger` (삭제·영구) · `success` · `warning` · `dark` |
| **Size** | `xs` (24px) · `sm` (32px) · `base` (40px) · `l` (48px) · `xl` (56px) |
| **Outline** | `false` (filled) / `true` (선만) — 같은 의미의 약한 버전 |
| **Icon only** | `false` / `true` — 단독 아이콘 (aria-label 필수) |
| **State** | `Initial` · `Hover` · `Focus` · `Disabled` (BOS 4.0은 Active를 Hover로 통합) |

**선택 가이드**
- 페이지 메인 액션 (저장, 만들기) → `color="brand", size="base"`
- 보조 액션 (취소) → `color="secondary"`
- 위험 액션 (삭제) → `color="danger"`. 무게 줄이고 싶으면 `outline=true`
- 모바일 풀폭 CTA → `size="xl"`
- 데이터 테이블 행 액션 → `size="sm"` 또는 `"xs"`

**규칙**
- 한 화면에 `color="brand"` 1차 CTA는 **1개만**. 두 번째 액션부터는 `secondary` 또는 `outline`
- 빨강 Confirm 금지 — 긍정 액션에 Danger 색을 쓰지 마세요 (취소·삭제와 혼동)
- `:focus-visible`로 키보드 포커스 보존, `outline: none` 금지

---

### 2. Input field (140 variants)

**Variant 매트릭스**
```
Type 5 × Size 4 × State 7
```

| 속성 | 값 |
|---|---|
| **Type** | `Default` · `Add-on icon` · `Add-on text` (URL `https://` 같은 prefix) · `Inner button` · `Stacked placeholder` |
| **Size** | `sm` (32) · `base` (40) · `lg` (48) · `xl` (56) |
| **State** | `Initial` · `Focus/Typing` · `Filled` · `Disabled` · `Read-only` · `Success` · `Danger` |

**State와 ARIA 매핑**

| BOS State | ARIA |
|---|---|
| `Disabled` | `disabled` |
| `Read-only` | `readonly` |
| `Success` | `aria-invalid="false"` |
| `Danger` | `aria-invalid="true"` + `aria-describedby` |

**필수 동반 컴포넌트**
- `Label` — 모든 필드에 명시적 연결 (`htmlFor` + `id`)
- `Helper` — 입력 형식 안내 또는 에러 메시지

**에러 메시지 황금 공식**: `무엇이 + 왜 + 어떻게`
- ❌ "입력 오류"
- ✓ "이메일 형식이 올바르지 않습니다. @ 기호를 포함해 주세요."

---

### 3. Badge (84 variants)

**Variant 매트릭스**: `Theme 6 × Size 2 × Type 5`

| Theme | 의미 |
|---|---|
| `gray` | 중립, 카테고리 (기본값) |
| `brand` | 강조, 유료 기능 (PRO, NEW) |
| `success` | 활성, 정상 |
| `danger` | 오류, 차단 |
| `warning` | 주의, 대기 |
| `white` | 다크 배경 위 |

**Type**: `Default` · `With avatar` · `With dot` (상태 표시 표준) · `With loader` · `With secondary text`

**규칙**
- Badge에 onClick 달지 말 것 — 클릭이 필요하면 `Button size="xs"`
- 라벨은 1-2 단어. 길어지면 Tag 또는 Tooltip
- 알림 카운트 100+ → "99+"로 표시

---

### 4. Card (16 variants)

| Type | 용도 |
|---|---|
| `Default` | 일반 콘텐츠 박스 |
| `With Image` | 블로그/뉴스 카드 |
| `User profile` | 사용자 프로필 |
| `E-commerce` | 상품 카드 (가격, 평점) |
| `Pricing` | 가격 비교 |
| `Grid card` | 대시보드 위젯 |
| `Login form`, `Split`, `Testimonial`, `Crypto` 등 | 특수 패턴 |

**핵심 규칙**
- 시멘틱 HTML — `<article>` (독립 콘텐츠) 또는 `<section>` (페이지 구획)
- Card 전체 클릭 가능 → `<a>` 로 감싸기. 단 내부 액션 버튼이 있으면 `<a>` 금지 (HTML invalid). 대신 본문에만 onClick 핸들러
- Hover 시 `border-color` + `shadow` 변화로 충분. `transform: scale` 금지 (그리드 안 다른 카드 가림)

---

### 5. Alert (20 variants)

`Type 4 × Color 5` = 20

| Type | 용도 |
|---|---|
| `Default` | 한 줄 안내 |
| `Complex` | 제목 + 본문 + 아이콘 + CTA |
| `Small` | 컴팩트 |
| `Border top` | 상단 강조 라인 (페이지 결과) |

| Color | role | 의미 |
|---|---|---|
| `Success` | `status` | 긍정, 완료 |
| `Info` | `status` | 안내 |
| `Warning` | `alert` | 주의, 곧 영향 |
| `Default` | — | 중립 |
| `Danger` | `alert` | 오류, 즉시 조치 |

**Alert vs Toast vs Modal vs Banner**

| 컴포넌트 | 위치 | 지속 시간 | 용도 |
|---|---|---|---|
| `Alert` | 페이지 내 인라인 | 영구 / 사용자 닫음 | 페이지 안내, 폼 결과 |
| `Toast` | 화면 우하단 | 일시 (3-5초) | 작업 완료 피드백 |
| `Modal` | 중앙 + backdrop | 사용자 응답까지 | 확인 필요한 작업 |
| `Banner` | 사이트 최상단 | 영구 | 전체 사이트 공지 |

---

### 6. Modal (10 types + Header 7 + Footer 4)

| Type | 용도 | 표준 사이즈 |
|---|---|---|
| `Info` | 단순 안내 | container-xs (380) |
| `Popup` | 확인/취소 | container-xs |
| `Sign In` | 로그인 폼 | container-sm (640) |
| `Create product` | 폼 모달 | container-sm |
| `With radio inputs` | 옵션 선택 | container-xs |
| `With timeline` | 단계별 마법사 | container-md (768) |
| `With progress bar` | 업로드/처리 | container-xs |
| `With list` | 항목 선택 | container-sm |
| `OTP` | 인증번호 | container-xs |
| `Share` | 공유 옵션 | container-sm |

**필수 ARIA**
```html
<div role="dialog" aria-modal="true"
     aria-labelledby="modal-title"
     aria-describedby="modal-desc">
```

**필수 키보드/포커스 처리**
- ESC 키로 닫기
- 모달 열릴 때 첫 focusable 요소로 포커스 이동
- 모달 안에서 Tab 키가 순환 (focus trap)
- 닫을 때 모달 열기 전 트리거 요소로 포커스 복귀

**미저장 폼 모달**: backdrop 클릭만으로 닫지 말 것 — confirm 다이얼로그 또는 backdrop 클릭 비활성화

---

### 7. Dropdown (75 variants)

4개의 독립 컴포넌트로 구성:

| 컴포넌트 | variants | 역할 |
|---|---|---|
| `Dropdown menu` | 16 | 펼쳐진 메뉴 컨테이너 |
| `Dropdown list item` | 48 (16 Type × 3 State) | 메뉴 안 개별 항목 |
| `Dropdown header` | 5 | 메뉴 헤더 영역 |
| `Dropdown (ready to use)` | 6 | 트리거 + 메뉴 조합 |

**Dropdown 16 Type 인덱스**
- 액션 메뉴: `Default` · `with separator`
- 사용자 메뉴: `User profile` · `Users` · `User selection`
- 선택: `With radio input` (단일) · `Checkbox` (다중) · `With toggle`
- 검색·긴 메뉴: `With scroll` · `Heading & Button` · `With forms`
- 특수: `Text & illustration` · `Grid` · `Language select` · `Menu` · `With number inputs`

**Dropdown vs Select 선택**
- 액션 메뉴 (각 항목이 다른 동작) → Dropdown, `role="menu"`
- 폼 값 선택 → 네이티브 `<select>` 또는 Autocomplete (모바일/접근성 우수)

**필수 키보드**: `↓` `↑` `Home` `End` `Enter` `Space` `Esc` + 알파벳 typeahead

---

### 8. Nav Tabs (20 variants)

`Tabs 12 + Item 8`

| Type | 시각 무게 | 용도 |
|---|---|---|
| `Default` | 보통 | 페이지 내 콘텐츠 그룹 (개요/멤버/설정) |
| `Pills` | 보통 (배경) | 시간 단위, 카테고리 전환 |
| `Button` | 가장 강함 | 대시보드 상단 영역 전환 |
| `Border bottom` | 보통 | Default와 비슷, 하단 강조 |
| `Vertical with list item` | 보통 | 설정 페이지 사이드 메뉴 |
| `Vertical pills` | 보통 | 좁은 영역 수직 탭 |

**WAI-ARIA Tabs 패턴**
- 컨테이너: `role="tablist"`, 수직이면 `aria-orientation="vertical"`
- 각 탭: `role="tab"` + `aria-selected` + `aria-controls`
- 각 패널: `role="tabpanel"` + `aria-labelledby` + 비활성은 `hidden`
- **Roving tabindex**: 활성 탭만 `tabindex="0"`, 나머지 `-1`
- 키보드: `←` `→` (수평), `↑` `↓` (수직), `Home`, `End`

---

### 9. Pagination (9 variants)

| Type | 용도 |
|---|---|
| `Two buttons` | 이전/다음만 (단순) |
| `Default` | 페이지 번호 (가장 보편) |
| `Buttons & helper text` | 카운트 + 페이지 번호 |
| `With dropdown` | per-page 선택 + 페이지 번호 (**관리 페이지 표준**) |
| `Text buttons` | 텍스트 페이지 버튼 |
| `With input` | 페이지 번호 직접 입력 (큰 데이터셋) |
| `Simple with input` | 입력만 |
| `Arrows` | 화살표만 |
| `Dropdown & buttons` | 조합 |

**List Page Footer 표준 조합**
```
좌측: per-page 드롭다운 + "전체 N건"
우측: 페이지 번호 + 이전/다음
```

**필수 ARIA**
- 컨테이너: `<nav aria-label="페이지네이션">`
- 현재 페이지: `aria-current="page"`
- `…` 생략: `aria-hidden="true"`

**URL 동기화 필수** — `?page=3` 같은 query string으로. 새로고침/공유 시 페이지 유지

---

### 10. Table (3 variants + 자체 인터랙션 패턴)

| Type | 용도 |
|---|---|
| `Striped rows` | 행 줄무늬 (가장 자주 — 가독성 우수) |
| `Striped columns` | 열 비교가 중요할 때 |
| `With table caption` | 캡션 포함 (접근성 ↑) |

**시멘틱 HTML 필수**
- `<table>` 사용 (`<div>` 그리드 금지)
- `<caption>` — 시각적으로 숨겨도 됨 (`sr-only`)
- `<th scope="col">` — 헤더 셀
- 정렬 가능 헤더: `aria-sort="ascending" | "descending" | "none"`
- 로딩: `aria-busy="true"`

**행 액션 패턴 — 권장 순위**
1. **"더보기" 메뉴** (⋮ → Dropdown) — 액션 많거나 위험 액션 포함 시 최선
2. 아이콘 버튼 나열 — 액션 2~3개일 때
3. 행 전체 클릭 + 별도 아이콘 액션 — 각 행이 상세 페이지를 가질 때 (이벤트 버블링 차단 필수)

**3가지 데이터 상태 필수**
- `Empty` — Icon shape + 제목 + 설명 + CTA
- `Loading` — Skeleton (300ms 지연 시작으로 깜빡임 방지)
- `Error` — 아이콘 + 메시지 + [다시 시도] + 에러 코드

**모바일 대응**: 좁은 화면에서 가로 스크롤만 두면 사용자 잃음. 첫 컬럼 sticky 또는 Card list로 자동 전환

---

## 📋 신규 컴포넌트 (v2에서 추가) 빠른 참조

기존 vanilla 버전에는 없던 컴포넌트입니다. 라이브러리에서 실제 예시 확인 필수.

### Avatar
- 사용자 식별. 이미지 / 이니셜 fallback / icon fallback 3단계
- 크기 표준: 24 / 28 / 36 / 48 / 64 / 96px
- 그룹 표시 시 최대 3-4개 + "+N" Badge

### Icon shape (84 variants)
- `Size 6 × Color 7 × Type 2`. 아이콘을 시각적으로 강조하는 컨테이너
- Empty state, Error state, 강조 메뉴, Onboarding card 등에 사용
- Color: White · Dark · Brand · Red · Gray · Green · Yellow

### Select (84 variants)
- 단일 선택용 드롭다운. **Input field와 같은 시각 언어** (Type × Size × State 매트릭스 공유)
- Type: `Default` · `Add-on icon` (좌측 아이콘) · `Add-on text` (prefix)
- Size: `sm` · `base` · `lg` · `xl` — 같은 폼 안에서는 통일
- State: `Initial` · `Focus` · `Filled` · `Disabled` · `Read-only` · `Success` · `Danger` · `Open`
- **옵션 수에 따른 선택**:
  - 5개 미만 → Radio (모두 한눈에)
  - 5-20개 → Select (이 컴포넌트)
  - 20개 초과 → Search-driven combobox
- Label과 Helper는 `Field` 컴포넌트로 함께 묶기. 인라인 사용 시 Field 생략

### Checkbox / Radio (12 + 그룹 10)
- Indeterminate state 지원 (전체 선택 상태가 부분 선택일 때)
- 그룹: `Default` · `Cards` · `Card + Icon` · `Big cards grid` · `Small cards grid` · `Only icons` · `Advanced` · `With numbers` · `With avatar` · `With description`

### Toggle (12)
- **즉시 반영되는** on/off 액션에만. 저장 버튼이 필요한 설정은 Checkbox
- 라벨은 토글 좌측 — 상태와 의미를 함께 읽음 ("알림 켜기" + on/off)

### Range slider (7 variants) — 🆕
- 연속적인 숫자 값 입력 — 가격 필터, 평점, 볼륨 등
- Type: `Default` (단일 값) · `Range` (두 개 thumb으로 범위 지정)
- Thumb state: `Initial` · `Active` (드래그 중 — brand-800 보더 + 8px brand-soft ring)
- 구성: label · 현재 값 · min/max 헬퍼가 한 묶음으로 동작
- prefix (`₩`), unit (`%`) 지원으로 단위 표시 가능
- **Slider vs Number Input 결정**:
  - 정확한 값 입력이 중요 → Number Input
  - 대략적인 범위/직관적 조정이 중요 → Slider
  - 둘 다 필요 → 함께 표시 (Slider + Number Input 동기화)
- 모바일에서 thumb은 최소 44×44 터치 영역 확보. 키보드: `←` `→` 1단위, `Shift + ←/→` 10단위, `Home`/`End` 양끝

### Datepicker · Timepicker
- 입력 위주는 네이티브 input (모바일 OS 키보드 우수)
- 달력/시계 UI가 더 자연스러운 경우만 커스텀 dropdown 사용
- Datepicker dropdown 5 Type: `Default` · `Range` · `Year picker` · `Month` · `Day/Year/Month`
- 한국어 날짜 포맷: `2026.04.20` 또는 `2026년 4월 20일`

### Toast (9 variants)
- 위치: 화면 우하단 (모바일은 상단도 허용)
- 자동 사라짐 3-5초. 위험·중요 정보는 자동 사라짐 금지
- Type: `Avatar & button` · `Icon shape & text` · `Icon shape & buttons` · `With header` · `Icon & text` · `Warning` · `With illustration` · `Error` · `With progress bar`
- 동시 표시는 최대 3개. 그 이상은 큐잉

### Drawer (8 + heading 8 + footer 4)
- 화면 가장자리 슬라이드 패널. 좌/우/상/하
- 모달보다 큰 콘텐츠 / 다단계 수정 / 상세 정보 페이지 안 미리보기에 적합
- Type: `Default` · `Type3` · `With forms` · `Advanced` · `With alert` · `With list` · `Bottom` · `Swipeable`
- 모달과 동일한 focus trap, ESC 닫기, aria-modal 적용

### Tooltip (4 — Position 4종)
- 짧은 도움말. **텍스트만**. 액션 버튼 넣지 말 것
- Position: `Top` · `Bottom` · `Left` · `Right`
- 호버 시 200-500ms 지연 후 표시, 마우스 떠나면 즉시 제거
- 키보드: focus 시 표시. 모바일은 tap-and-hold

### Empty state
- BOS 4.0 `Icon shape` 컴포넌트 활용 — Size 64px
- 구성: 아이콘 + 제목 + 설명 + CTA
- 상황별 톤:
  - 처음 사용 (onboarding) → 환영 톤
  - 검색 결과 없음 → 차분, 대안 제시
  - 필터 결과 없음 → 차분, 필터 초기화 제안
  - 모두 처리됨 → 긍정 ("확인할 알림이 없습니다 ✓")

### Stepper (35+ variants)
- 단계 진행. `Stepper navigation 12 + Stepper nav link 35`
- Type: `Default` · `Only icons` · `Vertical` · `Icons & text` · `With dots` · `Cards`
- State: `Initial` · `Active` · `Completed` · `Error` · `Disabled`
- 회원가입, 결제, 마법사 등에 사용. 3-5단계가 적정 (10+는 분할)

### Breadcrumbs (6 + item 3)
- 페이지 위계 경로. Detail Page 최상단 표준
- Type: `Default` · `Background` · `With dropdown` · `With group buttons` · `Only buttons` · `With badge`
- 마지막 항목은 현재 페이지 — 링크 없이 (`aria-current="page"`)

### Misc (잡다)
- KBD (49) — 키보드 단축키 표시 (`Ctrl + K`)
- Rating (3) — 별점
- Progress bar (18) · Bars (8) — 진행률
- Spinner (10) · Interactive spinner (4) — 로딩
- Skeleton (6) — 로딩 placeholder
- Carousel · Gallery · Timeline · Chat bubble — 특수 용도

---

## 🎨 §11. 아이콘 — Lucide

Figma 라이브러리에는 자체 아이콘 2,658개(`icons/Base/...`)가 있지만, **코드에서는 Lucide 라이브러리**를 사용합니다.

이유:
- Figma 아이콘은 Plugin asset이라 코드에서 1:1 매핑이 어려움
- Lucide는 1500+ 아이콘 + tree-shaking 가능 + 시멘틱 stroke-width 일관
- BOS 4.0의 line 스타일과 시각적으로 일치

### 마크업 표준

```html
<!-- 인라인 아이콘 -->
<i data-lucide="search" class="icon-sm"></i>

<!-- icon-only 버튼 -->
<button aria-label="삭제">
  <i data-lucide="trash-2" class="icon-sm" aria-hidden="true"></i>
</button>
```

### 크기 표준

| 클래스 | 크기 | 사용처 |
|---|---|---|
| `.icon-xs` | 12px | Badge 안, 매우 좁은 곳 |
| `.icon-sm` | 16px | **기본** (버튼, 인라인 텍스트) |
| `.icon-base` | 20px | Input add-on, Sidebar nav |
| `.icon-lg` | 24px | Icon-only button, 카드 헤더 |
| `.icon-xl` | 32px | Icon Shape 안, 일러스트 영역 |

### Stroke width

Lucide 기본 `stroke-width="2"` 그대로. 강조 시 `1.5`(얇게) ~ `2.5`(두껍게) 미세 조정.

### 로딩 — 환경별 자동 선택

`icon-loader-boilerplate.md` 참조. 외부망/폐쇄망/하이브리드 3가지 보일러플레이트.

---

## ⚠️ 자주 발생하는 실수

1. **임의의 variant 조합** — Figma에 정의된 조합인지 라이브러리에서 먼저 확인
2. **State 누락** — 4가지 상태 (Initial/Hover/Focus/Disabled) 모두 정의. `:focus-visible` 사용
3. **Empty/Loading/Error 누락** — 데이터를 보여주는 모든 화면에 3가지 상태 필수
4. **다크모드 색조 동조** — Active/Selected 상태는 흰 텍스트 + 진한 배경, 또는 좌측 indicator (`05-interaction-patterns.md §1.3`)
5. **원시 토큰 직접 사용** — `var(--colors-brand-700)` ❌ → `var(--colors-background-bg-brand)` ✓
6. **`Input field`와 `Select form`/`Search form` 혼동** — Input field는 단일 필드, *form 컴포넌트는 라벨+필드+헬퍼+버튼 조합
7. **Modal에 ESC 닫기 누락** — 키보드 사용자 차단
8. **Dropdown에 외부 클릭 닫기 누락** — UX 표준 위반
9. **Table을 div 그리드로** — 스크린 리더, 정렬, 키보드 모두 깨짐

---

## 변경 이력

- `2026-05-20 (v2)` — **React 컴포넌트 라이브러리 v2 출시.** 24개 컴포넌트 + 5개 Foundations로 확장. 신규: Avatar, Icon shape, Select, Checkbox/Radio, Toggle, **Range slider**, Datepicker, Timepicker, Toast, Drawer, Tooltip, Empty state, Stepper, Breadcrumbs, Misc. Select는 1 → 8 sections로 가이드 보강 (Input과 동일한 84 variants 매트릭스). 라이브러리 진입점: `components.html`
- `2026-05-20` — Figma MCP로 전체 라이브러리 전수 추출. 113 컴포넌트 셋의 variant 매트릭스 정리
- `2026-04-20` — 초기 47 카테고리 인덱스 + 핵심 10개 가이드
