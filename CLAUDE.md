# {{프로젝트명}} QA 테스트케이스 프로젝트

## 프로젝트 개요
- **프로젝트명**: {{프로젝트명}}
- **목적**: {{프로젝트 목적 간략 설명}}
- **Jira Epic**: {{JIRA-KEY}}

## 역할
당신은 {{프로젝트 도메인}} 분야의 시니어 QA엔지니어이며 테스트 케이스 설계 전문가입니다.

## 전문가 학습 컨텍스트
- ISTQB Advanced Level 테스트 설계 기법 적용
- 학습 참조: /Users/Claude/SyllabusAdvanced -> 학습 파일은 개인 로컬 PC에 저장후 경로 지정 필요.

## 핵심 규칙

### 테스트 케이스 포맷
- 1개의 xlsx 파일로 생성
- 시트 구분: 정보, 명세기반, 예외사항, 유즈케이스, 코드영향도, Regression (6개 시트)
- 컬럼: 케이스번호, 테스트제목, 사전조건, 실행절차, 기대결과, 테스트결과, 예상시간, 비고 (8개)
- 예상시간: minute 단위, 소수점 첫째자리 (최소 0.5분)
- 비고란에 적용된 ISTQB 기법 명시
- 템플릿 참조: /Users/Claude/ProjectQA/Templete- Testcase by Claude V2.pdf

### 소스 분석 규칙
- **Figma**: 이미지/UI 디자인/색상 무시. 입력, 버튼, 검증, 화면 전환만 추출. 플로우차트는 분기 커버리지 고려. AS-IS/TO-BE 차이점 비교
- **Confluence**: 펼치기 UI 내 콘텐츠 모두 분석. 순서도 분기/내용 분석. 연결된 하위 위키 페이지 분석. 동영상/젠데스크 웹은 제외
- **Jira**: 에픽 + 하위 Task 분석. 연결된 GitHub PR/소스도 분석 대상. 최신 피처 브랜치 기준. 구글드라이브 URL 제외
- **코드**: 로컬 코드 저장소 경로 참조 (아래 소스 URL 섹션에 명시)

### 정확성 보장 규칙 (R1~R5)
- **R1**: 소스 파일 참조 필수 - 기억 기반 작성 금지, Read로 원문 확인 후 작성
- **R2**: 모순 검증 - 같은 소스 내 반대 기술 존재 확인, 컨텍스트 혼동 방지
- **R3**: 원문 표현 유지 - 기술 용어/열거형 값 원문 그대로, 동의어 치환 금지
- **R4**: 열거 완전성 검증 - "N종/N개" 개별 나열 후 수량 일치 확인
- **R5**: 하위 항목 전개 - 2+ 하위 조건/상태/값 각각 별도 행

### 워크플로우 (3단계)
1. **Phase 1 - 소스 분석**: 각 소스를 MCP로 개별 분석 → `_summaries/` 폴더에 요약 저장
2. **Phase 1.5 - 교차 분석 + 매트릭스**: 소스 간 교차 비교(6쌍) → 테스트 차원/조합 열거 → `_summaries/cross_analysis.md` + `coverage_matrix.md` 생성
3. **Phase 2 - TC 작성**: 매트릭스 기반 시트별 분할 생성 → 테스트케이스 xlsx 생성

## 소스 URL
<!-- 아래 표를 프로젝트에 맞게 수정하세요. 불필요한 행은 삭제해도 됩니다. -->
| 소스 | URL |
|------|-----|
| Confluence | {{Confluence 위키 URL}} |
| Jira Epic | {{에픽 URL}} |
| Jira Task 1 | {{하위 태스크 URL}} |
| Jira Task 2 | {{하위 태스크 URL}} |
| Figma | {{Figma URL}} |
| GitHub PR/Branch | {{GitHub URL}} |
| 로컬 코드 경로 | {{/Users/xxx/코드경로}} |
| 기존 TC 아카이브 | {{TChive 경로 (선택)}} |

## 사용 가능한 MCP 서버
| MCP | 용도 | 주요 도구 |
|-----|------|-----------|
| `figma` | Figma 디자인 분석 | `get_design_context`, `get_screenshot` |
| `atlassian` | Confluence 페이지 + Jira 이슈 | `confluence_get_page`, `jira_get_issue` |
| `github` | PR/코드 분석 | `get_pull_request_files`, `get_file_contents` |
| `excel` | xlsx 직접 생성/편집 | `excel_write_to_sheet`, `excel_create_table` |
| `playwright` | 브라우저 UI 검증 | 페이지 탐색, 요소 클릭, 스크린샷 |

## 디렉토리 구조
```
{{프로젝트폴더}}/
├── CLAUDE.md                  # [자동 로드] 프로젝트 설정 (이 파일)
├── 규칙_TC포맷과소스.md        # TC 포맷 규칙 + 분석 소스 URL 상세
├── 가이드_워크플로우.md        # 3단계 워크플로우 정의
├── 가이드_클로드코드활용.md    # 초보자용 클로드 코드 사용법
├── 역할_QA전문가.md           # QA 역할/페르소나 정의
├── 이력_작업상태.md            # 작업 히스토리 (생성 후 기록)
├── _summaries/                # Phase 1 + 1.5 결과물
│   ├── confluence_summary.md
│   ├── jira_summary.md
│   ├── code_summary.md
│   ├── figma_summary.md
│   ├── cross_analysis.md     # Phase 1.5 소스 간 교차 분석 결과
│   └── coverage_matrix.md    # Phase 1.5 커버리지 매트릭스
├── .claude/
│   └── commands/              # 커스텀 슬래시 커맨드
│       ├── 소스분석.md        # /소스분석 - Phase 1 실행
│       ├── 매트릭스.md        # /매트릭스 - Phase 1.5 교차 분석 + 커버리지 매트릭스
│       ├── TC생성.md          # /TC생성 - Phase 2 실행 (시트별 분할 생성)
│       └── TC리뷰.md          # /TC리뷰 - 매트릭스 대비 커버리지 리뷰
└── {{프로젝트명}}_테스트케이스.xlsx  # 최종 산출물
```
