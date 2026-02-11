# /TC생성 - Phase 2: 테스트케이스 xlsx 생성

생성 옵션: $ARGUMENTS
(예: v3, 시트지정:명세기반, 추가:네트워크오류)

## 실행 절차

### Step 1: 컨텍스트 수집
1. `_summaries/` 폴더의 요약 파일 4종을 모두 읽기
2. `규칙_TC포맷과소스.md`에서 포맷 조건 확인
3. `역할_QA전문가.md`에서 역할 컨텍스트 확인
4. 템플릿 참조: `/Users/Claude/ProjectQA/Templete- Testcase by Claude V2.pdf`

### Step 2: 테스트케이스 설계 (플랜모드 권장)
- ISTQB Advanced Level 기법 적용:
  - 동치 분할 (Equivalence Partitioning)
  - 경계값 분석 (Boundary Value Analysis)
  - 조건 기반 테스팅 (Condition-based Testing)
  - 상태 전이 테스팅 (State Transition Testing)
  - 에러 추측 (Error Guessing)
  - 리스크 기반 우선순위 (Risk-based Prioritization)
- 각 시트별 커버리지를 확인하며 설계

### Step 3: xlsx 생성
- Excel MCP(`excel` 서버)를 사용하여 직접 xlsx 생성
- MCP 사용 불가 시 Python openpyxl 스크립트로 대체
- 파일명: `서포터WAi_테스트케이스_{버전}.xlsx`

### Step 4: 검증
- 각 시트별 케이스 수 집계
- 비고란에 적용 기법 명시 여부 확인
- 예상시간 입력 여부 확인 (최소 0.5분, 소수점 첫째자리)

## 시트 구성 (6개)
| 시트명 | 내용 | 주요 기법 |
|--------|------|-----------|
| 정보 | 프로젝트 개요, TC 통계, 소스 URL | - |
| 명세기반 | 기능 요구사항 기반 TC | 동치분할, 경계값, 조건기반 |
| 예외사항 | 에러/예외 상황 TC | 에러추측, 부정테스팅 |
| 유즈케이스 | 사용자 시나리오 TC | 상태전이, 분기커버리지 |
| 코드영향도 | 코드 변경 기반 TC | 데이터흐름, 영향도분석 |
| Regression | 기존 기능 회귀 TC | 리스크기반 우선순위 |

## 컬럼 (8개)
케이스번호, 테스트제목, 사전조건, 실행절차, 기대결과, 테스트결과, 예상시간, 비고
