# Design Guide

Loaded by Frontend Master when implementing visual/UI work or making styling decisions.

## Design Thinking Framework

Before implementing visual work, define:
1. **Purpose**: What should this UI communicate or accomplish?
2. **Tone**: What emotional register? (authoritative, playful, calm, urgent, etc.)
3. **Constraints**: Technical limits, brand requirements, accessibility needs
4. **Differentiation**: What makes this NOT look like generic AI output?

## Anti-AI Aesthetic Rules

NEVER produce generic "AI-looking" UI:

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

## Typography

**Banned fonts**: NEVER use Inter, Roboto, Arial, or system-ui as the primary identity font.

Rules:
- Pair a **distinctive display font** (headings, identity) with a **refined body font** (readability)
- Establish a strict hierarchy: h1 → h2 → h3 → body → small
- Use consistent spacing scale in 0.25rem (4px) increments

### Font Pairing by Aesthetic Direction

| Direction | Display Font (headings) | Body Font (reading) | Vibe |
|-----------|------------------------|---------------------|------|
| **Brutally minimal** | Space Grotesk, Syncopate | Inter, Space Grotesk (light) | Stark, geometric, unapologetic |
| **Maximalist chaos** | Abril Fatface, Rage Italic | Space Mono, IBM Plex Mono | Dense, loud, overwhelming |
| **Retro-futuristic** | Orbitron, Press Start 2P | Rajdhani, VT323 | Nostalgic tech, arcade |
| **Organic/natural** | Comfortaa, Alegreya | Quicksand, Lora | Flowing, warm, human |
| **Luxury/refined** | Cormorant Garamond, Playfair Display | Lato, EB Garamond | Premium, elegant, timeless |
| **Playful/toy-like** | Fredoka, Poppins | Nunito, Quicksand | Fun, approachable, dimensional |
| **Editorial/magazine** | Merriweather, Source Serif Pro | Libre Baskerville, Source Sans Pro | Content-first, journalistic |
| **Brutalist/raw** | Archivo Black, Bebas Neue | Archivo, Roboto Mono | Exposed, industrial, raw |
| **Art deco/geometric** | Monoton, Righteous | Montserrat, Work Sans | Sharp angles, symmetry |
| **Soft/pastel** | Nunito, Muli | Quicksand, Poppins | Gentle, rounded, light |
| **Industrial/utilitarian** | JetBrains Mono, Space Mono | IBM Plex Sans, Roboto | Functional, dense, technical |

### Safe Defaults (When Uncertain)

If the aesthetic direction is unclear, these work for most projects:
- **Tech/Startup**: Plus Jakarta Sans (display) + Inter (body)
- **Content/Editorial**: Source Serif Pro (display) + Source Sans Pro (body)
- **E-commerce**: Poppins (display) + Open Sans (body)
- **SaaS**: Geist (display) + Geist Sans (body)

All fonts above are available on Google Fonts (free, easy loading via `next/font/google`).

## Color

- Use **semantic tokens** (`text-primary`, `bg-surface`, `border-muted`), not raw hex values
- Define CSS variables for theme-dependent values
- Choose a dominant palette with 1-2 sharp accent colors
- Never use generic blue (#3B82F6) as primary unless explicitly requested
- Ensure WCAG 2.2 AA contrast ratios (4.5:1 normal text, 3:1 large text)

## Motion & Animation

- For static HTML: CSS-only animations (keyframes, transitions)
- For React components: prefer CSS transitions; use Motion library for complex sequences
- Use staggered reveals with `animation-delay` for lists and grids
- Respect `prefers-reduced-motion` — always provide a reduced-motion fallback
- Animations should serve purpose (guide attention, provide feedback), not decorate

## Spatial Composition

- Break predictable symmetry — introduce asymmetry, overlap, or diagonal flow
- Use whitespace as a deliberate design element, not default padding
- Create visual hierarchy through size contrast, not just color
- Avoid centering everything — use alignment to create visual tension

## Aesthetic Directions

When starting fresh, choose a direction that serves the project's purpose:

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
