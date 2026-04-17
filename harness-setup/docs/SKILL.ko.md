---
version: 1.0.0
updated: 2026-04-17
changelog:
  - "1.0.0: 최초 릴리스"
---

# Harness Setup

[Claude Harness](https://github.com/revfactory/harness) (에이전트 팀 & 스킬 아키텍트)를 현재 프로젝트에 설치하는 스킬.
**셋업 전용** 스킬이다. 설치 후에는 설치된 harness 스킬이 에이전트 팀 설계와 오케스트레이션을 담당한다.

## 설치 (Setup)

### Step 0 — 버전 확인 (필수 — 건너뛰지 않는다)
다른 작업 전에 반드시 아래 단계를 모두 수행한다:
1. 현재 프로젝트의 `.claude/skills/harness-setup/SKILL.md` 파일을 읽는다
2. 파일이 존재하지 않으면 → 신규 설치. Step 1로 이동.
3. 파일이 존재하면 → YAML frontmatter에서 `version:` 값을 추출
   - `version` 필드가 없으면 → `0.0.0`으로 간주
4. 로컬 버전과 `1.0.0` (이 파일의 버전)을 비교:
   - 로컬 < `1.0.0` → **업데이트 흐름** 실행
   - 로컬 = `1.0.0` → "harness-setup는 이미 최신 버전입니다 (v1.0.0)" 출력 후 중단.
5. 비교 결과를 반드시 출력한다: "로컬: v{local} / 원격: v1.0.0 → {action}"

### 업데이트 흐름
이전 버전에서 업데이트할 때:
1. Step 2의 모든 harness 파일을 다시 fetch해서 덮어쓰기
2. `.claude/skills/harness-setup/SKILL.md`를 이 파일로 덮어쓰기
3. `CLAUDE.md`의 `# Harness` 섹션을 아래 Step 3의 최신 내용으로 교체
4. 이전 버전과 현재 버전 사이의 changelog 항목을 사용자에게 표시
5. `.claude/agents/` 디렉토리와 기존 에이전트 정의는 보존 — 삭제하지 않음
6. "harness-setup 업데이트 완료: v{old} → v{new}" 출력

### Step 1 — 디렉토리 생성
다음 디렉토리를 프로젝트 루트에 생성 (이미 존재하면 건너뜀):
- `.claude/agents/`
- `.claude/skills/harness/`
- `.claude/skills/harness/references/`
- `.claude/skills/harness-setup/`

### Step 2 — harness 파일 fetch 및 설치
`https://raw.githubusercontent.com/revfactory/harness/main/` 에서 각 파일을 fetch해서 로컬 경로에 저장:

| 원격 경로 | 로컬 경로 |
|---|---|
| `skills/harness/SKILL.md` | `.claude/skills/harness/SKILL.md` |
| `skills/harness/references/agent-design-patterns.md` | `.claude/skills/harness/references/agent-design-patterns.md` |
| `skills/harness/references/orchestrator-template.md` | `.claude/skills/harness/references/orchestrator-template.md` |
| `skills/harness/references/team-examples.md` | `.claude/skills/harness/references/team-examples.md` |
| `skills/harness/references/skill-writing-guide.md` | `.claude/skills/harness/references/skill-writing-guide.md` |
| `skills/harness/references/skill-testing-guide.md` | `.claude/skills/harness/references/skill-testing-guide.md` |
| `skills/harness/references/qa-agent-guide.md` | `.claude/skills/harness/references/qa-agent-guide.md` |

그 다음, 이 SKILL.md를 fetch해서 `.claude/skills/harness-setup/SKILL.md`로 저장:
`https://raw.githubusercontent.com/itsjh1242/claude-skills/main/harness-setup/SKILL.md`

### Step 3 — CLAUDE.md에 추가
프로젝트 루트의 `CLAUDE.md` (없으면 생성) 에 다음 지침을 추가.
`# Harness` 섹션이 이미 존재하면 전체 교체한다.

````
# Harness

Claude Harness (Agent Team & Skill Architect)가 `.claude/skills/harness/SKILL.md`에 설치되어 있다.

다음 요청 시 harness 스킬을 활성화한다:
- 에이전트 팀 설계 또는 특정 도메인/프로젝트의 멀티 에이전트 시스템 구성
- "하네스 구성해줘", "build a harness", "design an agent team", "하네스 설계"
- 하네스 관련 아키텍처 또는 오케스트레이션 작업

harness가 에이전트 팀을 구성한 후, 해당 도메인의 트리거 규칙과 변경 이력을 담은 `## 하네스: {domain}` 블록이 여기에 추가된다.
````

### Step 4 — 검증
- `.claude/agents/` 존재 여부 확인
- `.claude/skills/harness/SKILL.md` 존재 여부 확인
- `.claude/skills/harness/references/`에 6개 참조 파일 존재 여부 확인
- `.claude/skills/harness-setup/SKILL.md` 존재 여부 확인
- `CLAUDE.md`에 `# Harness` 섹션 존재 여부 확인
- 신규 설치 시 → "Harness Setup 완료 (v1.0.0)" 출력
- 업데이트 시 → "Harness Setup 업데이트 완료: v{old} → v{new}" 출력

---

## 역할
revfactory/harness의 Claude Harness를 모든 프로젝트에 설치하는 셋업 전용 스킬. 설치 후에는 harness 스킬이 에이전트 팀 설계, 오케스트레이션, 멀티 에이전트 스캐폴딩을 자동으로 처리한다.

## 트리거 조건
사용자가 Claude Harness 설치 또는 업데이트를 명시적으로 요청할 때만 활성화한다. 설치 후 운영(하네스 구성, 에이전트 팀 설계 등)은 설치된 harness 스킬이 담당하므로 이 셋업 스킬이 다시 트리거될 필요가 없다.
