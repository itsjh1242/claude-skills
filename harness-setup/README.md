# harness-setup

Fetches and installs [Claude Harness](https://github.com/revfactory/harness) (Agent Team & Skill Architect) into any project with a single command.

---

## What it does

- Fetches the latest harness SKILL.md and all 6 reference files from [revfactory/harness](https://github.com/revfactory/harness)
- Installs them to `.claude/skills/harness/` and `.claude/skills/harness/references/`
- Creates `.claude/agents/` directory for agent definitions
- Adds a `# Harness` trigger entry to your project's `CLAUDE.md`
- Tracks setup version for future updates

After installation, the harness skill handles all agent team design and multi-agent orchestration automatically.

---

## Installation

Paste the following prompt into Claude Code to install harness-setup:

```
Install the harness-setup skill by following the setup instructions here:
https://raw.githubusercontent.com/itsjh1242/claude-skills/main/harness-setup/SKILL.md
```

Claude will fetch the SKILL.md, install harness from revfactory/harness, and update your `CLAUDE.md` automatically.

**Already installed?** Paste the same prompt again — the skill will compare versions and update only if a newer version is available.

---

## File structure created after setup

```
{project-root}/
├── .claude/
│   ├── agents/               ← harness will populate this
│   └── skills/
│       ├── harness/
│       │   ├── SKILL.md      ← harness meta-skill (from revfactory/harness)
│       │   └── references/
│       │       ├── agent-design-patterns.md
│       │       ├── orchestrator-template.md
│       │       ├── team-examples.md
│       │       ├── skill-writing-guide.md
│       │       ├── skill-testing-guide.md
│       │       └── qa-agent-guide.md
│       └── harness-setup/
│           └── SKILL.md      ← version tracking for this setup skill
└── CLAUDE.md                 ← # Harness section added
```

---

## After installation

Once harness is installed, say things like:

- `하네스 구성해줘` / `build a harness for this project`
- `design an agent team for this domain`
- `set up multi-agent orchestration`

Harness will analyze your project, design an agent team architecture, and generate the agent definitions and skill files automatically.
