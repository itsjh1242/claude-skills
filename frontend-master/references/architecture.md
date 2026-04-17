# Architecture Guide

Loaded by Frontend Master when designing component architecture, making structural decisions, or organizing file structure.

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

## Performance Budget

| Metric | Target |
|--------|--------|
| JS (gzipped) | < 200KB |
| LCP | ≤ 2.5s |
| INP | ≤ 200ms |
| CLS | ≤ 0.1 |

Follow Core Web Vitals guidelines. Use Server Components, `next/image` optimization, proper lazy-loading, and minimal client-side JavaScript.

## State Management Decision Tree

Follow this priority order:

1. **Local state** (`useState`) — component-scoped UI state
2. **Lifted state** — shared between parent and children
3. **URL state** — filters, pagination, shareable state (`useSearchParams`)
4. **React Context** — cross-component client state
5. **Server state** — data fetching via Server Components / Server Actions
6. **External store** — only if project already uses one (Zustand, Jotai, etc.)

Never add an external state library unless the project already depends on one.

### State Management Examples

**Local state:**
```tsx
const [isOpen, setIsOpen] = useState(false); // Toggle dropdown
```

**URL state:**
```tsx
const searchParams = useSearchParams();
const filter = searchParams.get('filter') || 'all'; // Shareable URL state
```

**Server state:**
```tsx
// Server Component — fetch at boundary
async function UserProfile({ userId }: { userId: string }) {
  const user = await fetchUser(userId); // Server-side fetch
  return <ProfileCard user={user} />;
}
```

## Server/Client Boundary

Default to Server Components. Add `"use client"` only when:
- Event handlers are needed (`onClick`, `onChange`, etc.)
- React hooks are used (`useState`, `useEffect`, etc.)
- Browser-only APIs are needed (`window`, `document`, etc.)
- Third-party client-side libraries are used

Keep client boundaries as low in the tree as possible — wrap only the interactive parts.

### Server/Client Example

```tsx
// Server Component (default)
export default function ProductsPage() {
  const products = await fetchProducts(); // Server-side

  return (
    <div>
      <h1>Products</h1>
      <ProductList products={products} /> {/* Server */}
      <FilterButton /> {/* Client — only interactive part */}
    </div>
  );
}

// Client Component
"use client";
export function FilterButton() {
  const [isOpen, setIsOpen] = useState(false); // Hook
  return <button onClick={() => setIsOpen(!isOpen)}>Filter</button>;
}
```

## Loading & Error Patterns

- Use **skeleton loading** (not spinners) — match the layout shape of expected content
- Support **optimistic updates** where appropriate (UI updates immediately, reverts on error)
- Use Next.js `loading.tsx` and `error.tsx` conventions
- Provide meaningful fallback UI for edge cases

## File Structure Conventions

```
src/
├── app/
│   ├── layout.tsx           # Root layout
│   ├── page.tsx             # Home page
│   ├── loading.tsx          # Root loading UI
│   ├── error.tsx            # Root error UI
│   └── {route}/
│       ├── page.tsx         # Route page
│       ├── layout.tsx       # Route layout (optional)
│       ├── loading.tsx      # Route loading UI (optional)
│       └── error.tsx        # Route error UI (optional)
│
├── components/
│   ├── ui/                  # shadcn/ui components (reusable primitives)
│   │   ├── button.tsx
│   │   ├── card.tsx
│   │   └── ...
│   ├── {feature}/           # Feature-specific components (colocated)
│   │   ├── {feature}-card.tsx
│   │   ├── {feature}-list.tsx
│   │   └── {feature}-skeleton.tsx
│   └── shared/              # Cross-feature components
│       ├── header.tsx
│       ├── footer.tsx
│       └── navigation.tsx
│
├── lib/
│   ├── utils.ts            # Utility functions (cn(), etc.)
│   └── db.ts               # Database utilities (if needed)
│
├── hooks/                   # Custom React hooks (client-only)
│   ├── use-data.ts
│   └── use-form.ts
│
└── types/                   # TypeScript type definitions
    ├── api.ts
    └── models.ts
```

### Component File Structure

Each component file should follow this structure:

```tsx
// 1. Imports (grouped: react/next → libraries → internal → types)
import { useState } from 'react';
import { Button } from '@/components/ui/button';
import { cn } from '@/lib/utils';
import type { Product } from '@/types/models';

// 2. Type definitions
interface ProductCardProps {
  product: Product;
  onEdit?: (id: string) => void;
}

// 3. Component function (named export)
export function ProductCard({ product, onEdit }: ProductCardProps) {
  // 4. Hooks (if client component)
  const [isHovered, setIsHovered] = useState(false);

  // 5. Event handlers
  const handleEdit = () => {
    onEdit?.(product.id);
  };

  // 6. Render
  return (
    <div
      className={cn(
        "p-4 rounded-lg border",
        isHovered && "border-primary"
      )}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      <h3>{product.name}</h3>
      <Button onClick={handleEdit}>Edit</Button>
    </div>
  );
}
```

## Project Type Detection

On project load, detect:

1. **Framework**: Next.js App Router / Pages Router / plain React
2. **Styling**: Tailwind CSS / CSS Modules / Styled Components / plain CSS
3. **UI Library**: shadcn/ui / Radix UI / Material UI / none
4. **TypeScript**: Yes / No
5. **State Library**: Zustand / Jotai / Redux / none

Adapt architecture recommendations based on detected setup.

## Component Cataloging

Maintain `.dev/frontend/components/` catalog:

```markdown
# Component Catalog

## UI Components (shadcn/ui)

### Button
- Path: `components/ui/button.tsx`
- Props: `variant`, `size`, `asChild`
- Usage: Primary/secondary actions

### Card
- Path: `components/ui/card.tsx`
- Props: `header`, `content`, `footer`
- Usage: Content containers

## Feature Components

### ProductCard
- Path: `components/products/product-card.tsx`
- Props: `product`, `onEdit`
- Dependencies: Button, Card
- Status: Server Component
```

Update catalog when components are added, modified, or removed.
