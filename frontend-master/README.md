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
- **Verifies** accessibility (WCAG AA), responsive design, and Core Web Vitals
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
| 2 | Design | Scope-dependent — design thinking, component architecture, implementation plan |
| 3 | Implementation | Always — component creation, styling, state management, design-to-code |
| 4 | Verification | Always — accessibility, responsive, performance, auto-fix |

### v2.0 Highlights
- **Routing table pattern**: SKILL.md as lightweight router (~200 lines), detailed guides in `references/` loaded on-demand to minimize token cost
- **Design aesthetics**: Design thinking framework, 11 aesthetic directions, anti-AI-slop rules, typography/color/motion/spatial guidelines
- **Component architecture**: Composition over configuration, colocated files, state management decision tree
- **Performance targets**: LCP ≤2.5s, INP ≤200ms, CLS ≤0.1

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
│           ├── SKILL.md
│           └── references/
│               ├── design-guide.md       (design aesthetics, loaded on-demand)
│               └── architecture-guide.md (component patterns, loaded on-demand)
├── .dev/
│   └── frontend/
│       ├── components/     (component catalog)
│       └── styles.md       (style guide)
└── CLAUDE.md               ← Frontend Master section added
```
