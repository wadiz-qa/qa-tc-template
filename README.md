# QA 테스트케이스 설계 템플릿 (Claude Code)

Claude Code를 활용하여 QA 테스트케이스를 효율적으로 설계하기 위한 프로젝트 템플릿입니다.

## 이 템플릿이 하는 일

1. **Figma / Confluence / Jira / GitHub** 명세를 MCP로 직접 분석
2. 소스별 요약을 자동 생성 (컨텍스트 윈도우 최적화)
3. **소스 간 교차 분석(6쌍)** + **커버리지 매트릭스** 생성으로 누락 방지
4. ISTQB Advanced Level 기법(11종)을 적용한 **테스트케이스 xlsx** 자동 생성
5. 매트릭스 대비 1:1 커버리지 리뷰 및 보완 제안

## 빠른 시작

```bash
# 1. 템플릿 clone
git clone https://github.com/sungkyu-QA/qa-tc-template.git 내프로젝트명
cd 내프로젝트명
rm -rf .git   # 기존 git 이력 제거

# 2. 소스 URL 설정 (2개 파일만 수정)
#    - CLAUDE.md          → 프로젝트명, 소스 URL 변경
#    - 규칙_TC포맷과소스.md → 소스 URL 붙여넣기

# 3. Claude Code 실행
claude

# 4. 슬래시 커맨드로 TC 생성 (3-Phase)
#    /소스분석 all     → Phase 1: 소스 4종 병렬 분석
#    /매트릭스         → Phase 1.5: 교차 분석 + 커버리지 매트릭스
#    /TC생성 v1        → Phase 2: 매트릭스 기반 시트별 TC 생성
#    /TC리뷰 latest    → 매트릭스 대비 커버리지 리뷰
```

## 3-Phase 워크플로우

```
Phase 1 (소스분석)     Phase 1.5 (매트릭스)     Phase 2 (TC생성)
   [소스별 요약]          [커버리지 기준점]        [시트별 분할 생성]

소스 4종 분석     →   교차분석+매트릭스   →   시트별 분할 생성
  ↓                      ↓                      ↓
_summaries/          cross_analysis.md        xlsx 시트별 순차 생성
  4개 요약 파일      coverage_matrix.md       + 매트릭스 대비 검증
```

### Phase 1.5가 해결하는 문제
- **AI의 완결성 판단 편향**: 대표 표본만 생성하고 멈추는 문제 → 수치 목표로 방지
- **소스 간 불일치 누락**: 개별 소스만 보면 발견 불가 → 6쌍 교차 비교로 식별
- **출력 토큰 한계**: 한 번에 전체 생성 불가 → 시트별 분할 생성으로 우회

## 사전 준비

### Claude Code 설치
```bash
npm install -g @anthropic-ai/claude-code
```

### MCP 서버 설정
`~/.claude/settings.json`의 `mcpServers`에 아래를 추가:

```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "figma-developer-mcp"],
      "env": { "FIGMA_API_KEY": "발급받은_키" }
    },
    "excel": {
      "command": "npx",
      "args": ["--yes", "@negokaz/excel-mcp-server"],
      "env": { "EXCEL_MCP_PAGING_CELLS_LIMIT": "4000" }
    },
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

> Atlassian(Jira/Confluence)과 GitHub MCP는 Claude Code에 기본 내장되어 있습니다.

## 파일 구조

```
프로젝트폴더/
├── CLAUDE.md                   # [자동 로드] 프로젝트 설정 ← 소스 URL 수정
├── 규칙_TC포맷과소스.md         # TC 포맷 규칙 + 소스 URL ← 소스 URL 수정
├── 가이드_워크플로우.md         # 3-Phase 워크플로우 (교차 분석 + 매트릭스 포함)
├── 가이드_클로드코드활용.md     # 초보자용 Claude Code 사용법
├── 역할_QA전문가.md            # QA 역할/페르소나 정의 (ISTQB 11종 기법)
├── _summaries/                 # Phase 1 + 1.5 분석 결과 (자동 생성됨)
│   ├── confluence_summary.md
│   ├── jira_summary.md
│   ├── code_summary.md
│   ├── figma_summary.md
│   ├── cross_analysis.md       # Phase 1.5: 소스 간 교차 분석 결과
│   └── coverage_matrix.md      # Phase 1.5: 커버리지 매트릭스
├── .claude/
│   ├── settings.local.json
│   └── commands/               # 슬래시 커맨드
│       ├── 소스분석.md          # /소스분석 - Phase 1
│       ├── 매트릭스.md          # /매트릭스 - Phase 1.5
│       ├── TC생성.md            # /TC생성 - Phase 2
│       └── TC리뷰.md            # /TC리뷰
└── README.md                   # 이 파일
```

## 슬래시 커맨드

| 커맨드 | 단계 | 설명 |
|--------|------|------|
| `/소스분석 all` | Phase 1 | Confluence, Jira, Figma, Code 4종 병렬 분석 |
| `/소스분석 confluence` | Phase 1 | Confluence만 단독 분석 |
| `/매트릭스` | Phase 1.5 | 소스 교차 분석(6쌍) + 커버리지 매트릭스 생성 |
| `/매트릭스 갱신` | Phase 1.5 | 기존 매트릭스를 요약 파일 변경분에 맞게 업데이트 |
| `/TC생성 v1` | Phase 2 | 매트릭스 기반 시트별 분할 TC 생성 |
| `/TC생성 시트:예외사항` | Phase 2 | 특정 시트만 생성/보완 |
| `/TC리뷰 latest` | 리뷰 | 매트릭스 대비 1:1 커버리지 검증 및 보완 제안 |

## 정확성 보장 규칙 (R1~R5)

AI 생성 오류를 방지하기 위해 전체 Phase에 적용되는 필수 규칙입니다:

| 규칙 | 핵심 |
|------|------|
| **R1** | 소스 파일 참조 필수 — 기억 기반 작성 금지 |
| **R2** | 모순 검증 — 같은 소스 내 반대 기술 확인 |
| **R3** | 원문 표현 유지 — 동의어 치환 금지 |
| **R4** | 열거 완전성 — "N종" 명시 시 N개 전부 나열 |
| **R5** | 하위 항목 전개 — 대표 1개만 쓰고 생략 금지 |

## 활용하는 Claude Code 핵심 기능

| 기능 | 역할 |
|------|------|
| **CLAUDE.md** | 프로젝트 컨텍스트 자동 로드 |
| **슬래시 커맨드** | 반복 작업을 한 줄로 실행 |
| **플랜모드** | TC 설계 전 계획 수립 → 승인 → 실행 |
| **서브에이전트** | 4개 소스를 병렬로 동시 분석 |
| **MCP** | Figma/Jira/Confluence/GitHub 직접 접근 |

> 자세한 사용법은 `가이드_클로드코드활용.md`를 참고하세요.
