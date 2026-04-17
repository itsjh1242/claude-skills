---
version: 1.0.0
updated: 2026-04-17
changelog:
  - "1.0.0: 최초 릴리스"
---

# Dev Starter

결합형 개발 환경 셋업: **LLM Wiki** (Karpathy-style 지식 베이스)와 **Claude Harness** (에이전트 팀 & 스킬 아키텍트)를 한 번에 설치한다.
**셋업 전용** 스킬이다. 설치 후에는 CLAUDE.md 규칙이 모든 위키 및 하네스 운영을 자동으로 관장한다.

## 설치 (Setup)

### Step 0 — 버전 확인 (필수 — 건너뛰지 않는다)
다른 작업 전에 반드시 아래 단계를 모두 수행한다:
1. 현재 프로젝트의 `.claude/skills/dev-starter/SKILL.md` 파일을 읽는다
2. 파일이 존재하지 않으면 → 신규 설치. Step 1로 이동.
3. 파일이 존재하면 → YAML frontmatter에서 `version:` 값을 추출
   - `version` 필드가 없으면 → `0.0.0`으로 간주
4. 로컬 버전과 `1.0.0` (이 파일의 버전)을 비교
5. 서브 스킬 버전도 별도 확인:
   - `.claude/skills/llm-wiki/SKILL.md` 읽기 → 버전 추출 (없으면 `0.0.0`)
   - `.claude/skills/harness-setup/SKILL.md` 읽기 → 버전 추출 (없으면 `0.0.0`)
6. 상태 테이블 출력:

   | 스킬 | 로컬 | 원격 | 액션 |
   |------|------|------|------|
   | dev-starter | v{local} | v1.0.0 | {install/update/up-to-date} |
   | llm-wiki | v{local} | v1.0.0 | {install/update/up-to-date} |
   | harness-setup | v{local} | v1.0.0 | {install/update/up-to-date} |

7. 세 스킬 모두 "up-to-date" 이면 → "Dev Starter는 이미 최신 버전입니다 (v1.0.0)" 출력 후 중단.
8. 그 외 → Step 1 진행 (필요한 액션만 수행).

### 업데이트 흐름
이전 버전에서 업데이트할 때:
1. 업데이트가 필요한 각 서브 스킬에 대해 해당 설치 단계를 재실행
2. `.claude/skills/dev-starter/SKILL.md`를 이 파일로 덮어쓰기
3. 이전 버전과 현재 버전 사이의 changelog 항목을 사용자에게 표시
4. 기존 데이터 (`.dev/wiki/`, `.claude/agents/`) 보존 — 삭제하지 않음
5. "Dev Starter 업데이트 완료: v{old} → v{new}" 출력

---

### Step 1 — LLM Wiki 설치

#### 1-A. 위키 디렉토리 생성
프로젝트 루트에 다음 경로 생성:

```
.dev/wiki/
├── raw/
│   ├── logs/
│   └── docs/
├── wiki/
│   ├── patterns/
│   ├── decisions/
│   └── insights/
├── graphify-out/
├── index.md
└── log.md
```

`.dev/wiki/index.md` 초기화:

```markdown
# Wiki Index

> Last updated: {오늘 날짜}

## Raw — Dev Logs

| File | Feature | Status | Created |
|------|---------|--------|---------|

## Raw — Dev Docs

| File | Title | Type | Created |
|------|-------|------|---------|

## Patterns

| File | Topic | Last updated |
|------|-------|--------------|

## Decisions

| File | Topic | Last updated |
|------|-------|--------------|

## Insights

| File | Topic | Last updated |
|------|-------|--------------|
```

`.dev/wiki/log.md` 초기화:

```markdown
# Wiki Log

Operation history in chronological order.
Format: `## [{YYYY-MM-DD}] {operation} | {target or result summary}`

---

## [{오늘 날짜}] init | Wiki structure initialized

- Directory structure created
- index.md, log.md initialized
```

#### 1-B. llm-wiki 스킬 파일 설치
`.claude/skills/llm-wiki/` 디렉토리 생성.
다음 URL에서 fetch해서 `.claude/skills/llm-wiki/SKILL.md`로 저장:
`https://raw.githubusercontent.com/itsjh1242/claude-skills/main/llm-wiki/SKILL.md`

---

### Step 2 — Claude Harness 설치

#### 2-A. harness 디렉토리 생성
다음 디렉토리를 프로젝트 루트에 생성 (이미 존재하면 건너뜀):
- `.claude/agents/`
- `.claude/skills/harness/`
- `.claude/skills/harness/references/`
- `.claude/skills/harness-setup/`

#### 2-B. harness 파일 fetch 및 설치
`https://raw.githubusercontent.com/revfactory/harness/main/` 에서 각 파일을 fetch해서 저장:

| 원격 경로 | 로컬 경로 |
|---|---|
| `skills/harness/SKILL.md` | `.claude/skills/harness/SKILL.md` |
| `skills/harness/references/agent-design-patterns.md` | `.claude/skills/harness/references/agent-design-patterns.md` |
| `skills/harness/references/orchestrator-template.md` | `.claude/skills/harness/references/orchestrator-template.md` |
| `skills/harness/references/team-examples.md` | `.claude/skills/harness/references/team-examples.md` |
| `skills/harness/references/skill-writing-guide.md` | `.claude/skills/harness/references/skill-writing-guide.md` |
| `skills/harness/references/skill-testing-guide.md` | `.claude/skills/harness/references/skill-testing-guide.md` |
| `skills/harness/references/qa-agent-guide.md` | `.claude/skills/harness/references/qa-agent-guide.md` |

harness-setup 스킬 파일 fetch 및 저장:
`https://raw.githubusercontent.com/itsjh1242/claude-skills/main/harness-setup/SKILL.md` → `.claude/skills/harness-setup/SKILL.md`

---

### Step 3 — 스킬 파일 설치 및 CLAUDE.md 업데이트

#### 3-A. dev-starter 스킬 파일 설치
`.claude/skills/dev-starter/` 디렉토리 생성.
이 SKILL.md를 fetch해서 `.claude/skills/dev-starter/SKILL.md`로 저장:
`https://raw.githubusercontent.com/itsjh1242/claude-skills/main/dev-starter/SKILL.md`

#### 3-B. CLAUDE.md에 추가
프로젝트 루트의 `CLAUDE.md` (없으면 생성) 에 다음 섹션 추가.
이미 존재하면 전체 교체한다.

(영문 SKILL.md의 Step 3-B 내용과 동일한 CLAUDE.md 블록을 삽입한다)

### Step 4 — 검증
- `.dev/wiki/raw/logs/` 존재 여부 확인
- `.dev/wiki/raw/docs/` 존재 여부 확인
- `.dev/wiki/wiki/patterns/` 존재 여부 확인
- `.dev/wiki/wiki/decisions/` 존재 여부 확인
- `.dev/wiki/wiki/insights/` 존재 여부 확인
- `.dev/wiki/graphify-out/` 존재 여부 확인
- `.dev/wiki/index.md` 존재 여부 확인
- `.dev/wiki/log.md` 존재 여부 확인
- `.claude/agents/` 존재 여부 확인
- `.claude/skills/harness/SKILL.md` 존재 여부 확인
- `.claude/skills/harness/references/` 에 6개 참조 파일 존재 여부 확인
- `.claude/skills/harness-setup/SKILL.md` 존재 여부 확인
- `.claude/skills/llm-wiki/SKILL.md` 존재 여부 확인
- `.claude/skills/dev-starter/SKILL.md` 존재 여부 확인
- `CLAUDE.md`에 `# LLM Wiki` 섹션 존재 여부 확인
- `CLAUDE.md`에 `# Harness` 섹션 존재 여부 확인
- 신규 설치 시 → "Dev Starter 셋업 완료 (v1.0.0)" 출력
- 업데이트 시 → "Dev Starter 업데이트 완료: v{old} → v{new}" 출력

---

## 역할
LLM Wiki (Karpathy-style 지식 베이스 + Graphify)와 Claude Harness (에이전트 팀 & 스킬 아키텍트)를 한 번에 설치하는 셋업 전용 스킬. 설치 후 CLAUDE.md 규칙이 모든 위키 운영을 담당하고, harness 스킬이 에이전트 팀 오케스트레이션을 처리한다.

## 트리거 조건
사용자가 전체 dev-starter 환경 설치 또는 업데이트를 명시적으로 요청할 때만 활성화한다. 개별 스킬 운영 (위키 로깅, 하네스 구성 등)은 설치된 스킬과 CLAUDE.md 규칙이 담당하므로 이 셋업 스킬이 다시 트리거될 필요가 없다.
