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
name: "{plugin-name}-{command}"
description: "What this skill does and when to use it. Claude uses this to decide whether to auto-invoke the skill."
---
```

| Field | Required | Description |
|---|---|---|
| `name` | Yes | Skill command name. **반드시 플러그인명을 접두사로 포함**해야 목록에서 `/{plugin}:{name}` 형태로 표시된다. |
| `description` | Yes | How Claude knows when to use this skill. Include trigger phrases and anti-triggers. `(plugin-name)` 접두사는 Claude Code가 자동 추가하므로 description에 넣지 않는다. |
| `disable-model-invocation` | No | Set `true` to disable auto-invocation (slash command only). Default: `false`. |

**No `version`, `updated`, `changelog` in SKILL.md frontmatter.** Those belong in `plugin.json`.

---

## Skill Naming Convention (중요)

스킬의 **디렉토리명**과 **frontmatter `name`**이 슬래시 명령어 표시 방식을 결정한다.

**공식 문서 (https://code.claude.com/docs/en/skills):**
> Plugin `skills/` subdirectory → Directory name, namespaced by plugin
> `name` field sets the display label shown in skill listings.

### 실제 동작 규칙

| 디렉토리명 | frontmatter name | 목록 표시 | 비고 |
|---|---|---|---|
| `init` | `init` | `/init (plugin-name)` | 플러그인 식별 불가, 다른 스킬과 충돌 위험 |
| `{plugin}-init` | `{plugin}-init` | `/{plugin}:{plugin}-init (plugin-name)` | 플러그인 식별 가능, 충돌 없음 |

### 규칙

1. **디렉토리명 = frontmatter `name`**: 항상 일치시킨다
2. **플러그인명을 접두사로 포함**: `{plugin-name}-{command}` 형태로 명명한다
   - 좋은 예: `daily-log-init`, `daily-log-log`, `daily-log-remind`
   - 나쁜 예: `init`, `log`, `remind` (다른 플러그인/빌트인과 충돌)
3. **description에 `(plugin-name)` 넣지 않기**: Claude Code가 자동으로 추가한다

---

## Invocation

Users invoke skills as:

```
/{plugin-name}:{skill-name}
/{plugin-name}:{skill-name} arguments
```

Example: `/daily-log:daily-log-init`

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
| `daily-log` | `/daily-log:daily-log-init` | tasks 레포 클론 및 초기 설정 |
| `daily-log` | `/daily-log:daily-log-log` | 오늘의 작업 로그 기록 (Completed + Remind) |
| `daily-log` | `/daily-log:daily-log-remind` | 최근 미완료 리마인드 항목 표시 |
