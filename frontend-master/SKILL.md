---
name: frontend-master
description: "React + Next.js frontend expert.\nTRIGGER when: create/modify/fix/improve/refactor components, pages, layouts, or styling; work with `.tsx`/`.jsx`/`.css`/`.scss` files; mention `Tailwind`/`shadcn`.\nDO NOT TRIGGER when: backend-only (API routes, DB, server logic), DevOps/CI-CD, non-frontend files (`.py`, `.go`, `.sql`)."
updated: 2026-04-17
version: 3.2.0
changelog:
  - "3.2.0: Optimize description — explicit \\n linebreaks, remove redundancy, add backticks (~33% token reduction)"
  - "3.1.0: Restructured description with TRIGGER/DO NOT TRIGGER pattern for precise activation"
  - "3.0.1: Improved description with follow-up task keywords (create, modify, improve, fix, update, refactor)"
  - "3.0.0: Radical simplification — 365 lines → 88 lines → 61 lines (83% reduction) following Anthropic's pattern"
  - "2.0.0: Routing table structure + design/architecture references"
  - "1.0.0: Initial release"
---

# Frontend Master

Takes full ownership of all frontend work in React + Next.js projects.

## Tech Stack Defaults

- **Framework**: Next.js (App Router) with Server Components by default
- **Styling**: Tailwind CSS + shadcn/ui when available
- **State Management**: Server Components → URL state → React Context → external (only if project uses it)
- **TypeScript**: Always with strict typing

## Key Rules

- **Anti-AI aesthetic**: Never produce generic AI-looking UI (purple gradients, rounded-2xl everywhere, stock card grids)
- **Typography**: Never use Inter, Roboto, Arial, or system-ui as identity font
- **Server Components first**: Default to Server Components, add `"use client"` only for interactivity
- **Composition over configuration**: Build small, composable components — not monolithic ones with boolean props
- **Skeleton loading**: Use skeletons matching expected content layout

## Workflow

### 1. Design Thinking
Before implementing UI/visual work, define: **Purpose** (what to communicate), **Tone** (emotional register), **Constraints** (technical limits), **Differentiation** (what makes it NOT generic AI).

→ Load `references/design-thinking.md` for aesthetic directions, font pairings, color strategies, anti-AI rules.

### 2. Component Architecture
Break down UI into component hierarchy, define props/interfaces, plan Server → Client boundary, apply composition over configuration.

→ Load `references/architecture.md` for patterns, state decision tree, file structure, performance targets.

### 3. Implementation
Use Tailwind CSS classes, mobile-first responsive design, Server Components first → Client only for interactivity, follow state management decision tree (Local → Lifted → URL → Context → Server → External).

### 4. Verification
Auto-fix accessibility (semantic HTML, ARIA labels, WCAG AA contrast) and responsive layout issues immediately. Flag architectural performance concerns to user.

## Loading Guide

- **Design/UI work**: `references/design-thinking.md`
- **Component architecture**: `references/architecture.md`
- **Brand reference needed**: `references/brand-systems.md`
- **Complex tasks** (full page, multi-feature): Load all relevant references above
- **Simple tasks** (single component fix, style tweak): Do NOT load references — apply inline rules only

---

## Setup

See [README.md](README.md) for installation and update instructions.
