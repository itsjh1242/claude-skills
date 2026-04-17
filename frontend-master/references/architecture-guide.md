# Architecture Guide

Loaded by Frontend Master when designing component architecture or making structural decisions.

## Component Architecture Principles

### Composition over Configuration
Build small, focused components that compose together — not monolithic components with boolean props.

**Bad**: One component with `showIcon`, `showBadge`, `variant`, `size` props controlling everything
**Good**: `<Card>`, `<CardHeader>`, `<CardBadge>`, `<CardIcon>` — compose as needed

### Colocated Files
Keep component-related files together:

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

### Separation of Concerns
- **Data fetching**: Server Components — fetch at the boundary, pass down as props
- **Presentation**: Presentational components — receive data, render UI
- **Interactivity**: Client Components (`"use client"`) — only when needed (event handlers, hooks, browser APIs)

### File-per-Component
One component per file, named exports. Default export only for Next.js pages.

## State Management Decision Tree

Follow this priority order:

1. **Local state** (`useState`) — component-scoped UI state
2. **Lifted state** — shared between parent and children
3. **URL state** — filters, pagination, shareable state (`useSearchParams`)
4. **React Context** — cross-component client state
5. **Server state** — data fetching via Server Components / Server Actions
6. **External store** — only if project already uses one (Zustand, Jotai, etc.)

Never add an external state library unless the project already depends on one.

## Server/Client Boundary

Default to Server Components. Add `"use client"` only when:
- Event handlers are needed (`onClick`, `onChange`, etc.)
- React hooks are used (`useState`, `useEffect`, etc.)
- Browser-only APIs are needed (`window`, `document`, etc.)
- Third-party client-side libraries are used

Keep client boundaries as low in the tree as possible — wrap only the interactive parts.

## Loading & Error Patterns

- Use **skeleton loading** (not spinners) — match the layout shape of expected content
- Support **optimistic updates** where appropriate (UI updates immediately, reverts on error)
- Use Next.js `loading.tsx` and `error.tsx` conventions
- Provide meaningful fallback UI for edge cases
