# interaction-patterns.md — BOS 4.0 인터랙션 패턴

> 이 문서는 **상태(state)와 인터랙션** 표준을 정의합니다.
> Vibe coding에서 가장 자주 빠뜨리는 영역이고, 사용자 경험 격차의 80%가 여기서 발생합니다.

---

## 🎯 핵심 원칙

> **"디폴트 상태만 만들면 디자인의 25%만 만든 것이다."**

BOS 4.0의 모든 인터랙티브 요소는 **4가지 상태**(Initial/Hover/Focus/Disabled)를 가지고, 데이터 표시 화면은 추가로 3가지 상태(Empty/Loading/Error)를 가집니다.

---

## 1. 인터랙티브 상태 — BOS 4.0의 4가지

BOS 4.0은 기존 Bos의 5가지(default/hover/active/disabled/loading)에서 **4가지로 단순화**했습니다.

| 상태 | 설명 | 시각적 단서 |
|---|---|---|
| `Initial` | 기본 상태 | — |
| `Hover` | 마우스 오버 또는 active/pressed | 배경/색상 한 단계 변화 |
| `Focus` | 키보드 포커스 또는 활성화 | brand 색상 outline 또는 ring |
| `Disabled` | 비활성 | `opacity: 0.5` 또는 `bg-disabled` + cursor: not-allowed |

> ⚠️ Loading은 별도 상태가 아니라 **컴포넌트 내부의 ARIA 속성**(`aria-busy`)이나 `Spinners` / `Skeleton` 컴포넌트로 표현합니다.

### 1.1 Button 상태 (Color: Brand 기준)

| 상태 | 시멘틱 토큰 |
|---|---|
| `Initial` | `bg-brand` (= brand-700) `text-white` |
| `Hover` | `bg-brand-strong` (= brand-800) |
| `Focus` | `bg-brand` + outline `border-brand` 2px |
| `Disabled` | `bg-disabled` `text-fg-disabled` `cursor: not-allowed` |

**트랜지션**
- 모든 상태 변화: `transition: all 150ms ease-out`
- hover 진입은 즉시, 이탈은 부드럽게

### 1.2 Secondary Button 상태

| 상태 | 시멘틱 토큰 |
|---|---|
| `Initial` | `bg-secondary` `border-base` `text-body` |
| `Hover` | `bg-tertiary` |
| `Focus` | `bg-secondary` + outline `border-brand` 2px |
| `Disabled` | `bg-disabled` `text-fg-disabled` |

### 1.3 Selected / Active 상태 (Nav, Tab, Toggle, List item)

선택됐음을 명확히 표시해야 하는 컴포넌트의 상태입니다. **Button의 Hover/Focus와는 완전히 다른 디자인 의도**예요 — Button은 "이게 누를 수 있는 거예요"고, Selected는 "이게 지금 선택된 거예요". 강도가 달라야 합니다.

#### ⛔ 흔한 함정: 같은 hue 내 채도 차이만으로 분리하기

Brand 컬러를 active 표시에 쓸 때, **배경과 텍스트가 같은 색조(hue)면 시각적으로 분리되지 않습니다.**

```css
/* ❌ 안 좋음 — 같은 보라 hue 안에서만 명도 차이 */
.nav--active {
  background: var(--bg-brand-soft);    /* brand-100 (Light) / brand-900 (Dark) */
  color: var(--text-fg-brand-strong);  /* 같은 brand 계열 텍스트 */
}
```

다크모드에서 `brand-900` 배경 + `brand-300` 텍스트는 **둘 다 보라 계열**이라 동조 효과(color assimilation)로 묻혀 보입니다. WCAG 대비비를 계산해도 3:1 이하로 떨어지는 경우가 많아요.

#### ✅ 권장 패턴 — 3가지 중 선택

**패턴 A: 진한 brand 배경 + 흰 텍스트** (가장 표준적, Material/Linear/GitHub)

```css
.nav--active {
  background: var(--bg-brand);      /* brand-700 (Light) / brand-600 (Dark) */
  color: var(--text-white);          /* 항상 흰색 — 다크모드도 동일 */
}
```
- 대비비 7:1+ (WCAG AAA)
- "선택됨"이 강하게 인지됨

**패턴 B: 옅은 brand 배경 + brand 텍스트** (라이트모드만 권장)

```css
[data-theme="light"] .nav--active {
  background: var(--bg-brand-softer);  /* brand-050 — 매우 옅음 */
  color: var(--text-fg-brand);          /* brand-700 — 진함 */
}
/* 다크모드는 패턴 A 사용 */
[data-theme="dark"] .nav--active {
  background: var(--bg-brand);
  color: var(--text-white);
}
```
- 라이트모드: 부드러운 강조 (Notion 스타일)
- 다크모드: 색조 동조 회피를 위해 패턴 A로 자동 전환

**패턴 C: 좌측 indicator + 미니멀 배경** (사이드바 LNB에 자주)

```css
.nav--active {
  background: var(--bg-tertiary);     /* 약한 회색 배경 */
  color: var(--text-heading);
  position: relative;
}
.nav--active::before {
  content: '';
  position: absolute; left: 0; top: 25%; bottom: 25%;
  width: 3px;
  background: var(--bg-brand);        /* 좌측에 brand 막대만 */
  border-radius: 0 2px 2px 0;
}
```
- 가장 미니멀, 색조 동조 문제 없음
- VSCode, Notion 사이드바 패턴

#### 의사결정

| 컴포넌트 | 권장 패턴 |
|---|---|
| Sidebar LNB (active item) | A 또는 C |
| Top Navbar | A 또는 B |
| Tabs | B |
| Toggle (선택된 옵션) | A |
| Pagination (현재 페이지) | A |
| Dropdown (선택된 항목) | B |

#### 핵심 룰

> **다크모드에서 active 상태를 디자인할 때, 배경과 텍스트가 같은 hue 계열이라면 둘 중 하나는 흰색이거나 회색이어야 한다.**

### 1.4 Input field 상태 (BOS 4.0 7가지 — 더 풍부함)

| BOS 4.0 State | 시각 |
|---|---|
| `Initial` | `border-base` `bg-primary` |
| `Focus/Typing` | `border-brand` + ring `border-brand-subtle` 2px |
| `Filled` | `border-base` (값이 있는 비포커스 상태) |
| `Disabled` | `bg-disabled` `text-fg-disabled` |
| `Read-only` | `bg-secondary` `text-body` |
| `Success` | `border-success` + 우측 ✓ 아이콘 |
| `Danger` | `border-danger` + 우측 ⚠ 아이콘 + 하단 에러 텍스트 |

**Focus 우선순위**
- 키보드 포커스는 항상 시각적으로 명확해야 함 (접근성)
- 마우스 클릭 후에도 focus ring 유지 (`:focus-visible` 활용)

---

## 2. 데이터 표시 상태 (3가지) — 가장 많이 빠뜨리는 영역

데이터를 보여주는 모든 화면은 **반드시** 다음 3가지 상태를 정의합니다.

### 2.1 Empty State — 데이터 없음

```
┌──────────────────────────────────────────┐
│                                          │
│              [Icon shape]                │
│                                          │
│         아직 등록된 프로젝트가 없습니다  │
│      첫 프로젝트를 만들어 협업해 보세요  │
│                                          │
│            [+ 프로젝트 추가]              │
│                                          │
└──────────────────────────────────────────┘
```

**구성 요소** (BOS 4.0 컴포넌트 활용)
- 아이콘: BOS 4.0 `Icon shape` 컴포넌트 (64px) — 상태에 맞는 색상
- 제목: `text-base font-medium`, `text-heading`
- 설명: `text-sm font-normal`, `text-body`, max-width 360px
- CTA: `Button color="Brand"` 또는 `Button color="Secondary"`

**필수 규칙**
- ❌ "데이터 없음" 한 줄로 끝내기
- ❌ 빈 화면 그대로 두기
- ✅ 왜 비어있는지 + 무엇을 할 수 있는지 + 어떻게 할 수 있는지

**상황별 톤**
| 상황 | 톤 | 예시 |
|---|---|---|
| 처음 사용 (onboarding) | 환영, 안내 | "첫 프로젝트를 만들어 보세요" |
| 검색 결과 없음 | 차분, 대안 제시 | "검색 결과가 없습니다. 다른 키워드로 시도해 보세요" |
| 필터 결과 없음 | 차분, 필터 초기화 제안 | "조건에 맞는 항목이 없습니다. [필터 초기화]" |
| 모두 처리됨 | 긍정적 | "확인할 알림이 없습니다 ✓" |

---

### 2.2 Loading State — 로딩 중

**선택 기준**

| 상황 | 어떤 로딩? | BOS 4.0 컴포넌트 |
|---|---|---|
| 페이지 첫 로드 | Skeleton | `Skeleton` |
| 부분 데이터 로드 | Skeleton | `Skeleton` |
| 액션 결과 대기 (저장/삭제) | Button 내부 spinner | `Spinners` |
| 백그라운드 처리 | 상단 progress bar | `Progress Bars` |
| 전체 화면 차단 | Overlay spinner | `Spinners` (최소 사용) |

**Skeleton 규칙**
- 실제 컨텐츠와 같은 크기/위치
- 베이스: `bg-tertiary` + shimmer 애니메이션
- 애니메이션: `1.5s ease-in-out infinite`
- 표시 시작 `300ms` 지연 (빠르게 끝나는 로드는 깜빡임 방지)

**Spinner 규칙**
- 크기: 버튼 내 `16px`, 페이지 `32px`, 풀스크린 `48px`
- 색상: 컨텍스트에 맞춰 (Brand 영역은 `text-fg-brand`, 일반은 `text-body`)
- 텍스트 동반: 5초 이상 걸리면 "처리 중..." 또는 진행 상황 표시

---

### 2.3 Error State — 에러

**3가지 에러 레벨**

| 레벨 | 컴포넌트 | 예시 |
|---|---|---|
| Field-level | Helper (state: Danger) | 폼 필드 검증 실패 |
| Section-level | `Alert (Color: Danger)` | 일부 위젯 로드 실패 |
| Page-level | 전체 페이지 에러 + 재시도 | API 전체 실패 |

#### Field-level Error

```
이메일
[example@                     ] ← Input field State: Danger
⚠ 이메일 형식이 올바르지 않습니다     ← Helper, text-fg-danger
```

- Input field `State: Danger`
- Helper 컴포넌트로 에러 메시지
- 표시 시점: blur 후 또는 submit 후
- 타이핑 시작하면 즉시 사라짐

#### Section-level Error

```
┌──────────────────────────────────────────┐
│ ⚠  데이터를 불러올 수 없습니다              │
│    네트워크를 확인하고 다시 시도해 주세요    │
│    [다시 시도]                            │
└──────────────────────────────────────────┘
```

- BOS 4.0 `Alert` 컴포넌트, `Type: Default` 또는 `Complex`, `Color: Danger`
- 기존 데이터는 그대로 두고 해당 섹션만 에러 표시

#### Page-level Error

- BOS 4.0 `Modals Type: Info` 또는 풀폭 빈 상태 (Empty 패턴 활용)
- 항상 다음 행동 제공 (재시도, 이전 페이지, 홈)
- 에러 코드는 작게 표시 (`text-xs`, `text-fg-disabled`)

---

## 3. 피드백 패턴

### 3.1 즉시 피드백 (300ms 이내)

사용자가 액션을 취하면 **300ms 이내** 시각적 반응이 있어야 합니다.

| 액션 | 즉시 피드백 |
|---|---|
| 버튼 클릭 | `Hover` 또는 `Focus` 상태 (색상 변화) |
| 토글/체크박스 | 즉시 상태 전환 |
| 입력 | 입력값 즉시 반영 |
| 선택 | 선택 시각화 즉시 |

### 3.2 결과 피드백 (Toast 사용)

작업 완료 후 사용자에게 결과 알림. BOS 4.0 `Toast` 9가지 변형 활용.

| 상황 | Toast 변형 | 표시 시간 |
|---|---|---|
| 저장 성공 | `Icon & text` 또는 `Icon shape & text` | 3초 |
| 저장 실패 | `Error` | 6초 또는 dismiss |
| 삭제 완료 | `Avatar & button` (Undo CTA 포함) | 5초 |
| 일괄 작업 진행 | `With progress bar` | 진행 시간 + 3초 |
| 새 알림 (사용자) | `Avatar & button` 또는 `With header` | 사용자 dismiss까지 |
| 업데이트 필요 | `Icon shape & buttons` | 사용자 dismiss까지 |
| 중요 변경 (권한 등) | `Modal Type: Info` | 사용자 액션까지 |
| 시스템 점검 안내 | `Banner` (영구) | 영구 |

**Undo 패턴**
- 삭제 같은 파괴적 액션은 가능하면 Toast에 Undo 제공
- Toast 변형: `Avatar & button` 또는 커스텀
- Undo 가능 시간: 5초 또는 사용자가 닫기 전까지

---

## 4. 확인(Confirmation) 패턴

### 4.1 언제 확인이 필요한가

| 액션 | 확인 필요? | 방식 |
|---|---|---|
| 텍스트 저장 | ❌ | Toast로 결과만 |
| 데이터 수정 | ❌ | 즉시 적용 + Toast |
| 단일 항목 삭제 | ✅ | `Modal Type: Popup` |
| 다수 항목 삭제 | ✅✅ | `Modal Type: Popup` + 항목 수 명시 + 입력 검증 |
| 결제/구매 | ✅ | `Modal Type: Info` |
| 권한 변경 | ✅ | `Modal Type: Popup` |
| 페이지 이탈 (저장 안 됨) | ✅ | `Modal Type: Popup` |

### 4.2 확인 Modal 표준

```
┌──────────────────────────────────────────┐
│  프로젝트를 삭제하시겠습니까?             │
├──────────────────────────────────────────┤
│                                          │
│  '프로젝트 알파'를 삭제하면              │
│  연결된 12개 작업도 함께 삭제됩니다.     │
│  이 작업은 되돌릴 수 없습니다.           │
│                                          │
├──────────────────────────────────────────┤
│              [취소]   [삭제]               │
└──────────────────────────────────────────┘
```

**필수 요소**
- 제목: 무엇을 확인하는지 (질문 형식)
- 본문: 무슨 일이 일어나는지 + 영향 범위
- 되돌릴 수 없는 액션은 명시
- Primary 액션 라벨: 일반적인 "확인"이 아니라 **실제 동작 명시** ("삭제", "전송", "결제")
- Destructive 액션의 Primary 버튼: `Button Color: Danger`

---

## 5. 호버/툴팁 패턴

### 5.1 Tooltip 표준 (BOS 4.0 `Tooltips` 컴포넌트)

| 상황 | Tooltip 사용 |
|---|---|
| 아이콘 단독 버튼 (`Icon only: true`) | ✅ 필수 |
| 잘린 텍스트 (truncate) | ✅ 풀 텍스트 표시 |
| 추가 설명 필요 | ✅ |
| 명확한 라벨 있는 버튼 | ❌ |

**Tooltip 규칙**
- 트리거: hover 후 `500ms` 지연
- 위치: 자동 (화면 경계 고려)
- 폰트: `text-xs`
- 배경: `bg-dark`, `text-white`
- 패딩: `spacing-2 spacing-3` (8px 12px)
- Border radius: `rounded`

### 5.2 Popover (BOS 4.0 `Popovers` 컴포넌트)

- 사용자/항목 클릭 시 미니 정보 카드
- 트리거: 클릭 (Tooltip과 다름)
- 위치: 트리거 옆
- 닫기: 외부 클릭 또는 Esc

---

## 6. 키보드 인터랙션

업무용 도구는 키보드 단축키가 생산성에 직결됩니다.

### 6.1 글로벌 단축키 (모든 화면)

| 키 | 동작 |
|---|---|
| `Cmd/Ctrl + K` | 글로벌 검색/커맨드 팔레트 |
| `Cmd/Ctrl + /` | 단축키 도움말 |
| `Esc` | Modal 닫기, 드롭다운 닫기 |
| `?` | 페이지별 단축키 가이드 |

### 6.2 폼 단축키

| 키 | 동작 |
|---|---|
| `Tab` | 다음 필드 |
| `Shift + Tab` | 이전 필드 |
| `Cmd/Ctrl + Enter` | 폼 제출 |
| `Esc` | 폼 닫기 (변경사항 있으면 확인) |

### 6.3 리스트/테이블 단축키

| 키 | 동작 |
|---|---|
| `↑/↓` | 행 이동 |
| `Enter` | 선택한 행 열기 |
| `Space` | 선택/해제 (체크박스 있을 때) |
| `Cmd/Ctrl + A` | 모두 선택 |
| `Delete` | 선택 항목 삭제 (확인 다이얼로그) |

---

## 7. 트랜지션 & 애니메이션

### 7.1 Duration 표준

| 변화 | Duration | Easing |
|---|---|---|
| Hover 효과 | `150ms` | `ease-out` |
| 색상/투명도 변경 | `200ms` | `ease-out` |
| Modal/Drawer 진입 | `250ms` | `ease-out` |
| Modal/Drawer 퇴장 | `200ms` | `ease-in` |
| 페이지 전환 | `300ms` | `ease-in-out` |
| Skeleton shimmer | `1500ms` | `ease-in-out` infinite |

### 7.2 원칙

- **빠르게 들어오고 빠르게 나간다**: 300ms 넘지 않기
- **들어올 때보다 나갈 때 빠르게**: 사용자 흐름 방해 최소화
- **불필요한 애니메이션 금지**: 장식이 아니라 정보 전달의 보조

---

## 8. 테이블 행 인터랙션 패턴

업무용 화면에서 가장 많이 쓰이는 패턴.

### 8.1 행 Hover 동작 표준

테이블 행에 hover 시 다음 3가지가 동시에 일어납니다:

```
1. 배경 변화      → bg-secondary 또는 bg-tertiary
2. 액션 노출      → 우측 끝의 수정/삭제 등 아이콘 버튼 페이드인
3. 커서 변화      → 행 클릭 가능하면 cursor: pointer
```

**트랜지션**
- 배경: `transition: background 150ms ease-out`
- 액션 페이드인: `transition: opacity 150ms ease-out`
- 액션 기본 상태: `opacity: 0`, hover 시 `opacity: 1`

### 8.2 행 액션 버튼

- **Button Color: Ghost, Icon only: true, Size: sm**
- Destructive: `Button Color: Danger, Outline: true, Icon only: true`
- 버튼 사이 간격: `spacing-1` (4px)

### 8.3 액션 노출 방식 — 3가지 옵션

| 방식 | 언제 |
|---|---|
| **Hover 시 노출** (기본) | 일반적인 상황 |
| **항상 노출** | 모바일/터치 환경, 액션 1개뿐, 접근성 우선 |
| **더보기 메뉴 (`⋯`)** | 액션 4개 이상 → BOS 4.0 `Dropdown menu` 활용 |

### 8.4 행 클릭 동작 규칙

- **행 클릭** → 디테일 페이지 진입 (Primary 액션)
- **행 내부 버튼/링크 클릭** → 해당 액션 실행 (이벤트 버블링 차단)
- **체크박스 클릭** → 선택만 (페이지 이동 없음)

```tsx
// ✅ Good — 이벤트 버블링 차단
<tr onClick={() => navigate(`/items/${item.id}`)}>
  <td>...</td>
  <td>
    <button onClick={(e) => { e.stopPropagation(); handleDelete(item.id); }}>
      삭제
    </button>
  </td>
</tr>
```

### 8.5 키보드 접근성

- `<tr tabindex="0">` 또는 `<a>`/`<button>` 안에 감싸기
- `:focus-within`으로 hover와 동일한 시각 피드백
- 키보드 단축키 (위 6.3 참조)

---

## 9. 빈 상태 / 에러 아이콘 시각 표준

### 9.1 BOS 4.0 `Icon shape` 컴포넌트 활용

BOS 4.0의 `Icon shape` 컴포넌트(84 variants)를 사용해 일관된 아이콘 컨테이너를 만듭니다.

```
┌─────────────────┐
│   ┌───────┐     │  ← Icon shape: size 64px
│   │   ◯   │     │     배경: 상태별 시멘틱 토큰
│   │       │     │
│   └───────┘     │  ← 아이콘: 28px
│                 │     색상: 상태별
└─────────────────┘
```

**상태별 매핑** (Icon shape의 색상 variant)

| 상태 | 컨테이너 배경 | 아이콘 색상 |
|---|---|---|
| Empty (일반) | `bg-secondary` | `text-fg-disabled` |
| Empty (성공/완료) | `bg-success-soft` | `text-fg-success` |
| Empty (브랜드/유도) | `bg-brand-softer` | `text-fg-brand` |
| Error | `bg-danger-soft` | `text-fg-danger` |
| Warning | `bg-warning-soft` | `text-fg-warning` |

### 9.2 아이콘 자체 규격

**선 굵기 (stroke-width)**: `1.6px`
**Stroke**:
```svg
stroke-width="1.6"
stroke-linecap="round"
stroke-linejoin="round"
fill="none"
```

### 9.3 텍스트 영역 표준

```
[Icon shape 64px]
       ↓ spacing-3 (12px)
   [제목] text-base font-medium, text-heading
       ↓ spacing-1 (4px)
   [설명] text-sm font-normal, text-body, max-width: 360px
       ↓ spacing-3 (12px)
   [CTA 버튼]
       ↓ spacing-2 (8px) — 선택
   [에러 코드 등 메타] text-xs font-normal, text-fg-disabled, monospace
```

### 9.4 영역 전체 패딩

빈 상태/에러 컨테이너의 패딩:
- 좌우: `spacing-6` (24px)
- 상하: `spacing-16` (64px)

좁은 영역 (사이드바 위젯 등):
- 좌우: `spacing-4` (16px)
- 상하: `spacing-8` (32px)
- 아이콘 컨테이너: `40px` 축소
- 아이콘: `20px` 축소

---

## 10. 빠뜨리지 않는 체크리스트

새 화면을 만들 때 반드시 자문할 것:

### 인터랙티브 요소
- [ ] BOS 4.0의 4가지 상태 (Initial/Hover/Focus/Disabled)가 모두 정의되었는가?
- [ ] focus ring이 키보드 사용자에게 보이는가? (`:focus-visible`)
- [ ] 비동기 액션은 Spinner 또는 Skeleton으로 표현되는가?

### 데이터 표시
- [ ] 데이터가 없을 때 (empty)는 어떻게 보이는가?
- [ ] 로딩 중일 때 (loading)는 어떻게 보이는가?
- [ ] 로드 실패할 때 (error)는 어떻게 보이는가?
- [ ] 검색 결과 없을 때는?
- [ ] 필터 결과 없을 때는?

### 액션 결과
- [ ] 사용자 액션에 300ms 이내 피드백이 있는가?
- [ ] 작업 완료 시 적합한 Toast 변형이 선택되었는가?
- [ ] 파괴적 액션에 Modal 확인 다이얼로그가 있는가?
- [ ] 가능한 경우 Undo가 제공되는가?

### 키보드 & 접근성
- [ ] 마우스 없이 모든 기능을 쓸 수 있는가?
- [ ] Esc로 Modal을 닫을 수 있는가?
- [ ] Enter로 폼을 제출할 수 있는가?

### 테이블 / 리스트 (해당 시)
- [ ] 행 hover 시 배경 변화 + 액션 노출이 있는가?
- [ ] 행 액션 클릭 시 이벤트 버블링이 차단되는가? (e.stopPropagation)
- [ ] 키보드로도 행 선택과 액션 실행이 가능한가? (focus-within)

### 빈 상태 / 에러 (해당 시)
- [ ] BOS 4.0 `Icon shape` 컴포넌트를 활용했는가?
- [ ] 시멘틱 토큰으로 색상 매핑이 되었는가?
- [ ] 일러스트레이션이나 이모지를 쓰지 않았는가?

### 다크 모드
- [ ] 모든 색상이 시멘틱 토큰으로 작성되었는가?
- [ ] 다크 모드에서도 시각적 위계가 유지되는가?

---

## 📝 변경 이력

- `2026-04-20` — 초기 버전 (구 Bos)
- `2026-04-20 (BOS 4.0)` — BOS 4.0의 4가지 상태로 단순화, Input field 7가지 상태 추가, Icon shape 컴포넌트 활용, Toast 9가지 변형과 매핑, 다크 모드 체크리스트 추가
- `2026-04-20 (1.3 추가)` — Selected/Active 상태 디자인 가이드라인 신설. 다크모드 색조 동조 효과 함정과 3가지 권장 패턴(진한 배경+흰 텍스트, 옅은 배경+brand 텍스트, 좌측 indicator) 정의
