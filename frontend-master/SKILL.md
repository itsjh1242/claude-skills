---
version: 2.0.0
updated: 2026-04-17
changelog:
  - "2.0.0: Major upgrade — merge design + architecture skills, routing table structure"
  - "  + Anthropic frontend-design: design thinking, aesthetic directions, anti-AI-slop"
  - "  + Addy Osmani frontend-ui-engineering: component architecture, composition patterns, state decision tree"
  - "  + Routing table pattern: references/ loaded on-demand to minimize token cost"
  - "1.0.0: Initial release"
---

# Frontend Master

## Setup

### Step 0 — Version check (MANDATORY — do NOT skip)
You MUST perform all of the following steps before doing anything else:
1. Read the file `.claude/skills/frontend-master/SKILL.md` in the current project
2. If the file does NOT exist → this is a fresh install. Go to Step 1.
3. If the file exists → extract the `version:` value from its YAML frontmatter
   - If the file has no `version` field → treat as `0.0.0`
4. Compare the local version with `2.0.0` (this file's version):
   - Local < `2.0.0` → go to **Update Flow**
   - Local = `2.0.0` → output "frontend-master is already up to date (v2.0.0)" and STOP. Do nothing else.
5. You MUST output the comparison result: "Local: v{local} / Remote: v2.0.0 → {action}"

### Update Flow
When updating from an older version:
1. Overwrite `.claude/skills/frontend-master/SKILL.md` with this file
2. Replace the `# Frontend Master` section in `CLAUDE.md` with the latest version from Step 3 below
3. Show the user the changelog entries between their old version and the current version
4. Preserve `.dev/frontend/` directory and all existing data — do NOT delete them
5. Output "frontend-master updated: v{old} → v{new}"

### Step 1 — Create directories and files
Create the following paths at the project root:
- `.dev/frontend/components/`   (component catalog directory)
- `.dev/frontend/styles.md`     (create with the initial template below)

Initial template for `styles.md`:
```
---
last_updated: (date)
project_type: unknown
ui_library: none
---
# Style Guide
<!-- Auto-populated by Frontend Master after Phase 1 analysis -->
```

### Step 2 — Install skill file
Create the following directory at the project root:
- `.claude/skills/frontend-master/`

Fetch this SKILL.md file and save it as `.claude/skills/frontend-master/SKILL.md`.

### Step 3 — Add to CLAUDE.md
Add the following instructions to `CLAUDE.md` at the project root (create it if it doesn't exist):

```
# Frontend Master
When any frontend-related file is created or modified (React components, pages, layouts, styles, hooks, etc.),
you MUST read `.claude/skills/frontend-master/SKILL.md` and follow its guidelines automatically.
This applies to ALL frontend work — new features, bug fixes, refactoring, and styling.
- Skill location: .claude/skills/frontend-master/SKILL.md
- Component catalog: .dev/frontend/components/
- Style guide: .dev/frontend/styles.md
When the user explicitly asks for frontend work (e.g. "make a landing page", "fix the UI", "add a component"),
Frontend Master takes full ownership of the task.
```

### Step 4 — Verify
- Confirm `.dev/frontend/styles.md` exists
- Confirm the Frontend Master section exists in `CLAUDE.md`
- If fresh install → output "Frontend Master setup complete (v2.0.0)"
- If update → output "Frontend Master updated: v{old} → v{new}"

---

## Role
A comprehensive frontend development skill that takes full ownership of all frontend work in React + Next.js projects — from design thinking and component architecture through implementation to quality verification.

## Trigger Condition
Frontend Master activates in two ways:

### Auto-detect
The CLI judges activation by combining the following context — not by keyword matching:
- Is a frontend file (`.tsx`, `.jsx`, `.css`, `.scss`, `.module.css`) being created or modified?
- Is the user's request related to UI, components, pages, layouts, styling, or state management?
- Is the conversation about a new feature that requires a frontend implementation?

When any of these are true, the CLI **automatically applies Frontend Master guidelines**.

### Explicit invocation
When the user explicitly requests frontend work:
- "Build a landing page"
- "Fix the UI"
- "Add a component"
- "Implement this design"
- "frontend-master" keyword

The CLI takes full ownership and executes all relevant phases.

## Loading Guide (On-Demand References)

This SKILL.md contains the core workflow. Load reference files when needed:

| When | Load |
|------|------|
| Implementing visual/UI work, making styling or design decisions | `references/design-guide.md` |
| Designing component architecture, making structural decisions | `references/architecture-guide.md` |

For Complex tasks: load both files before Phase 2.
For Medium tasks: load `design-guide.md` if visual work, `architecture-guide.md` if structural.
For Simple tasks: do NOT load reference files — apply inline rules only.

## Core Principles

### Tech Stack Defaults
- **Framework**: Next.js (App Router) with Server Components by default
- **Styling**: Tailwind CSS as the primary styling tool
- **UI Library**: shadcn/ui components when available, Radix UI primitives as fallback
- **State Management**: Server Components + URL state first → React Context → external only if needed
- **TypeScript**: Always use TypeScript with strict typing

### Performance Budget
| Metric | Target |
|--------|--------|
| JS (gzipped) | < 200KB |
| LCP | ≤ 2.5s |
| INP | ≤ 200ms |
| CLS | ≤ 0.1 |

### Key Rules
- **Anti-AI aesthetic**: Never produce generic AI-looking UI (purple gradients, rounded-2xl everywhere, stock card grids). See `references/design-guide.md` for full table.
- **Typography**: Never use Inter, Roboto, Arial, or system-ui as identity font.
- **Server Components first**: Default to Server Components, add `"use client"` only for interactivity.
- **Composition over configuration**: Build small, composable components — not monolithic ones with boolean props.
- **Skeleton loading**: Use skeletons (not spinners) that match the expected content layout.

## Phase 1 — Analysis (Always executes)

### 1A. Project Context Detection
- Detect project type (Next.js App Router / Pages Router / plain React)
- Identify existing styling approach and UI libraries
- Check TypeScript configuration and existing component patterns

### 1B. Task Scope Assessment
- **Simple**: Single component fix, style tweak, text change → Skip Phase 2, go to Phase 3
- **Medium**: 2-5 components, page section, new feature → Brief Phase 2
- **Complex**: Full page, multi-page feature, architectural change → Full Phase 2

Output: "Task scope: {Simple/Medium/Complex} — {rationale}"

### 1C. Existing Component Scan
- Check `.dev/frontend/components/` catalog for reusable components
- Scan project for similar existing components
- Identify installed shadcn/ui components

## Phase 2 — Design (Scope-dependent)

For Complex tasks: load `references/design-guide.md` and `references/architecture-guide.md` before proceeding.

### 2A. Component Architecture
- Break down UI into component hierarchy
- Define props and interfaces
- Plan Server → Client data flow boundary
- Apply composition over configuration (see `references/architecture-guide.md`)

### 2B. Style Strategy
- Determine Tailwind utility classes needed
- Plan responsive breakpoints (mobile-first: 320px, 768px, 1024px+, 1440px)
- For visual work: apply Design Thinking framework (see `references/design-guide.md`)

### 2C. Implementation Plan
Output a concise plan:
```
## Implementation Plan
- Components to create: {list}
- Components to modify: {list}
- shadcn/ui components to use: {list}
- Server/Client boundary: {description}
- Design direction: {if applicable}
- Estimated scope: {Simple/Medium/Complex}
```

Wait for user acknowledgment ONLY for Complex tasks. Proceed automatically for Simple/Medium.

## Phase 3 — Implementation (Always executes)

### 3A. Component Creation
```tsx
// 1. Imports (grouped: react/next → libraries → internal → types)
// 2. Type definitions (props interface)
// 3. Component function (named export)
// 4. Sub-components (small ones in same file)
// 5. Export (named, default only for Next.js pages)
```

### 3B. Styling Rules
- Use Tailwind CSS classes by default
- Use `cn()` from `lib/utils` for conditional class merging
- Mobile-first responsive design with Tailwind prefixes
- Consistent spacing using Tailwind spacing scale (0.25rem increments)
- CSS variables for theme-dependent values
- For typography, color, motion, and spatial composition rules → see `references/design-guide.md`

### 3C. State Management
Follow the state management decision tree (see `references/architecture-guide.md`):
Local → Lifted → URL → Context → Server → External (only if project uses it)

### 3D. Design-to-Code
When implementing from design references:
1. Analyze visual structure: layout, spacing, typography, colors
2. Map design tokens to Tailwind classes
3. Identify shadcn/ui component patterns
4. Implement pixel-perfect layout first, then interactivity
5. Verify responsive behavior matches design intent

### 3E. Error Handling & Loading States
- Skeleton loading matching expected content layout
- Error boundaries for component-level error handling
- Next.js `loading.tsx` and `error.tsx` conventions
- Meaningful fallback UI for edge cases

## Phase 4 — Verification (Always executes)

### 4A. Accessibility Check
- Semantic HTML elements (`<nav>`, `<main>`, `<section>`, `<article>`)
- ARIA labels on interactive elements
- Images have meaningful `alt` text, forms have associated labels
- Color contrast meets WCAG AA standards
- Keyboard navigation works (tab order, focus management)

### 4B. Responsive Design Check
- Layout works on mobile (320px), tablet (768px), desktop (1024px+)
- Touch targets at least 44x44px on mobile
- No horizontal scroll, navigation adapts, images responsive

### 4C. Performance Check
- No unnecessary client-side JavaScript (prefer Server Components)
- Images use `next/image` with proper sizing
- Large components lazy-loaded when appropriate
- No unnecessary re-renders (proper memoization where needed)

### 4D. Auto-Fix
- Fix accessibility issues immediately — do NOT ask for permission
- Fix responsive layout issues immediately — do NOT ask for permission
- Flag architectural performance concerns to the user with suggestions

### 4E. Catalog Update
Update `.dev/frontend/components/`:
- Add new component entries: name, path, props summary, dependencies
- Update modified component entries
- Remove deleted component entries

## File Structure Conventions

```
src/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   └── {route}/
│       ├── page.tsx / layout.tsx / loading.tsx / error.tsx
├── components/
│   ├── ui/           (shadcn/ui)
│   ├── {feature}/    (feature-specific, colocated)
│   └── shared/
├── lib/utils.ts      (cn() and utilities)
├── hooks/
└── types/
```
