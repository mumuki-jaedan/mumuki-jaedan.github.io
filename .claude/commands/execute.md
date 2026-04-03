mumuki.net 웹사이트의 계획된 작업을 실행합니다.

## 수행 절차

1. **계획 확인**: PLAN.md에서 현재 실행할 작업을 확인한다.
2. **디자인 시스템 참조**: `assets/design-system.md`의 색상, 타이포그래피, 간격 체계를 준수한다.
3. **작업 실행**: 계획된 순서대로 작업을 수행한다.
4. **Tufte 스타일 준수**:
   - 최소주의: 불필요한 장식 배제
   - 타이포그래피 우선: 깔끔한 서체와 여백
   - 브랜드 색상만 사용 (Deep Teal #2C5F5C, Warm Coral #E07A5F, Sand Gold #D4A574)
5. **PROGRESS.md 업데이트**: 각 작업 완료 시 즉시 진행 현황을 갱신한다.

## 실행 규칙

### Quarto 파일 (.qmd) 작성 시
- YAML front matter 필수 (title, description, lang: ko)
- 이미지는 `images/` 폴더의 SVG 파일 참조
- 한국어 우선, 기술 용어는 원어 유지

### SVG 작성 시
- Tufte 스타일: 선 두께 1-2px, 채우기 반투명
- 브랜드 팔레트만 사용, 최대 3-4색
- 그림자, 3D 효과, 글로우 금지

### SCSS 작성 시
- BEM 네이밍
- 8px 그리드 시스템
- `_brand.yml` 변수 활용

## 진행 보고

작업 진행 중 다음을 보고한다:
- 현재 작업 항목
- 완료된 파일 목록
- 발견된 이슈

사용자의 입력: $ARGUMENTS
