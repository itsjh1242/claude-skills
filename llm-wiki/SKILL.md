---
name: llm-wiki
description: "Karpathy-style LLM Wiki + Graphify knowledge base setup skill.\nTRIGGER when: user says 'llm-wiki install', 'llm-wiki setup', 'wiki install', 'wiki setup', or similar install/setup intent for LLM Wiki.\nDO NOT TRIGGER when: wiki query, log, ingest, graphify, lint operations (post-setup operations handled by CLAUDE.md rules)."
version: 1.0.0
updated: 2026-04-17
changelog:
  - "1.0.0: Initial release"
---

# LLM Wiki

Karpathy-style LLM Wiki + Graphify knowledge base setup skill.
This is a **setup-only** skill. After setup, operational rules in CLAUDE.md govern all wiki operations automatically.

## Setup

### Step 0 — Version check (MANDATORY — do NOT skip)
You MUST perform all of the following steps before doing anything else:
1. Read the file `.claude/skills/llm-wiki/SKILL.md` in the current project
2. If the file does NOT exist → this is a fresh install. Go to Step 1.
3. If the file exists → extract the `version:` value from its YAML frontmatter
   - If the file has no `version` field → treat as `0.0.0`
4. Compare the local version with `1.0.0` (this file's version):
   - Local < `1.0.0` → go to **Update Flow**
   - Local = `1.0.0` → output "llm-wiki is already up to date (v1.0.0)" and STOP. Do nothing else.
5. You MUST output the comparison result: "Local: v{local} / Remote: v1.0.0 → {action}"

### Update Flow
When updating from an older version:
1. Overwrite `.claude/skills/llm-wiki/SKILL.md` with this file
2. Replace the `# LLM Wiki` section in `CLAUDE.md` with the latest version from Step 3 below
3. Show the user the changelog entries between their old version and the current version
4. Preserve `.dev/wiki/` directory and all existing data — do NOT delete them
5. Output "llm-wiki updated: v{old} → v{new}"

### Step 1 — Create directories and files
Create the following paths at the project root:

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

Initialize `.dev/wiki/index.md`:

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

Initialize `.dev/wiki/log.md`:

```markdown
# Wiki Log

Operation history in chronological order.
Format: `## [{YYYY-MM-DD}] {operation} | {target or result summary}`

---

## [{today's date}] init | Wiki structure initialized

- Directory structure created
- index.md, log.md initialized
```

### Step 2 — Install skill file
Create the following directory at the project root:
- `.claude/skills/llm-wiki/`

Fetch this SKILL.md file and save it as `.claude/skills/llm-wiki/SKILL.md`.

### Step 3 — Add to CLAUDE.md
Add the following instructions to `CLAUDE.md` at the project root (create it if it doesn't exist).
If a `# LLM Wiki` section already exists, replace it entirely.

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
- Confirm `CLAUDE.md` contains `# LLM Wiki` section
- If fresh install → output "LLM Wiki setup complete (v1.0.0)"
- If update → output "LLM Wiki updated: v{old} → v{new}"

---

## Role
Setup-only skill that installs the LLM Wiki + Graphify knowledge base structure into any project. After installation, CLAUDE.md rules handle all ongoing wiki operations (Log, Doc, Ingest, Graphify, Query, Lint) automatically.

## Trigger Condition
Activate only when the user explicitly requests to install or update the LLM Wiki skill. Post-setup operations (logging, ingesting, querying, etc.) are governed by the rules written into CLAUDE.md and do not require this skill to trigger again.
