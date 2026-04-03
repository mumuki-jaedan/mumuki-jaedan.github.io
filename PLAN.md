# 현재 단계 계획

> **현재 Phase**: Phase 3 — 핵심 콘텐츠 개발 (진행 중)  
> **이전 Phase**: Phase 1~2 완료 (2026-04-03)  
> **상태**: docs/ 웹사이트 초기 구축 완료, 콘텐츠 보강 필요

---

## 완료된 작업

- [x] tech_document 폴더 생성 및 추진계획서 작성
- [x] 디자인 시스템 수립 + 로고 4종 + 일러스트 8종 + 아이콘 7종
- [x] CLAUDE.md / PLAN.md / PROGRESS.md / .claude/commands/ 생성
- [x] docs/ Quarto Website 초기 구축 (17페이지, 클린 빌드)
- [x] Plan → Execute → Review → Evaluate 사이클 1회 완료
- [x] 기술 기록 문서 작성

## 다음 작업

- [ ] .gitignore 설정 (docs/_site/, .quarto/ 등)
- [ ] git commit (웹사이트 초기 구축)
- [ ] quarto preview로 로컬 브라우저 확인
- [ ] 콘텐츠 보강: 실사진, 실제 연락처 등
- [ ] Google Fonts 연동 (Noto Serif KR, Noto Sans KR)
- [ ] GitHub Pages 또는 Cloudflare Pages 배포 설정

---

## 전체 로드맵 요약

| Phase | 기간 | 핵심 | 상태 |
|-------|------|------|------|
| 1. 초기화 | 4/3 | 프로젝트 셋업, 디자인 시스템 | **완료** |
| 2. 기본 구조 | 4/3 | Navbar, Footer, 페이지 구조 | **완료** |
| **3. 콘텐츠** | 4/3~ | 콘텐츠 작성, 회계 시각화 | **진행 중 (80%)** |
| 4. 배포 | 4/28~5/4 | Cloudflare Pages, DNS, CI/CD | 예정 |
| 5. QA & 런칭 | 5/5~11 | 테스트, SEO, 런칭 | 예정 |
| 6. 운영 | 5/12~ | 콘텐츠 축적, 월간 업데이트 | 예정 |

---

## 결정 사항 로그

| 날짜 | 결정 | 근거 |
|------|------|------|
| 4/3 | Quarto 1.9.16 선정 | 한국어 지원, R/OJS 통합, 블로그 내장 |
| 4/3 | Cloudflare Pages 선정 | 대역폭 무제한, 한국 CDN, Workers 통합 |
| 4/3 | Tufte 디자인 스타일 | 최소주의, 신뢰감, 콘텐츠 중심 |
| 4/3 | docs/ 폴더에 웹사이트 | 루트와 분리, 심링크로 이미지/에셋 공유 |
| 4/3 | footer 절대 경로 사용 | 서브디렉토리 페이지에서 링크 해결 문제 방지 |
| 4/3 | 인라인 스타일 → SCSS 클래스 | 유지보수성, 디자인 시스템 일관성 |
