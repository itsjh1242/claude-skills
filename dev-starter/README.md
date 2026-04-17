# dev-starter

Complete development environment setup in one command: installs **LLM Wiki** (Karpathy-style knowledge base) + **Claude Harness** (Agent Team & Skill Architect).

---

## What it does

- **LLM Wiki**: Sets up a structured knowledge base (`.dev/wiki/`) with dev logs, docs, wiki pages, and a knowledge graph (Graphify). Adds wiki operation rules to `CLAUDE.md` so Claude automatically manages knowledge capture.
- **Claude Harness**: Fetches and installs [revfactory/harness](https://github.com/revfactory/harness) — a meta-skill for designing and scaffolding multi-agent systems. Adds a harness trigger to `CLAUDE.md`.

Both are installed in one step, with version tracking for each.

---

## Installation

Paste the following prompt into Claude Code to install dev-starter:

```
Install the dev-starter skill by following the setup instructions here:
https://raw.githubusercontent.com/itsjh1242/claude-skills/main/dev-starter/SKILL.md
```

Claude will install both LLM Wiki and Claude Harness, update your `CLAUDE.md`, and report the status of each component.

**Already installed?** Paste the same prompt again — each component is version-checked independently and only updated if a newer version is available.

---

## File structure created after setup

```
{project-root}/
├── .dev/
│   └── wiki/
│       ├── raw/logs/         ← feature dev logs
│       ├── raw/docs/         ← design specs, architecture docs
│       ├── wiki/patterns/    ← recurring patterns
│       ├── wiki/decisions/   ← design decisions
│       ├── wiki/insights/    ← cross-analysis insights
│       ├── graphify-out/     ← knowledge graph
│       ├── index.md
│       └── log.md
├── .claude/
│   ├── agents/               ← harness will populate this
│   └── skills/
│       ├── llm-wiki/
│       │   └── SKILL.md
│       ├── harness/
│       │   ├── SKILL.md
│       │   └── references/   ← 6 harness reference files
│       ├── harness-setup/
│       │   └── SKILL.md
│       └── dev-starter/
│           └── SKILL.md
└── CLAUDE.md                 ← # LLM Wiki + # Harness sections added
```

---

## After installation

**LLM Wiki** — Claude will suggest logging completed features, and respond to wiki operations:
- Log a dev session → `wiki log`
- Save a design doc → `wiki doc`
- Build wiki pages from logs → `wiki ingest`
- Build knowledge graph → `wiki graphify`

**Harness** — Claude will respond to agent team design requests:
- `하네스 구성해줘` / `build a harness for this project`
- `design an agent team for {domain}`

---

## Install individually

Prefer to install just one?

- **LLM Wiki only**: [llm-wiki](../llm-wiki/README.md)
- **Harness only**: [harness-setup](../harness-setup/README.md)
