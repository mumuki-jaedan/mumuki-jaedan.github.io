# mumuki.net 웹사이트 추진계획 및 기술검토서

> **작성일**: 2026-04-03  
> **프로젝트**: 무무키 파운데이션(무파) 공식 웹사이트  
> **도메인**: mumuki.net  
> **기술 스택**: Quarto Website (v1.9.16)  
> **작성 근거**: 「다이렉트 기부 솔루션을 위한 복지단체 설립안」(2026.02.28)

---

## 1. 프로젝트 개요

### 1.1 목표

무무키 파운데이션의 설립 취지(다이렉트 기부 솔루션)를 효과적으로 전달하고,
**회계 전면 공개 원칙**을 기술적으로 구현하여 기부자·수혜자·복지단체 간의
신뢰를 확보하는 공식 웹사이트를 구축한다.

### 1.2 핵심 요구사항 (설립안 기반)

| 구분 | 요구사항 | 설립안 근거 |
|------|----------|-------------|
| 투명성 | 계좌/카드 내역 전체 공개 (비식별화 적용) | §3 투명성과 효율성 |
| 매칭 공시 | 가상계좌 발급 → 매칭 지출 내역 병기 | §3 회계 투명성 |
| 비용 분리 | 사업수행비 vs 공익목적사업비 별도 표시 | §3 Charity Water 사례 준용 |
| 활동 안내 | 도서관 동행, 직업 교육, 다이렉트 기부 | §4 주요 활동 예시 |
| 조직 소개 | 이사회, 사무국, 회장단, 감사 구조 | §3 조직도 |
| 감사 보고 | 외부 회계감사보고서 연차 공개 | §3 감사 |

---

## 2. 기술 아키텍처

### 2.1 기술 스택 확정

```
┌─────────────────────────────────────────────────────────┐
│                    mumuki.net                            │
├─────────────────────────────────────────────────────────┤
│  정적 사이트 생성기  │  Quarto 1.9.16 (Website 프로젝트) │
│  언어/엔진          │  R 4.5.2 (knitr) + OJS            │
│  CSS 프레임워크     │  Bootstrap 5 + Custom SCSS         │
│  검색               │  Quarto 내장 전문검색              │
│  아이콘             │  Font Awesome (quarto-ext)         │
│  이미지             │  Lightbox (quarto-ext)             │
├─────────────────────────────────────────────────────────┤
│  호스팅             │  Cloudflare Pages                  │
│  DNS/CDN            │  Cloudflare (한국 엣지 포함)       │
│  SSL/HTTPS          │  Cloudflare Universal SSL (자동)   │
│  서버리스 함수      │  Cloudflare Workers (결제 승인 등) │
├─────────────────────────────────────────────────────────┤
│  소스 관리          │  GitHub (Private Repository)       │
│  CI/CD              │  GitHub Actions → Cloudflare Pages │
│  폼 서비스          │  Google Forms 임베딩 (초기)        │
│  결제 (3차년)       │  토스페이먼츠 + Cloudflare Workers │
└─────────────────────────────────────────────────────────┘
```

### 2.2 기술 선정 근거

#### Quarto Website 선정 이유

1. **한국어 완전 지원**: `lang: ko` 설정으로 UI 요소 전체 한국어화
2. **정적 사이트**: 서버 운영비 0원, 보안 리스크 최소화
3. **Bootstrap 5 기반 반응형**: 모바일 자동 대응
4. **블로그/뉴스 내장**: Listing Pages로 소식 섹션 즉시 구현
5. **데이터 시각화**: OJS(Observable JS)로 회계 데이터 인터랙티브 시각화
6. **R/Python 통합**: 회계 데이터 전처리 → `ojs_define()` → 시각화 파이프라인
7. **확장성**: 343+ 커뮤니티 Extensions 활용 가능

#### Cloudflare Pages 선정 이유

1. **대역폭 무제한**: 트래픽 급증(기부 캠페인 등)에도 비용 0원
2. **한국 CDN 엣지**: 300+ PoP, 한국 내 빠른 응답속도
3. **Workers 통합**: 결제 승인 등 서버사이드 로직을 같은 플랫폼에서 처리
4. **HTTPS 자동**: 커스텀 도메인 설정 시 SSL 자동 적용
5. **월 500회 빌드**: 비영리 단체 업데이트 빈도에 충분

### 2.3 Quarto 프로젝트 구조 설계

```
mumuki/
├── _quarto.yml              # 프로젝트 설정 (navbar, sidebar, i18n)
├── _brand.yml               # 브랜드 색상/폰트 정의
├── custom.scss              # 커스텀 SCSS 테마
├── _language.yml             # 한국어 커스텀 번역
│
├── index.qmd                # 홈페이지 (히어로 + 미션 + CTA)
├── about/
│   ├── index.qmd            # 재단 소개
│   ├── mission.qmd          # 설립 취지 & 다이렉트 기부 솔루션 개요
│   ├── organization.qmd     # 조직도 (이사회, 사무국, 회장단, 감사)
│   └── history.qmd          # 연혁
│
├── programs/
│   ├── index.qmd            # 사업 안내 개요
│   ├── library.qmd          # 도서관 동행 및 식사
│   ├── career.qmd           # 주말 직업 교육
│   └── direct-donation.qmd  # 다이렉트 기부 솔루션 (3차년)
│
├── transparency/
│   ├── index.qmd            # 투명한 운영 개요
│   ├── finance.qmd          # 회계 공시 (계좌/카드 내역 공개)
│   ├── audit.qmd            # 감사보고서
│   └── donation-usage.qmd   # 기부금 사용 내역 (매칭 지출)
│
├── participate/
│   ├── index.qmd            # 참여하기 개요
│   ├── donate.qmd           # 후원 안내 (계좌, 토스 송금)
│   ├── volunteer.qmd        # 봉사자/멘토 모집
│   └── partner.qmd          # 복지단체 협력
│
├── news/
│   ├── index.qmd            # 소식 (listing page)
│   └── posts/               # 개별 포스트
│       └── YYYY-MM-DD-title/
│           └── index.qmd
│
├── legal/
│   ├── privacy.qmd          # 개인정보 처리방침
│   └── terms.qmd            # 이용약관
│
├── data/                    # 회계 데이터 (CSV/JSON)
│   ├── finance_2026.csv
│   └── donations_2026.csv
│
├── images/                  # 이미지 자산
│   ├── logo.svg
│   ├── hero/
│   └── programs/
│
├── _extensions/             # Quarto Extensions
│   ├── lightbox/
│   └── fontawesome/
│
├── tech_document/           # 기술 문서 (개발용, 사이트 미포함)
│   └── 20260403_mumuki_website_추진계획_및_기술검토.md
│
└── document/                # 설립안 등 원본 문서
    └── 5-다이렉트 기부 솔루션을 위한 복지단체 설립안_20260228.docx
```

---

## 3. 사이트맵 & 네비게이션 설계

### 3.1 Navbar (상단 메뉴)

```yaml
navbar:
  logo: images/logo.svg
  title: "무무키 파운데이션"
  left:
    - text: "재단소개"
      menu:
        - text: "설립 취지"
          href: about/mission.qmd
        - text: "조직 구성"
          href: about/organization.qmd
        - text: "연혁"
          href: about/history.qmd
    - text: "사업안내"
      menu:
        - text: "도서관 동행"
          href: programs/library.qmd
        - text: "직업 교육"
          href: programs/career.qmd
        - text: "다이렉트 기부"
          href: programs/direct-donation.qmd
    - text: "투명한 운영"
      menu:
        - text: "회계 공시"
          href: transparency/finance.qmd
        - text: "감사보고서"
          href: transparency/audit.qmd
        - text: "기부금 사용 내역"
          href: transparency/donation-usage.qmd
    - text: "참여하기"
      href: participate/index.qmd
    - text: "소식"
      href: news/index.qmd
  right:
    - icon: heart-fill
      text: "후원하기"
      href: participate/donate.qmd
```

### 3.2 Footer

```
무무키 파운데이션 | 대표: OOO | 고유번호: XXX-XX-XXXXX
주소: 인천광역시 계양구 ...
이메일: contact@mumuki.net | 전화: 032-XXX-XXXX
개인정보처리방침 | 이용약관
© 2026 무무키 파운데이션. All rights reserved.
```

---

## 4. 주요 기능 구현 상세

### 4.1 회계 투명성 시스템 (핵심 차별화)

설립안의 **전부 공개 원칙**을 기술적으로 구현한다.

```
[데이터 파이프라인]
  
  (1) 회계 데이터 입력
      ├── 무파 명의 계좌 내역 (CSV 추출)
      ├── 무파 명의 카드 사용 내역 (CSV 추출)
      └── 가상계좌 기부금 입금 내역

  (2) R 전처리 (렌더링 시점)
      ├── 비식별화 처리 (기부자/수혜자명 마스킹)
      ├── 계정 분류 (사업수행비 / 일반관리비 / 모금비)
      ├── 매칭 테이블 생성 (가상계좌 입금 ↔ 지출)
      └── ojs_define()으로 JavaScript 전달

  (3) OJS 인터랙티브 시각화
      ├── 월별/분기별 수입·지출 차트
      ├── 사업수행비 vs 운영비 비율 게이지
      ├── 기부금 → 수혜자 매칭 추적 테이블
      └── 필터: 기간, 계정 유형, 사업 분류
```

**구현 예시 (transparency/finance.qmd)**:
```yaml
---
title: "회계 공시"
format:
  html:
    page-layout: full
---
```
```r
#| echo: false
library(readr)
library(dplyr)

finance <- read_csv("../data/finance_2026.csv") |>
  mutate(
    donor_name = str_replace(donor_name, "(?<=.).(?=.)", "*"),
    beneficiary = str_replace(beneficiary, "(?<=.).(?=.)", "*")
  )

ojs_define(finance_data = finance)
```
```js
// OJS cell
viewof period = Inputs.select(["전체", "1분기", "2분기", "3분기", "4분기"])
filtered = finance_data.filter(d => period === "전체" || d.quarter === period)
Plot.plot({
  marks: [
    Plot.barY(filtered, Plot.groupX({y: "sum"}, {x: "month", y: "amount", fill: "type"}))
  ]
})
```

### 4.2 기부/후원 시스템 (단계별)

| 단계 | 시기 | 구현 방식 |
|------|------|-----------|
| **1단계** | 1차년 (즉시) | 계좌번호 게시 + 토스 송금 링크(toss.me/mumuki) + 카카오페이 QR |
| **2단계** | 2차년 | Cloudflare Workers + 토스페이먼츠 SDK로 온라인 결제 (정기후원 CMS) |
| **3단계** | 3차년 | 다이렉트 기부 솔루션 통합 (가상계좌 → 수혜자 매칭 → 실시간 추적) |

**1단계 구현 (participate/donate.qmd)**:
```markdown
## 후원 안내

### 계좌이체
- **은행**: OO은행
- **계좌번호**: XXX-XXXX-XXXX-XX
- **예금주**: (사)무무키 파운데이션

### 간편 송금
- [토스로 후원하기](https://toss.me/mumuki)
- 카카오페이 QR: [이미지]

### 기부금 영수증
연말정산 기부금 영수증이 필요하신 분은 
[신청서 작성](Google Forms 링크)을 부탁드립니다.
```

### 4.3 문의/연락 시스템

- **초기**: Google Forms 임베딩 (무료, 무제한, 스프레드시트 자동 연동)
- **확장**: Formspree 또는 Cloudflare Workers + 이메일 발송 API

### 4.4 법적 필수 페이지

| 페이지 | 법적 근거 | 내용 |
|--------|-----------|------|
| 개인정보 처리방침 | 개인정보보호법 제30조 | 수집 목적, 항목, 보유 기간, 보호책임자 등 |
| 기부금 공시 | 상속세 및 증여세법 제50조의3 | 재무제표, 기부금 모금/활용 실적, 국세청 공시 링크 |
| 법인 정보 | 정보통신망법 | 단체명, 대표자, 고유번호, 소재지, 연락처 |

---

## 5. 연차별 추진 로드맵

### 5.1 전체 일정 요약

```
2026년
├── 4월: Phase 1~2 (프로젝트 셋업 & 기본 구조)
├── 5월: Phase 3~4 (콘텐츠 개발 & 핵심 기능)
├── 6월: Phase 5 (배포 & 런칭)
├── 7~12월: Phase 6 (운영 & 콘텐츠 축적)
│
2027년 (2차년)
├── 다이렉트 기부 플랫폼 기획/UX 설계
├── 토스페이먼츠 연동 (정기후원)
├── 회계 데이터 자동화 파이프라인
│
2028년 (3차년)
├── 다이렉트 기부 솔루션 웹사이트 통합
├── 가상계좌 자동 매칭 시스템
└── 기부자 대시보드
```

### 5.2 Phase 1: 프로젝트 초기화 (4/3 ~ 4/6)

| 날짜 | 작업 | 산출물 | 비고 |
|------|------|--------|------|
| 4/3 | 기술 검토 및 추진계획 수립 | 본 문서 | ✅ 완료 |
| 4/3 | tech_document 폴더 생성 | 폴더 구조 | ✅ 완료 |
| 4/4 | GitHub 리포지토리 생성 (Private) | mumuki/mumuki.net | .gitignore 포함 |
| 4/4 | Quarto 프로젝트 초기화 | _quarto.yml, index.qmd | `quarto create project website` |
| 4/5 | 브랜드 가이드라인 확정 | _brand.yml, custom.scss | 색상, 폰트, 로고 |
| 4/6 | Quarto Extensions 설치 | _extensions/ | lightbox, fontawesome |

### 5.3 Phase 2: 기본 구조 구축 (4/7 ~ 4/13)

| 날짜 | 작업 | 산출물 | 비고 |
|------|------|--------|------|
| 4/7 | Navbar & Footer 구현 | _quarto.yml 네비게이션 | 반응형 확인 |
| 4/7 | 한국어 설정 | _language.yml, lang: ko | UI 요소 한국어화 |
| 4/8 | 홈페이지(index.qmd) 레이아웃 | 히어로 섹션, 미션, CTA | Bootstrap Grid 활용 |
| 4/9 | about/ 섹션 페이지 생성 | mission.qmd, organization.qmd | 설립안 §1, §3 기반 |
| 4/10 | programs/ 섹션 페이지 생성 | library.qmd, career.qmd | 설립안 §4 기반 |
| 4/11 | participate/ 섹션 생성 | donate.qmd, volunteer.qmd | 계좌 + 토스 링크 |
| 4/12 | news/ 블로그 구조 생성 | listing page + 샘플 포스트 | RSS 피드 설정 |
| 4/13 | legal/ 법적 페이지 작성 | privacy.qmd, terms.qmd | 개인정보 처리방침 |

### 5.4 Phase 3: 핵심 콘텐츠 개발 (4/14 ~ 4/27)

| 날짜 | 작업 | 산출물 | 비고 |
|------|------|--------|------|
| 4/14~15 | 재단소개 콘텐츠 작성 | 설립 취지, 조직도, 연혁 | 설립안 내용 정리 |
| 4/16~17 | 사업안내 콘텐츠 작성 | 도서관 동행, 직업 교육 상세 | 그림4,5 재구성 |
| 4/18~19 | 다이렉트 기부 솔루션 설명 페이지 | 구조도, 정보 흐름도 시각화 | 그림1,2 재구성 |
| 4/20~21 | 투명성 섹션 데이터 구조 설계 | data/ CSV 스키마, R 전처리 | 비식별화 로직 |
| 4/22~23 | 회계 공시 페이지 구현 | OJS 인터랙티브 차트 | 수입·지출 시각화 |
| 4/24~25 | 기부금 매칭 추적 테이블 | 가상계좌 → 지출 매칭 뷰 | 설립안 §3 핵심 |
| 4/26~27 | 참여/후원 페이지 완성 | 기부 안내, 봉사 모집 폼 | Google Forms 연동 |

### 5.5 Phase 4: 배포 & 도메인 (4/28 ~ 5/4)

| 날짜 | 작업 | 산출물 | 비고 |
|------|------|--------|------|
| 4/28 | Cloudflare 계정 설정 | Pages 프로젝트 생성 | GitHub 리포 연결 |
| 4/29 | mumuki.net 도메인 DNS 설정 | Cloudflare DNS로 이관 | 네임서버 변경 |
| 4/30 | CI/CD 파이프라인 구축 | GitHub Actions 워크플로우 | push → build → deploy |
| 5/1 | HTTPS & 리다이렉트 설정 | SSL 인증서, www 리다이렉트 | Cloudflare 자동 |
| 5/2 | 스테이징 환경 테스트 | 프리뷰 배포 URL 확인 | 브랜치별 프리뷰 |
| 5/3~4 | 크로스 브라우저/디바이스 테스트 | 테스트 결과 보고서 | Chrome, Safari, Mobile |

### 5.6 Phase 5: QA & 런칭 (5/5 ~ 5/11)

| 날짜 | 작업 | 산출물 | 비고 |
|------|------|--------|------|
| 5/5 | SEO 최적화 | meta tags, sitemap.xml, robots.txt | 검색엔진 노출 |
| 5/6 | 접근성 검사 | WCAG 2.1 AA 준수 확인 | lighthouse 감사 |
| 5/7 | 성능 최적화 | 이미지 압축, lazy loading | Core Web Vitals |
| 5/8 | 콘텐츠 최종 검수 | 오탈자, 링크 확인 | linkrot Extension |
| 5/9 | 준비위 내부 리뷰 | 피드백 반영 | |
| 5/10 | 프로덕션 배포 | mumuki.net 라이브 | DNS 전파 확인 |
| 5/11 | 런칭 공지 & 모니터링 | 소식 포스트, Analytics | Plausible Analytics |

### 5.7 Phase 6: 운영 & 확장 (5/12~)

| 기간 | 작업 | 비고 |
|------|------|------|
| 월 1회 | 회계 데이터 업데이트 | CSV 업로드 → 자동 시각화 |
| 월 1회 | 활동 소식 포스트 작성 | 도서관 동행, 직업 교육 후기 |
| 분기 1회 | 사이트 점검 & 업데이트 | Quarto 버전, Extension 업데이트 |
| 연 1회 | 감사보고서 업로드 | PDF 다운로드 링크 |
| 2차년 | 토스페이먼츠 결제 연동 | Cloudflare Workers 개발 |
| 3차년 | 다이렉트 기부 솔루션 통합 | 가상계좌 매칭 시스템 |

---

## 6. 기술 검토 상세

### 6.1 Quarto 1.9.16 적합성 평가

| 평가 항목 | 점수 | 평가 내용 |
|-----------|------|-----------|
| 한국어 지원 | ★★★★★ | `lang: ko` 완전 지원, 34개 언어 중 하나 |
| 반응형 디자인 | ★★★★★ | Bootstrap 5 기반 자동 반응형, 모바일 햄버거 메뉴 |
| 블로그/뉴스 | ★★★★★ | Listing Pages 내장, RSS 자동 생성, 카테고리 필터 |
| 회계 시각화 | ★★★★☆ | OJS + R 전처리로 인터랙티브 차트 구현 가능 |
| 테마 커스터마이징 | ★★★★★ | SCSS 4단계 레이어링, _brand.yml 지원 |
| 검색 | ★★★★★ | 전문검색 내장, 한국어 지원, 키보드 단축키 |
| 정적 사이트 보안 | ★★★★★ | 공격 표면 최소화, XSS/SQL Injection 원천 차단 |
| 다국어(i18n) | ★★★☆☆ | babelquarto 패키지 필요, 콘텐츠 수동 번역 |
| 동적 기능 | ★★★☆☆ | 서버리스 함수 별도 구성 필요 |
| 사용자 인증 | ★☆☆☆☆ | 정적 사이트 한계, 3차년 별도 시스템 필요 |

**종합 평가**: 1~2차년 무파 웹사이트 요구사항에 **적합**. 3차년 다이렉트 기부 솔루션의 
사용자 인증/실시간 매칭 기능은 별도 백엔드(Cloudflare Workers + D1/KV) 또는 
별도 웹앱으로 구현 필요.

### 6.2 호스팅 비교 최종 평가

| 항목 | Cloudflare Pages | Netlify | GitHub Pages |
|------|-----------------|---------|-------------|
| **대역폭** | **무제한** | 100GB/월 | 100GB/월 |
| **빌드** | 500회/월 | 300분/월 | 10회/시간 |
| **서버리스** | Workers (무료) | Functions (125K/월) | 없음 |
| **한국 CDN** | **300+ PoP** | 제한적 | 제한적 |
| **커스텀 도메인** | 무료 | 무료 | 무료 (Pro 필요시) |
| **SSL** | 자동 | 자동 | 자동 |
| **폼** | Workers 필요 | **내장** | 없음 |
| **비용** | **$0** | **$0** | **$0** |
| **프리뷰 배포** | 브랜치별 자동 | 브랜치별 자동 | 없음 |

**선정: Cloudflare Pages** — 대역폭 무제한 + 한국 CDN + Workers 통합이 결정적.

### 6.3 보안 고려사항

| 위험 | 대응 방안 |
|------|-----------|
| 개인정보 노출 | R 전처리 단계에서 비식별화 (이름 마스킹, 계좌 뒷자리만 표시) |
| 원본 데이터 유출 | data/ 폴더는 .gitignore에 포함, 렌더링 결과만 배포 |
| 결제 시크릿키 | Workers 환경변수에 저장, 프론트엔드 노출 차단 |
| XSS/주입 공격 | 정적 HTML이므로 원천 차단 |
| DDoS | Cloudflare 기본 DDoS 보호 포함 |

### 6.4 비용 분석 (연간)

| 항목 | 비용 | 비고 |
|------|------|------|
| 도메인 (mumuki.net) | ~₩15,000/년 | .net 도메인 기준 |
| 호스팅 (Cloudflare Pages) | ₩0 | 무료 티어 |
| SSL 인증서 | ₩0 | Cloudflare 자동 |
| 폼 서비스 (Google Forms) | ₩0 | 무료 |
| Analytics (Plausible) | ₩0 (셀프호스팅) 또는 ~€9/월 | 또는 Cloudflare Analytics 무료 |
| **연간 총 비용** | **~₩15,000** | 도메인 비용만 발생 |

### 6.5 다국어(i18n) 전략

**1차년 (현재)**: 한국어 단일 언어
- `lang: ko` 설정으로 UI 전체 한국어화
- 콘텐츠는 한국어로만 작성

**향후 확장 시**: babelquarto 패키지 도입
```
about.qmd        → 한국어 (기본)
about.en.qmd     → 영어
about.ja.qmd     → 일본어
```
- 언어 전환 버튼 자동 생성
- 콘텐츠는 수동 번역 또는 AI 번역 후 검수

---

## 7. `_quarto.yml` 초안

```yaml
project:
  type: website
  output-dir: _site

website:
  title: "무무키 파운데이션"
  description: "다이렉트 기부 솔루션으로 기부자와 수혜자를 잇는 투명한 복지단체"
  site-url: https://mumuki.net
  favicon: images/favicon.ico

  navbar:
    pinned: true
    logo: images/logo.svg
    left:
      - text: "재단소개"
        menu:
          - about/mission.qmd
          - about/organization.qmd
          - about/history.qmd
      - text: "사업안내"
        menu:
          - programs/library.qmd
          - programs/career.qmd
          - programs/direct-donation.qmd
      - text: "투명한 운영"
        menu:
          - transparency/finance.qmd
          - transparency/audit.qmd
          - transparency/donation-usage.qmd
      - text: "참여하기"
        href: participate/index.qmd
      - text: "소식"
        href: news/index.qmd
    right:
      - icon: heart-fill
        text: "후원하기"
        href: participate/donate.qmd

  search:
    location: navbar
    type: overlay

  page-footer:
    left: |
      무무키 파운데이션 | 대표: OOO | 고유번호: XXX-XX-XXXXX
    center: |
      [개인정보처리방침](legal/privacy.qmd) | [이용약관](legal/terms.qmd)
    right: |
      © 2026 무무키 파운데이션

  open-graph: true

format:
  html:
    theme:
      - cosmo
      - custom.scss
    lang: ko
    toc: true
    code-fold: true
    css: styles.css
```

---

## 8. CI/CD 파이프라인 설계

### GitHub Actions → Cloudflare Pages

```yaml
# .github/workflows/deploy.yml
name: Deploy to Cloudflare Pages

on:
  push:
    branches: [main]

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Quarto
        uses: quarto-dev/quarto-actions/setup@v2
        with:
          version: "1.9.16"
      
      - name: Setup R
        uses: r-lib/actions/setup-r@v2
      
      - name: Install R packages
        uses: r-lib/actions/setup-r-dependencies@v2
        with:
          packages: |
            readr
            dplyr
            stringr
      
      - name: Render Quarto
        run: quarto render
      
      - name: Deploy to Cloudflare Pages
        uses: cloudflare/pages-action@v1
        with:
          apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          accountId: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          projectName: mumuki-net
          directory: _site
```

---

## 9. 리스크 & 대응 계획

| 리스크 | 영향도 | 발생 가능성 | 대응 방안 |
|--------|--------|------------|-----------|
| 설립안 내용 변경 | 높음 | 중간 | 모듈화 설계로 섹션별 독립 수정 가능 |
| 회계 데이터 형식 미확정 | 중간 | 높음 | CSV 스키마 초안 먼저 확정, R 전처리로 유연 대응 |
| 도메인 등록 지연 | 낮음 | 낮음 | 스테이징 URL로 개발 진행, 도메인은 별도 트랙 |
| 디자인 리소스 부족 | 중간 | 중간 | Bootswatch 테마(cosmo) + 최소 커스텀 SCSS |
| 3차년 다이렉트 기부 시스템 복잡도 | 높음 | 높음 | 2차년 기획 단계에서 별도 기술 검토 수행 |
| 비식별화 처리 미흡 | 높음 | 낮음 | 비식별화 로직 코드 리뷰 + 배포 전 수동 확인 |

---

## 10. 즉시 실행 가능 체크리스트

### 오늘 (4/3) ✅
- [x] tech_document 폴더 생성
- [x] 기술 스택 검토 및 선정
- [x] 추진계획서 작성

### 내일 (4/4) 예정
- [ ] GitHub Private Repository 생성 (`mumuki/mumuki.net`)
- [ ] `quarto create project website mumuki` 실행
- [ ] `_quarto.yml` 초기 설정 (위 §7 초안 기반)
- [ ] `.gitignore` 설정 (data/ 폴더, _site/ 등)
- [ ] 초기 커밋 & 푸시

### 이번 주 (4/5~6) 예정
- [ ] 로고/브랜드 가이드라인 확인 (준비위에 문의)
- [ ] `_brand.yml` 작성 (임시 색상으로 시작)
- [ ] `custom.scss` 기본 테마 설정
- [ ] Quarto Extensions 설치:
  ```bash
  quarto add quarto-ext/lightbox
  quarto add quarto-ext/fontawesome
  ```
- [ ] 전체 폴더 구조 생성 및 placeholder .qmd 파일 배치

---

## 부록 A: 개발 환경 정보

| 항목 | 버전 | 비고 |
|------|------|------|
| Quarto | 1.9.16 | 최신 |
| R | 4.5.2 | knitr 1.50, rmarkdown 2.30 |
| Python | 3.9.6 | Jupyter 5.7.1 |
| Node.js | 24.3.0 | OJS 렌더링용 |
| TinyTeX | 2025 | PDF 출력 (필요시) |
| OS | macOS Darwin 23.6.0 | ARM64 (Apple Silicon) |

## 부록 B: 참고 자료

- [Quarto Website Documentation](https://quarto.org/docs/websites/)
- [Cloudflare Pages Documentation](https://developers.cloudflare.com/pages/)
- [설립안] 다이렉트 기부 솔루션을 위한 복지단체 설립(안), 2026.02.28
- [Charity Water 투명성 모델](https://www.charitywater.org/our-approach)
- [국세청 공익법인 공시시스템](https://www.hometax.go.kr)
- [개인정보보호위원회 가이드라인](https://www.pipc.go.kr)
