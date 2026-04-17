# Frontend Master

A comprehensive frontend development skill that takes full ownership of all frontend work in React + Next.js projects — from design thinking through implementation to quality verification.

## What It Does

- **Auto-detects** project tech stack and adapts accordingly
- **Applies Design Thinking** framework — Purpose, Tone, Constraints, Differentiation
- **Implements** components following Next.js App Router best practices with Server Components
- **Styles** with Tailwind CSS + shadcn/ui by default, following anti-AI aesthetic rules
- **Architects** with composition over configuration, state management decision tree
- **Verifies** accessibility (WCAG AA), responsive design, and Core Web Vitals
- **Auto-fixes** a11y and responsive issues without asking

### Tech Stack Defaults

- React + Next.js (App Router)
- TypeScript (strict)
- Tailwind CSS + shadcn/ui
- Server Components by default

### Performance Targets

See `references/architecture.md` for Core Web Vitals targets (LCP ≤2.5s, INP ≤200ms, CLS ≤0.1).

## v3.0 Highlights

**Radical simplification following Anthropic's pattern:**
- **SKILL.md**: 365 lines → 61 lines (83% reduction)
- **References**: On-demand loading system (design-thinking, architecture, brand-systems)
- **Setup**: Moved to README (no bloat in core skill file)
- **Philosophy**: Framework over prescription — guides decisions, doesn't make them for you

## Installation

### Quick Install

Paste the following prompt into Claude Code:

```
Install the frontend-master skill by following the setup instructions here:
https://raw.githubusercontent.com/itsjh1242/claude-skills/main/frontend-master/SKILL.md
```

Claude will:
1. Read the SKILL.md file
2. Create required directories (`.dev/frontend/`, `.claude/skills/frontend-master/`)
3. Create initial template files
4. Add Frontend Master section to your `CLAUDE.md`
5. Verify installation

### Manual Install

If auto-install fails, follow these steps:

#### Step 1: Create directories

```bash
mkdir -p .dev/frontend/components
mkdir -p .claude/skills/frontend-master
```

#### Step 2: Create style guide template

Create `.dev/frontend/styles.md`:

```markdown
---
last_updated: YYYY-MM-DD
project_type: unknown
ui_library: none
---

# Style Guide
<!-- Auto-populated by Frontend Master after design analysis -->
```

#### Step 3: Download skill files

1. Download [SKILL.md](https://raw.githubusercontent.com/itsjh1242/claude-skills/main/frontend-master/SKILL.md) to `.claude/skills/frontend-master/SKILL.md`
2. Download references to `.claude/skills/frontend-master/references/`:
   - [design-thinking.md](https://raw.githubusercontent.com/itsjh1242/claude-skills/main/frontend-master/references/design-thinking.md)
   - [architecture.md](https://raw.githubusercontent.com/itsjh1242/claude-skills/main/frontend-master/references/architecture.md)
   - [brand-systems.md](https://raw.githubusercontent.com/itsjh1242/claude-skills/main/frontend-master/references/brand-systems.md)

#### Step 4: Update CLAUDE.md

Add to your project's `CLAUDE.md`:

```markdown
## Frontend Master

When any frontend-related file is created or modified (React components, pages, layouts, styles, hooks, etc.),
you MUST read `.claude/skills/frontend-master/SKILL.md` and follow its guidelines automatically.

- Skill location: .claude/skills/frontend-master/SKILL.md
- Component catalog: .dev/frontend/components/
- Style guide: .dev/frontend/styles.md

When the user explicitly asks for frontend work (e.g. "make a landing page", "fix the UI", "add a component"),
Frontend Master takes full ownership of the task.
```

### Update

To update to the latest version:

1. Check your current version in `.claude/skills/frontend-master/SKILL.md` (see `version:` in YAML frontmatter)
2. Run the install prompt again — it will compare versions and update only if newer
3. Changelog is in the SKILL.md frontmatter

## File Structure

After installation, your project will have:

```
{project-root}/
├── .claude/
│   └── skills/
│       └── frontend-master/
│           ├── SKILL.md                    (88 lines — core workflow)
│           └── references/
│               ├── design-thinking.md      (162 lines — Design Thinking + Aesthetics)
│               ├── architecture.md         (237 lines — Component patterns + file structure)
│               └── brand-systems.md        (236 lines — Brand reference guide)
├── .dev/
│   └── frontend/
│       ├── components/     (component catalog)
│       └── styles.md       (style guide)
└── CLAUDE.md               (Frontend Master section added)
```

## Usage

Frontend Master activates automatically when:

- You create/modify `.tsx`, `.jsx`, `.css`, `.scss` files
- You request frontend work: "build a landing page", "fix the UI", "add a component"
- Frontend-related work is detected in conversation

### Workflow

1. **Design Thinking** (for visual work) — Apply Purpose, Tone, Constraints, Differentiation framework
2. **Component Architecture** — Design component hierarchy, define props, plan Server/Client boundary
3. **Implementation** — Build with Server Components, Tailwind CSS, composition patterns
4. **Verification** — Auto-fix accessibility, responsive, performance issues

### Reference Loading

SKILL.md contains the core workflow (~88 lines). Detailed references load on-demand:

| When | Load |
|------|------|
| Design/UI work, styling | `references/design-thinking.md` |
| Component architecture | `references/architecture.md` |
| Brand reference needed | `references/brand-systems.md` |
| Complex tasks | All relevant references above |

For simple tasks (single component fix, style tweak): No references loaded — applies inline rules only.

## Key Features

### Design Thinking Framework

Before implementing UI, define:
- **Purpose**: What should this UI communicate?
- **Tone**: What emotional register?
- **Constraints**: Technical limits, brand requirements
- **Differentiation**: What makes this NOT generic AI output?

### Anti-AI Aesthetic Rules

Never produce generic "AI-looking" UI:
- ❌ Purple gradients on white, rounded-2xl everywhere, stock card grids
- ✅ Intentional color schemes, varied border-radius, editorial layouts

### Typography

- Never use Inter, Roboto, Arial, or system-ui as identity font
- Pair distinctive display font with refined body font
- 11 aesthetic directions with font pairings (Brutally minimal, Retro-futuristic, etc.)

### State Management Decision Tree

1. Local state → 2. Lifted → 3. URL → 4. Context → 5. Server → 6. External (only if project uses it)

### Server Components First

Default to Server Components. Add `"use client"` only when:
- Event handlers needed
- React hooks used
- Browser APIs needed
- Client-side libraries used

### Brand Systems Integration

Reference real-world brand design systems via [awesome-design-md](https://github.com/VoltAgent/awesome-design-md):
- 68+ brand DESIGN.md files (Vercel, Stripe, Linear, Notion, etc.)
- Extract color palettes, typography, component patterns
- Adapt principles, don't clone pixels

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 3.0.1 | 2025-04-17 | Improved description with follow-up task keywords (Grade A) |
| 3.0.0 | 2025-04-17 | Radical simplification — 365→61 lines (83% reduction), Anthropic pattern |
| 2.0.0 | 2025-04-17 | Routing table structure, design/architecture references |
| 1.0.0 | 2025-04-16 | Initial release |

## License

MIT License — See [LICENSE](LICENSE) for details.

## Contributing

Contributions welcome! Feel free to:
- Report issues
- Suggest improvements
- Submit pull requests

## Credits

Inspired by [Anthropic's frontend-design plugin](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design) — follows the same philosophy of framework over prescription.
