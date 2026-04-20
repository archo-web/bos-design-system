# layout-patterns.md — BOS 4.0 레이아웃 패턴

> 이 문서는 BOS 4.0 시스템의 **페이지 레이아웃 표준**을 정의합니다.
> 새 화면을 만들 때, 처음부터 그리지 말고 여기 정의된 5가지 패턴 중 하나를 선택해서 시작하세요.

---

## 🎯 왜 레이아웃 패턴이 필요한가

업무용/마케팅용 화면은 같은 종류의 화면이 반복됩니다 (리스트, 디테일, 폼, 대시보드, 인증).
같은 종류의 화면이 매번 다르게 생기면 사용자는 매번 새로 학습해야 합니다.
**5개 패턴으로 화면의 95%를 커버**하고, 사용자가 어디 가든 익숙한 구조를 보게 합니다.

---

## 📐 글로벌 레이아웃 (모든 페이지 공통)

```
┌─────────────────────────────────────────────────────┐
│  Navbar (height: 64px, fixed top)                   │
├──────┬──────────────────────────────────────────────┤
│      │                                              │
│ Side │  Main Content Area                           │
│ bar  │  padding: spacing-6 spacing-8 (24~32px)      │
│ 240px│  max-width: container-2xl (1440px)           │
│ (접기│                                              │
│ 60px)│                                              │
│      │                                              │
└──────┴──────────────────────────────────────────────┘
```

**구성 요소** (BOS 4.0 컴포넌트)
- **Navbar**: 상단 고정. 높이 `64px`. `bg-primary` 배경, `border-base` 하단 보더.
- **Sidebar**: 좌측. 펼침 `240px`, 접힘 `60px`. `bg-secondary` 배경.
- **Main Content**: 좌우 패딩 `spacing-8` (32px), 최대 폭 `container-2xl` (1440px).

**원칙**
- Navbar와 Sidebar는 **모든 페이지에서 동일**해야 함
- Main Content 영역에서만 페이지별 패턴이 달라짐
- 마케팅 페이지는 Sidebar 없이 Navbar만 사용 (전체 폭)

---

## 🗂 패턴 1: List Page — 리스트형 페이지

가장 많이 쓰는 패턴. 항목들의 목록을 보여주고, 검색/필터/정렬/추가가 가능한 화면.

**용도**: 멤버 목록, 프로젝트 목록, 파일 목록, 주문 목록 등

```
┌───────────────────────────────────────────────────────┐
│  [Title]                            [Primary Action]  │  ← Header (height: 56px)
│  설명 텍스트 (선택, text-body)                         │
├───────────────────────────────────────────────────────┤
│  [Search] [Filter] [Filter]    [View Toggle] [Sort]   │  ← Toolbar (height: 48px)
├───────────────────────────────────────────────────────┤
│                                                       │
│   Table or Card Grid                                  │  ← Content
│   (스크롤 가능)                                       │
│                                                       │
├───────────────────────────────────────────────────────┤
│                              [Pagination] [10/page ▾] │  ← Footer (height: 48px)
└───────────────────────────────────────────────────────┘
```

### 구조 상세

**Header 영역**
- 타이틀: `text-3xl font-bold`, 색상 `text-heading`
- 서브 설명 (선택): `text-sm font-normal`, 색상 `text-body`
- Primary Action: 우측 상단, `Button color="Brand" size="base"`
- 헤더 하단 마진: `spacing-6` (24px)

**Toolbar 영역**
- 좌측: Input field (`Type: Add-on icon` 검색), Dropdown menu (필터)
- 우측: Button group (뷰 전환), Dropdown menu (정렬)
- 좌우 그룹 사이는 `flex justify-between`
- 컨테이너: `bg-primary` + `border-base` + `rounded-base`
- 툴바 하단 마진: `spacing-4` (16px)

**Content 영역**
- 테이블 뷰: BOS 4.0 `Tables` 컴포넌트
- 카드 뷰: BOS 4.0 `Card` 컴포넌트 (Type: Default 또는 With Image), gap `spacing-4`

**Footer 영역**
- 페이지네이션: BOS 4.0 `Pagination` 컴포넌트, 우측 정렬
- per-page 선택: Dropdown menu

### 빈/로딩/에러 상태

```
empty:    Card empty state + Brand CTA
loading:  Skeleton 컴포넌트로 행 모양 유지
error:    Alert (Color: Danger) + 재시도 버튼
```

---

## 📄 패턴 2: Detail Page — 디테일/상세 페이지

하나의 항목을 깊이 보여주는 화면. 정보 + 관련 액션 + 하위 데이터.

**용도**: 멤버 프로필, 프로젝트 상세, 주문 상세, 문서 보기 등

```
┌───────────────────────────────────────────────────────┐
│  [Breadcrumbs > 경로]                                 │
│  [Title]              [Edit] [Delete] [More ⋯]        │  ← Header
│  Status Badge · Meta info (작성자, 날짜)               │
├───────────────────────────────────────────────────────┤
│  [Tab1] [Tab2] [Tab3]                                 │  ← Nav Tabs (선택)
├───────────────────────────────────────────────────────┤
│  ┌─────────────────────────┬─────────────────────┐    │
│  │                         │                     │    │
│  │  Main Content           │  Sidebar            │    │
│  │  (정보 섹션들)          │  (메타 정보,        │    │
│  │                         │   관련 액션)        │    │
│  │  flex: 1                │   width: 320px      │    │
│  └─────────────────────────┴─────────────────────┘    │
└───────────────────────────────────────────────────────┘
```

### 구조 상세

**Header**
- Breadcrumbs: BOS 4.0 `Breadcrumbs` 컴포넌트 (3 depth 이상일 때)
- 타이틀: `text-3xl font-bold`
- 액션 버튼: `Button color="Secondary"` (수정), `Button color="Danger" outline` (삭제), 더보기는 `Dropdown menu Type: Default`
- 메타 정보: `text-xs`, `text-body`, `Badge` (Type: With dot)로 상태 표시

**Tabs (선택)**
- BOS 4.0 `Nav Tabs` 컴포넌트
- 탭이 3개 이하면 생략 가능 (섹션으로 처리)

**Main + Sidebar 레이아웃**
- 데스크톱: `flex` 좌우 분할, gap `spacing-6` (24px)
- Sidebar 폭: `320px` 고정
- 모바일/좁은 화면 (≤ 1024px): Sidebar가 Main 위로 이동 (세로 스택)

**섹션 구성 (Main 내부)**
- 섹션 제목: `text-xl font-bold`
- 섹션 사이 간격: `spacing-8` (32px)
- 섹션 내 그룹 간격: `spacing-4` (16px)

---

## ✏️ 패턴 3: Form Page — 폼 페이지

데이터를 입력/수정하는 화면. 신규 등록, 수정, 설정 등.

**용도**: 멤버 초대, 프로젝트 생성, 회원 가입, 설정 변경 등

### 3-1. Modal Form (간단한 폼, 5개 필드 이하)

```
┌─────────────────────────────────────┐
│  [Title]                        [✕] │  ← Modal header
├─────────────────────────────────────┤
│                                     │
│  Label                              │
│  [Input Field            ]          │
│  Helper text                        │
│                                     │
│  Label                              │
│  [Input Field            ]          │
│                                     │
│  Label                              │
│  [Select form        ▾   ]          │
│                                     │
├─────────────────────────────────────┤
│              [취소]  [Primary CTA]   │  ← Modal footer
└─────────────────────────────────────┘
```

- BOS 4.0 `Modals` 컴포넌트 (Type: `Create product` 또는 `Popup`)
- 폭: `container-xs (380px)` ~ `container-sm (640px)`
- 패딩: `spacing-6` (24px)
- Primary CTA: 우측 하단 (`Color: Brand`), 취소는 좌측 (`Color: Secondary`)

### 3-2. Page Form (긴 폼, 6개 이상 필드 또는 섹션 분리 필요)

```
┌───────────────────────────────────────────────────────┐
│  [Breadcrumbs]                                        │
│  [Title]                                              │
├───────────────────────────────────────────────────────┤
│  ┌────────────────────────────────────────────────┐   │
│  │  [Section 1: 기본 정보]                       │   │
│  │  ──────────────────────                       │   │
│  │  Label                                        │   │
│  │  [Input Field            ]                    │   │
│  │                                               │   │
│  │  Label                                        │   │
│  │  [Input Field            ]                    │   │
│  └────────────────────────────────────────────────┘   │
│                                                       │
│  ┌────────────────────────────────────────────────┐   │
│  │  [Section 2: 추가 정보]                       │   │
│  │  ...                                          │   │
│  └────────────────────────────────────────────────┘   │
│                                                       │
│                          [취소]  [Primary CTA]         │
└───────────────────────────────────────────────────────┘
```

### 폼 공통 규칙

**라벨 - 입력 간격**: `spacing-2` (8px)
**필드 사이 간격**: `spacing-5` (20px)
**섹션 사이 간격**: `spacing-8` (32px)
**폼 최대 폭**: `container-sm (640px)` (가독성을 위해 제한)

**필수 표시**
- 필수 필드는 라벨 옆에 빨간 별표 `*` (`text-fg-danger`)
- 헬퍼 텍스트: BOS 4.0 `Helper` 컴포넌트, `text-xs`, `text-body`
- 에러 메시지: `Helper` 컴포넌트, `text-fg-danger`, 필드 하단 즉시 표시

**Primary CTA 위치 규칙**
- 모달: 우측 하단 (Modal footer)
- 페이지 폼: 우측 하단 (sticky 또는 페이지 끝)
- 취소는 항상 Primary 좌측

---

## 📊 패턴 4: Dashboard — 대시보드

여러 데이터를 한눈에 보여주는 화면. 카드/차트/요약 위주.

**용도**: 워크스페이스 대시보드, 분석 화면, 홈 페이지

```
┌───────────────────────────────────────────────────────┐
│  [Title]                  [Date Range ▾] [Export]     │
├───────────────────────────────────────────────────────┤
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐      │
│  │  KPI 1  │ │  KPI 2  │ │  KPI 3  │ │  KPI 4  │      │  ← Stat Cards
│  │  1,234  │ │  98%    │ │  $45k   │ │  +12    │      │     (4열 권장)
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘      │
│                                                       │
│  ┌─────────────────────────┬─────────────────────┐    │
│  │                         │                     │    │
│  │  Chart Card             │  Summary Card       │    │  ← 2열 분할
│  │  (큰 영역)              │  또는 Top 5 List    │    │
│  │                         │                     │    │
│  └─────────────────────────┴─────────────────────┘    │
│                                                       │
│  ┌────────────────────────────────────────────────┐   │
│  │  Recent Activity / Detailed Table              │   │  ← 풀폭
│  └────────────────────────────────────────────────┘   │
└───────────────────────────────────────────────────────┘
```

### 구조 상세

**Stat Card** (BOS 4.0 `Card Type: Default` 활용)
- 폭: `flex: 1`, 4열 그리드 (gap `spacing-4` = 16px)
- 패딩: `spacing-5` (20px)
- 라벨: `text-sm`, `text-body`
- 숫자: `text-3xl font-bold`, `text-heading`
- 변화량 (선택): `text-xs`, `Badge` 컴포넌트로 표시

**섹션 간격**: `spacing-6` (24px) (대시보드는 약간 좁게)

**그리드 비율**: `2:1` 또는 `3:1` 권장

---

## 🔐 패턴 5: Auth Page — 인증 페이지

로그인, 회원가입, 비밀번호 재설정 등.

**용도**: 로그인, 회원가입, OTP 인증, 비밀번호 재설정

```
┌───────────────────────────────────────────────────────┐
│                                                       │
│              ┌─────────────────────┐                  │
│              │  [Logo]             │                  │
│              │  Title              │                  │
│              │  Subtitle           │                  │
│              │                     │                  │
│              │  Label              │                  │
│              │  [Input Field    ]  │                  │
│              │                     │                  │
│              │  Label              │                  │
│              │  [Input Field    ]  │                  │
│              │                     │                  │
│              │  [Primary CTA]      │                  │
│              │                     │                  │
│              │  ──── or ────       │                  │
│              │  [Social login]     │                  │
│              │                     │                  │
│              │  보조 링크          │                  │
│              └─────────────────────┘                  │
│                                                       │
└───────────────────────────────────────────────────────┘
```

- BOS 4.0 `Card Type: Login form` 활용
- 카드 폭: `container-xs (380px)` ~ `420px`
- 화면 중앙 정렬 (수직/수평)
- Navbar 없음 (또는 미니멀)
- Sidebar 없음 (전체 폭)
- 배경: `bg-secondary` 또는 `bg-primary`

---

## 🎨 그리드 시스템

```
컬럼 수: 12
gutter:  spacing-4 (16px) — 일반
         spacing-3 (12px) — 고밀도 (대시보드)
margin:  spacing-6 ~ 8 (24~32px) — 좌우
```

**자주 쓰는 분할**
- 1/2 + 1/2: Detail 페이지 Main + Sidebar
- 2/3 + 1/3: Dashboard 차트 + 요약
- 1/4 × 4: KPI 카드 4열
- 1/3 × 3: 카드 그리드 3열
- 1/4 × 4: 카드 그리드 4열 (권장 기본)

---

## 📱 반응형 브레이크포인트

```css
/* BOS 4.0은 데스크톱 우선 (업무용 + 마케팅 모두 지원) */
@media (max-width: 1280px) { /* container-xl */
  /* Sidebar 240px → 60px 자동 접기 */
}
@media (max-width: 1024px) { /* container-lg */
  /* Detail의 Sidebar는 Main 위로 */
  /* Dashboard 4열 KPI → 2열 */
}
@media (max-width: 768px)  { /* container-md */
  /* Sidebar overlay 모드 (Drawer로 전환) */
  /* 폼 풀폭 */
  /* Bottom navigation 사용 */
}
```

**원칙**: BOS 4.0은 데스크톱과 모바일을 모두 지원합니다. 모바일은 `Bottom navigation` + `Mobile menu` 컴포넌트로 전환됩니다.

---

## 🚦 패턴 선택 의사결정 트리

```
이 화면의 주된 목적은?
├─ 여러 항목 중 선택/탐색  → List Page (패턴 1)
├─ 한 항목 자세히 보기     → Detail Page (패턴 2)
├─ 데이터 입력/수정       → Form (패턴 3)
│   ├─ 5개 필드 이하 + 단순 → Modal Form (3-1)
│   └─ 6개 이상 또는 섹션 분리 → Page Form (3-2)
├─ 여러 지표 한눈에 보기   → Dashboard (패턴 4)
└─ 로그인/가입/인증       → Auth Page (패턴 5)
```

---

## ❌ 흔한 실수

1. **List Page에서 검색을 빼먹기** — 항목 10개 이상이면 검색은 필수
2. **Form에서 라벨을 placeholder로 대체** — 입력 시작하면 라벨 사라짐, 절대 금지
3. **Detail Page의 Sidebar를 Main과 같은 폭** — Sidebar는 보조 정보, `320px` 고정
4. **Dashboard에 KPI 8개 이상 나열** — 4개가 적정선, 6개가 한계
5. **Modal에서 필드 10개 이상** — 페이지 폼으로 전환하세요
6. **다크 모드 무시** — 시멘틱 토큰 사용했는지 점검

---

## 📝 변경 이력

- `2026-04-20` — 초기 버전 (구 Bos)
- `2026-04-20 (BOS 4.0)` — Auth Page 패턴 추가, BOS 4.0 컴포넌트 매핑, container 토큰 적용
