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
- **Icons** — 2698개 아이콘
- **Shadows & Borders** — 그림자, 테두리
- **Patterns** — 배경 패턴 (20개)

---

## 📋 핵심 10개 컴포넌트 깊이 가이드

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

---

### 6. Card (18 types)

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
| `CTA card` | 행동 유도 카드 |
| `Pricing card` | 요금제 카드 |
| `Testimonial card` | 후기/리뷰 카드 |
| `Crypto` | 암호화폐 정보 카드 |
| `Grid card` | 그리드 레이아웃용 |

**모바일 대응**: `Mobile Version: True` 옵션으로 자동 반응형

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

## 🔍 컴포넌트가 위 10개 외에 필요할 때

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
