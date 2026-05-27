# Changelog

이 파일은 NSTAR 1.0 Design System의 주요 변경 사항을 기록합니다. 형식은 [Keep a Changelog](https://keepachangelog.com/) 1.1을 따릅니다.

---

## [2.1.0] — 2026-05-27

Figma → 코드 상세 가이드 신설. 변형 매트릭스 + UX 의도 + Do/Don't + 시나리오 레시피.

### Added

- **`docs/09-figma-component-guide.md`** (2,013 lines)
  - Figma Plugin API 로 113 COMPONENT_SET 의 variantGroupProperties + componentPropertyDefinitions 일괄 추출
  - 38 카테고리 × 변형 매트릭스 표 (Property × Values · variant 합계)
  - Component Props (BOOLEAN · TEXT · INSTANCE_SWAP) 카운트 + 명세
  - 사용 규칙 (ARIA · 키보드 · 시멘틱 HTML)
  - 10 핵심 컴포넌트(Button · Input · Alert · Modal · Toast · Tooltip · Dropdown · Card · Tabs · Stepper · Pagination · Table) 에 4-블록 UX 가이드:
    - 🎯 UX 의도 — 시선 흐름 · 상태 피드백 · 타격 영역 · 인지 패턴
    - ✅ 권장 (Do) — Vue 코드 예시 3-5개
    - ❌ 지양 (Don't) — 흔한 실수 + 대체안
    - 📐 추가 가이드 — 라벨 규칙 · 키보드 · 모바일 · ARIA 매핑
  - **부록 C — 전사 UX 권장/지양 요약** (22 Do + 22 Don't 항목)
  - **부록 D — 시나리오별 컴포넌트 조합 레시피** 4종:
    - 거래 상세 페이지 (Breadcrumbs → Header → Tabs → KV/Table/Stat/Timeline → Action)
    - 신규 등록 폼 모달 (Modal+Header+Form+Footer + Toast + Alert)
    - 데이터 목록 페이지 (Filter → 3상태 처리 → Table+Dropdown → Pagination)
    - T5 Step-up MFA + Maker-Checker 결재 5단계

### Changed

- `docs/00-CLAUDE.md` — 문서 라우팅 표에 09 항목 추가, 변경 이력에 한 줄 추가

---

## [2.0.0] — 2026-05-20

컴포넌트 라이브러리 v2 출시. 10 → 24 컴포넌트로 확장, React 기반 단일 HTML SPA로 재구축.

### Added

#### 신규 컴포넌트 15개

- **Avatar** — 사용자 식별. 이미지/이니셜/icon fallback
- **Icon shape** (84 variants) — 아이콘을 강조하는 컨테이너. Empty state, 강조 메뉴 등에 사용
- **Select** (84 variants) — 단일 선택 드롭다운. Input field와 같은 시각 언어 (Type × Size × State)
- **Checkbox/Radio** — 다중/단일 선택 + 그룹 패턴 10종
- **Toggle switch** (12 variants) — 즉시 반영되는 on/off
- **Range slider** (7 variants) — 연속 숫자 값 입력 (단일/범위). 가격 필터, 평점, 볼륨
- **Datepicker** — 날짜 / 범위 선택
- **Timepicker** (84 variants) — 시간 입력 매트릭스
- **Toast** (9 variants) — 일시적 결과 피드백
- **Drawer** (8 variants) — 화면 가장자리 슬라이드 패널
- **Tooltip** (4 variants) — 짧은 도움말
- **Empty state** — 데이터 없음 / 검색 결과 없음 / 에러
- **Stepper** (35+ variants) — 단계 진행 안내
- **Breadcrumbs** (6 variants) — 경로 표시
- **Misc** — KBD, Rating, Progress, Spinner, Skeleton, Carousel, Gallery, Timeline, Chat bubble

#### Foundations 5개 페이지

- **Colors** — 9개 컬러 램프 + 시멘틱 매핑 라이브 뷰
- **Typography** — Wanted Sans + Inter, 12 size step 카탈로그
- **Spacing** — 4px base 토큰 33개 시각 카탈로그
- **Radii & Shadows** — 모서리 9단계 + 그림자 6단계
- **Icons** — Lucide 아이콘 라이브러리 가이드

#### 신규 파일

- `site/components.html` — v2 라이브러리. 단일 HTML SPA. React + Babel + Lucide + Wanted Sans 모두 인라인 (외부 의존성 0)
- `CHANGELOG.md` — 이 파일

### Changed

- **`docs/03-component-usage.md`** — 핵심 10개 가이드 → 24 컴포넌트 + 5 Foundations 인덱스 + 핵심 10개 깊이 가이드로 재작성
- **`site/components.html`** — 기존 10개 컴포넌트 갤러리 → React SPA 컴포넌트 라이브러리로 교체. 5개 카테고리 29개 페이지를 hash 라우팅으로 탐색 (`#button`, `#input`, ...). 라이브러리 자체에 사이드바·라우터·테마 토글·검색이 모두 포함되어 별도 외피가 필요 없음
- **`README.md`** — 폴더 구조 단순화 (site/는 components.html 단일 파일), 빠른 시작에서 라이브러리 진입 방식 명확화, 최근 변경 이력 갱신
- **`docs/00-CLAUDE.md`** — "핵심 10개" → "24개" 표현 갱신, 변경 이력에 v2 출시 라인 추가

### Removed

- `site/index.html`, `site/tokens.html`, `site/patterns.html`, `site/shared.css`
  → v2 라이브러리(`components.html`)에 모든 콘텐츠가 단일 SPA로 통합되어 별도 진입점·토큰 카탈로그·패턴 페이지가 불필요해졌습니다. 라이브러리 안 `colors`, `type`, `spacing`, `radii`, `icons` Foundations 페이지가 기존 tokens.html을 대체하며, 레이아웃 패턴은 `docs/04-layout-patterns.md`에서 참조하세요.
- `site/components/` 폴더의 개별 컴포넌트 페이지 10개 (button.html / input.html / badge.html / card.html / alert.html / modal.html / dropdown.html / tabs.html / pagination.html / table.html)
  → v2 라이브러리로 통합. 더 풍부한 콘텐츠가 단일 SPA 안에 들어 있습니다.
- `bos4-app.html` (439KB SPA, 2026-05-20에 만든 단일 HTML 통합본)
  → v2 라이브러리가 대체합니다.

### Migration Guide

#### 외부에서 이전 컴포넌트 페이지를 링크하고 있던 경우

기존 URL 패턴:
```
site/components/button.html
site/components/input.html
...
```

이제는 v2 라이브러리 안에서 hash 라우팅으로 접근:
```
site/components.html#button
site/components.html#input
...
```

기존 외부 링크가 `site/components/button.html` 같은 경로를 가리키면 404가 됩니다. `site/components.html`로 안내한 뒤 사용자가 사이드바에서 컴포넌트를 선택하도록 유도하세요.

#### Project Knowledge 사용자

Claude Project Knowledge에 업로드한 파일 중 변경된 것은 다음 4개입니다 — 새 버전으로 교체하세요:

- `docs/00-CLAUDE.md`
- `docs/03-component-usage.md`
- `README.md`
- `CHANGELOG.md` (신규)

---

## [1.x] — 2026-04-20 ~ 2026-04-22

### Added (요약)

- BOS 4.0 디자인 시스템으로 전체 재구축 (Figma `2vaJ9nJK4k6RZ6Sw1aZ9Xp` → `jmK75D3yVgpYh0wHAlsAwy`)
- 시멘틱 토큰 + Tailwind 팔레트 시스템 도입 (Tier 1/2/3 구조)
- 다크 모드 정식 지원 (`data-theme` 속성으로 자동 전환)
- 컴포넌트 철학 변경: 목적별 → 속성 조합형
- 가이드 문서 9개: `00-CLAUDE.md`, `01-design-tokens.md`, ..., `08-dependency-guardrails.md`, `icon-loader-boilerplate.md`
- 489 토큰 (`tokens/bos4-design-tokens.css`)
- 정적 사이트 4개: `index.html`, `tokens.html`, `components.html` (v1: 10 컴포넌트), `patterns.html`
- 핵심 10개 컴포넌트 가이드 (Button / Input / Alert / Toast / Modal / Card / Badge / Dropdown / Navbar / Sidebar)
- 5가지 레이아웃 패턴 (List / Detail / Form / Dashboard / Auth)
- 한국어 UX 라이팅 가이드
- WCAG 2.1 AA 접근성 가이드
- 아이콘 로더 환경별 자동 선택 (외부망/폐쇄망/하이브리드)

### Changed (요약)

- 2026-04-21 — Figma Tailwind v4 팔레트 기준으로 코드 토큰 전수 동기화. Primitive 23개 hex 수정, 4개 추가, 시멘틱 토큰 9개 추가/수정
- 2026-04-20 — Navigation Active 표준화 (다크 모드 색조 동조 함정 해결)
- 2026-04-20 — Figma-코드 4단계 동기화 룰 명시
