# QA 테스트케이스 설계 템플릿 (Claude Code)

Claude Code를 활용하여 QA 테스트케이스를 효율적으로 설계하기 위한 프로젝트 템플릿입니다.

## 이 템플릿이 하는 일

1. **Figma / Confluence / Jira / GitHub** 명세를 MCP로 직접 분석
2. 소스별 요약을 자동 생성 (컨텍스트 윈도우 최적화)
3. ISTQB Advanced Level 기법을 적용한 **테스트케이스 xlsx** 자동 생성
4. 커버리지 갭 리뷰 및 보완 제안

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

# 4. 슬래시 커맨드로 TC 생성
#    /소스분석 all     → 소스 4종 병렬 분석
#    /TC생성 v1        → 테스트케이스 xlsx 생성
#    /TC리뷰 latest    → 커버리지 리뷰
```

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
├── 가이드_워크플로우.md         # Phase 1→2 워크플로우
├── 가이드_클로드코드활용.md     # 초보자용 Claude Code 사용법
├── 역할_QA전문가.md            # QA 역할/페르소나 정의
├── _summaries/                 # Phase 1 분석 결과 (자동 생성됨)
├── .claude/
│   ├── settings.local.json
│   └── commands/               # 슬래시 커맨드
│       ├── 소스분석.md          # /소스분석
│       ├── TC생성.md            # /TC생성
│       └── TC리뷰.md            # /TC리뷰
└── README.md                   # 이 파일
```

## 슬래시 커맨드

| 커맨드 | 단계 | 설명 |
|--------|------|------|
| `/소스분석 all` | Phase 1 | Confluence, Jira, Figma, Code 4종 병렬 분석 |
| `/소스분석 confluence` | Phase 1 | Confluence만 단독 분석 |
| `/TC생성 v1` | Phase 2 | 요약 기반 테스트케이스 xlsx 생성 |
| `/TC생성 시트지정:예외사항` | Phase 2 | 특정 시트만 생성/보완 |
| `/TC리뷰 latest` | 리뷰 | 커버리지 갭 분석 및 보완 제안 |

## 활용하는 Claude Code 핵심 기능

| 기능 | 역할 |
|------|------|
| **CLAUDE.md** | 프로젝트 컨텍스트 자동 로드 |
| **슬래시 커맨드** | 반복 작업을 한 줄로 실행 |
| **플랜모드** | TC 설계 전 계획 수립 → 승인 → 실행 |
| **서브에이전트** | 4개 소스를 병렬로 동시 분석 |
| **MCP** | Figma/Jira/Confluence/GitHub 직접 접근 |

> 자세한 사용법은 `가이드_클로드코드활용.md`를 참고하세요.
