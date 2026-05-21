# flutter-supabase-setup

Flutter + Supabase **development environment** setup from planning documents. Creates project structure, installs dependencies, configures environment, and verifies the build. Generates zero feature code — only infrastructure.

## Installation

```bash
# 1. Add marketplace
/plugin marketplace add itsjh1242/claude-skills

# 2. Install plugin
/plugin install flutter-supabase-setup@itsjh-plugins
```

## Commands

### `/flutter-supabase-setup:init`

Main setup. Reads a planning doc and creates a ready-to-run project.

1. Analyzes planning document
2. Collects project name, org, platforms (no Supabase credentials needed)
3. Proposes tech stack with justifications
4. User confirms
5. Scaffolds: `flutter create` → folders → dependencies → boilerplate → config
6. Verifies: `flutter analyze` + `build_runner`

### `/flutter-supabase-setup:env`

Configures Supabase credentials after project creation.

1. Asks for Supabase URL and Anon Key
2. Creates `.env` file
3. Verifies connection

## Fixed Stack

- **Flutter** (latest stable)
- **Supabase** (`supabase_flutter`)

## Baseline Stack (adjusted per planning doc)

| Category | Default |
|---|---|
| State management | Riverpod (codegen) |
| Routing | go_router |
| Models | freezed + json_serializable |
| Architecture | Feature-first + lightweight Clean |

## Generated Project Structure

```
{project_name}/
├── lib/
│   ├── main.dart
│   ├── app.dart
│   └── core/
│       ├── router/
│       ├── theme/
│       ├── providers/
│       ├── constants/
│       └── extensions/
├── docs/SPEC.md
├── .env.example
└── analysis_options.yaml
```

`lib/features/` is intentionally empty — create feature modules during development.

## Plugin Structure

```
flutter-supabase-setup/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── init/
│   │   └── SKILL.md
│   └── env/
│       └── SKILL.md
└── README.md
```
