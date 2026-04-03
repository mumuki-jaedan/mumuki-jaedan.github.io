# 배포 방식 비교: GitHub Pages vs Cloudflare Pages

> **작성일**: 2026-04-03  
> **목적**: r2bit.com(GitHub Pages) 방식과 Cloudflare Pages 방식을 비교하여 mumuki.net 배포 전략 확정  
> **참고 사례**: r2bit.com → github.com/bit2r

---

## 1. 현황 조사

### 1.1 r2bit.com (GitHub Pages 방식) 실제 구성

```
DNS 조회 결과:
  NS:    bit2r.github.io.
  A:     185.199.108.153 ~ 185.199.111.153 (GitHub Pages IP)
  CNAME: www.r2bit.com → bit2r.github.io.
  서버:   GitHub.com (Varnish CDN, ICN 엣지 서빙)
```

| 항목 | 값 |
|------|------|
| GitHub Org | `github.com/bit2r` |
| Pages 리포 | `bit2r/bit2r.github.io` |
| 커스텀 도메인 | `r2bit.com` |
| 호스팅 | GitHub Pages |
| CDN | Fastly (Varnish), 한국 ICN 엣지 |
| SSL | GitHub 자동 (Let's Encrypt) |
| 서버 함수 | 없음 |

**구성 방법:**

1. `bit2r` GitHub Organization 생성
2. `bit2r.github.io` 리포 생성 (Organization Pages)
3. `r2bit.com` 도메인 구매 (외부 등록기관)
4. DNS 설정: A 레코드 → GitHub Pages IP 4개 + CNAME(www → bit2r.github.io)
5. GitHub 리포 Settings → Pages → Custom domain: `r2bit.com`
6. HTTPS 자동 활성화

### 1.2 mumuki.net 현재 상태

```
DNS 조회 결과:
  mumuki.net → NXDOMAIN (미등록)
```

| 항목 | 현재 |
|------|------|
| 도메인 | **미등록** (구매 필요) |
| GitHub 리포 | `statkclee/mumuki.net` (Private) |
| GitHub Org | 없음 (개인 계정) |
| 빌드 시스템 | Quarto 1.9.16 |
| 배포 | 미설정 |

---

## 2. 두 방식 상세 비교

### 2.1 아키텍처 비교

**방식 A: GitHub Pages (r2bit.com 방식)**

```
┌──────────┐     push      ┌─────────────────┐     serve     ┌──────────────┐
│ 로컬     │ ────────────→ │ GitHub Repo      │ ────────────→ │ GitHub Pages │
│ quarto   │               │ mumuki.github.io │               │              │
│ render   │               │                  │               │ mumuki.net   │
│          │               │ docs/ 또는       │               │ Fastly CDN   │
│          │               │ gh-pages 브랜치  │               │ (한국 엣지)   │
└──────────┘               └─────────────────┘               └──────────────┘

DNS: mumuki.net → A 185.199.108~111.153 (GitHub IPs)
```

**방식 B: Cloudflare Pages**

```
┌──────────┐     push      ┌────────────┐   webhook   ┌────────────────────┐
│ 로컬     │ ────────────→ │ GitHub     │ ──────────→ │ Cloudflare Pages   │
│ (소스만  │               │ Private    │             │                    │
│  push)   │               │ Repo       │   빌드+배포  │ mumuki.net         │
│          │               │            │ ──────────→ │ Cloudflare CDN     │
│          │               │            │             │ (300+ PoP)         │
│          │               │            │             │ + Workers (함수)    │
└──────────┘               └────────────┘             └────────────────────┘

DNS: mumuki.net → Cloudflare 네임서버 (자동 관리)
```

### 2.2 항목별 비교

| 항목 | GitHub Pages (A) | Cloudflare Pages (B) | 비고 |
|------|:-:|:-:|------|
| **비용** | ₩0 | ₩0 | 동일 |
| **대역폭** | 100GB/월 | **무제한** | B 우위 |
| **빌드** | GitHub Actions 필요 | **서버 빌드** 또는 Actions | B가 유연 |
| **빌드 횟수** | 무제한 (Actions 2000분/월) | 500회/월 | A 우위 (빈번한 빌드 시) |
| **CDN** | Fastly (한국 ICN) | **Cloudflare (한국 ICN)** | 동급 |
| **HTTPS** | 자동 (Let's Encrypt) | 자동 (Universal SSL) | 동일 |
| **커스텀 도메인** | O | O | 동일 |
| **프리뷰 배포** | X (별도 설정 필요) | **PR별 자동 프리뷰 URL** | B 우위 |
| **서버 함수** | **X** | **Workers (무료 10만/일)** | B 결정적 우위 |
| **Private 리포** | 지원 (무료) | 지원 | 동일 |
| **DNS 관리** | 외부 등록기관 | **Cloudflare 통합** | B 편리 |
| **DDoS 방어** | GitHub 자체 | **Cloudflare 엔터프라이즈급** | B 우위 |
| **설정 난이도** | ★★☆ (간단) | ★★★ (약간 복잡) | A 우위 |
| **생태계 성숙도** | ★★★★★ | ★★★★☆ | A 약간 우위 |

### 2.3 mumuki.net 특수 요건 대입

| 요건 | GitHub Pages | Cloudflare Pages | 판정 |
|------|:-:|:-:|:-:|
| 1차년: 정적 사이트 | O | O | 무승부 |
| 2차년: 토스페이먼츠 결제 | **X** (별도 서버 필요) | **Workers로 해결** | **B** |
| 3차년: 가상계좌 매칭 API | **X** | **Workers로 해결** | **B** |
| 기부 캠페인 트래픽 급증 | 100GB 제한 | **무제한** | **B** |
| 회계 데이터 보안 | 동일 | 동일 | 무승부 |
| 설정 간편함 | **간단** | 보통 | **A** |

---

## 3. 방식별 구축 절차

### 3.1 방식 A: GitHub Pages (r2bit.com 방식)

**Step 1: 도메인 구매**
```
등록기관: Namecheap, Google Domains, 가비아 등
도메인: mumuki.net
비용: ~₩15,000/년
```

**Step 2: GitHub Organization 생성 (선택)**
```
Organization: mumuki
리포: mumuki/mumuki.github.io (또는 개인 계정 유지)
```

**Step 3: GitHub Actions 워크플로우**
```yaml
# .github/workflows/deploy.yml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: quarto-dev/quarto-actions/setup@v2
        with:
          version: "1.9.16"
      - run: quarto render
      - uses: actions/upload-pages-artifact@v3
        with:
          path: docs

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

**Step 4: DNS 설정 (도메인 등록기관에서)**
```
A     @      185.199.108.153
A     @      185.199.109.153
A     @      185.199.110.153
A     @      185.199.111.153
CNAME www    statkclee.github.io.  (또는 mumuki.github.io.)
```

**Step 5: GitHub Settings**
```
Repository → Settings → Pages
  Source: GitHub Actions
  Custom domain: mumuki.net
  ✅ Enforce HTTPS
```

**Step 6: CNAME 파일 생성**
```
# 프로젝트 루트에 CNAME 파일 생성
echo "mumuki.net" > CNAME
```

**총 소요 시간: ~30분** (DNS 전파 제외)

---

### 3.2 방식 B: Cloudflare Pages

**Step 1: 도메인 구매**
```
Cloudflare Registrar에서 직접 구매 가능 (원가 제공, 마진 없음)
또는 외부 등록기관에서 구매 후 Cloudflare DNS로 이관
```

**Step 2: Cloudflare 계정 및 도메인 추가**
```
1. dash.cloudflare.com 가입
2. "Add a site" → mumuki.net
3. Free 플랜 선택
4. 네임서버 변경 (도메인 등록기관에서):
   - NS: xxx.ns.cloudflare.com
   - NS: yyy.ns.cloudflare.com
```

**Step 3: GitHub Actions + Cloudflare Pages 배포**
```yaml
# .github/workflows/deploy.yml
name: Deploy to Cloudflare Pages
on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      deployments: write
    steps:
      - uses: actions/checkout@v4
      - uses: quarto-dev/quarto-actions/setup@v2
        with:
          version: "1.9.16"
      - run: quarto render
      - uses: cloudflare/wrangler-action@v3
        with:
          apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          accountId: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          command: pages deploy docs --project-name=mumuki-net
```

**Step 4: 커스텀 도메인 연결**
```
Cloudflare Dashboard → Pages → mumuki-net → Custom domains
→ mumuki.net 추가 (DNS 자동 설정)
```

**총 소요 시간: ~45분** (DNS 전파 제외)

---

## 4. 비용 비교

### 1차년 (정적 사이트)

| 항목 | GitHub Pages | Cloudflare Pages |
|------|:-:|:-:|
| 호스팅 | ₩0 | ₩0 |
| CDN/SSL | ₩0 | ₩0 |
| CI/CD | ₩0 | ₩0 |
| 도메인 | ~₩15,000 | ~₩12,000 (Cloudflare Registrar) |
| **합계** | **~₩15,000/년** | **~₩12,000/년** |

### 2~3차년 (결제 연동 필요 시)

| 항목 | GitHub Pages | Cloudflare Pages |
|------|:-:|:-:|
| 호스팅 | ₩0 | ₩0 |
| 서버 함수 | **별도 필요** | Workers ₩0 (10만 요청/일) |
| 결제 API 서버 | AWS Lambda ~₩30,000/년 또는 Vercel ₩0 | **₩0** (Workers) |
| **합계** | **~₩45,000/년** | **~₩12,000/년** |

---

## 5. 추천

### 단기 (지금 ~ 런칭): GitHub Pages 방식 A

**이유:**
- 설정이 간단하고 r2bit.com에서 검증된 방식
- 이미 GitHub에 리포가 있으므로 바로 적용 가능
- 1차년에는 정적 사이트만 필요 → 서버 함수 불필요
- Quarto 공식 GitHub Actions 지원

### 중기 (2차년 결제 연동 시): Cloudflare Pages로 전환

**이유:**
- 토스페이먼츠 연동 시 서버 사이드 로직 필요 → Workers
- 전환 비용 낮음 (GitHub Actions 워크플로우만 변경)
- DNS를 Cloudflare로 이관하면 자동 완료

### 전환 용이성

```
GitHub Pages → Cloudflare Pages 전환 시:

변경 사항:
1. DNS 네임서버 → Cloudflare로 변경 (~5분)
2. GitHub Actions deploy.yml 수정 (~10분)
3. Cloudflare Pages 프로젝트 생성 (~5분)
4. GitHub Pages 비활성화 (~1분)

변경 없는 사항:
- GitHub 리포 (동일)
- 소스 코드 (동일)
- Quarto 빌드 설정 (동일)
- 도메인 (동일)
```

---

## 6. 최종 실행 계획

```
┌─────────────────────────────────────────────────────────┐
│  Phase 4 (지금)                                          │
│                                                          │
│  1. mumuki.net 도메인 구매 (가비아 또는 Cloudflare)       │
│  2. GitHub Pages 활성화 (GitHub Actions)                  │
│  3. DNS 설정 → mumuki.net 라이브                         │
│                                                          │
│  ※ r2bit.com과 동일한 검증된 방식으로 빠르게 런칭         │
├─────────────────────────────────────────────────────────┤
│  Phase 7 (2차년, 결제 필요 시)                            │
│                                                          │
│  4. Cloudflare 계정 생성                                  │
│  5. DNS → Cloudflare 네임서버 이관                        │
│  6. Cloudflare Pages 전환                                │
│  7. Workers로 토스페이먼츠 연동                           │
│                                                          │
│  ※ 기존 코드 변경 없이 배포 파이프라인만 전환             │
└─────────────────────────────────────────────────────────┘
```

---

## 부록: r2bit.com 방식 그대로 따라하기 (Quick Start)

mumuki.net을 r2bit.com과 동일한 구조로 배포하는 최소 단계:

```bash
# 1. 도메인 구매 후 DNS 설정
#    A 레코드 4개 → GitHub Pages IP
#    CNAME www → statkclee.github.io

# 2. CNAME 파일 생성
echo "mumuki.net" > CNAME
git add CNAME
git commit -m "[배포] 커스텀 도메인 CNAME 설정"
git push

# 3. GitHub Actions 워크플로우 추가 (위 3.1절 참조)
mkdir -p .github/workflows
# deploy.yml 작성 후 push

# 4. GitHub Settings → Pages → Custom domain: mumuki.net
# 5. ✅ Enforce HTTPS 체크
# 6. 완료! https://mumuki.net 접속 확인
```
