---
version: 1.0.0
updated: 2026-04-17
changelog:
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
4. Compare the local version with `1.0.0` (this file's version):
   - Local < `1.0.0` → go to **Update Flow**
   - Local = `1.0.0` → output "frontend-master is already up to date (v1.0.0)" and STOP. Do nothing else.
5. You MUST output the comparison result: "Local: v{local} / Remote: v1.0.0 → {action}"

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
- If fresh install → output "Frontend Master setup complete (v1.0.0)"
- If update → output "Frontend Master updated: v{old} → v{new}"

---

## Role
A comprehensive frontend development skill that takes full ownership of all frontend work in React + Next.js projects, from analysis through implementation to quality verification.

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

## Core Principles

### Tech Stack Defaults
- **Framework**: Next.js (App Router) with Server Components by default
- **Styling**: Tailwind CSS as the primary styling tool
- **UI Library**: shadcn/ui components when available, Radix UI primitives as fallback
- **State Management**: Detect and follow existing patterns. For new projects: Server Components + URL state first, then React Context, then external libraries only if needed
- **TypeScript**: Always use TypeScript with strict typing

### Coding Standards
1. **Component Structure**: One component per file, named exports, explicit prop types
2. **Server Components First**: Default to Server Components, add `"use client"` only when interactivity is needed
3. **File Organization**: Follow Next.js App Router conventions (`app/`, `components/`, `lib/`, `hooks/`)
4. **Accessibility**: Semantic HTML, ARIA attributes, keyboard navigation, screen reader support
5. **Responsive Design**: Mobile-first approach, Tailwind responsive prefixes
6. **Performance**: Lazy loading, image optimization, bundle size awareness

## Phase 1 — Analysis (Always executes)

### 1A. Project Context Detection
- Detect project type (Next.js App Router / Pages Router / plain React)
- Identify existing styling approach (Tailwind, CSS Modules, styled-components, etc.)
- Scan for existing UI libraries (shadcn/ui, MUI, Chakra, etc.)
- Check TypeScript configuration
- Identify existing component patterns and conventions

### 1B. Task Scope Assessment
Evaluate the scope of the current task:
- **Simple**: Single component fix, style tweak, text change → Skip Phase 2, go directly to Phase 3
- **Medium**: New component, page section, or feature with 2-5 components → Execute Phase 2 briefly
- **Complex**: Full page, multi-page feature, architectural change → Execute full Phase 2

Output scope assessment: "Task scope: {Simple/Medium/Complex} — {rationale}"

### 1C. Existing Component Scan
- Check `.dev/frontend/components/` catalog for reusable components
- Scan the project for similar existing components that could be extended or reused
- Identify shadcn/ui components already installed

## Phase 2 — Design (Scope-dependent)

This phase executes fully for Complex tasks, briefly for Medium tasks, and is skipped for Simple tasks.

### 2A. Component Architecture
For Complex tasks:
1. Break down the UI into component hierarchy
2. Define component props and interfaces
3. Plan data flow (Server → Client boundary decisions)
4. Identify shared state requirements
5. Plan route structure (for page-level features)

For Medium tasks:
1. Identify the main component and its props
2. Check if existing components can be composed

### 2B. Style Strategy
- Determine Tailwind utility classes needed
- Identify if custom CSS or Tailwind plugins are needed
- Plan responsive breakpoints and layout strategy
- Consider dark mode support if the project uses it

### 2C. Implementation Plan
Output a concise plan before coding:
```
## Implementation Plan
- Components to create: {list}
- Components to modify: {list}
- shadcn/ui components to use: {list}
- Server/Client boundary: {description}
- Estimated scope: {Simple/Medium/Complex}
```

Wait for user acknowledgment ONLY for Complex tasks. Proceed automatically for Simple/Medium.

## Phase 3 — Implementation (Always executes)

### 3A. Component Creation
When creating components, follow this structure:
```tsx
// 1. Imports (grouped: react/next → libraries → internal → types)
// 2. Type definitions (props interface)
// 3. Component function (named export)
// 4. Sub-components (if any, in same file for small ones)
// 5. Export (named, default only for Next.js pages)
```

### 3B. Styling Rules
- Use Tailwind CSS classes by default
- Use `cn()` utility from `lib/utils` for conditional class merging
- Apply responsive design with mobile-first approach
- Ensure consistent spacing using Tailwind's spacing scale
- Use CSS variables for theme-dependent values

### 3C. State Management
- Prefer Server Components and Server Actions for data mutations
- Use URL search params for filters, pagination, and shareable state
- Use React Context only for client-side shared state
- Use external state libraries only if the project already uses them

### 3D. Design-to-Code
When implementing from a design reference (image, Figma, screenshot):
1. Analyze the visual structure: layout, spacing, typography, colors
2. Map design tokens to Tailwind classes
3. Identify component patterns that match shadcn/ui
4. Implement pixel-perfect layout first, then add interactivity
5. Verify responsive behavior matches the design intent

### 3E. Error Handling & Loading States
- Always add loading states (skeletons, spinners) for async operations
- Implement error boundaries for component-level error handling
- Use Next.js `loading.tsx` and `error.tsx` conventions
- Provide meaningful fallback UI for edge cases

## Phase 4 — Verification (Always executes)

### 4A. Accessibility Check
- Semantic HTML elements used correctly (`<nav>`, `<main>`, `<section>`, `<article>`)
- ARIA labels on interactive elements
- Keyboard navigation works (tab order, focus management)
- Color contrast meets WCAG AA standards
- Images have meaningful `alt` text
- Form inputs have associated labels

### 4B. Responsive Design Check
- Layout works on mobile (320px), tablet (768px), desktop (1024px+)
- Touch targets are at least 44x44px on mobile
- Text is readable without horizontal scroll
- Navigation adapts to screen size
- Images and media are responsive

### 4C. Performance Check
- No unnecessary client-side JavaScript (prefer Server Components)
- Images use `next/image` with proper sizing
- Large components are lazy-loaded when appropriate
- No unnecessary re-renders (proper memoization where needed)
- Bundle impact is minimal

### 4D. Auto-Fix
If Phase 4 checks reveal issues:
1. Fix accessibility issues immediately
2. Fix responsive layout issues immediately
3. Flag performance concerns to the user with specific suggestions
4. Do NOT ask the user before fixing a11y/responsive issues — just fix them

### 4E. Catalog Update
After implementation, update `.dev/frontend/components/`:
- Add new component entries with: name, path, props summary, dependencies
- Update modified component entries
- Remove entries for deleted components

## File Structure Conventions

### Recommended Next.js App Router Structure
```
src/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── {route}/
│   │   ├── page.tsx
│   │   ├── layout.tsx
│   │   ├── loading.tsx
│   │   └── error.tsx
│   └── globals.css
├── components/
│   ├── ui/           (shadcn/ui components)
│   ├── {feature}/    (feature-specific components)
│   └── shared/       (shared components)
├── lib/
│   ├── utils.ts      (cn() and utilities)
│   └── ...
├── hooks/
├── types/
└── styles/
```

## State Management (`.dev/frontend/styles.md`)
- Tracks the project's style guide and conventions
- Updated automatically when new patterns are established
- Serves as a reference for consistent styling decisions
