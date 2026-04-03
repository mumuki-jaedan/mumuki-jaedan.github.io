# mumuki.net - 무무키 파운데이션 웹사이트

## 프로젝트 개요

무무키 파운데이션(무파)의 공식 웹사이트. 다이렉트 기부 솔루션을 통해 기부자와 수혜자를 직접 연결하고, 회계 전면 공개 원칙을 기술적으로 구현한다.

- **웹사이트**: https://mumuki.net (라이브)
- **GitHub**: https://github.com/mumuki-jaedan/mumuki-jaedan.github.io (Public)
- **백업 리포**: https://github.com/statkclee/mumuki.net (Private)
- **기술 스택**: Quarto 1.9.16 + GitHub Pages + GitHub Actions
- **도메인**: mumuki.net (가비아 등록)

## 배포 아키텍처

```
로컬 (quarto preview)
  ↓ git push
GitHub (mumuki-jaedan/mumuki-jaedan.github.io)
  ↓ GitHub Actions (자동 빌드)
  ↓ quarto render → docs/
GitHub Pages → https://mumuki.net
  DNS: 가비아 → A 레코드 4개 (GitHub IPs)
  SSL: GitHub 자동 (HTTPS)
```

### Git 리모트 구성

```
origin  → git@github.com:statkclee/mumuki.net.git (Private 백업)
mumuki  → git@github-mumuki:mumuki-jaedan/mumuki-jaedan.github.io.git (배포)
```

### SSH 설정 (~/.ssh/config)

```
Host github.com       → statkclee (id_ed25519)
Host github-mumuki    → mumuki-jaedan (id_ed25519_mumuki)
```

## 디렉토리 구조

```
mumuki/
├── CLAUDE.md             ← 이 파일 (프로젝트 컨텍스트)
├── PLAN.md               ← 현재 단계 계획
├── PROGRESS.md           ← 진행 상황 추적
├── _quarto.yml           ← Quarto 프로젝트 설정
├── index.qmd             ← 홈페이지 (Hero + 프로그램 + 투명성 + CTA)
├── custom.scss           ← Tufte 스타일 SCSS 테마
├── CNAME                 ← GitHub Pages 커스텀 도메인
├── .github/workflows/    ← GitHub Actions CI/CD
│   └── deploy.yml        ← Quarto 빌드 → Pages 배포
├── about/*.qmd           ← 재단소개 (설립 취지, 조직, 연혁)
├── programs/*.qmd        ← 사업안내 (도서관 동행, 직업 교육, 다이렉트 기부)
├── transparency/*.qmd    ← 투명한 운영 (원칙, 회계 공시, 감사보고서)
├── participate/*.qmd     ← 참여하기 (후원, 봉사, 기관 협력)
├── news/**/*.qmd         ← 소식 (블로그 listing)
├── legal/*.qmd           ← 법적 페이지 (개인정보, 이용약관)
├── assets/               ← 브랜드 에셋 (로고 4종, 색상 팔레트)
├── images/               ← Tufte 스타일 SVG 일러스트 (16종)
├── tech_document/        ← 기술 문서 (Git 포함, 사이트 미포함)
├── document/             ← 설립안 원본 (Git 제외, Google Drive만)
├── data/                 ← 회계 데이터 (Git 제외, 향후 사용)
├── docs/                 ← 빌드 출력 (Git 제외, GitHub Actions가 빌드)
└── .claude/commands/     ← Claude Code 커스텀 커맨드
```

## 핵심 원칙

1. **설립안 준수**: 모든 콘텐츠와 기능은 설립안을 근거로 한다
2. **Tufte 스타일**: 최소주의, 높은 데이터-잉크 비율, 타이포그래피 우선
3. **투명성 최우선**: 회계 데이터는 비식별화 후 전면 공개
4. **정적 사이트**: 보안 최대화, 비용 최소화 (연 ~₩22,000 도메인만)
5. **단계적 확장**: 1차년(웹사이트) → 2차년(결제연동) → 3차년(다이렉트 기부 솔루션)

## 브랜드 색상

- Primary: `#2C5F5C` (Deep Teal) — 신뢰, 성장
- Secondary: `#E07A5F` (Warm Coral) — 따뜻함, 돌봄, CTA 버튼
- Accent: `#D4A574` (Sand Gold) — 나눔, 강조, 섹션 구분선
- Background: `#F5F1EB` (Off White) — Tufte 스타일 배경
- Text: `#2D3436` (Charcoal) — 본문

## 타이포그래피

- 제목: Noto Serif KR (Google Fonts)
- 본문: Noto Sans KR (Google Fonts)
- 줄간격: 1.7, 문단 최대 65ch

## 작업 흐름

이 프로젝트는 Plan → Execute → Review → Evaluate 순환 구조로 진행한다.
커스텀 커맨드(`/plan`, `/review`, `/evaluate`, `/execute`)를 사용한다.

```
/plan     → 작업 계획 수립, PLAN.md 업데이트
/execute  → 계획에 따른 실행
/review   → 결과물 품질 검토
/evaluate → 설립안 부합성 + 기술 적합성 평가
```

## 배포 명령

```bash
# 로컬 미리보기
quarto preview --port 4567

# 배포 (push하면 GitHub Actions가 자동 빌드+배포)
git push mumuki main

# 백업
git push origin main
```

## 코드 컨벤션

- Quarto 문서: `.qmd` 확장자, YAML front matter 필수
- SVG: Tufte 스타일, 선 두께 1-2px, 브랜드 팔레트만 사용
- SCSS: BEM 네이밍, 8px 그리드 시스템
- 한국어 우선, 기술 용어는 원어 유지
- 커밋 메시지: 한국어, `[카테고리] 설명` 형식

## 기술 문서 목록

| 파일 | 내용 |
|------|------|
| `tech_document/20260403_mumuki_website_추진계획_및_기술검토.md` | 전체 추진계획, 6 Phase 로드맵 |
| `tech_document/20260403_웹사이트_초기구축_기술기록.md` | Session 2 초기 구축 기록 |
| `tech_document/20260403_프로젝트구조_리팩토링_및_디자인개선.md` | 구조 변경, 디자인 개선 상세 |
| `tech_document/20260403_소스코드_저장_및_배포_전략.md` | GitHub + Cloudflare 배포 전략 |
| `tech_document/20260403_배포방식_비교_GitHub_Pages_vs_Cloudflare_Pages.md` | 배포 방식 비교 |

## 참고 자료

- [Quarto Website 문서](https://quarto.org/docs/websites/)
- [GitHub Pages 문서](https://docs.github.com/en/pages)
- [Charity Water 투명성 모델](https://www.charitywater.org/our-approach)
- [r2bit.com 참고 사례](https://r2bit.com/) — bit2r/bit2r.github.io (동일 구조)

## 설립안 원본

- `document/5-다이렉트 기부 솔루션을 위한 복지단체 설립안_20260228.docx`
- **Git 미포함** (.gitignore) — Google Drive에서만 관리
