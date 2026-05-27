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
name: "{command}"
description: "({plugin-name}) What this skill does and when to use it."
---
```

| Field | Required | Description |
|---|---|---|
| `name` | Yes | 스킬 명령어 이름. 디렉토리명과 일치시킨다. |
| `description` | Yes | **`(plugin-name)` 접두사를 반드시 포함**한다. Claude가 auto-invoke 여부를 판단하는 데 사용. trigger/anti-trigger 포함. |
| `disable-model-invocation` | No | Set `true` to disable auto-invocation (slash command only). Default: `false`. |

**No `version`, `updated`, `changelog` in SKILL.md frontmatter.** Those belong in `plugin.json`.

---

## Skill Naming Convention (중요)

스킬의 **디렉토리명**과 **frontmatter**가 슬래시 명령어 표시 방식을 결정한다.

**공식 문서 (https://code.claude.com/docs/en/skills):**
> Plugin `skills/` subdirectory → Directory name, namespaced by plugin
> Example: `my-plugin/skills/review/SKILL.md` → `/my-plugin:review`

### 규칙

1. **디렉토리명 = frontmatter `name`**: 항상 일치시킨다
2. **디렉토리명은 단순하게**: `init`, `log`, `remind` 등. 호출은 `/{plugin}:{name}` 형태
3. **description에 `(plugin-name)` 접두사 필수**: 목록에서 어떤 플러그인의 스킬인지 식별 가능하게 함

### 예시

```yaml
# 플러그인: daily-log, 디렉토리: skills/init/SKILL.md
---
name: init
description: "(daily-log) 초기 설정. tasks 레포 URL을 입력받아..."
---
```

호출: `/daily-log:init`

---

## Invocation

Users invoke skills as:

```
/{plugin-name}:{skill-name}
/{plugin-name}:{skill-name} arguments
```

Example: `/daily-log:init`

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

`.claude-plugin/marketplace.json`을 레포 루트에 생성한다.

**Source 형식** (공식 문서: https://code.claude.com/docs/en/plugin-marketplaces):

| Source 종류 | 형식 | 설명 |
|---|---|---|
| 상대 경로 | `"./plugins/my-plugin"` | 같은 레포 내 플러그인. `./`로 시작. 마켓플레이스 루트 기준 |
| GitHub | `{"source": "github", "repo": "owner/repo"}` | 외부 GitHub 레포 |
| git-subdir | `{"source": "git-subdir", "url": "...", "path": "..."}` | 외부 레포의 하위 디렉토리 |
| npm | `{"source": "npm", "package": "..."}` | npm 패키지 |

**같은 레포 내 플러그인은 상대 경로(`"./plugins/..."`)를 사용한다:**

```json
{
  "name": "itsjh-plugins",
  "owner": {
    "name": "itsjh1242"
  },
  "plugins": [
    {
      "name": "my-plugin",
      "source": "./plugins/my-plugin",
      "description": "Plugin description"
    }
  ]
}
```

**사용자 설치:**
```
/plugin marketplace add owner/repo
/plugin install {plugin-name}@{marketplace-name}
/plugin marketplace update          # 원격 변경사항 갱신
```

---

## Reference: Existing Plugins

| Plugin | Command | Description |
|---|---|---|
| `flutter-supabase-setup` | `/flutter-supabase-setup:init` | Flutter + Supabase 개발환경 셋업 (기획서 → 프로젝트 생성) |
| `flutter-supabase-setup` | `/flutter-supabase-setup:env` | Supabase 크레덴셜 설정 (.env 생성) |
| `daily-log` | `/daily-log:init` | tasks 레포 클론 및 초기 설정 |
| `daily-log` | `/daily-log:log` | 오늘의 작업 로그 기록 (Completed + Remind) |
| `daily-log` | `/daily-log:remind` | 최근 미완료 리마인드 항목 표시 |
