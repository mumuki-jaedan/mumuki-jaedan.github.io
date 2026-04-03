# mumuki.net - 무무키 파운데이션 웹사이트

## 프로젝트 개요

무무키 파운데이션(무파)의 공식 웹사이트. 다이렉트 기부 솔루션을 통해 기부자와 수혜자를 직접 연결하고, 회계 전면 공개 원칙을 기술적으로 구현한다.

- **도메인**: mumuki.net
- **기술 스택**: Quarto 1.9.16 Website + Cloudflare Pages
- **설립안**: `document/5-다이렉트 기부 솔루션을 위한 복지단체 설립안_20260228.docx`
- **기술 문서**: `tech_document/`
- **디자인 시스템**: `assets/design-system.md`

## 디렉토리 구조

```
mumuki/
├── CLAUDE.md          ← 이 파일 (프로젝트 컨텍스트)
├── PLAN.md            ← 현재 단계 계획
├── PROGRESS.md        ← 진행 상황 추적
├── _quarto.yml        ← Quarto 프로젝트 설정
├── assets/            ← 디자인 시스템 (로고, 색상, 브랜드)
├── images/            ← Tufte 스타일 SVG 일러스트레이션
├── tech_document/     ← 기술 문서 (추진계획서 등)
├── document/          ← 원본 설립안 문서
└── .claude/commands/  ← Claude Code 커스텀 커맨드
```

## 핵심 원칙

1. **설립안 준수**: 모든 콘텐츠와 기능은 설립안을 근거로 한다
2. **Tufte 스타일**: 최소주의, 높은 데이터-잉크 비율, 타이포그래피 우선
3. **투명성 최우선**: 회계 데이터는 비식별화 후 전면 공개
4. **정적 사이트**: 보안 최대화, 비용 최소화 (연 ~₩15,000)
5. **단계적 확장**: 1차년(웹사이트) → 2차년(결제연동) → 3차년(다이렉트 기부 솔루션)

## 브랜드 색상

- Primary: `#2C5F5C` (Deep Teal) — 신뢰, 성장
- Secondary: `#E07A5F` (Warm Coral) — 따뜻함, 돌봄
- Accent: `#D4A574` (Sand Gold) — 나눔, 강조
- Background: `#F5F1EB` (Off White) — Tufte 스타일 배경

## 작업 흐름

이 프로젝트는 Plan → Execute → Review → Evaluate 순환 구조로 진행한다.
커스텀 커맨드(`/plan`, `/review`, `/evaluate`, `/execute`)를 사용한다.

```
/plan     → 작업 계획 수립, PLAN.md 업데이트
/execute  → 계획에 따른 실행
/review   → 결과물 품질 검토
/evaluate → 설립안 부합성 + 기술 적합성 평가
```

## 코드 컨벤션

- Quarto 문서: `.qmd` 확장자, YAML front matter 필수
- SVG: Tufte 스타일, 선 두께 1-2px, 브랜드 팔레트만 사용
- SCSS: BEM 네이밍, 8px 그리드 시스템
- 한국어 우선, 기술 용어는 원어 유지
- 커밋 메시지: 한국어, `[카테고리] 설명` 형식

## 참고 자료

- [Quarto Website 문서](https://quarto.org/docs/websites/)
- [Cloudflare Pages 문서](https://developers.cloudflare.com/pages/)
- [Charity Water 투명성 모델](https://www.charitywater.org/our-approach)
