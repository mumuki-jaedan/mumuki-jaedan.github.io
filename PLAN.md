# 현재 단계 계획

> **현재 Phase**: Phase 1 — 프로젝트 초기화  
> **기간**: 2026-04-03 ~ 2026-04-06  
> **상태**: 진행 중

---

## Phase 1 목표

프로젝트 기반 구조를 확립하고, 디자인 시스템을 완성하며, Quarto 프로젝트를 초기화한다.

## 작업 목록

### 완료
- [x] tech_document 폴더 생성 및 추진계획서 작성
- [x] 디자인 시스템 수립 (assets/design-system.md)
- [x] 로고 SVG 4종 제작 (primary, white, icon, favicon)
- [x] 컬러 팔레트 SVG 제작
- [x] 브랜드 설정 파일 (_brand.yml)
- [x] Tufte 스타일 SVG 일러스트레이션 8종 제작
- [x] UI 아이콘 세트 7종 제작
- [x] CLAUDE.md / PLAN.md / PROGRESS.md 생성
- [x] .claude/commands/ 커스텀 커맨드 생성

### 다음 작업 (4/4~6)
- [ ] GitHub Private Repository 생성
- [ ] `quarto create project website` 실행
- [ ] `_quarto.yml` 초기 설정
- [ ] `.gitignore` 설정
- [ ] custom.scss 테마 파일 작성
- [ ] Quarto Extensions 설치 (lightbox, fontawesome)
- [ ] 전체 페이지 구조 (.qmd placeholder 파일) 생성
- [ ] 초기 커밋

---

## 전체 로드맵 요약

| Phase | 기간 | 핵심 |
|-------|------|------|
| **1. 초기화** | 4/3~6 | 프로젝트 셋업, 디자인 시스템 ← **현재** |
| 2. 기본 구조 | 4/7~13 | Navbar, Footer, 페이지 구조 |
| 3. 콘텐츠 | 4/14~27 | 콘텐츠 작성, 회계 시각화 |
| 4. 배포 | 4/28~5/4 | Cloudflare Pages, DNS, CI/CD |
| 5. QA & 런칭 | 5/5~11 | 테스트, SEO, 런칭 |
| 6. 운영 | 5/12~ | 콘텐츠 축적, 월간 업데이트 |

---

## 결정 사항 로그

| 날짜 | 결정 | 근거 |
|------|------|------|
| 4/3 | Quarto 1.9.16 선정 | 한국어 지원, R/OJS 통합, 블로그 내장 |
| 4/3 | Cloudflare Pages 선정 | 대역폭 무제한, 한국 CDN, Workers 통합 |
| 4/3 | Tufte 디자인 스타일 | 최소주의, 신뢰감, 콘텐츠 중심 |
| 4/3 | 1차년 한국어 단일 언어 | babelquarto로 향후 다국어 확장 가능 |
