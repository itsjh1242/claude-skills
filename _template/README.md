# {{SKILL_NAME}}

{{SKILL_DESC_EN}}

---

## What it does

<!-- Describe the skill's features here -->

---

## Installation

Paste the following prompt into Claude Code to install {{SKILL_NAME}}:

```
Install the {{SKILL_NAME}} skill by following the setup instructions here:
https://raw.githubusercontent.com/{{GITHUB_USER}}/{{GITHUB_REPO}}/main/{{SKILL_NAME}}/SKILL.md
```

Claude will read the SKILL.md file and set up the required directories, install the skill, and update your `CLAUDE.md` automatically.

**Already installed?** Paste the same prompt again — the skill will compare versions and update only if a newer version is available.

---

## File structure created after setup

```
{project-root}/
├── .claude/
│   └── skills/
│       └── {{SKILL_NAME}}/
│           └── SKILL.md
├── .dev/
│   └── {{DATA_DIR}}/
│       └── ...
└── CLAUDE.md          ← {{SKILL_TITLE}} section added
```
