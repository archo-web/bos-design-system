# design-principles.md — BOS 4.0 디자인 원칙

> 이 문서는 Claude가 새 화면을 만들 때 **"왜 이렇게 만드는가"**의 기준이 됩니다.
> 컴포넌트 선택이 애매할 때, 레이아웃이 갈릴 때, 이 원칙으로 결정합니다.

---

## 🧭 BOS 4.0이 추구하는 것

BOS 4.0은 **Flowbite + Tailwind 시스템을 기반으로 한 광범위한 다목적 디자인 시스템**입니다.
업무용 대시보드부터 마케팅 페이지, 모바일 앱까지 다양한 컨텍스트를 커버합니다. 그래서 우리는:

- **차분하게** — 화려함보다 가독성 우선
- **정확하게** — 정보 위계가 한눈에 들어와야 함
- **빠르게** — 클릭 한 번이면 끝나야 할 일을 두 번 만들지 않음
- **일관되게** — 같은 행동은 같은 모양으로
- **확장 가능하게** — 다크 모드, 다양한 컨텍스트, 새 기능 추가에 유연

---

## 📐 6가지 디자인 원칙

### 1. Clarity over Cleverness — 명료함이 영리함보다 우선

사용자는 우리의 디자인을 감상하러 오지 않습니다. 일을 끝내러 옵니다.

**Do**
- 라벨은 직관적인 단어로 ("저장" ✓, "데이터 영속화" ✗)
- 아이콘 단독 사용 시 반드시 tooltip 제공
- 기본 색상은 Gray 스케일, 강조가 필요할 때만 Brand

**Don't**
- 장식용 일러스트레이션, 그라데이션, 그림자 남발
- "여기를 클릭하세요" 같은 모호한 라벨
- 같은 화면에 3가지 이상의 강조 색상

---

### 2. Semantic Tokens First — 시멘틱 토큰이 먼저

BOS 4.0의 가장 중요한 원칙입니다. **원시 토큰(`gray/900`, `brand/500`)을 직접 쓰지 않고, 시멘틱 토큰(`text-heading`, `bg-brand`)을 씁니다.**

**왜?**
- 다크 모드 자동 대응 — 시멘틱 토큰이 모드별로 다른 원시 값을 참조
- 의미 변화에 강함 — "본문 색상"의 정의가 바뀌어도 한 곳만 수정
- 코드 가독성 — `bg: text-heading`이 `bg: gray/900`보다 의도가 명확

**예시**
```css
/* ❌ 원시 토큰 직접 사용 — 다크 모드에서 깨짐 */
.title { color: var(--colors-gray-900); }

/* ✅ 시멘틱 토큰 — Light/Dark 자동 대응 */
.title { color: var(--colors-text-text-heading); }
```

---

### 3. Information Hierarchy First — 정보 위계가 먼저

화면을 만들 때 **타이포그래피와 간격으로 위계**를 만듭니다. 색상은 마지막 수단입니다.

**위계 만드는 순서**
1. 타이포 크기/굵기 (`text-3xl bold` → `text-xl bold` → `text-base medium` → `text-sm regular`)
2. 간격 (`spacing-8 = 32px`로 섹션 분리, `spacing-4 = 16px`로 그룹 내부)
3. 색상 (`text-heading` 본문, `text-body` 보조, `text-fg-disabled` 비활성)
4. 배경 (`bg-secondary`로 영역 구분)

색상 강조는 **"꼭 봐야 하는 것"에만**.

---

### 4. Component Variants are a Contract — 컴포넌트 variant는 계약이다

BOS 4.0은 **속성 조합형 컴포넌트**입니다. Button만 봐도 500개 variant가 있습니다.
Variant 매트릭스는 **계약**입니다. 임의로 새 variant를 만들지 않습니다.

**Button의 variant 매트릭스 (예시)**
- `Color`: Brand / Secondary / Tertiary / Success / Danger / Warning / Dark / Ghost (8)
- `Size`: xs / sm / base / l / xl (5)
- `State`: Initial / Hover / Focus / Disabled (4)
- `Outline`: false / true (2)
- `Icon only`: false / true (2)

→ Color × Size × State 조합이 정해져 있음. **이 외 조합은 만들지 않음**.

**원칙**
- 새 variant가 필요하면 → 디자이너에게 라이브러리 추가 요청
- 기존 variant의 색상/사이즈만 살짝 바꾸기 → ❌ 금지
- 완전히 새로운 컴포넌트가 필요하면 → 우선 Figma 라이브러리에 동일 목적의 것이 있는지 검색

---

### 5. State is Not Optional — 상태는 옵션이 아니다

가장 많이 빠뜨리는 영역. **모든 인터랙티브 요소**는 4가지 상태를 가집니다 (BOS 4.0 표준).

| 상태 | BOS 4.0 명칭 | 설명 |
|---|---|---|
| 기본 | `Initial` | 평상시 |
| 호버 | `Hover` | 마우스 오버 |
| 포커스 | `Focus` | 키보드 포커스 또는 활성화 |
| 비활성 | `Disabled` | 사용 불가 |

> ⚠️ BOS 4.0은 `Active/Pressed`를 별도 상태로 두지 않고, Hover의 변형 또는 Focus로 통합합니다.

**데이터 표시 화면**은 추가로 3가지:
- `empty` — 데이터 없음 (안내 문구 + 다음 행동 CTA)
- `loading` — 데이터 로딩 중 (skeleton 또는 spinner)
- `error` — 로딩 실패 (에러 메시지 + 재시도 버튼)

---

### 6. Predictable Interactions — 예측 가능한 인터랙션

같은 행동은 같은 위치에 같은 모양으로.

**위치 규칙**
- Primary CTA (저장/확인/등록): 모달은 우측 하단, 페이지는 상단 우측
- Destructive (삭제): Primary와 시각적으로 구분 (`Color: Danger`), 확인 다이얼로그 필수
- Cancel/취소: Primary 좌측에 위치, `Color: Secondary` 사용

**컬러 규칙 (시멘틱)**
- `bg-brand` (Brand): 주요 행동, 활성 상태 → `Color: Brand`
- `bg-danger`: 에러, 삭제, 위험 → `Color: Danger`
- `bg-success`: 성공, 완료 → `Color: Success`
- `bg-warning`: 경고, 주의 → `Color: Warning`

색상의 의미를 **절대 섞지 않습니다**. 빨간색 "확인" 버튼은 만들지 않습니다.

---

## 🎨 색상 사용 의사결정 트리

```
이 요소가 사용자의 주의를 끌어야 하는가?
├─ Yes → 의미는?
│   ├─ 주요 행동/링크 → text-fg-brand / bg-brand
│   ├─ 에러/삭제      → text-fg-danger / bg-danger
│   ├─ 성공/완료      → text-fg-success / bg-success
│   ├─ 경고/주의      → text-fg-warning / bg-warning
│   └─ 정보/알림      → text-fg-info
└─ No → Gray 시멘틱
    ├─ 제목       → text-heading
    ├─ 본문       → text-body
    ├─ 보조 텍스트 → text-body-subtle
    ├─ 비활성     → text-fg-disabled
    ├─ 구분선     → border-base
    └─ 배경       → bg-secondary 또는 bg-primary
```

---

## 📏 간격 의사결정 트리

```
두 요소가 같은 그룹인가?
├─ 같은 그룹, 강하게 연결 → spacing-1 ~ 2 (4 ~ 8px)
│   예: 아이콘 + 라벨, 버튼 내부 padding
├─ 같은 그룹, 일반적 → spacing-3 (12px)
│   예: 폼 라벨과 입력 필드 사이
├─ 같은 섹션 내 다른 그룹 → spacing-4 ~ 5 (16 ~ 20px)
│   예: 폼 필드 사이
└─ 다른 섹션 → spacing-6 ~ 8 (24 ~ 32px)
    예: 페이지 내 섹션 분리
```

---

## 🌗 다크 모드 원칙

BOS 4.0은 **시멘틱 토큰의 Light/Dark 모드 매핑으로 다크 모드를 정식 지원**합니다.

**적용 방법**
```html
<!-- HTML 루트에 data-theme 속성 -->
<html data-theme="light"> <!-- 또는 "dark" -->
```

**원칙**
- 모든 컴포넌트는 시멘틱 토큰만 사용 → 다크 모드 자동 대응
- 다크 모드 전용 색상 만들지 말 것 → 토큰 시스템에 맡길 것
- 이미지/아이콘은 다크 모드에서 충분한 명도 대비 확보 (필요 시 다크 전용 자산 별도 제공)
- 순수 검정 (`#000`)을 피하고 `gray-950 (#030712)` 사용 → 눈 피로도 감소
- **같은 hue 동조 효과 주의** — 다크 배경 위에 같은 색조 텍스트를 올리면 분리되지 않습니다. Active/Selected 상태처럼 강조가 필요한 곳은 흰색 텍스트 + 채도 높은 배경을 사용하세요. 상세는 `interaction-patterns.md §1.3` 참조.

**다크 모드 색조 함정 — 빠른 체크**

| 상황 | 문제 | 해결 |
|---|---|---|
| 다크 배경 + brand 계열 텍스트 | 색조 동조로 묻힘 | 흰색 텍스트로 |
| 다크 배경 + 빨강/초록 텍스트 (소프트 토큰) | 채도 부족으로 묻힘 | 더 밝은 단계 (`-300` ~ `-400`)로 매핑 |
| 진한 색 위에 같은 색 보더 | 보더가 안 보임 | 보더 생략 또는 `bg-quaternary` 톤 |

---

## ✍️ UX 라이팅 원칙 (간략)

상세는 `ux-writing.md` 참조.

- **버튼 라벨**: 동사로 시작, 4자 이내 권장 ("저장", "삭제", "취소", "다음")
- **에러 메시지**: 무엇이 / 왜 / 어떻게 해결할지
- **빈 상태**: 상태 설명 + 다음 행동
- **존댓말 통일** — "~하세요" 톤

---

## 🚦 의사결정이 안 될 때

이 순서로 자문합니다:

1. **사용자가 이걸 보고 헷갈리는가?** → 헷갈리면 다시.
2. **다른 화면에서 같은 일을 어떻게 처리했는가?** → 일관성 우선.
3. **이 디자인이 다크 모드에서도 동작하는가?** → 시멘틱 토큰 사용 점검.
4. **이 디자인이 1년 뒤에도 말이 되는가?** → 트렌드 의존 금지.
5. **컴포넌트 라이브러리에 이미 있는가?** → 있으면 그걸 쓴다 (47개 카테고리 중 검색).

---

## 📝 변경 이력

- `2026-04-20` — 초기 버전 (구 Bos)
- `2026-04-20 (BOS 4.0)` — BOS 4.0 시멘틱 토큰 시스템 + variant 계약 + 다크 모드 정식 지원
- `2026-04-20 (다크 모드 함정)` — 다크 모드 원칙에 색조 동조 효과 회피 룰 추가. 색조 함정 빠른 체크 표 신설
