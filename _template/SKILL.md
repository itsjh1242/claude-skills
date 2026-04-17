---
name: {{SKILL_NAME}}
description: "{{SKILL_DESC_SHORT}}\nTRIGGER when: {{TRIGGER_CONDITIONS}}\nDO NOT TRIGGER when: {{NO_TRIGGER_CONDITIONS}}"
version: {{SKILL_VERSION}}
updated: {{SKILL_DATE}}
changelog:
  - "{{CHANGELOG_EN}}"
---

# {{SKILL_TITLE}}

## Setup

### Step 0 — Version check (MANDATORY — do NOT skip)
You MUST perform all of the following steps before doing anything else:
1. Read the file `.claude/skills/{{SKILL_NAME}}/SKILL.md` in the current project
2. If the file does NOT exist → this is a fresh install. Go to Step 1.
3. If the file exists → extract the `version:` value from its YAML frontmatter
   - If the file has no `version` field → treat as `0.0.0`
4. Compare the local version with `{{SKILL_VERSION}}` (this file's version):
   - Local < `{{SKILL_VERSION}}` → go to **Update Flow**
   - Local = `{{SKILL_VERSION}}` → output "{{SKILL_NAME}} is already up to date (v{{SKILL_VERSION}})" and STOP. Do nothing else.
5. You MUST output the comparison result: "Local: v{local} / Remote: v{{SKILL_VERSION}} → {action}"

### Update Flow
When updating from an older version:
1. Overwrite `.claude/skills/{{SKILL_NAME}}/SKILL.md` with this file
2. Replace the `# {{SKILL_TITLE}}` section in `CLAUDE.md` with the latest version from Step 3 below
3. Show the user the changelog entries between their old version and the current version
4. Preserve `.dev/{{DATA_DIR}}/` directory and all existing data — do NOT delete them
5. Output "{{SKILL_NAME}} updated: v{old} → v{new}"

### Step 1 — Create directories and files
Create the following paths at the project root:
- `.dev/{{DATA_DIR}}/`       (empty directory — add subdirectories as needed)

<!-- Add any initial files your skill needs here -->

### Step 2 — Install skill file
Create the following directory at the project root:
- `.claude/skills/{{SKILL_NAME}}/`

Fetch this SKILL.md file and save it as `.claude/skills/{{SKILL_NAME}}/SKILL.md`.

### Step 3 — Add to CLAUDE.md
Add the following instructions to `CLAUDE.md` at the project root (create it if it doesn't exist):

```
{{CLAUDE_MD_EN}}
```

### Step 4 — Verify
- Confirm `.dev/{{DATA_DIR}}/` exists
- Confirm the {{SKILL_TITLE}} section exists in `CLAUDE.md`
- If fresh install → output "{{SKILL_TITLE}} setup complete (v{{SKILL_VERSION}})"
- If update → output "{{SKILL_TITLE}} updated: v{old} → v{new}"

---

## Role
<!-- Describe what this skill does in one sentence -->

## Trigger Condition
<!-- Define when this skill should activate -->

<!-- Add your skill-specific phases below -->
<!-- Example:
## Phase 1 — Analysis
## Phase 2 — Execution
## Phase 3 — Report
-->
