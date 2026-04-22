# Lucide 아이콘 로드 — 환경별 보일러플레이트

> 이 문서는 BOS 4.0 디자인 시스템에서 **Lucide 아이콘을 어떻게 로드하는지**의 표준 코드입니다.
> 새 화면을 만들 때 환경에 맞는 보일러플레이트를 복사해서 사용하세요.

---

## 📌 환경 선택 가이드

| 환경 | 사용 케이스 | 보일러플레이트 |
|---|---|---|
| **CDN** | 사내 외부망 가능, 빠른 프로토타입 | §1 |
| **Self-host** | 폐쇄망, 금융/공공/방산, 오프라인 데모 | §2 |
| **하이브리드** | 같은 코드를 여러 환경에 배포 | §3 (권장) |

---

## §1. CDN 보일러플레이트 (외부망)

### HTML

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <!-- Lucide CDN — 버전 핀 (08-dependency-guardrails.md §2) -->
  <script src="https://unpkg.com/lucide@0.460.0/dist/umd/lucide.min.js"
          integrity="sha384-여기에_실제_해시_입력"
          crossorigin="anonymous"></script>
</head>
<body>
  <!-- 아이콘 마크업 -->
  <i data-lucide="users" aria-hidden="true"></i>

  <!-- 페이지 끝에서 한 번만 호출 -->
  <script>
    lucide.createIcons();
  </script>
</body>
</html>
```

### React (Vite/Next.js)

```bash
npm install lucide-react@0.460.0
```

```jsx
import { Users } from 'lucide-react';

export function Nav() {
  return (
    <a href="/users">
      <Users size={18} strokeWidth={1.8} aria-hidden="true" />
      회원 관리
    </a>
  );
}
```

---

## §2. Self-host 보일러플레이트 (폐쇄망)

### 셋업 (한 번만)

```bash
# 외부망 PC에서 라이브러리 다운로드
curl -O https://unpkg.com/lucide@0.460.0/dist/umd/lucide.min.js

# 디자인 시스템 레포 또는 사내 정적 서버에 배포
mkdir -p assets/vendor
mv lucide.min.js assets/vendor/lucide-0.460.0.min.js

# 예시 배포 경로:
# - 디자인 시스템 사내 서버: https://ds.archo.io/assets/vendor/lucide-0.460.0.min.js
# - 또는 각 서비스의 정적 자산: /static/vendor/lucide-0.460.0.min.js
```

### HTML

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <!-- Self-host: 사내 정적 서버 -->
  <script src="/assets/vendor/lucide-0.460.0.min.js"></script>
</head>
<body>
  <i data-lucide="users" aria-hidden="true"></i>

  <script>
    lucide.createIcons();
  </script>
</body>
</html>
```

### React (사내 npm 미러)

`.npmrc` 설정:
```
registry=https://npm.archo.io
```

```bash
npm install lucide-react@0.460.0
```

이후 사용은 §1 React와 동일.

---

## §3. 하이브리드 보일러플레이트 (가장 안전, 권장)

CDN 시도 → 실패 시 self-host로 폴백. 같은 코드가 외부망/폐쇄망 모두에서 동작.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <!-- 1차 시도: CDN -->
  <script src="https://unpkg.com/lucide@0.460.0/dist/umd/lucide.min.js"></script>

  <!-- 2차 폴백: self-host -->
  <script>
    if (typeof lucide === 'undefined') {
      document.write(
        '<script src="/assets/vendor/lucide-0.460.0.min.js"><\/script>'
      );
    }
  </script>
</head>
<body>
  <i data-lucide="users" aria-hidden="true"></i>

  <script>
    if (typeof lucide !== 'undefined') {
      lucide.createIcons();
    } else {
      console.error(
        'Lucide 로드 실패. CDN 및 /assets/vendor/ 경로 확인 필요.'
      );
    }
  </script>
</body>
</html>
```

---

## §4. AI 코드 생성 시 적용 규칙

`00-CLAUDE.md`의 보일러플레이트 규칙에 따라, Claude는 다음과 같이 자동 판단합니다:

```
사용자 요청에 [환경] 명시 여부 확인
   ↓
┌──────────────┬──────────────────────────────┐
│ [환경] 외부망 │ → §1 CDN 보일러플레이트     │
│ [환경] 폐쇄망 │ → §2 Self-host 보일러플레이트│
│ 명시 없음     │ → §3 하이브리드 보일러플레이트│
└──────────────┴──────────────────────────────┘
```

**사용자가 화면 요청 시 환경을 함께 명시하면 가장 정확:**

```
[패턴] List Page
[목적] 주문 목록
[환경] 폐쇄망          ← 이 한 줄이 §2 적용을 트리거
```

---

## §5. SRI 해시 생성 방법

CDN 사용 시 변조 방지를 위한 SRI 해시:

```bash
# macOS / Linux
curl -s https://unpkg.com/lucide@0.460.0/dist/umd/lucide.min.js | \
  openssl dgst -sha384 -binary | openssl base64 -A

# 결과 예시:
# 8wJh2Yw1YQ4zxLqJN3M9... (실제 해시값)
```

이 값을 `<script integrity="sha384-...">` 속성에 사용하세요.

---

## §6. 폐쇄망 검증 체크리스트

폐쇄망에 배포 전 확인:

- [ ] 외부 도메인(`unpkg.com`, `cdn.jsdelivr.net` 등) 호출 없음
- [ ] 모든 `<script src>`가 `'self'` 또는 사내 도메인 참조
- [ ] CSP 정책: `script-src 'self' https://ds.archo.io`로 충분
- [ ] 라이브러리 파일이 정적 서버에 배포되어 있고 접근 가능
- [ ] 버전 명시 경로 사용 (`lucide-0.460.0.min.js`)
- [ ] 같은 화면을 외부망 차단 환경에서 열어 아이콘 정상 표시 확인

---

## §7. 버전 업그레이드 절차

Lucide를 v0.460.0 → v0.480.0으로 올리고 싶다면:

1. **CHANGELOG 확인** — https://github.com/lucide-icons/lucide/releases
2. **breaking change 점검** — 아이콘 이름 변경, API 변경 여부
3. **외부망 PC에서 새 버전 다운로드**
   ```bash
   curl -O https://unpkg.com/lucide@0.480.0/dist/umd/lucide.min.js
   mv lucide.min.js lucide-0.480.0.min.js
   ```
4. **새 SRI 해시 생성** (§5 참조)
5. **테스트 환경에서 검증** — 모든 아이콘 정상 표시
6. **PR 생성** — `08-dependency-guardrails.md`의 표준 PR description 사용
7. **머지 후** — 사내 정적 서버에 새 버전 배포, 기존 버전은 일정 기간 유지

---

## 📝 변경 이력

- `2026-04-22` — 초기 버전. 환경별 3가지 보일러플레이트 정의.
