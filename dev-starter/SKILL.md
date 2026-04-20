---
name: dev-starter
description: "Combined development environment setup skill. Installs LLM Wiki (Karpathy-style knowledge base) + Claude Harness (Agent Team & Skill Architect) in one step.\nTRIGGER when: user says 'dev-starter install', 'dev starter setup', 'install dev starter', 'set up dev environment', 'dev 환경 설치', 'dev-starter 설치', or combined install intent for both LLM Wiki and Claude Harness.\nDO NOT TRIGGER when: user asks to install only llm-wiki or only harness individually, or asks about post-setup operations (wiki log, ingest, harness build, etc.)."
version: 1.0.0
updated: 2026-04-17
changelog:
  - "1.0.0: Initial release"
---

# Dev Starter

Combined development environment setup: installs **LLM Wiki** (Karpathy-style knowledge base) and **Claude Harness** (Agent Team & Skill Architect) in one step.
This is a **setup-only** skill. After setup, the CLAUDE.md rules govern all wiki and harness operations automatically.

## Setup

### Step 0 — Version check (MANDATORY — do NOT skip)
You MUST perform all of the following steps before doing anything else:
1. Read the file `.claude/skills/dev-starter/SKILL.md` in the current project
2. If the file does NOT exist → this is a fresh install. Go to Step 1.
3. If the file exists → extract the `version:` value from its YAML frontmatter
   - If the file has no `version` field → treat as `0.0.0`
4. Compare the local version with `1.0.0` (this file's version):
   - Local < `1.0.0` → go to **Update Flow**
   - Local = `1.0.0` → check sub-skill versions separately (see below) and STOP only if all are up to date.
5. Also check sub-skill versions:
   - Read `.claude/skills/llm-wiki/SKILL.md` → extract version (treat missing as `0.0.0`)
   - Read `.claude/skills/harness-setup/SKILL.md` → extract version (treat missing as `0.0.0`)
6. Output a status table:

   | Skill | Local | Remote | Action |
   |-------|-------|--------|--------|
   | dev-starter | v{local} | v1.0.0 | {install/update/up-to-date} |
   | llm-wiki | v{local} | v1.0.0 | {install/update/up-to-date} |
   | harness-setup | v{local} | v1.0.0 | {install/update/up-to-date} |

7. If all three show "up-to-date" → output "Dev Starter is already up to date (v1.0.0)" and STOP.
8. Otherwise → proceed to Step 1, running only the actions needed.

### Update Flow
When updating from an older version:
1. For each sub-skill that needs updating, re-run its installation steps
2. Overwrite `.claude/skills/dev-starter/SKILL.md` with this file
3. Show the user the changelog entries between their old version and the current version
4. Preserve all existing data (`.dev/wiki/`, `.claude/agents/`) — do NOT delete them
5. Output "Dev Starter updated: v{old} → v{new}"

---

### Step 1 — Install LLM Wiki

> **Skip condition**: If the status table from Step 0 shows llm-wiki as "up-to-date", skip this entire step.

#### 1-A. Create wiki directories
Create the following paths at the project root (skip any that already exist — do NOT overwrite existing files):

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

Initialize `.dev/wiki/index.md` (skip if file already exists):

```markdown
# Wiki Index

> Last updated: {today's date}

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

Initialize `.dev/wiki/log.md` (skip if file already exists):

```markdown
# Wiki Log

Operation history in chronological order.
Format: `## [{YYYY-MM-DD}] {operation} | {target or result summary}`

---

## [{today's date}] init | Wiki structure initialized

- Directory structure created
- index.md, log.md initialized
```

#### 1-B. Install llm-wiki skill file
Create `.claude/skills/llm-wiki/` directory.
Fetch and save as `.claude/skills/llm-wiki/SKILL.md`:
`https://raw.githubusercontent.com/itsjh1242/claude-skills/main/llm-wiki/SKILL.md`

---

### Step 2 — Install Claude Harness

> **Skip condition**: If the status table from Step 0 shows harness-setup as "up-to-date", skip this entire step.

#### 2-A. Create harness directories
Create the following directories at the project root (skip any that already exist):
- `.claude/agents/`
- `.claude/skills/harness/`
- `.claude/skills/harness/references/`
- `.claude/skills/harness-setup/`

#### 2-B. Fetch and install harness files
Fetch each file from `https://raw.githubusercontent.com/revfactory/harness/main/` and save:

| Remote path | Local path |
|---|---|
| `skills/harness/SKILL.md` | `.claude/skills/harness/SKILL.md` |
| `skills/harness/references/agent-design-patterns.md` | `.claude/skills/harness/references/agent-design-patterns.md` |
| `skills/harness/references/orchestrator-template.md` | `.claude/skills/harness/references/orchestrator-template.md` |
| `skills/harness/references/team-examples.md` | `.claude/skills/harness/references/team-examples.md` |
| `skills/harness/references/skill-writing-guide.md` | `.claude/skills/harness/references/skill-writing-guide.md` |
| `skills/harness/references/skill-testing-guide.md` | `.claude/skills/harness/references/skill-testing-guide.md` |
| `skills/harness/references/qa-agent-guide.md` | `.claude/skills/harness/references/qa-agent-guide.md` |

Fetch and save the harness-setup skill file:
`https://raw.githubusercontent.com/itsjh1242/claude-skills/main/harness-setup/SKILL.md` → `.claude/skills/harness-setup/SKILL.md`

---

### Step 3 — Install skill file and update CLAUDE.md

#### 3-A. Install dev-starter skill file
Create `.claude/skills/dev-starter/` directory.
Fetch this SKILL.md and save as `.claude/skills/dev-starter/SKILL.md`:
`https://raw.githubusercontent.com/itsjh1242/claude-skills/main/dev-starter/SKILL.md`

#### 3-B. Add to CLAUDE.md
Add the following sections to `CLAUDE.md` at the project root (create it if it doesn't exist).
- If a section already exists **and** the corresponding sub-skill was skipped (up-to-date), leave it as-is.
- If a section already exists **but** the corresponding sub-skill was installed or updated, replace it entirely.
- If a section does not exist, add it.

````
# LLM Wiki

Karpathy-style LLM Wiki + Graphify development knowledge base.
Wiki root: `.dev/wiki/`

## Structure

```
.dev/wiki/
├── raw/logs/{slug}.md          # Feature-scoped dev logs (immutable)
├── raw/docs/{slug}.md          # Dev docs — design specs, feature specs, etc. (immutable)
├── wiki/patterns/{topic}.md    # Recurring development patterns
├── wiki/decisions/{topic}.md   # Design & implementation decisions
├── wiki/insights/{topic}.md    # Cross-analysis insights
├── graphify-out/graph.json     # Knowledge Graph
├── index.md                    # Full page index (entry point)
└── log.md                      # Operation history
```

## Operations

### Log — Dev Log

When a feature-scoped task (implementation, bugfix, refactor, etc.) is considered complete, naturally suggest logging it.

`.dev/wiki/raw/logs/{feature-slug}.md`:

```markdown
---
feature: {feature name}
slug: {feature-slug}
type: {feature | bugfix | refactor | config | docs}
status: {done | wip}
created: {YYYY-MM-DD}
updated: {YYYY-MM-DD}
files_changed:
  - {changed file paths}
tags: [{keywords}]
---

# {feature name}

## Goal
{Problem to solve or goal to achieve — 1-3 sentences}

## Implementation Summary
{What and how — code structure, approach}

## Key Decisions
- **Decision**: {A vs B — why A was chosen}

## Problems & Solutions
- **Problem**: {symptom} → **Cause**: {root cause} → **Solution**: {fix}

## Retrospective (optional)
{What would be done differently next time}
```

- slug: kebab-case English
- If a file with the same slug exists, append to the existing file
- `status: wip` → update when work continues, change to `done` when complete

### Doc — Dev Document

Save dev documents (design specs, feature specs, architecture docs, etc.) when writing or organizing them.

`.dev/wiki/raw/docs/{doc-slug}.md`:

```markdown
---
title: {document title}
slug: {doc-slug}
doc_type: {design | spec | architecture | runbook | guide | adr}
status: {draft | final | deprecated}
created: {YYYY-MM-DD}
updated: {YYYY-MM-DD}
related_features: [{related feature slugs}]
tags: [{keywords}]
---

# {document title}

{body}
```

- Body format is flexible depending on doc_type
- Use `related_features` to link to dev logs

### Ingest — Wiki Page Create/Update

Cross-analyze raw logs + raw docs to create/update wiki pages.

1. Read `index.md` to understand current wiki state
2. Identify unreflected items among `status: done` (logs) or `status: final` (docs)
3. Create three types of wiki pages:

**patterns/{topic}.md — Recurring development patterns**:
Not question types, but approaches/problems/solutions repeatedly observed in the *development process*.
Link sources in case table with `[[raw/logs/{slug}]]` or `[[raw/docs/{slug}]]`.

**decisions/{topic}.md — Design & implementation decisions**:
Context, alternatives (pros/cons comparison), final decision and rationale.
Backlinks to related logs and docs.

**insights/{topic}.md — Cross-analysis insights**:
Non-obvious insights discovered by cross-referencing multiple logs and docs.
Supporting data, implications, related page links.

4. Update `index.md` and `log.md`

### Graphify — Knowledge Graph Build

Structure the entire wiki into `.dev/wiki/graphify-out/graph.json`.

Node types: `feature`, `document`, `pattern`, `decision`, `insight`, `file`, `tag`
Edge types: `implements`, `decided_in`, `modifies`, `related_to`, `supersedes`, `discovered_from`, `documents`, `tagged`
Hyperedges: when multiple nodes participate in a single flow

### Query — Knowledge Lookup

When starting new work or on request, search `index.md` and `graph.json` for related knowledge:
- Previously discovered patterns in similar areas
- Related decision and design document history
- Insights to watch out for

### Lint — Wiki Health Check

Check for orphan pages, broken wikilinks, unreflected raw items, stale WIP entries, and graph coverage.
Record results in log.md with Grade A-E.

## Constraints

- `raw/` files: only status field changes and appends allowed — no modification or deletion of existing content
- Always update `index.md` when creating or deleting pages
- All Operations must be recorded in `log.md`
- Wikilinks: `[[path/filename]]` (without extension)
- slug: kebab-case English

# Harness

Claude Harness (Agent Team & Skill Architect) is installed at `.claude/skills/harness/SKILL.md`.

Activate the harness skill when the user requests:
- Agent team design or multi-agent system setup for a domain/project
- "하네스 구성해줘", "build a harness", "design an agent team", "하네스 설계"
- Any harness-related architecture or orchestration work

After harness builds an agent team, it will append a `## 하네스: {domain}` block here with the trigger rules and change history for that domain.
````

### Step 4 — Verify
- Confirm `.dev/wiki/raw/logs/` exists
- Confirm `.dev/wiki/raw/docs/` exists
- Confirm `.dev/wiki/wiki/patterns/` exists
- Confirm `.dev/wiki/wiki/decisions/` exists
- Confirm `.dev/wiki/wiki/insights/` exists
- Confirm `.dev/wiki/graphify-out/` exists
- Confirm `.dev/wiki/index.md` exists
- Confirm `.dev/wiki/log.md` exists
- Confirm `.claude/agents/` exists
- Confirm `.claude/skills/harness/SKILL.md` exists
- Confirm `.claude/skills/harness/references/` contains 6 reference files
- Confirm `.claude/skills/harness-setup/SKILL.md` exists
- Confirm `.claude/skills/llm-wiki/SKILL.md` exists
- Confirm `.claude/skills/dev-starter/SKILL.md` exists
- Confirm `CLAUDE.md` contains `# LLM Wiki` section
- Confirm `CLAUDE.md` contains `# Harness` section
- If fresh install → output "Dev Starter setup complete (v1.0.0)"
- If update → output "Dev Starter updated: v{old} → v{new}"

---

## Role
Setup-only skill that installs a complete development environment: LLM Wiki (Karpathy-style knowledge base + Graphify) and Claude Harness (Agent Team & Skill Architect) in one command. After installation, CLAUDE.md rules govern all wiki operations and the harness skill handles agent team orchestration.

## Trigger Condition
Activate only when the user explicitly requests to install or update the full dev-starter environment. Individual skill operations (wiki logging, harness building, etc.) are governed by the installed skills and CLAUDE.md rules — they do not require this setup skill to trigger again.
