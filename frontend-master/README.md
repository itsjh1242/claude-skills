# frontend-master

A comprehensive frontend development skill that takes full ownership of all frontend work in React + Next.js projects.

---

## What it does

- **Auto-detects** project tech stack and adapts accordingly
- **Analyzes** task complexity and adjusts workflow (simple tasks skip planning, complex tasks get full architecture design)
- **Implements** components following Next.js App Router best practices with Server Components
- **Styles** with Tailwind CSS + shadcn/ui by default
- **Converts** design references (images, Figma) to pixel-perfect code
- **Verifies** accessibility (WCAG AA), responsive design, and performance automatically
- **Auto-fixes** a11y and responsive issues without asking

### Tech Stack
- React + Next.js (App Router)
- TypeScript (strict)
- Tailwind CSS + shadcn/ui
- Server Components by default

### 4 Phases
| Phase | Name | When |
|-------|------|------|
| 1 | Analysis | Always — project context, task scope, existing components |
| 2 | Design | Scope-dependent — component architecture, style strategy, implementation plan |
| 3 | Implementation | Always — component creation, styling, state management, error handling |
| 4 | Verification | Always — accessibility, responsive, performance checks with auto-fix |

---

## Installation

Paste the following prompt into Claude Code to install frontend-master:

```
Install the frontend-master skill by following the setup instructions here:
https://raw.githubusercontent.com/itsjh1242/claude-skills/main/frontend-master/SKILL.md
```

Claude will read the SKILL.md file and set up the required directories, install the skill, and update your `CLAUDE.md` automatically.

**Already installed?** Paste the same prompt again — the skill will compare versions and update only if a newer version is available.

---

## File structure created after setup

```
{project-root}/
├── .claude/
│   └── skills/
│       └── frontend-master/
│           └── SKILL.md
├── .dev/
│   └── frontend/
│       ├── components/     (component catalog)
│       └── styles.md       (style guide)
└── CLAUDE.md               ← Frontend Master section added
```
