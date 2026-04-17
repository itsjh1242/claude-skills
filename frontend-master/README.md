# frontend-master

A comprehensive frontend development skill that takes full ownership of all frontend work in React + Next.js projects.

---

## What it does

- **Auto-detects** project tech stack and adapts accordingly
- **Analyzes** task complexity and adjusts workflow (simple tasks skip planning, complex tasks get full architecture design)
- **Designs** with a design thinking framework — Purpose, Tone, Constraints, Differentiation
- **Implements** components following Next.js App Router best practices with Server Components
- **Styles** with Tailwind CSS + shadcn/ui by default, following anti-AI aesthetic rules
- **Converts** design references (images, Figma) to pixel-perfect code
- **Verifies** accessibility (WCAG 2.2 AA), responsive design, Core Web Vitals, and performance budget
- **Tests** with Playwright browser testing and automated accessibility scripts
- **Auto-fixes** a11y, responsive, and performance issues without asking

### Tech Stack
- React + Next.js (App Router)
- TypeScript (strict)
- Tailwind CSS + shadcn/ui
- Server Components by default
- Playwright for browser testing

### 4 Phases
| Phase | Name | When |
|-------|------|------|
| 1 | Analysis | Always — project context, task scope, existing components, design direction |
| 2 | Design | Scope-dependent — design thinking, component architecture, style strategy, implementation plan |
| 3 | Implementation | Always — component creation, styling, state management, design-to-code, error handling |
| 4 | Verification | Always — WCAG 2.2 AA accessibility, responsive design, Core Web Vitals, browser testing with auto-fix |

### v2.0 Highlights
- **Design aesthetics**: 11 aesthetic directions, anti-AI-slop rules, typography/color/motion/spatial guidelines
- **Component architecture**: composition over configuration, colocated files, state management decision tree
- **Performance**: Core Web Vitals targets (LCP ≤2.5s, INP ≤200ms, CLS ≤0.1), performance budget, anti-pattern detection
- **Accessibility**: WCAG 2.2 AA with automated Playwright test scripts for focus, reflow, spacing, target size
- **Browser testing**: Playwright-based with reconnaissance-then-action pattern

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
