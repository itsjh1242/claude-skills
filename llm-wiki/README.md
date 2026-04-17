# llm-wiki

Karpathy-style LLM Wiki + Graphify knowledge base setup skill for development projects.

---

## What it does

Installs a structured development knowledge base into any project:

- **Raw Logs** — Feature-scoped dev logs capturing goals, decisions, problems, and solutions
- **Raw Docs** — Design specs, feature specs, architecture docs, and other dev documents
- **Wiki Pages** — Cross-analyzed patterns, decisions, and insights derived from raw data
- **Knowledge Graph** — JSON-based graph connecting all wiki entities (Graphify)
- **Operations** — Log, Doc, Ingest, Graphify, Query, Lint — all governed by CLAUDE.md rules post-setup

---

## Installation

Paste the following prompt into Claude Code to install llm-wiki:

```
Install the llm-wiki skill by following the setup instructions here:
https://raw.githubusercontent.com/itsjh1242/claude-skills/main/llm-wiki/SKILL.md
```

Claude will read the SKILL.md file and set up the required directories, install the skill, and update your `CLAUDE.md` automatically.

**Already installed?** Paste the same prompt again — the skill will compare versions and update only if a newer version is available.

---

## File structure created after setup

```
{project-root}/
├── .claude/
│   └── skills/
│       └── llm-wiki/
│           └── SKILL.md
├── .dev/
│   └── wiki/
│       ├── raw/
│       │   ├── logs/           # Feature-scoped dev logs
│       │   └── docs/           # Dev documents
│       ├── wiki/
│       │   ├── patterns/       # Recurring dev patterns
│       │   ├── decisions/      # Design & implementation decisions
│       │   └── insights/       # Cross-analysis insights
│       ├── graphify-out/       # Knowledge Graph output
│       ├── index.md            # Full page index
│       └── log.md              # Operation history
└── CLAUDE.md                   ← LLM Wiki section added
```
