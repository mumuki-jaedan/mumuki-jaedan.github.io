# 무무키 파운데이션 디자인 시스템

> **Version**: 1.0  
> **Date**: 2026-04-03  
> **Style**: Tufte-inspired (최소주의, 높은 데이터-잉크 비율, 타이포그래피 중심)

---

## 1. 디자인 철학

Edward Tufte의 원칙을 비영리 웹사이트에 적용한다:

- **최소주의**: 불필요한 장식 요소 배제, 콘텐츠 자체가 주인공
- **높은 데이터-잉크 비율**: 모든 시각 요소는 정보를 전달하는 역할
- **타이포그래피 우선**: 깔끔한 서체와 여백으로 가독성 극대화
- **정직한 시각화**: 왜곡 없는 차트, 적절한 축 비율
- **차분한 색상**: 신뢰감을 주는 뮤트 톤 팔레트

## 2. 색상 체계

### Primary Palette

| 이름 | HEX | RGB | 용도 |
|------|-----|-----|------|
| **Deep Teal** | `#2C5F5C` | 44, 95, 92 | 주요 브랜드 색상, 헤더, 강조 |
| **Warm Coral** | `#E07A5F` | 224, 122, 95 | CTA 버튼, 하트/후원 아이콘 |
| **Sand Gold** | `#D4A574` | 212, 165, 116 | 보조 강조, 뱃지, 하이라이트 |

### Neutral Palette

| 이름 | HEX | RGB | 용도 |
|------|-----|-----|------|
| **Charcoal** | `#2D3436` | 45, 52, 54 | 본문 텍스트 |
| **Warm Gray** | `#636E72` | 99, 110, 114 | 보조 텍스트, 캡션 |
| **Light Gray** | `#B2BEC3` | 178, 190, 195 | 경계선, 구분선 |
| **Off White** | `#F5F1EB` | 245, 241, 235 | 배경 (따뜻한 톤) |
| **Pure White** | `#FFFFFF` | 255, 255, 255 | 카드 배경 |

### Semantic Colors

| 이름 | HEX | 용도 |
|------|-----|------|
| **Success** | `#27AE60` | 성공/완료 상태 |
| **Info** | `#2980B9` | 정보/안내 |
| **Warning** | `#F39C12` | 주의/경고 |
| **Error** | `#C0392B` | 오류/위험 |

## 3. 타이포그래피

### 서체 스택

```css
/* 제목 */
font-family: 'Noto Serif KR', 'Georgia', serif;

/* 본문 */
font-family: 'Noto Sans KR', 'Pretendard', -apple-system, sans-serif;

/* 코드/데이터 */
font-family: 'JetBrains Mono', 'D2Coding', monospace;
```

### 크기 체계 (Major Third Scale, 1.250)

| 레벨 | 크기 | 용도 |
|------|------|------|
| Display | 3.052rem (48.83px) | 히어로 타이틀 |
| H1 | 2.441rem (39.06px) | 페이지 제목 |
| H2 | 1.953rem (31.25px) | 섹션 제목 |
| H3 | 1.563rem (25.00px) | 하위 제목 |
| H4 | 1.25rem (20.00px) | 소제목 |
| Body | 1rem (16px) | 본문 |
| Small | 0.8rem (12.80px) | 캡션, 주석 |

### Tufte 스타일 규칙

- 제목: Serif (Noto Serif KR) — 권위감과 신뢰
- 본문: Sans-serif (Noto Sans KR) — 화면 가독성
- 줄간격: 본문 1.7, 제목 1.2
- 문단 최대 너비: 65ch (가독성 최적 폭)
- 마진 노트 영역: 우측 30% 활용 (넓은 화면에서)

## 4. 간격 체계

8px 기반 그리드:

| 토큰 | 값 | 용도 |
|------|------|------|
| `--space-xs` | 4px | 아이콘-텍스트 간격 |
| `--space-sm` | 8px | 요소 내부 패딩 |
| `--space-md` | 16px | 카드 패딩 |
| `--space-lg` | 32px | 섹션 간 간격 |
| `--space-xl` | 64px | 페이지 섹션 구분 |
| `--space-2xl` | 128px | 히어로 여백 |

## 5. 로고 사용 가이드

### 변형

| 변형 | 파일 | 용도 |
|------|------|------|
| Primary | `logo/mumuki-logo-primary.svg` | 밝은 배경 위 기본 사용 |
| White | `logo/mumuki-logo-white.svg` | 어두운/컬러 배경 위 |
| Icon Only | `logo/mumuki-logo-icon.svg` | 파비콘, 소셜 프로필, 좁은 공간 |
| Favicon | `logo/mumuki-favicon.svg` | 브라우저 탭 아이콘 |

### 보호 영역

로고 주변에 로고 아이콘 높이의 50% 이상 여백을 확보할 것.

### 금지 사항

- 로고 비율 변형 금지
- 로고 위에 텍스트 오버레이 금지
- 브랜드 색상 외의 색상 적용 금지
- 복잡한 배경 위 사용 시 화이트 배경 박스 적용

## 6. SVG 일러스트레이션 가이드 (Tufte Style)

### 원칙

- 선 두께: 1~2px (가는 선 위주)
- 색상: 브랜드 팔레트 내에서만 사용, 최대 3~4색
- 채우기: 반투명(opacity 0.1~0.3) 또는 단색, 그라데이션 최소화
- 텍스트 레이블: 직접 배치 (범례 대신 데이터 옆에 라벨링)
- 여백: 충분한 여백, 밀집 배치 지양
- 장식: 그림자, 3D 효과, 글로우 등 일절 사용 금지

### 파일 목록

```
images/
├── hero/hero-banner.svg              — 메인 히어로 일러스트
├── programs/library-companion.svg    — 도서관 동행 프로그램
├── programs/career-education.svg     — 직업 교육 프로그램
├── programs/direct-donation.svg      — 다이렉트 기부 흐름도
├── about/organization-chart.svg      — 조직도
├── about/mission-diagram.svg         — 미션/비전 다이어그램
├── transparency/donation-flow.svg    — 기부금 흐름도 (그림2 재구성)
├── transparency/info-flow.svg        — 정보 흐름도 (그림1 재구성)
├── participate/support-methods.svg   — 후원 방법 안내
├── icons/icon-*.svg                  — UI 아이콘 세트
```
