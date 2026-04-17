---
name: frontend-master
description: "React + Next.js frontend expert.\nTRIGGER when: create/modify/fix/improve/refactor components, pages, layouts, or styling; work with `.tsx`/`.jsx`/`.css`/`.scss` files; mention `Tailwind`/`shadcn`.\nDO NOT TRIGGER when: backend-only (API routes, DB, server logic), DevOps/CI-CD, non-frontend files (`.py`, `.go`, `.sql`)."
updated: 2026-04-17
version: 3.3.0
changelog:
  - "3.3.0: Restore Setup section (Step 0~4) for proper installation and CLAUDE.md trigger registration"
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

### Step 0 — Version check (MANDATORY — do NOT skip)
You MUST perform all of the following steps before doing anything else:
1. Read the file `.claude/skills/frontend-master/SKILL.md` in the current project
2. If the file does NOT exist → this is a fresh install. Go to Step 1.
3. If the file exists → extract the `version:` value from its YAML frontmatter
   - If the file has no `version` field → treat as `0.0.0`
4. Compare the local version with `3.3.0` (this file's version):
   - Local < `3.3.0` → go to **Update Flow**
   - Local = `3.3.0` → output "frontend-master is already up to date (v3.3.0)" and STOP. Do nothing else.
5. You MUST output the comparison result: "Local: v{local} / Remote: v3.3.0 → {action}"

### Update Flow
When updating from an older version:
1. Overwrite `.claude/skills/frontend-master/SKILL.md` with this file
2. Replace the `# Frontend Master` section in `CLAUDE.md` with the latest version from Step 3 below
3. Show the user the changelog entries between their old version and the current version
4. Output "frontend-master updated: v{old} → v{new}"

### Step 1 — Install skill file
Create the following directory at the project root:
- `.claude/skills/frontend-master/`

Fetch this SKILL.md file and save it as `.claude/skills/frontend-master/SKILL.md`.

### Step 2 — Add to CLAUDE.md
Add the following instructions to `CLAUDE.md` at the project root (create it if it doesn't exist):

```
# Frontend Master
프론트엔드 관련 작업(컴포넌트 생성/수정/개선/수정, 페이지 작업, 스타일링, 레이아웃, React/Next.js/Tailwind/shadcn 관련)을 요청받으면:
1. `.claude/skills/frontend-master/SKILL.md`를 읽는다
2. 해당 스킬의 모든 규칙과 워크플로우를 순서대로 따른다
3. Loading Guide에 따라 필요한 reference 파일을 로드한다
백엔드 전용 작업(API routes, DB, 서버 로직)이나 DevOps/CI-CD 작업에는 적용하지 않는다.
```

### Step 3 — Verify
- Confirm `.claude/skills/frontend-master/SKILL.md` exists
- Confirm the Frontend Master section exists in `CLAUDE.md`
- If fresh install → output "Frontend Master setup complete (v3.3.0)"
- If update → output "Frontend Master updated: v{old} → v{new}"
