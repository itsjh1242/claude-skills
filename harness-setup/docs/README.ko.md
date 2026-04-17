# harness-setup

[Claude Harness](https://github.com/revfactory/harness) (에이전트 팀 & 스킬 아키텍트)를 모든 프로젝트에 한 번에 설치한다.

---

## 기능 설명

- [revfactory/harness](https://github.com/revfactory/harness)에서 최신 harness SKILL.md와 6개 참조 파일을 fetch
- `.claude/skills/harness/` 및 `.claude/skills/harness/references/`에 설치
- 에이전트 정의를 위한 `.claude/agents/` 디렉토리 생성
- 프로젝트 `CLAUDE.md`에 `# Harness` 트리거 항목 추가
- 향후 업데이트를 위한 셋업 버전 추적

설치 후에는 harness 스킬이 에이전트 팀 설계와 멀티 에이전트 오케스트레이션을 자동으로 처리한다.

---

## 설치

다음 프롬프트를 Claude Code에 붙여넣으면 harness-setup이 설치된다:

```
Install the harness-setup skill by following the setup instructions here:
https://raw.githubusercontent.com/itsjh1242/claude-skills/main/harness-setup/SKILL.md
```

Claude가 SKILL.md를 읽고 revfactory/harness에서 harness를 설치하며, `CLAUDE.md`를 자동으로 업데이트한다.

**이미 설치했다면?** 동일한 프롬프트를 다시 붙여넣으면 된다 — 버전을 비교해서 새 버전이 있을 때만 업데이트된다.

---

## 설치 후 생성되는 파일 구조

```
{project-root}/
├── .claude/
│   ├── agents/               ← harness가 채운다
│   └── skills/
│       ├── harness/
│       │   ├── SKILL.md      ← harness 메타 스킬 (revfactory/harness 출처)
│       │   └── references/
│       │       ├── agent-design-patterns.md
│       │       ├── orchestrator-template.md
│       │       ├── team-examples.md
│       │       ├── skill-writing-guide.md
│       │       ├── skill-testing-guide.md
│       │       └── qa-agent-guide.md
│       └── harness-setup/
│           └── SKILL.md      ← 이 셋업 스킬의 버전 추적
└── CLAUDE.md                 ← # Harness 섹션 추가됨
```

---

## 설치 후 사용법

harness 설치 후, 다음과 같이 요청하면 된다:

- `하네스 구성해줘` / `build a harness for this project`
- `design an agent team for this domain`
- `에이전트 팀 설계해줘`

harness가 프로젝트를 분석하고, 에이전트 팀 아키텍처를 설계하며, 에이전트 정의 파일과 스킬 파일을 자동으로 생성한다.
