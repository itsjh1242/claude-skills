# Skill Plugin Template

This repository distributes Claude Code skills as **marketplace plugins**.
Every skill must follow the plugin structure below.

---

## Plugin Directory Structure

```
{skill-name}/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest (required)
├── skills/
│   └── {command-name}/
│       └── SKILL.md         # Skill definition (required)
└── README.md                # Plugin documentation
```

- **`.claude-plugin/plugin.json`**: Manifest file. Only this file goes inside `.claude-plugin/`.
- **`skills/{command-name}/SKILL.md`**: The actual skill logic. One directory per slash command.
- **`README.md`**: User-facing documentation (install, usage, what it does).

A single plugin can contain multiple skills (multiple directories under `skills/`).

---

## plugin.json Schema

```json
{
  "name": "{skill-name}",
  "description": "One-line description shown in plugin manager",
  "version": "1.0.0",
  "author": {
    "name": "itsjh1242"
  },
  "repository": "https://github.com/itsjh1242/claude-skills",
  "license": "MIT"
}
```

| Field | Required | Description |
|---|---|---|
| `name` | Yes | Unique identifier (kebab-case). Becomes the `/name:command` namespace. |
| `description` | Yes | Shown in plugin manager and marketplace catalog. |
| `version` | No | Semver string. If omitted, git commit SHA is used (every commit = new version). |
| `author` | No | Attribution. |
| `repository` | No | Source repository URL. |
| `license` | No | License identifier. |

---

## SKILL.md Frontmatter

```yaml
---
name: "skill-command-name"
description: "What this skill does and when to use it. Claude uses this to decide whether to auto-invoke the skill."
---
```

| Field | Required | Description |
|---|---|---|
| `name` | Yes | Skill command name. Used in `/{plugin-name}:{name}`. |
| `description` | Yes | How Claude knows when to use this skill. Include trigger phrases and anti-triggers. |
| `disable-model-invocation` | No | Set `true` to disable auto-invocation (slash command only). Default: `false`. |

**No `version`, `updated`, `changelog` in SKILL.md frontmatter.** Those belong in `plugin.json`.

---

## Invocation

Users invoke skills as:

```
/{plugin-name}:{skill-name}
/{plugin-name}:{skill-name} arguments
```

Example: `/flutter-supabase-setup:init`

---

## Creating a New Skill

1. Create a directory at repo root: `{skill-name}/`
2. Create `.claude-plugin/plugin.json` with manifest
3. Create `skills/{command-name}/SKILL.md` with frontmatter + instructions
4. Create `README.md` with install/usage docs
5. Test locally: `claude --plugin-dir ./{skill-name}`
6. Validate: `claude plugin validate ./{skill-name}`

---

## Publishing

### Option A: Community Marketplace

1. Validate: `claude plugin validate ./{skill-name}`
2. Submit at [claude.ai/settings/plugins/submit](https://claude.ai/settings/plugins/submit)
3. Plugin is pinned to a commit SHA in the community catalog

### Option B: Self-hosted Marketplace

Create a `marketplace.json` in a separate repo:

```json
{
  "name": "itsjh-plugins",
  "owner": {
    "name": "itsjh1242"
  },
  "description": "Custom Claude Code plugins",
  "plugins": [
    {
      "name": "flutter-supabase-setup",
      "source": {
        "source": "git-subdir",
        "url": "https://github.com/itsjh1242/claude-skills.git",
        "path": "plugins/flutter-supabase-setup"
      },
      "description": "Flutter + Supabase project scaffolding from planning docs"
    }
  ]
}
```

Users add with: `/plugin marketplace add owner/repo`

---

## Reference: Existing Plugins

| Plugin | Command | Description |
|---|---|---|
| `flutter-supabase-setup` | `/flutter-supabase-setup:init` | Flutter + Supabase 개발환경 셋업 (기획서 → 프로젝트 생성) |
| `flutter-supabase-setup` | `/flutter-supabase-setup:env` | Supabase 크레덴셜 설정 (.env 생성) |
| `daily-log` | `/daily-log:daily-log-init` | tasks 레포 클론 및 초기 설정 |
| `daily-log` | `/daily-log:daily-log-log` | 오늘의 작업 로그 기록 (Completed + Remind) |
| `daily-log` | `/daily-log:daily-log-remind` | 최근 미완료 리마인드 항목 표시 |
