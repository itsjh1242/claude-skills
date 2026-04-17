---
name: harness-setup
description: "Claude Harness (Agent Team & Skill Architect) installation skill. Fetches and installs revfactory/harness into the current project.\nTRIGGER when: user says 'harness install', 'harness setup', 'install harness', 'set up harness', 'harness 설치', 'harness 업데이트', or similar install/update intent for Claude Harness.\nDO NOT TRIGGER when: user asks to build/design/configure a harness system (that is handled by the installed harness skill itself, not this setup skill)."
version: 1.0.0
updated: 2026-04-17
changelog:
  - "1.0.0: Initial release"
---

# Harness Setup

Fetches and installs [Claude Harness](https://github.com/revfactory/harness) (Agent Team & Skill Architect) into the current project.
This is a **setup-only** skill. After setup, the installed harness skill governs all agent team design and orchestration.

## Setup

### Step 0 — Version check (MANDATORY — do NOT skip)
You MUST perform all of the following steps before doing anything else:
1. Read the file `.claude/skills/harness-setup/SKILL.md` in the current project
2. If the file does NOT exist → this is a fresh install. Go to Step 1.
3. If the file exists → extract the `version:` value from its YAML frontmatter
   - If the file has no `version` field → treat as `0.0.0`
4. Compare the local version with `1.0.0` (this file's version):
   - Local < `1.0.0` → go to **Update Flow**
   - Local = `1.0.0` → output "harness-setup is already up to date (v1.0.0)" and STOP. Do nothing else.
5. You MUST output the comparison result: "Local: v{local} / Remote: v1.0.0 → {action}"

### Update Flow
When updating from an older version:
1. Re-fetch all harness files from Step 2 and overwrite local copies
2. Overwrite `.claude/skills/harness-setup/SKILL.md` with this file
3. Replace the `# Harness` section in `CLAUDE.md` with the latest version from Step 3 below
4. Show the user the changelog entries between their old version and the current version
5. Preserve `.claude/agents/` directory and all existing agent definitions — do NOT delete them
6. Output "harness-setup updated: v{old} → v{new}"

### Step 1 — Create directories
Create the following directories at the project root (skip any that already exist):
- `.claude/agents/`
- `.claude/skills/harness/`
- `.claude/skills/harness/references/`
- `.claude/skills/harness-setup/`

### Step 2 — Fetch and install harness files
Fetch each file from `https://raw.githubusercontent.com/revfactory/harness/main/` and save to the corresponding local path:

| Remote path | Local path |
|---|---|
| `skills/harness/SKILL.md` | `.claude/skills/harness/SKILL.md` |
| `skills/harness/references/agent-design-patterns.md` | `.claude/skills/harness/references/agent-design-patterns.md` |
| `skills/harness/references/orchestrator-template.md` | `.claude/skills/harness/references/orchestrator-template.md` |
| `skills/harness/references/team-examples.md` | `.claude/skills/harness/references/team-examples.md` |
| `skills/harness/references/skill-writing-guide.md` | `.claude/skills/harness/references/skill-writing-guide.md` |
| `skills/harness/references/skill-testing-guide.md` | `.claude/skills/harness/references/skill-testing-guide.md` |
| `skills/harness/references/qa-agent-guide.md` | `.claude/skills/harness/references/qa-agent-guide.md` |

Then fetch this SKILL.md and save it as `.claude/skills/harness-setup/SKILL.md`:
`https://raw.githubusercontent.com/itsjh1242/claude-skills/main/harness-setup/SKILL.md`

### Step 3 — Add to CLAUDE.md
Add the following instructions to `CLAUDE.md` at the project root (create it if it doesn't exist).
If a `# Harness` section already exists, replace it entirely.

````
# Harness

Claude Harness (Agent Team & Skill Architect) is installed at `.claude/skills/harness/SKILL.md`.

Activate the harness skill when the user requests:
- Agent team design or multi-agent system setup for a domain/project
- "하네스 구성해줘", "build a harness", "design an agent team", "하네스 설계"
- Any harness-related architecture or orchestration work

After harness builds an agent team, it will append a `## 하네스: {domain}` block here with the trigger rules and change history for that domain.
````

### Step 4 — Verify
- Confirm `.claude/agents/` exists
- Confirm `.claude/skills/harness/SKILL.md` exists
- Confirm `.claude/skills/harness/references/` contains all 6 reference files
- Confirm `.claude/skills/harness-setup/SKILL.md` exists
- Confirm `CLAUDE.md` contains `# Harness` section
- If fresh install → output "Harness Setup complete (v1.0.0)"
- If update → output "Harness Setup updated: v{old} → v{new}"

---

## Role
Setup-only skill that fetches and installs Claude Harness (revfactory/harness) into any project. After installation, the harness skill handles all agent team design, orchestration, and multi-agent scaffolding automatically.

## Trigger Condition
Activate only when the user explicitly requests to install or update Claude Harness. Post-setup operations (building harnesses, designing agent teams, etc.) are governed by the installed harness skill and do not require this setup skill to trigger again.
