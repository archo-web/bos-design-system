# 08-dependency-guardrails.md — 외부 의존성 가드레일

> 이 문서는 BOS 4.0 디자인 시스템에 **외부 라이브러리/CDN을 추가**할 때 따라야 할 가드레일을 정의합니다.
> 작은 한 줄 추가가 보안·인프라·안정성 사고로 이어질 수 있어요. 이 문서는 그걸 막기 위한 합의입니다.

---

## 🎯 왜 가드레일이 필요한가

디자인 시스템에 라이브러리 한 줄을 추가하는 건 단순한 코드 변경이 아니라, **이 시스템을 쓰는 모든 서비스의 보안·운영 결정**이에요.

예: Lucide CDN을 한 줄 추가했을 때 일어날 수 있는 일

```html
<script src="https://unpkg.com/lucide@latest"></script>
```

- 🔒 **보안팀**: "이 도메인 화이트리스트 안 되어 있는데요?"
- 🏢 **금융/공공 서비스**: "내부망이라 외부 CDN 못 씁니다"
- 💥 **어느 날 갑자기**: 라이브러리가 v1.0으로 메이저 업데이트 → 전체 화면 깨짐
- 🦠 **공급망 공격**: 누가 npm 계정 탈취 → 자동으로 악성코드 다운로드

이 문서는 위 4가지를 **사전에** 점검하는 체크리스트예요.

---

## 📋 외부 의존성 추가 시 체크리스트

새 라이브러리/CDN을 도입하기 전에 다음을 모두 확인하세요.

### 1. 라이센스 확인

- [ ] **허용 라이센스 사용**: MIT, ISC, Apache 2.0, BSD 등
- [ ] **금지 라이센스 회피**: GPL, AGPL (상업 서비스에 부적합한 경우)
- [ ] 라이센스 파일을 레포에 동봉 (필요 시)

### 2. 버전 핀 (Version Pinning)

- [ ] CDN URL에 **정확한 버전 명시** (`@latest` 금지)
- [ ] `package.json`에서 캐럿(`^`) 대신 정확한 버전 사용
- [ ] 가능하면 **SRI 해시(integrity)** 추가

### 3. 보안 정책 (CSP)

- [ ] 적용 대상 서비스의 CSP 정책 확인
- [ ] 외부 CDN 도메인 화이트리스트 필요 여부 확인
- [ ] 보안팀에 사전 공유

### 4. 네트워크 환경 대응

- [ ] 폐쇄망/내부망 환경에서도 동작하는지 검토
- [ ] 오프라인 환경 대응 전략 (self-host 또는 폴백)
- [ ] 사내 프록시 통과 여부 확인

### 5. 영향 범위

- [ ] 번들 크기 증가량 측정
- [ ] tree-shaking 가능 여부
- [ ] 기존 의존성과 충돌 여부

---

## 🛡 가드레일 1: 라이센스

### 허용 라이센스

| 라이센스 | 사용 가능 여부 | 비고 |
|---|---|---|
| MIT | ✅ | 가장 일반적, 제약 거의 없음 |
| ISC | ✅ | MIT와 거의 동일 |
| Apache 2.0 | ✅ | 특허 보호 조항 추가 |
| BSD-2/3-Clause | ✅ | MIT와 유사 |

### 검토 필요 라이센스

| 라이센스 | 검토 사항 |
|---|---|
| GPL-2.0/3.0 | ⚠️ 동일 라이센스로 공개 배포 의무 → 상업 서비스는 법무 검토 |
| AGPL | ⚠️ 네트워크로 제공해도 공개 의무 → 사실상 사용 불가 |
| Commons Clause | ⚠️ 상업 사용 제한 가능 → 라이센스 본문 확인 |
| Custom/Proprietary | ⚠️ 무조건 법무 검토 |

### 라이센스 확인 방법

```bash
# npm 패키지의 라이센스 확인
npm view lucide-react license
# > "ISC"

# 전체 의존성 트리의 라이센스 검사
npx license-checker --summary
```

---

## 🛡 가드레일 2: 버전 핀 (Version Pinning)

### 왜 핀이 필요한가

```html
<!-- ❌ 위험: @latest -->
<script src="https://unpkg.com/lucide@latest/dist/umd/lucide.min.js"></script>
```

`@latest`의 위험:
- **메이저 업데이트 시 깨짐**: 어느 날 v1.0이 나오면서 API가 바뀌면 코드 한 줄 안 건드렸는데 화면이 깨짐
- **마이너 변경의 영향**: 아이콘 이름이 바뀌는 경우도 있음 (`more-vertical` → `ellipsis-vertical`)
- **재현 불가능한 버그**: "어제는 됐는데 오늘은 안 돼요" 디버깅 지옥
- **공급망 공격(Supply Chain Attack)**: npm 계정 탈취로 악성코드 배포되면 자동 다운로드

### 올바른 버전 핀

**CDN 사용 시**

```html
<!-- ✅ 좋음: 정확한 버전 명시 -->
<script src="https://unpkg.com/lucide@0.460.0/dist/umd/lucide.min.js"></script>

<!-- ✅ 더 좋음: SRI 해시까지 (변조 방지) -->
<script
  src="https://unpkg.com/lucide@0.460.0/dist/umd/lucide.min.js"
  integrity="sha384-실제해시값"
  crossorigin="anonymous">
</script>
```

SRI 해시 생성 방법:
```bash
curl -s https://unpkg.com/lucide@0.460.0/dist/umd/lucide.min.js | \
  openssl dgst -sha384 -binary | openssl base64 -A
```

**npm 사용 시 — `package.json`**

```json
{
  "dependencies": {
    "lucide-react": "0.460.0"      // ✅ 정확한 버전
    // "lucide-react": "^0.460.0"   // ⚠️ 마이너까지 자동 업데이트
    // "lucide-react": "*"           // ❌ 절대 금지
  }
}
```

**`package-lock.json` / `yarn.lock` / `pnpm-lock.yaml` 커밋 필수**
잠금 파일이 있어야 모든 환경에서 동일한 버전이 설치됩니다.

### 버전 업그레이드 정책

- **자동 업데이트 금지** — Renovate Bot이나 Dependabot으로 PR만 자동 생성, 머지는 수동
- **3~6개월 정기 점검** — CHANGELOG 확인 → 테스트 → 핀 변경 → PR
- **메이저 업그레이드는 별도 PR** — 마이그레이션 가이드 작성 필수

---

## 🛡 가드레일 3: CSP (Content Security Policy)

### CSP란

브라우저에 "이 사이트에서는 이런 출처의 스크립트/스타일/이미지만 허용해"라고 선언하는 보안 헤더예요. XSS 공격을 막는 1차 방어선입니다.

### 외부 CDN을 추가하면 무엇이 깨지나

흔한 CSP 설정:
```http
Content-Security-Policy: script-src 'self' https://www.googletagmanager.com;
```

이 상태에서 `unpkg.com` 스크립트를 불러오면 **브라우저가 차단**합니다. 콘솔에 빨간 에러가 뜨고 라이브러리 자체가 로드되지 않아요.

⚠️ **개발자 본인 노트북에서는 CSP 안 걸려있어 문제가 안 보입니다.** 스테이징/프로덕션에 올라가는 순간 사고가 발생하는 클래식한 패턴.

### 해결 방법 3가지 (강한 순)

**옵션 1: 화이트리스트 추가** (가장 간단)
```http
Content-Security-Policy: script-src 'self' https://unpkg.com;
```

**옵션 2: SRI 해시로 특정 버전만 허용** (보안 강화)
```http
Content-Security-Policy: script-src 'self' 'sha384-실제해시값';
```

**옵션 3: Self-host로 전환** (가장 안전, 권장)
```http
# CDN 안 쓰면 'self'만으로 충분
Content-Security-Policy: script-src 'self';
```

### CSP 확인 방법

브라우저 개발자 도구 → Network 탭 → 메인 문서 응답 헤더에서 `Content-Security-Policy` 확인.

또는 사내 보안팀에 직접 문의.

---

## 🛡 가드레일 4: 네트워크 환경

### 외부 CDN 접근 불가 환경

| 환경 | 상황 |
|---|---|
| 폐쇄망/내부망 | 금융, 공공, 방산. 외부 인터넷 자체가 차단 |
| 오프라인 데모 | 박람회, 비행기 안, VPN 끊김 |
| 중국 시장 | unpkg, jsdelivr가 자주 느리거나 차단 |
| 사내 프록시 | 모든 외부 요청이 사내 프록시를 거쳐야 함 |
| 일부 회사 와이파이 | unpkg 등 차단되어 있는 경우 있음 |

이런 환경에서 CDN에 의존하면 **앱이 부팅 자체를 못 합니다.** 더 나쁜 건 라이브러리가 정의되지 않은 상태에서 후속 JS가 줄줄이 멈추는 거예요.

### 해결 방법

**방법 A: 폴백 전략** (CDN 우선, 실패 시 self-host)

```html
<!-- 1차: CDN 시도 -->
<script src="https://unpkg.com/lucide@0.460.0/dist/umd/lucide.min.js"></script>

<!-- 2차: 실패 시 self-host로 폴백 -->
<script>
  if (typeof lucide === 'undefined') {
    document.write('<script src="/assets/vendor/lucide.min.js"><\/script>');
  }
</script>
```

**방법 B: Self-host 통일** (가장 깔끔, 권장)

빌드 시 npm 패키지에서 라이브러리 파일을 추출해 `/assets/vendor/`에 복사:

```html
<script src="/assets/vendor/lucide.min.js"></script>
```

장점:
- 외부 의존 0
- 캐시 통제 가능
- CSP 단순(`'self'`만 허용해도 됨)
- 내부망에서도 동작

단점:
- 빌드 파이프라인에 한 단계 추가됨

### 우리 시스템의 권장 방식

> **권장**: Self-host. CDN은 데모/프로토타입 용으로만 사용.

이 디자인 시스템을 쓰는 서비스 중에 **하나라도** 폐쇄망/내부망이 있다면 처음부터 self-host로 통일하세요. 가이드 문서에서도 self-host를 기본으로 안내하세요.

---

## 📝 PR description에 넣을 표준 문구

외부 의존성을 추가하는 PR이라면 description 끝에 아래 단락을 복사해서 채워주세요:

```markdown
## ⚠️ 보안/인프라팀 확인 필요 사항

이 변경은 외부 라이브러리/CDN(`__라이브러리명__`)에 대한 의존성을 추가합니다.
머지 전 다음을 확인해주세요:

- [ ] **라이센스**: __MIT/ISC/Apache 2.0__ (허용 라이센스 확인)
- [ ] **버전 핀**: `@latest` 대신 정확한 버전(`@x.y.z`)으로 고정
- [ ] **CSP 정책**: `script-src` 디렉티브에 `https://__cdn-domain__` 허용 또는 self-host 전환 검토
- [ ] **폐쇄망 환경**: 적용 대상 서비스 중 외부 CDN 접근 불가 환경 점검
- [ ] **SRI 적용**: 가능하면 `integrity` 속성으로 변조 방지

대안: 처음부터 self-host로 가이드를 통일하면 위 4가지가 한 번에 해결됩니다.
검토 의견 부탁드립니다.
```

---

## 🚫 절대 하지 말 것

- ❌ `@latest` 또는 `*`로 버전 명시 — 깨짐의 99% 원인
- ❌ `package-lock.json` `.gitignore`에 추가 — 환경별 버전 불일치 발생
- ❌ "내 노트북에서는 되는데" 로 머지 — CSP/내부망 사고 반복
- ❌ 보안팀 확인 없이 새 외부 도메인 도입
- ❌ 라이센스 확인 없이 도입 — GPL/AGPL이면 추후 제거 비용 큼

---

## 📚 참고

- MDN — [Content Security Policy 가이드](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
- MDN — [Subresource Integrity (SRI)](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity)
- npm — [Semantic Versioning](https://docs.npmjs.com/about-semantic-versioning)
- [SPDX License List](https://spdx.org/licenses/) — 라이센스 식별자 표준
- [licensee.js](https://github.com/jslicense/licensee.js) — 자동 라이센스 검사 도구

---

## 📝 변경 이력

- `2026-04-22` — 초기 버전. Lucide CDN 도입을 계기로 외부 의존성 가드레일 정립.
  - 라이센스, 버전 핀, CSP, 네트워크 환경의 4가지 가드레일 정의
  - PR description 표준 문구 추가
  - 우리 시스템 권장 방식: self-host
