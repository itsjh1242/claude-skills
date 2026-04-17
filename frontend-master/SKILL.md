---
version: 2.0.0
updated: 2026-04-17
changelog:
  - "2.0.0: Major upgrade — merge 5 external skills into unified frontend mastery"
  - "  + Anthropic frontend-design: design thinking, aesthetic directions, typography/color/motion/spatial rules, anti-AI-slop"
  - "  + Addy Osmani frontend-ui-engineering: component architecture, composition patterns, anti-AI aesthetic table, state decision tree"
  - "  + Addy Osmani performance-optimization: Core Web Vitals, performance budget, anti-patterns, measurement workflow"
  - "  + masuP9 a11y-specialist-skills: WCAG 2.2 AA, automated test scripts (axe-core + Playwright), 4-phase audit"
  - "  + Anthropic webapp-testing: Playwright browser testing, reconnaissance-then-action pattern"
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
A comprehensive frontend development skill that takes full ownership of all frontend work in React + Next.js projects — from design thinking and component architecture through implementation to performance verification and accessibility auditing.

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
- **Testing**: Playwright for browser testing, axe-core for accessibility automation

### Performance Budget
| Metric | Target |
|--------|--------|
| JavaScript (gzipped) | < 200KB |
| CSS | < 50KB |
| Images (per image) | < 200KB |
| Fonts | < 100KB |
| API response (p95) | < 200ms |
| TTI on 4G | < 3.5s |
| Lighthouse score | ≥ 90 |
| LCP | ≤ 2.5s |
| INP | ≤ 200ms |
| CLS | ≤ 0.1 |

### Anti-AI Aesthetic Rules
NEVER produce generic "AI-looking" UI. The following patterns are banned:

| Pattern | Instead |
|---------|---------|
| Purple/indigo gradient on white | Use intentional, distinctive color schemes |
| Excessive rounded corners (rounded-2xl everywhere) | Vary border-radius intentionally |
| Generic hero sections ("Welcome to the future") | Lead with value, specific messaging |
| Lorem ipsum or placeholder copy | Write real, meaningful content |
| Oversized padding/hero sections | Respect density — not everything needs breathing room |
| Stock card grids (3-column icon+title+desc) | Break the grid, use editorial layouts |
| Shadow-heavy card design | Use borders, contrast, or background shifts |
| Predictable symmetric layouts | Introduce asymmetry, overlap, visual tension |

**Typography bans**: NEVER use Inter, Roboto, Arial, or system-ui as the primary identity font. Choose distinctive fonts that serve the project's aesthetic direction.

### State Management Decision Tree
Follow this priority order:
1. **Local state** (`useState`) — component-scoped UI state
2. **Lifted state** — shared between parent and children
3. **URL state** — filters, pagination, shareable state (`useSearchParams`)
4. **React Context** — cross-component client state
5. **Server state** — data fetching via Server Components / Server Actions
6. **External store** — only if project already uses one (Zustand, Jotai, etc.)

Never add an external state library unless the project already depends on one.

## Phase 1 — Analysis (Always executes)

### 1A. Project Context Detection
- Detect project type (Next.js App Router / Pages Router / plain React)
- Identify existing styling approach (Tailwind, CSS Modules, styled-components, etc.)
- Scan for existing UI libraries (shadcn/ui, MUI, Chakra, etc.)
- Check TypeScript configuration
- Identify existing component patterns and conventions
- Detect existing test infrastructure (Playwright, Jest, Vitest, etc.)
- Check for accessibility testing tools (axe-core, eslint-plugin-jsx-a11y, etc.)

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
- Check for existing Playwright test files and configuration

### 1D. Design Direction Assessment
For visual/UI tasks, determine the aesthetic direction:
- Review existing project design language and patterns
- If the project has an established visual identity → follow it
- If starting fresh → assess what direction serves the project's purpose:
  - **Brutally minimal**: stark contrast, whitespace, restraint
  - **Maximalist chaos**: dense, layered, overwhelming detail
  - **Retro-futuristic**: nostalgic + modern tech aesthetic
  - **Organic/natural**: flowing shapes, earth tones, soft edges
  - **Luxury/refined**: premium, elegant, understated
  - **Playful/toy-like**: fun, approachable, dimensional
  - **Editorial/magazine**: strong typography, grid-based, content-first
  - **Brutalist/raw**: exposed structure, raw materials, intentional roughness
  - **Art deco/geometric**: sharp angles, symmetry, metallic accents
  - **Soft/pastel**: gentle gradients, rounded forms, light palette
  - **Industrial/utilitarian**: functional, dense, data-dense

## Phase 2 — Design (Scope-dependent)

This phase executes fully for Complex tasks, briefly for Medium tasks, and is skipped for Simple tasks.

### 2A. Component Architecture
For Complex tasks:
1. Break down the UI into component hierarchy
2. Define component props and interfaces
3. Plan data flow (Server → Client boundary decisions)
4. Identify shared state requirements
5. Plan route structure (for page-level features)

Component architecture principles:
- **Composition over configuration**: Build small, focused components that compose together — not monolithic components with boolean props
- **Colocated files**: Keep component-related files together (component, styles, tests, stories in same directory)
- **Separation of concerns**: Separate data fetching (Server Components) from presentation (presentational components)
- **File-per-component**: One component per file, named exports

```
components/
├── {feature}/
│   ├── feature-card.tsx
│   ├── feature-list.tsx
│   ├── feature-skeleton.tsx
│   └── use-feature-data.ts
├── ui/                    (shadcn/ui components)
└── shared/                (cross-feature components)
```

For Medium tasks:
1. Identify the main component and its props
2. Check if existing components can be composed

### 2B. Style Strategy
- Determine Tailwind utility classes needed
- Identify if custom CSS or Tailwind plugins are needed
- Plan responsive breakpoints and layout strategy
- Consider dark mode support if the project uses it

Typography strategy:
- Pair a **distinctive display font** (headings, identity) with a **refined body font** (readability)
- Establish a strict hierarchy: h1 → h2 → h3 → body → small
- Use consistent spacing scale in 0.25rem (4px) increments

Color strategy:
- Use **semantic tokens** (`text-primary`, `bg-surface`, `border-muted`), not raw hex values
- Define CSS variables for theme-dependent values
- Choose a dominant palette with 1-2 sharp accent colors
- Ensure WCAG 2.2 AA contrast ratios

### 2C. Design Thinking Framework
Before implementing visual work, define:
1. **Purpose**: What should this UI communicate or accomplish?
2. **Tone**: What emotional register? (authoritative, playful, calm, urgent, etc.)
3. **Constraints**: Technical limits, brand requirements, accessibility needs
4. **Differentiation**: What makes this NOT look like a generic AI output?

### 2D. Implementation Plan
Output a concise plan before coding:
```
## Implementation Plan
- Components to create: {list}
- Components to modify: {list}
- shadcn/ui components to use: {list}
- Server/Client boundary: {description}
- Design direction: {aesthetic direction}
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
- Ensure consistent spacing using Tailwind's spacing scale (0.25rem increments)
- Use CSS variables for theme-dependent values

**Color implementation**:
- Use semantic tokens: `text-primary`, `bg-surface`, `border-muted`
- Define dominant colors with sharp accents in CSS variables
- Never use generic blue (#3B82F6) as primary unless explicitly requested

**Typography implementation**:
- Use distinctive fonts, never Inter/Roboto/Arial as identity font
- Maintain strict hierarchy: h1 → h2 → h3 → body → small
- Pair display font (identity) with body font (readability)

**Motion & animation**:
- For static HTML: CSS-only animations (keyframes, transitions)
- For React components: prefer CSS transitions; use Motion library for complex sequences
- Use staggered reveals with `animation-delay` for lists and grids
- Respect `prefers-reduced-motion` — always provide a reduced-motion fallback
- Animations should serve purpose (guide attention, provide feedback), not decorate

**Spatial composition**:
- Break predictable symmetry — introduce asymmetry, overlap, or diagonal flow
- Use whitespace as a deliberate design element, not default padding
- Create visual hierarchy through size contrast, not just color
- Avoid centering everything — use alignment to create visual tension

### 3C. State Management
- Prefer Server Components and Server Actions for data mutations
- Use URL search params for filters, pagination, and shareable state
- Use React Context only for client-side shared state
- Use external state libraries only if the project already uses them
- Follow the state management decision tree (see Core Principles)

### 3D. Design-to-Code
When implementing from a design reference (image, Figma, screenshot):
1. Analyze the visual structure: layout, spacing, typography, colors
2. Map design tokens to Tailwind classes
3. Identify component patterns that match shadcn/ui
4. Implement pixel-perfect layout first, then add interactivity
5. Verify responsive behavior matches the design intent
6. Ensure the implementation captures the design's spatial rhythm and visual weight

### 3E. Error Handling & Loading States
- Use **skeleton loading** (not spinners) for content areas — match the layout shape of the expected content
- Implement error boundaries for component-level error handling
- Use Next.js `loading.tsx` and `error.tsx` conventions
- Provide meaningful fallback UI for edge cases
- Support optimistic updates where appropriate (UI updates immediately, reverts on error)

## Phase 4 — Verification (Always executes)

### 4A. Accessibility Check (WCAG 2.2 AA)

#### Automated checks (always run)
- Semantic HTML elements used correctly (`<nav>`, `<main>`, `<section>`, `<article>`)
- ARIA labels on interactive elements
- Images have meaningful `alt` text
- Form inputs have associated labels
- Color contrast meets WCAG 2.2 AA standards (4.5:1 for normal text, 3:1 for large text)
- Focus indicators are visible (minimum 2px solid outline or equivalent)

#### Interactive checks (for Complex tasks)
- Keyboard navigation works (tab order, focus management)
- Focus is trapped correctly in modals and dialogs
- Skip-to-content link is present on pages
- All interactive elements are reachable via keyboard

#### Automated test scripts (when Playwright is configured)
If the project has Playwright set up, generate accessibility test scripts:

**Focus indicator test** (WCAG 2.4.7, 2.4.12, 3.2.1):
```ts
import { test, expect } from '@playwright/test';

test('focus indicators are visible', async ({ page }) => {
  await page.goto('/');
  const focusable = page.locator('a, button, input, select, textarea, [tabindex]');
  const count = await focusable.count();
  for (let i = 0; i < Math.min(count, 20); i++) {
    await focusable.nth(i).focus();
    const outline = await focusable.nth(i).evaluate(el =>
      getComputedStyle(el).outline + getComputedStyle(el).boxShadow
    );
    expect(outline).not.toBe('nonenone');
  }
});
```

**Reflow test at 320px** (WCAG 1.4.10):
```ts
test('content reflows at 320px without horizontal scroll', async ({ page }) => {
  await page.setViewportSize({ width: 320, height: 568 });
  await page.goto('/');
  const scrollWidth = await page.evaluate(() => document.documentElement.scrollWidth);
  expect(scrollWidth).toBeLessThanOrEqual(320);
});
```

**Text spacing test** (WCAG 1.4.12):
```ts
test('text remains readable with increased spacing', async ({ page }) => {
  await page.goto('/');
  await page.addStyleTag({ content: `
    * { line-height: 1.5 !important; letter-spacing: 0.12em !important;
        word-spacing: 0.16em !important; }
    p, li, h1, h2, h3 { margin-bottom: 2em !important; }
  `});
  // Check no text overflows its container
  const overflows = await page.evaluate(() =>
    Array.from(document.querySelectorAll('p, li, h1, h2, h3, span')).filter(el =>
      el.scrollWidth > el.clientWidth + 2
    ).length
  );
  expect(overflows).toBe(0);
});
```

**Target size test** (WCAG 2.5.5, 2.5.8):
```ts
test('touch targets meet minimum size', async ({ page }) => {
  await page.goto('/');
  const targets = page.locator('a, button, input[type="submit"], input[type="button"]');
  const count = await targets.count();
  for (let i = 0; i < Math.min(count, 30); i++) {
    const box = await targets.nth(i).boundingBox();
    if (box) {
      expect(box.width).toBeGreaterThanOrEqual(44);
      expect(box.height).toBeGreaterThanOrEqual(44);
    }
  }
});
```

### 4B. Responsive Design Check
- Layout works on mobile (320px), tablet (768px), desktop (1024px+), ultrawide (1440px)
- Touch targets are at least 44x44px on mobile
- Text is readable without horizontal scroll
- Navigation adapts to screen size
- Images and media are responsive

### 4C. Performance Check

#### Core Web Vitals measurement
| Metric | Good | Needs improvement | Poor |
|--------|------|-------------------|------|
| LCP | ≤ 2.5s | 2.5s – 4.0s | > 4.0s |
| INP | ≤ 200ms | 200ms – 500ms | > 500ms |
| CLS | ≤ 0.1 | 0.1 – 0.25 | > 0.25 |

#### Performance verification workflow
1. **Measure**: Check Core Web Vitals and bundle size
2. **Identify**: Use symptom-based diagnosis:
   - Slow LCP → check image optimization, font loading, server response time
   - High INP → check event handlers, unnecessary re-renders, heavy computations
   - High CLS → check unsized images/fonts, dynamic content injection, late-loading ads
3. **Fix**: Apply targeted optimizations
4. **Verify**: Re-measure to confirm improvement
5. **Guard**: Ensure no regression in CI

#### Common anti-patterns to check
- **N+1 data fetching**: Multiple sequential requests instead of batched/parallel
- **Unbounded data fetching**: Missing pagination or limits on API responses
- **Missing image optimization**: Not using `next/image`, wrong sizes, no lazy loading
- **Unnecessary client JS**: Using `"use client"` when Server Component suffices
- **Unnecessary re-renders**: Missing memoization for expensive computations
- **Large bundle**: Importing entire libraries instead of specific modules
- **Missing loading states**: No skeleton/spinner for async content → layout shift
- **Render-blocking resources**: Synchronous scripts/stylesheets in critical path

#### Performance anti-pattern code examples

**Bad: N+1 fetching**
```tsx
// Each child fetches independently
items.map(item => <ItemDetail id={item.id} />) // each makes its own fetch
```
**Good: Batched fetching**
```tsx
const allDetails = await fetchDetails(items.map(i => i.id))
items.map(item => <ItemDetail detail={allDetails[item.id]} />)
```

**Bad: Unsized image (causes CLS)**
```tsx
<img src="/hero.jpg" alt="Hero" />
```
**Good: Sized with next/image**
```tsx
<Image src="/hero.jpg" alt="Hero" width={1200} height={630} priority />
```

**Bad: Entire library import**
```tsx
import { debounce } from 'lodash' // imports all of lodash
```
**Good: Specific import**
```tsx
import debounce from 'lodash/debounce'
```

### 4D. Browser Testing (when applicable)

For Complex tasks or when the user requests visual verification, use Playwright-based browser testing.

#### Testing decision tree
- Is the page static HTML (no JS required)? → Use direct HTML inspection
- Is it a dynamic webapp? → Use Playwright
  - Server already running? → Connect directly
  - No server? → Use server lifecycle management to start/stop

#### Reconnaissance-then-action pattern
1. **Navigate** to the target page
2. **Wait** for `networkidle` (or specific element)
3. **Screenshot** the current state
4. **Inspect DOM** to identify correct selectors
5. **Execute actions** using descriptive selectors
6. **Verify** the result visually and structurally

#### Best practices
- Use Playwright in sync mode (`sync_playwright()`)
- Prefer descriptive selectors: `get_by_role()`, `get_by_label()`, `get_by_test_id()`
- Take screenshots before and after actions for visual comparison
- Wait for network idle before asserting state
- Handle server lifecycle automatically — start before tests, stop after

### 4E. Auto-Fix
If Phase 4 checks reveal issues:
1. Fix accessibility issues immediately — do NOT ask for permission
2. Fix responsive layout issues immediately — do NOT ask for permission
3. Fix performance anti-patterns immediately (sized images, bundle imports, missing skeletons)
4. Flag architectural performance concerns to the user with specific suggestions
5. Generate Playwright test files for verified components (Complex tasks)

### 4F. Catalog Update
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
│   ├── {feature}/    (feature-specific, colocated)
│   └── shared/       (shared components)
├── lib/
│   ├── utils.ts      (cn() and utilities)
│   └── ...
├── hooks/
├── types/
├── styles/
└── tests/
    └── e2e/          (Playwright tests)
```

## State Management (`.dev/frontend/styles.md`)
- Tracks the project's style guide and conventions
- Updated automatically when new patterns are established
- Serves as a reference for consistent styling decisions
