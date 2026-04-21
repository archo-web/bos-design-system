# CLAUDE.md — BOS 4.0 Design System 프로젝트

> 이 문서는 모든 vibe coding 세션의 **진입점**입니다.
> Claude가 새 화면이나 컴포넌트를 만들기 전에 반드시 이 파일과 연결된 가이드 문서를 참조해야 합니다.

---

## 🎯 프로젝트 개요

**BOS 4.0**은 Flowbite 기반 + Tailwind 토큰 시스템을 차용한 광범위한 디자인 시스템입니다.
디자이너가 Figma에서 정의한 토큰과 컴포넌트가 코드로 자동 동기화되어, vibe coding 시 일관된 UI를 생성할 수 있도록 합니다.

- **Figma File Key**: `jmK75D3yVgpYh0wHAlsAwy`
- **Library**: BOS 4.0 design system
- **타이포그래피**: Wanted Sans (한국어) + Inter (영문 fallback)
- **그리드**: 4px 기반 (Tailwind 호환)
- **다크 모드**: Light / Dark 정식 지원

---

## 🆕 v3 → v4 (BOS4.0) 마이그레이션

이 가이드라인은 기존 **Bos team library** (`2vaJ9nJK4k6RZ6Sw1aZ9Xp`) 에서 **BOS 4.0** (`jmK75D3yVgpYh0wHAlsAwy`)로 전환됐습니다. 핵심 변화:

| 항목 | 구 Bos | BOS 4.0 |
|---|---|---|
| 컴포넌트 철학 | 목적별 (`button_add`, `button_delete`) | 속성 조합형 (`Color × Size × State`) |
| Button variant 수 | ~10개 | **500개** (8 Color × 5 Size × 4 State × 옵션) |
| 색상 시스템 | Primary Blue + 자체 Status | Tailwind 팔레트 + 시멘틱 토큰 (3-tier) |
| 다크 모드 | 미지원 (수동 파생) | **Light / Dark 모드 변수로 정식 지원** |
| Brand 색상 | `#3851DD` (PB500) | `colors/brand/500` = `#3851DD` (동일) |
| 컴포넌트 카테고리 | ~15개 | **47개+** (Flowbite 풀세트) |

→ Brand 컬러는 호환되지만, **나머지는 모두 새 토큰/컴포넌트로 작성**해야 합니다.

---

## 📚 문서 라우팅

vibe coding 작업 유형에 따라 아래 문서를 **반드시 먼저** 참조하세요.

| 작업 | 참조 문서 | 언제 |
|---|---|---|
| 새 화면/컴포넌트 만들기 | `design-principles.md` | 디자인 의사결정이 필요할 때 |
| 컴포넌트 선택/조합 | `component-usage.md` | 어떤 컴포넌트의 어떤 variant를 쓸지 정할 때 |
| 페이지 레이아웃 잡기 | `layout-patterns.md` | 새 페이지 구조 설계 시 |
| 상태/인터랙션 정의 | `interaction-patterns.md` | hover/loading/empty/error 등 정의 시 |
| 한국어 문구 작성 | `ux-writing.md` | 라벨, 메시지, 안내 문구 작성 시 |
| 웹 접근성 준수 | `accessibility.md` | 모든 구현 작업 (WCAG AA 기준) |
| 디자인 토큰 (색상/간격/폰트) | `bos4-design-tokens.css` | 모든 스타일링 작업 |

---

## ⚡ 핵심 원칙 (TL;DR)

Claude는 코드를 생성할 때 다음을 **항상** 따릅니다.

1. **시멘틱 토큰 우선** — 원시 토큰(`colors/gray/900`)이 아니라 시멘틱 토큰(`colors/text/text-heading`)을 사용. 다크 모드 자동 대응.
2. **컴포넌트 variant 매트릭스 준수** — Color/Size/State 조합은 정해진 값만 사용. 임의 조합 금지.
3. **상태 누락 금지** — 인터랙티브 요소는 `Initial / Hover / Focus / Disabled` 4가지 상태를 모두 정의 (BOS 4.0의 표준)
4. **빈/로딩/에러 상태 필수** — 데이터를 표시하는 모든 화면에 3가지 상태 정의
5. **한국어 우선, 명확한 문구** — UX 라이팅은 짧고 행동 지향적으로 (예: "저장됨" ✓, "성공적으로 저장이 완료되었습니다" ✗)

---

## 🔄 Figma 동기화 워크플로우

토큰/컴포넌트가 변경되었을 때:

```
사용자: "피그마 업데이트 반영해줘"
↓
Claude:
  1. figma.use_figma → 로컬 변수 컬렉션 조회 (Themes / Primitives)
  2. figma.search_design_system → 컴포넌트 변경 확인
  3. bos4-design-tokens.css 업데이트 (Light/Dark 모드 모두)
  4. component-usage.md의 영향 범위 점검
  5. 변경 요약 보고
```

**중요**: BOS 4.0은 다크 모드를 정식 지원하므로, 토큰 동기화 시 **항상 Light/Dark 두 모드 값을 함께** 가져와야 합니다.

### 토큰 변경 시 4단계 동기화 룰

토큰 하나를 바꾸려면 아래 **네 곳을 모두 동시에 챙겨야** 합니다. 한 곳만 빠뜨리면 디자인-코드 불일치가 발생합니다.

```
Figma 변수 (Themes 컬렉션)
    ↓
tokens/bos4-design-tokens.css (단일 진실 공급원)
    ↓
site/*.html — inline된 토큰 (4개 파일 모두)
    ↓
Figma "Publish library" — 다른 Figma 파일에 변경사항 전파 (수동)
```

**놓치기 쉬운 부분**
- HTML 4개 파일에 토큰이 `<style>` 안에 inline돼 있어요. `site/shared.css`만 바꾸면 안 됩니다.
- Figma Publish는 Plugin API로 안 되니 **사람이 직접** 해야 합니다.

### 정기 검증 — Figma-코드 일치 점검

세션 시작 시 또는 큰 작업 전에, **Figma 실제 값과 코드 토큰 CSS 값이 일치하는지** 한 번 점검하면 좋습니다. 과거에 어긋남이 발견된 적 있어요 (`text-fg-brand-strong`의 Dark mode 매핑이 Figma는 `brand/100`, 코드는 `brand/400`이었음).

빠른 검증 방법:
```js
// figma.use_figma 안에서
const themes = (await figma.variables.getLocalVariableCollectionsAsync())
  .find(c => c.name === 'Themes');
// 의심되는 토큰의 Light/Dark mode 값 추출 → 코드 CSS와 비교
```

---

## 🧭 Vibe Coding 시 Claude의 의사결정 순서

새 화면을 요청받으면 Claude는 이 순서로 사고합니다:

```
1. 무엇을 만드는가? (페이지 / 컴포넌트 / 패턴)
   ↓
2. design-principles.md의 어떤 원칙이 적용되는가?
   ↓
3. layout-patterns.md의 패턴 중 어떤 것인가?
   (List / Detail / Form / Dashboard / Auth)
   ↓
4. component-usage.md에서 사용할 컴포넌트의 variant 결정
   - Color: Brand / Secondary / Danger / ...?
   - Size: sm / base / lg?
   - Type: Default / Outline / Icon only?
   ↓
5. 어떤 시멘틱 토큰을 쓸 것인가? (bos4-design-tokens.css)
   ↓
6. interaction-patterns.md 기준으로 4가지 상태 + 빈/로딩/에러 정의
   ↓
7. ux-writing.md 기준으로 모든 문구 검토
   ↓
8. accessibility.md 체크리스트 점검 (WCAG AA, 키보드, ARIA, 대비비)
   ↓
9. 코드 생성
```

---

## 🚫 절대 하지 말 것

- ❌ Tailwind 임의 색상 클래스 사용 (`bg-blue-500` → `bg-[var(--colors-background-bg-brand)]`)
- ❌ 원시 토큰 직접 사용 — 시멘틱 토큰 사용 (`colors/gray/900` ✗ → `colors/text/text-heading` ✓)
- ❌ Figma에 없는 새로운 시각 스타일 임의 도입 (그라데이션, 새 색상, 새 폰트)
- ❌ 컴포넌트 variant를 임의 조합 (정의된 매트릭스 외 조합 금지)
- ❌ 인라인 스타일로 토큰 우회 (`style={{ color: '#3851DD' }}` 금지)
- ❌ `lorem ipsum` — 한국어 더미 텍스트 사용 (예: "프로젝트 알파", "홍길동")
- ❌ 다크 모드 무시 — 모든 컴포넌트는 Light/Dark 모두 동작해야 함
- ❌ 접근성 무시 — `<button>` 대신 `<div onClick>` 금지, 아이콘 버튼에 `aria-label` 누락 금지, `<label>` 없는 폼 필드 금지 (상세: `accessibility.md`)
- ❌ `outline: none` — focus ring 제거 금지 (키보드 사용자 차단). `:focus-visible` 사용

---

## ⚠️ BOS 4.0 작업 시 주의 사항

**컴포넌트가 47개 카테고리, 변형이 수백 개**인 광범위한 시스템입니다. Claude는 다음을 유의합니다:

1. **모든 컴포넌트를 외우려 하지 않음** — `figma.search_design_system`을 적극적으로 사용
2. **자주 쓰이는 핵심 10개 컴포넌트는 `component-usage.md`에 인덱스화** — 나머지는 필요할 때 검색
3. **variant 매트릭스가 클수록 신중하게 선택** — Button은 500개 variant가 있으니 "Color/Size/State"를 명확히 정해야 함
4. **다크모드 색조 동조 함정** — 같은 hue 계열 배경+텍스트는 다크모드에서 묻혀 보입니다. Active/Selected 상태는 흰 텍스트 또는 좌측 indicator 사용. 상세: `05-interaction-patterns.md §1.3`
5. **접근성은 구현 단계에서 체크** — 디자인만 이쁘게 만들고 끝이 아님. 모든 인터랙티브 요소는 키보드 접근 가능해야 하고, 폼 필드는 label 연결, 아이콘 버튼은 aria-label 필수. 상세: `07-accessibility.md`

---

## 📝 변경 이력

- `2026-04-20` — 초기 버전 (구 Bos 라이브러리 기반)
- `2026-04-20 (v2)` — Tier 2 문서 추가, StatusDot/Avatar 패턴
- `2026-04-20 (v4 - BOS 4.0)` — **BOS 4.0 디자인 시스템으로 전체 재구축**
  - Figma file: `2vaJ9nJK4k6RZ6Sw1aZ9Xp` → `jmK75D3yVgpYh0wHAlsAwy`
  - 시멘틱 토큰 + Tailwind 팔레트 시스템 도입
  - 다크 모드 정식 지원
  - 컴포넌트 철학 변경: 목적별 → 속성 조합형
- `2026-04-20 (동기화 워크플로우 보강)` — Figma-코드 4단계 동기화 룰 명시 (Figma → CSS → HTML inline → Publish). 정기 검증 방법 추가
- `2026-04-20 (접근성 가이드 추가)` — `07-accessibility.md` 신설. WCAG 2.1 AA 기준 실무 가이드. 시멘틱 토큰 대비비 검증, 키보드/ARIA/폼 접근성, 한국어 특수사항, 개발자 체크리스트 포함. 문서 라우팅·의사결정 순서·금지 사항에도 접근성 반영
- `2026-04-21 (Figma 토큰 전수 동기화)` — Figma Tailwind v4 팔레트 기준으로 코드 토큰 전면 업데이트. Primitive 23개 hex 수정 (emerald/rose/indigo/pink/purple/teal/cyan/sky), 4개 추가 (emerald-400, emerald-950, cyan-600, lime-500/600), 시멘틱 토큰 9개 추가·수정 (text-fg-yellow/purple/cyan/indigo/pink/lime, border-purple/orange, text-fg-success Dark). 6개 파일 모두 동기화 (tokens.css + shared.css + 4 HTML). Figma와 100% 일치 확인
