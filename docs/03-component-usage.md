# component-usage.md — BOS 4.0 컴포넌트 사용 가이드라인

> 이 문서는 BOS 4.0 라이브러리(`jmK75D3yVgpYh0wHAlsAwy`)의 47개 컴포넌트 카테고리를 다룹니다.
> Vibe coding 시 새 컴포넌트를 만들기 전에 **반드시** 이 문서에서 사용할 컴포넌트와 variant를 결정하세요.

---

## 🗂 전체 컴포넌트 인덱스

BOS 4.0은 광범위한 시스템입니다. 외울 수 없으니 **카테고리 인덱스**로 관리합니다.
필요한 컴포넌트가 어디 있는지 빠르게 찾는 용도입니다.

### Action / Input
- **Buttons** — Color × Size × State 매트릭스 (500 variants)
- **Button group** — 여러 버튼을 묶어서 표시
- **Forms** — Input field, Select, Search, Number, Phone, Copy to clipboard, File upload, Tag input, OTP, Textarea, Rich text editor, Label, Helper
- **Floating label inputs** — 떠있는 라벨 스타일
- **Checkbox & radio** — 체크박스, 라디오, 그룹
- **Toggle** — 스위치, 그룹, 라벨, 헬퍼
- **Range slider** — 범위 슬라이더
- **Datepicker** — 날짜 선택 (cell, controls, dropdown menu)
- **Timepicker** — 시간 선택
- **Autocomplete input** — 자동완성

### Display / Content
- **Cards** — Default, With Image, Split, Login form, E-commerce, Pricing, Testimonial, etc. (15 types)
- **Badges** — Theme(Gray/Brand/Danger/...) × Size × Type (84 variants)
- **List** — List item, List
- **Tables** — 데이터 테이블
- **Accordion** — 접고 펼치기
- **Carousel** — 슬라이드
- **Gallery (Masonry)** — 이미지 갤러리
- **Stepper** — 단계 표시
- **Timelines** — 타임라인
- **Rating** — 별점
- **KBD** — 키보드 단축키 표시
- **Icon shape** — 색 배경 + 아이콘 박스 (84 variants)

### Feedback
- **Alerts** — Complex/Default/Small/Border top × Color (20 variants)
- **Banner** — 페이지 상단 배너
- **Toasts** — 9가지 변형 (Icon & text, Avatar & button, Warning, Error, ...)
- **Tooltips** — 툴팁
- **Popovers** — 팝오버
- **Modals** — 10가지 type (Info, Popup, Sign In, OTP, Share, ...)
- **Drawer** — 사이드 슬라이드 패널
- **Progress Bars** — 진행률
- **Spinners** — 로딩 스피너
- **Skeleton** — 로딩 스켈레톤
- **Speed Dial** — 플로팅 액션 버튼

### Navigation
- **Navbar** — Nav link, Navbar, Navbar content, Megamenu, Mobile menu
- **Bottom navigation** — 모바일 하단 네비
- **Breadcrumbs** — 경로 표시
- **Sidebar** — 사이드바
- **Nav Tabs** — 탭
- **Pagination** — 페이지네이션
- **Dropdowns** — 드롭다운 메뉴 (16 types)

### Patterns / Marketing
- **Jumbotron** — 큰 히어로 영역 (21 types)
- **Chat bubble** — 채팅 말풍선
- **Device mockups** — 디바이스 목업 (8 types)
- **Download App Button** — 앱 다운로드 버튼
- **Patterns** — 배경 패턴
- **Illustrations** — 일러스트 (Flowbite illustrations v1.0, 108개)

### Foundation
- **Spacing** — 4px base 스케일
- **Icons** — 2698개 아이콘 (코드에서는 **Lucide** 라이브러리 사용, 아래 §19 참조)
- **Shadows & Borders** — 그림자, 테두리
- **Patterns** — 배경 패턴 (20개)

---

## 📋 핵심 18개 컴포넌트 깊이 가이드

가장 자주 쓰이는 컴포넌트를 깊이 다룹니다. 나머지는 필요할 때 `figma.use_figma`로 검색하세요.

---

### 1. Button (500 variants — 가장 자주 쓰임)

**Variant 매트릭스**
```
Color × Size × State × Outline × Icon-only
  8   ×  5   ×   4   ×    2    ×     2
```

| 속성 | 값 |
|---|---|
| **Color** | `Brand` (주요), `Secondary` (보조), `Tertiary` (3차), `Success`, `Danger`, `Warning`, `Dark`, `Ghost` (배경 없음) |
| **Size** | `xs` (24px), `sm` (32px), `base` (40px), `l` (48px), `xl` (56px) |
| **State** | `Initial`, `Hover`, `Focus`, `Disabled` |
| **Outline** | `false` (filled) / `true` (선만) |
| **Icon only** | `false` / `true` (정사각형) |

**선택 가이드**

| 상황 | Color | Size |
|---|---|---|
| 페이지의 메인 액션 | `Brand` | `base` 또는 `l` |
| 폼 안의 저장 버튼 | `Brand` | `base` |
| 모달의 취소 버튼 | `Secondary` | `base` |
| 삭제, 영구 액션 | `Danger` | `base` |
| 강조 없는 보조 액션 | `Ghost` | `sm` |
| 데이터 테이블 행 액션 | `Ghost` icon-only | `sm` |
| 모바일 풀폭 CTA | `Brand` | `xl` |

**언제 어떤 Outline?**
- Filled (default) — 한 화면의 1순위 행동
- Outline — 같은 행동의 보조 버전 또는 2순위 행동

```tsx
// ✅ Good
<Button color="Brand" size="base">저장</Button>
<Button color="Secondary" size="base">취소</Button>

// ❌ Bad — 같은 화면에 Brand 버튼이 2개 (위계 흐림)
<Button color="Brand">저장</Button>
<Button color="Brand">미리보기</Button>
```

---

### 2. Input field (140 variants)

**Variant 매트릭스**
| 속성 | 값 |
|---|---|
| **Type** | `Default`, `Add-on icon`, `Add-on text`, `Inner button`, `Stacked placeholder` |
| **Size** | `sm`, `base`, `lg`, `xl` |
| **State** | `Initial`, `Focus/Typing`, `Filled`, `Disabled`, `Read-only`, `Success`, `Danger` |

**선택 가이드**

| 상황 | Type | 예시 |
|---|---|---|
| 일반 텍스트/이메일 | `Default` | 이름, 이메일 |
| 검색 필드 | `Add-on icon` (왼쪽 돋보기) | 검색 |
| URL/도메인 입력 | `Add-on text` (왼쪽 "https://") | 사이트 주소 |
| 비밀번호 (토글) | `Inner button` (오른쪽 눈 아이콘) | 비밀번호 |
| 검증 통과 표시 | `State: Success` | 이메일 인증 완료 |
| 검증 실패 표시 | `State: Danger` | 형식 오류 |

**필수 동반 컴포넌트**
- `Label` — 위에 표시되는 라벨 (placeholder만 사용 ✗)
- `Helper` — 아래 보조 설명 또는 에러 메시지

---

### 3. Alert (20 variants)

**Variant 매트릭스**
| 속성 | 값 |
|---|---|
| **Type** | `Default`, `Complex`, `Small`, `Border top` |
| **Color** | `Default`, `Info`, `Success`, `Warning`, `Danger` |

**선택 가이드**

| 상황 | Type | Color |
|---|---|---|
| 페이지 상단 영구 안내 | `Border top` | `Info` 또는 `Warning` |
| 일반 알림 | `Default` | 상황에 맞게 |
| 제목+본문 모두 필요 | `Complex` | 상황에 맞게 |
| 좁은 영역 | `Small` | 상황에 맞게 |

**Toast vs Alert 선택**
- 일시적 알림 (3~6초 후 사라짐) → `Toast`
- 영구 알림 (사용자가 닫을 때까지) → `Alert`

---

### 4. Toast (9 variants)

**Variant 종류** (BOS 4.0)

| 변형 | 너비 | 사용 시점 |
|---|---|---|
| `Icon & text` | 380px | 가장 가벼운 알림 (저장됨, 전송됨) |
| `Icon shape & text` | 320px | Success 강조가 필요한 알림 |
| `Avatar & button` | 380px | 사용자 관련 알림 + Reply CTA |
| `With header` | 320px | 헤더 + 본문 + 시간 + 2버튼 |
| `Icon shape & buttons` | 320px | 업데이트 알림 + 2버튼 |
| `With progress bar` | 320px | 업로드 진행률 |
| `Warning` | 640px | 큰 경고 (인보이스 업로드 등) |
| `Error` | 384px | 에러 + 재시도 |
| `With illustration` | 384px | 온보딩, 안내성 알림 |

**규칙**
- 한 번에 최대 3개까지만 스택
- 우측 상단 또는 우측 하단 고정
- Undo가 필요한 액션 → 5초 표시
- 일반 success → 3초 표시

---

### 5. Modal (10 types)

**Type별 사용처**

| Type | 사용처 |
|---|---|
| `Info` | 단순 정보 확인 |
| `Popup` | 액션 확인 (삭제, 변경) |
| `Sign In` | 로그인 폼 |
| `Create product` | 신규 등록 폼 |
| `With radio inputs` | 선택지 제공 |
| `With timeline` | 진행 단계 표시 |
| `With progress bar` | 다단계 작업 |
| `With list` | 항목 선택 |
| `OTP` | 인증 코드 입력 |
| `Share` | 공유 옵션 |

**필수 규칙**
- Esc 키로 닫기 가능
- 백드롭 클릭 시 닫기 (단, 폼 작성 중이면 확인 다이얼로그)
- Primary CTA 우측 하단, 취소는 좌측

**동반 컴포넌트 (조립용)**

Modal은 단일 컴포넌트로도 사용 가능하지만, 더 정밀하게 조립할 때
다음 두 ComponentSet을 함께 사용합니다.

- `Modal header` (7 variants) — 헤더 스타일 7종
  - `Default` · `Icon + Heading & helper text` · `Breadcrumb`
  - `Avatar group` · `With Logo` · `Logo & icons` · `Heading & supporting text`
- `Modal footer` (4 variants) — 푸터 액션 영역 4종
  - `Buttons` (가장 흔함) · `Buttons & links` · `3 buttons` · `Pagination`

**조합 예시**: Detail Page에서 항목 편집 모달 →
`Modal header: Heading & supporting text` + 본문 폼 + `Modal footer: Buttons` (취소 + 저장).

---

### 6. Card (16 variants)

> 14가지 Type × Mobile Version 옵션 = 16 variants. 일부 Type만 모바일 전용 변형 보유.

**Type별 사용처**

| Type | 사용처 |
|---|---|
| `Default` | 일반 정보 카드 |
| `With Image` | 썸네일 있는 항목 |
| `Split` | 좌우 분할 (이미지 + 콘텐츠) |
| `Centered with full image` | 풀이미지 중앙 정렬 |
| `User profile` | 사용자 프로필 |
| `Login form` | 로그인 카드 |
| `E-commerce` | 상품 카드 |
| `With full width tabs` | 카드 내부 탭(풀폭) — 대시보드 위젯에 적합 |
| `With nav tabs` | 카드 내부 탭(인라인) — 좁은 영역의 다중 뷰 |
| `Pricing card` | 요금제 카드 |
| `Testimonial card` | 후기/리뷰 카드 |
| `Cars with list` | 카드 + 리스트 결합 |
| `Crypto` | 암호화폐 정보 카드 |
| `Grid card` | 그리드 레이아웃용 |

**모바일 대응**: `Mobile Version: True` 옵션. 현재 모바일 전용 변형이 별도로 정의된 Type은
`Split` · `With nav tabs` · `Testimonial card` 3종.

> 🔄 **2026-05-08 업데이트** — 이전 문서에 있던 `CTA card`는 Figma에 존재하지 않아 제거했습니다.
> 반대로 `With full width tabs` · `With nav tabs` · `Cars with list`를 새로 추가했습니다.
> variant 수도 `18 types` → `16 variants`로 정정했습니다.

---

### 7. Badge (84 variants)

**Variant 매트릭스**
| 속성 | 값 |
|---|---|
| **Theme** | `Gray`, `White`, `Brand`, `Danger`, `Warning`, `Success` |
| **Size** | `sm`, `lg` |
| **Type** | `Default`, `With avatar`, `With dot`, `With loader`, `With secondary text` |
| **Only icon / Only text** | `false` / `true` |

**Tag/Status 의미별 선택**
- 카테고리 분류 (값 고정) → `Theme: Gray`, `Type: Default`
- 활성/비활성 상태 → `Type: With dot`, Theme은 의미에 맞게
- 사용자 멘션 → `Type: With avatar`
- 로딩 중 → `Type: With loader`
- 카운트 (숫자만) → `Only text: true`

---

### 8. Dropdown menu (16 types)

**Type별 사용처**

| Type | 사용처 |
|---|---|
| `Default` | 일반 메뉴 |
| `User profile` | 사용자 메뉴 (GNB 우측) |
| `with separator` | 그룹 분리 |
| `With radio input` | 단일 선택 |
| `With toggle` | 옵션 on/off |
| `Checkbox` | 다중 선택 |
| `Users` | 사용자 목록 |
| `With scroll` | 긴 목록 |
| `Heading & Button` | 헤더 + 메인 액션 |
| `User selection` | 멤버 선택 |
| `Text & illustration` | 빈 상태 |
| `With forms` | 검색 필드 포함 |
| `Grid` | 격자 (이모지 등) |
| `Language select` | 언어 선택 |
| `Menu` | 일반 메뉴 |
| `With number inputs` | 숫자 조절 |

**조립 단위 컴포넌트**

`Dropdown menu`는 컨테이너이고, 실제 항목은 `Dropdown list item`을 조립해서 사용합니다.
Vibe coding 시 가장 자주 조립하는 단위입니다.

- **`Dropdown list item`** (48 variants) — Type 16종 × State 3종 (`Initial` / `Hover` / `Disabled`)
  - Type 16종: `Default`, `Secondary text`, `Two icons`, `Left form`, `Right form`,
    `With avatar & form`, `Only form`, `With badge`, `User select`, `Boxed`,
    `Icon shapes`, `With flag`, `Flag & checkbox`, `Form & dot`, `Form & icons`, `Checkbox & icon`
- **`Dropdown header`** (5 variants) — `Text & helper` · `With avatar` · `Form` · `Selector` · `Big avatar`
- **`Dropdown (ready to use examples)`** (6 variants) — 트리거 + 메뉴 완성형
  - `Button` · `Ghost button` · `Icon only button` · `Avatar & name` · `Link`

---

### 9. Navbar (15 + 27 + 메가메뉴)

**구성 요소**
- `Nav link` (52 variants) — 메뉴 항목 하나
- `Navbar` (15 variants) — 컨테이너
- `Navbar content` (27 variants) — 콘텐츠 영역
- `Megamenu (full-width)` — 풀폭 메가 메뉴
- `Megamenu (fixed-width)` — 고정 너비 메가 메뉴
- `Mobile menu` — 모바일 햄버거 메뉴

**선택 가이드**
- 단순 메뉴 4~7개 → 일반 `Navbar`
- 카테고리가 많고 하위 메뉴 필요 → `Megamenu`
- 모바일 → 자동으로 `Mobile menu` 전환

---

### 10. Sidebar

`---- Sidebar` 페이지 참조. LNB 또는 마스터-디테일 패턴의 좌측 패널로 사용.

**전형적 구조**
- 상단: 로고/워크스페이스 선택
- 중간: 그룹별 메뉴 (`Nav link` 컴포넌트 활용)
- 하단: 사용자 메뉴

---

### 11. Tooltip (4 variants)

호버 시 추가 정보를 짧게 보여주는 비휘발성 안내. 클릭이 아닌 **호버 전용**이며,
모바일에서는 사용하지 말 것 (touch에선 hover가 없으므로 정보 손실).

**Variant 매트릭스**
| 속성 | 값 |
|---|---|
| **Type** | `Default` |
| **Position** | `Top`, `Bottom`, `Left`, `Right` |

**위치 선택 가이드**
- 페이지 상단의 요소 → `Bottom`
- 페이지 하단의 요소 → `Top` (기본)
- 사이드바 항목 → `Right`
- 좁은 우측 영역 → `Left`

**Tooltip vs Popover**

| 구분 | Tooltip | Popover |
|---|---|---|
| 트리거 | Hover (또는 키보드 focus) | Click |
| 콘텐츠 | 짧은 텍스트 1~2줄 | 긴 콘텐츠, 인터랙션 가능 |
| 모바일 | 사용 비추천 | 사용 가능 |

**필수 규칙**
- 텍스트만, 1줄 권장 (최대 2줄)
- 키보드 포커스 시에도 표시 (`focus-visible`)
- **아이콘 단독 버튼**에는 반드시 Tooltip 또는 `aria-label` 제공

---

### 12. Nav Tabs (12 variants)

페이지 내부에서 동등한 위계의 뷰들을 전환할 때 사용. 페이지 간 이동은 `Sidebar`/`Navbar`로.

**Variant 매트릭스**
| 속성 | 값 |
|---|---|
| **Type** | `Default`, `Pills`, `Button`, `Button group`, `Border bottom`, `Vertical with list item`, `Vertical pills` |
| **Mobile** | `False`, `True` |

**선택 가이드**
| 상황 | Type |
|---|---|
| 일반 페이지 내부 탭 (가장 흔함) | `Default` 또는 `Border bottom` |
| 시각적으로 강조 필요 | `Pills` |
| 명확한 액션 느낌 | `Button` 또는 `Button group` |
| 사이드바 안의 서브 메뉴 | `Vertical with list item` |
| 모바일 → 자동 변환 | 임의 Type + `Mobile: True` |

**조립 단위**: `Nav item` (8 variants) — Type 4종 (`Default`/`Pill`/`Border bottom`/`User select`)
× State 2종 (`Initial`/`Hover/Active`).

**Tabs vs Steps vs Accordion**
- 동등한 뷰 전환 → `Nav Tabs`
- 순차적 진행 → `Stepper`
- 접고 펼치기 → `Accordion`

---

### 13. Toggle (12 + 8 + 3 variants)

즉시 반영되는 on/off 설정. 폼 제출 후 적용되는 옵션은 `Checkbox`를 사용합니다.

| Component | Variants | 매트릭스 |
|---|---|---|
| `Toggle switches` | 12 | Type 1 × Size 2 (`base`/`lg`) × Status 3 (`Initial`/`Focus`/`Disabled`) × Checked 2 |
| `Toggle inputs` | 8 | Type 8 — `Default` · `Left & Right label` · `Left & Right icons` · `Advanced` · `Simple card` · `Logo & text` · `Advanced with icon` · `Only icons` |
| `Toggle group` | 3 | Type 3 — `Default` · `Integration` · `Settings` |
| `Label (toggle)` / `Helper (toggle)` | 2 + 1 | 동반 컴포넌트 |

**Toggle vs Checkbox**
- 토글 변경 즉시 반영 (예: 알림 켜기/끄기) → `Toggle`
- 폼 제출 후 적용 (예: 약관 동의) → `Checkbox`

---

### 14. Checkbox & Radio (12 + 54 + 10 variants)

다중 선택은 Checkbox, 상호 배타적인 단일 선택은 Radio.

| Component | Variants | 매트릭스 |
|---|---|---|
| `Checkbox input` | 12 | Type 2 (`Checkbox input`/`Radio input`) × Size × Status 3 × Checked 2 |
| `Checkbox/Radio form` | 54 | 풍부한 form 변형 — `With helper`, `Left checkbox card`, `With side icon & text`, `Icon & text`, `Box icon & logo` 등 |
| `Checkbox/Radio group` | 10 | Type 10 — `Default` · `Cards` · `Card + Icon` · `Big cards grid` · `Small cards grid` · `Only icons` · `Advanced` · `With numbers` · `With avatar` · `With description` |
| `Label (checkbox&radio)` | 4 | `Default` · `Required & icon` · `With number` · `With avatar` |
| `Helper (checkbox&radio)` | 1 | 동반 컴포넌트 |

> ⚠️ **Figma 정합 이슈** — `Checkbox input`의 Size 옵션이 `["base", "Default"]`로 등록되어 있어 사실상 base 하나만 의미가 있습니다. 디자이너 측에 정리 요청 권고.

---

### 15. Floating label inputs (84 variants)

라벨이 필드 안에서 위로 떠오르는 입력. 공간 절약형 폼에 적합.

**Variant 매트릭스 (3 × 4 × 7 = 84)**
| 속성 | 값 |
|---|---|
| **Type** | `Default` · `With background` · `Bordered` |
| **Size** | `sm` · `base` · `lg` · `xl` |
| **State** | `Initial` · `Focus` · `Filled` · `Disabled` · `Read only` · `Success` · `Danger` |

**Floating label vs Stacked label**
- 일반 폼은 `Input field` + 위에 `Label` (stacked 방식)
- 공간이 좁거나 시각적 임팩트가 필요할 때만 `Floating label`
- **두 방식을 한 화면에 섞지 말 것**

---

### 16. Range slider (2 + 1 + 2 + 2 variants)

연속적인 숫자 값 입력. 가격, 평점, 볼륨 등. 4개의 ComponentSet으로 분리되어 있어 조립해서 사용합니다.

| Component | Variants | 매트릭스 |
|---|---|---|
| `Range Slider examples` | 2 | Type 2 — `Default` / `Range` |
| `Range Slider bars` | 2 | Type 2 — `Default` / `Range` |
| `Range Slider thumb` | 2 | State 2 — `Initial` / `Active` |
| `Range Slider helper` | 1 | min/max 표시 + tooltip 토글 |

**언제 Range slider인가**
- 범위 직관성이 중요할 때 (예: 가격 필터)
- 정확한 값 입력이 더 중요하면 `Number form`
- 둘 다 필요하면 둘을 함께 표시 (`Number form: Range`)

---

### 17. Datepicker (8 + 1 + 5 + 2 variants)

날짜 선택. `Datepicker cell`이 8가지 상태를 모두 지원하므로 단일 날짜뿐 아니라 **범위 선택**도 가능합니다.

| Component | Variants | 매트릭스 |
|---|---|---|
| `Datepicker cell` | 8 | State 8 — `Initial` · `Hover` · `Active` · `Active start` · `Active between` · `Active end` · `Disabled` · `Outlined` |
| `Datepicker controls` | 1 | 월 이동 / 연도 / 모드 전환 |
| `Datepicker dropdown menu` | 5 | Type 5 — `Default` · `Range` · `Year picker` · `Month` · `Day/Year/Month` |
| `Datepicker (ready to use examples)` | 2 | Type 2 — `Default` / `Range` |

**한국어 라벨 가이드**
- 요일은 `일/월/화/수/목/금/토` 단음절 (가장 짧음)
- 포맷은 `2026.05.08` 또는 `2026년 5월 8일` 중 하나로 통일 (`06-ux-writing.md §8.2`)

---

### 18. Timepicker (84 + 6 + 4 variants)

시간 선택. Input 자체가 84 variants로 매우 풍부합니다.

| Component | Variants | 매트릭스 |
|---|---|---|
| `Timepicker input` | 84 | Type 3 (`Default`/`Left add-on`/`Right add-on`) × Size 4 × State 7 |
| `Timepicker form` | 6 | Type 6 — `Default` · `Inline` · `With Dropdown` · `With button` · `With select` · `With input` |
| `Timepicker dropdown menus` | 4 | Type 4 — `Default` · `Timepicker dropdown menus` · `Advanced with buttons` · `Advanced with slider` |

**한국어 시간 표기**
- `오후 2:30` 또는 `14:30` 중 하나로 통일
- 시스템/관리 화면은 24시간제 권장 (`06-ux-writing.md §8.2`)

---

### 19. Icons — 코드에서의 아이콘 사용 규칙

BOS 4.0의 Figma에는 2,698개의 아이콘이 등록되어 있지만, **코드에서는 이 모든 아이콘을 SVG로 번들링하지 않습니다.** 대신 **Lucide 아이콘 라이브러리**를 외부 의존성으로 사용합니다.

**왜 Lucide인가**
- Flowbite 공식 아이콘 세트가 Lucide 기반이라 BOS 4.0 Figma 라이브러리의 시각 언어와 일치
- 오픈소스(ISC), 1,400+개 아이콘, 활발한 유지보수
- tree-shaking 지원으로 번들 크기 부담 없음
- Figma 아이콘 이름과 Lucide 아이콘 이름이 대부분 동일 (`users`, `settings`, `chevron-down` 등)

#### 19.1 사용 방법

**React/Vue/Svelte 프로젝트** (권장)
```bash
npm install lucide-react   # React
npm install lucide-vue-next # Vue
npm install lucide-svelte   # Svelte
```
```jsx
import { Users, LayoutDashboard, Settings } from 'lucide-react';

<Users size={18} strokeWidth={1.8} aria-hidden="true" />
```

**일반 HTML/정적 페이지**
```html
<!-- CDN 1회 로드 -->
<script src="https://unpkg.com/lucide@latest"></script>

<!-- 사용 -->
<i data-lucide="users"></i>

<script>lucide.createIcons();</script>
```

**아이콘 카탈로그 검색**: https://lucide.dev/icons/

#### 19.2 크기 & 스타일 규칙

| 상황 | 크기 | stroke-width |
|---|---|---|
| 네비게이션 링크 (Nav link) | `18px` | `1.8` |
| 버튼 내부 (일반) | `16px` | `1.8` |
| 버튼 내부 (sm) | `14px` | `1.8` |
| Input field add-on | `16px` | `1.8` |
| Icon shape (빈 상태/에러) | `28px` | `1.6` |
| 대형 히어로/일러스트 보조 | `32px+` | `1.5` |

> **stroke-width**: Flowbite 표준은 `1.8`. `Icon shape` 컴포넌트 내부는 `interaction-patterns.md §9.2` 기준 `1.6` 사용.

#### 19.3 색상 규칙

아이콘 색상은 **항상 시멘틱 토큰** 사용. `currentColor`가 기본이므로 부모 텍스트 색상 상속 활용.

```jsx
// ✅ Good — 부모 color 상속 (기본)
<button className="nav-link">  {/* color: var(--text-body) */}
  <Users />                    {/* 자동으로 text-body 색상 */}
  회원 관리
</button>

// ✅ Good — 명시적 시멘틱 토큰
<Users style={{ color: 'var(--colors-text-text-fg-brand)' }} />

// ❌ Bad — 원시 색상 직접 지정
<Users color="#3851dd" />
```

#### 19.4 접근성 규칙 (`07-accessibility.md §6.2`와 연결)

**라벨과 함께 있는 아이콘** → `aria-hidden="true"` (스크린 리더가 중복 읽기 방지)
```jsx
<button>
  <Users aria-hidden="true" />
  회원 관리
</button>
```

**아이콘 단독 사용** → 부모에 `aria-label`
```jsx
<button aria-label="검색">
  <Search aria-hidden="true" />
</button>
```

**장식용 아이콘** → `aria-hidden="true"`
```jsx
<h2>
  <Star aria-hidden="true" /> 추천 항목
</h2>
```

#### 19.5 Figma에만 있는 커스텀 아이콘이 필요할 때

Lucide에 없는 도메인 특화 아이콘(예: 브랜드 로고, 서비스 고유 심볼)이 필요하면:

1. **먼저 Lucide에서 유사 아이콘 검색** — 90%는 대체 가능
2. **Figma Icons 페이지에서 export** — `figma.use_figma`로 SVG 추출
3. **프로젝트에 `src/icons/`로 저장** — 재사용 가능한 React 컴포넌트로 래핑
4. **stroke-width `1.8`, 24x24 viewBox 규격 통일**

```jsx
// src/icons/CustomBrand.tsx — Figma에서 추출한 커스텀 아이콘
export const CustomBrand = ({ size = 18, ...props }) => (
  <svg width={size} height={size} viewBox="0 0 24 24" fill="none"
    stroke="currentColor" strokeWidth="1.8" strokeLinecap="round"
    strokeLinejoin="round" aria-hidden="true" {...props}>
    {/* Figma에서 추출한 path */}
  </svg>
);
```

#### 19.6 자주 쓰는 아이콘 매핑 (네비/액션 기준)

| 용도 | Lucide 이름 |
|---|---|
| 대시보드 | `layout-dashboard` |
| 회원/사용자 | `users` (복수), `user` (단수) |
| 프로젝트 | `folder-kanban`, `briefcase` |
| 리포트/분석 | `bar-chart-3`, `line-chart` |
| 설정 | `settings`, `cog` |
| 검색 | `search` |
| 추가 | `plus` |
| 수정 | `pencil` |
| 삭제 | `trash-2` |
| 더보기 (⋯) | `more-vertical`, `more-horizontal` |
| 알림 | `bell` |
| 테마 전환 | `moon`, `sun` |
| 닫기 | `x` |
| 이전/다음 | `chevron-left`, `chevron-right` |
| 펼치기 | `chevron-down` |
| 에러 | `alert-circle`, `x-circle` |
| 성공 | `check`, `check-circle` |
| 경고 | `alert-triangle` |
| 다시 시도 | `refresh-cw` |

---

## 🔍 컴포넌트가 위 18개 외에 필요할 때

다음 순서로 진행:

1. **이 문서의 전체 인덱스에서 카테고리 확인**
2. **`figma.use_figma`로 해당 페이지 노드를 조회**:
   ```js
   // 예: Stepper 컴포넌트 보기
   const node = await figma.getNodeByIdAsync('1149:XXX');
   ```
3. **변형 구조(`componentPropertyDefinitions`)부터 파악**
4. **사용 사례에 맞는 variant 선택**

---

## 🚦 컴포넌트 선택 의사결정 트리

### "이런 화면을 만들고 싶다" 케이스별

```
사용자가 행동을 트리거 → Button (Color/Size/State 매트릭스)
사용자가 데이터 입력 → Forms 카테고리 (적합한 Input type 선택)
사용자에게 정보 표시:
  ├─ 항목 비교/선택 → Card (Type 선택)
  ├─ 비교/정렬 필요 → Tables
  ├─ 시간 흐름     → Timelines
  ├─ 단계 진행     → Stepper
  ├─ 카테고리/상태 → Badge
  └─ 별점/평가     → Rating
사용자에게 피드백:
  ├─ 일시적 알림   → Toast (9가지 변형)
  ├─ 영구 알림     → Alert
  ├─ 페이지 상단   → Banner
  ├─ 결정/확인     → Modal (10가지 type)
  ├─ 호버 정보     → Tooltips
  ├─ 클릭 정보     → Popovers
  └─ 사이드 패널   → Drawer
사용자 내비게이션:
  ├─ 전역 네비     → Navbar (또는 Sidebar)
  ├─ 모바일 하단   → Bottom navigation
  ├─ 페이지 내 탭  → Nav Tabs
  ├─ 페이지 경로   → Breadcrumbs
  ├─ 페이지 이동   → Pagination
  └─ 메뉴 선택     → Dropdowns (Type 선택)
```

---

## 🔧 컴포넌트가 라이브러리에 없을 때

BOS 4.0은 47개 카테고리, 수천 개 variant가 있어서 거의 모든 케이스를 커버합니다.
정말 없다고 판단된다면:

1. **유사 컴포넌트의 variant 조합으로 가능한지** 다시 확인
2. **Figma 검색 (`figma.search_design_system`) 으로 키워드 검색**
3. 정말 새 컴포넌트가 필요하다면, 사용자에게 다음과 같이 제안:

```
"이 화면에는 [컴포넌트명]이 필요해 보이는데, BOS 4.0에서 찾지 못했습니다.
유사한 카테고리: [____]
임시로 다음과 같이 만들어두고, Figma 라이브러리에 추가하시는 걸 추천드립니다:

- 컴포넌트명: ___
- 목적: ___
- 참고할 기존 컴포넌트: ___"
```

4. 임시 컴포넌트는 반드시 `bos4-design-tokens.css`의 시멘틱 토큰만 사용

---

## 📝 변경 이력

- `2026-04-20` — 초기 버전 (구 Bos)
- `2026-04-20 (BOS 4.0)` — BOS 4.0 47개 카테고리 인덱스화 + 핵심 10개 깊이 가이드. 컴포넌트 철학을 "목적별"에서 "속성 조합형"으로 전환.
- `2026-04-21 (Icons 섹션 추가)` — §11 Icons 전용 가이드 신설. Lucide 라이브러리 사용을 표준으로 정의. 크기/stroke/색상/접근성 규칙, Figma 커스텀 아이콘 추출 플로우, 자주 쓰는 아이콘 매핑 표 포함.
- `2026-05-08 (핵심 18개로 확장)` — Figma 공식 파일(`jmK75D3yVgpYh0wHAlsAwy`)과의 1:1 정합화 + 깊이 가이드 8종 추가:
  - **Card §6 전면 교체** — `CTA card` 제거, `With full width tabs`/`With nav tabs`/`Cars with list` 추가, variant 수 `18 types` → `16 variants` 정정
  - **Modal §5** — 동반 컴포넌트 `Modal header` (7), `Modal footer` (4) 추가
  - **Dropdown §8** — 조립 단위 `Dropdown list item` (48), `Dropdown header` (5), `ready to use examples` (6) 추가
  - **§11 Tooltip 신설** — 4 variants (Position 4종)
  - **§12 Nav Tabs 신설** — 12 variants (Type 7종 × Mobile)
  - **§13 Toggle** (12 + 8 + 3 variants) — Switches / Inputs / Group + Label/Helper
  - **§14 Checkbox & Radio** (12 + 54 + 10 variants) — Input / Form 54 / Group 10 + Label/Helper
  - **§15 Floating label inputs** (84 variants) — 3 Type × 4 Size × 7 State
  - **§16 Range slider** (2 + 1 + 2 + 2 variants) — Default / Range, 4 ComponentSets 조립
  - **§17 Datepicker** (8 + 1 + 5 + 2 variants) — Cell 8 state + Range 선택 라이브 데모
  - **§18 Timepicker** (84 + 6 + 4 variants) — Input 84 + Form 6 + Dropdown 4
  - 기존 Icons § 자리이동: §11 → **§19**로 이동, 핵심 컴포넌트 카운트 10 → **18**로 갱신
