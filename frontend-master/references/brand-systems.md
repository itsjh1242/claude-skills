# Brand Systems Guide

Loaded by Frontend Master when brand reference is needed or when user requests brand-aligned design.

## What is awesome-design-md?

[awesome-design-md](https://github.com/VoltAgent/awesome-design-md) is a collection of 68+ brand design systems in markdown format. Each brand has a `DESIGN.md` file that captures:

- **Visual Theme**: Overall aesthetic direction
- **Color Palette**: Primary, secondary, accent colors with hex codes
- **Typography**: Font families, sizes, weights, line heights
- **Component Stylings**: Buttons, cards, inputs, modals, etc.
- **Layout Principles**: Spacing, grids, alignment
- **Depth & Elevation**: Shadows, borders, layering
- **Do's and Don'ts**: Common patterns to avoid
- **Responsive Behavior**: Mobile/tablet/desktop adaptations

## When to Use Brand Systems

Use brand design systems when:

### 1. Brand Alignment Request
- Client says "Make it look like Stripe/Linear/Vercel"
- User references specific brand: "Apple-style clean", "Discord-like playful"
- Competitive analysis needed: "Similar to [competitor] but different"

### 2. Design Blockage
- Aesthetic direction unclear → Use brand as starting point
- Multiple stakeholders with different tastes → Reference respected brand
- Fast prototyping needed → Brand system as foundation

### 3. Component Pattern Reference
- Need specific component patterns: tables, modals, navigation, dashboards
- Best practices for complex components: data grids, forms, charts
- Responsive patterns for specific component types

### 4. Color & Typography Inspiration
- Color palette structure: how many colors, naming conventions
- Typography scale: font sizes, line heights, spacing
- Dark mode implementations: color shifts, contrast ratios

## How to Use Brand Systems

### Step 1: Browse awesome-design-md

Visit [https://github.com/VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md)

Browse available brands and find 2-3 that match:
- Your aesthetic direction (minimal, playful, brutalist, etc.)
- Your industry/sector (fintech, SaaS, e-commerce, etc.)
- Your target audience (developers, consumers, businesses)

### Step 2: Read DESIGN.md Files

Click into brand directories and read their `DESIGN.md` files.

Extract:
- **Color palette**: Hex codes, semantic naming
- **Typography**: Font families, pairings, scales
- **Component patterns**: Implementation details
- **Layout principles**: Spacing, grid systems

### Step 3: Adapt, Don't Copy

Use brand systems as **reference, not template**:

- ✅ Extract principles: "Vercel uses monochrome + 1 accent color"
- ✅ Adapt patterns: "Linear uses subtle animations on hover"
- ✅ Learn structure: "Stripe uses 8px spacing scale"
- ❌ Don't clone: Exact hex codes, copy text, steal logos

**Always customize** for your project:
- Different brand colors
- Different content/context
- Different technical constraints
- Different user needs

### Step 4: Combine Selectively

Mix and match from multiple brands:

```
Color palette: Stripe (vibrant gradients)
Typography: Notion (editorial serif)
Layout: Vercel (minimal, generous whitespace)
Animations: Linear (subtle micro-interactions)
```

## Popular Brand Quick Reference

### Developer Tools & Technical Products

| Brand | Aesthetic | Key Characteristics |
|-------|-----------|-------------------|
| **Vercel** | Minimal + Monochrome + Geometric | Black/white, Inter, sharp edges, generous whitespace |
| **GitHub** | Functional + Dense | Green accent, Inter/Primer, data-dense, utilitarian |
| **Linear** | Dark-first + Subtle animations | Dark mode default, Inter/Geist, micro-interactions, purple accent |
| **Figma** | Creative + Multi-color | Colorful tools, Inter, playful interactions, canvas-based |

### Fintech & Payments

| Brand | Aesthetic | Key Characteristics |
|-------|-----------|-------------------|
| **Stripe** | Vibrant gradients + Clean | Blurple gradients, Inter, polished, trust-building |
| **PayPal** | Blue + Trustworthy | Blue primary, PayPal Sans, clean, professional |
| **Square** | Industrial + Utilitarian | Black/white, Square Sans, functional, bold typography |

### Consumer & Lifestyle

| Brand | Aesthetic | Key Characteristics |
|-------|-----------|-------------------|
| **Airbnb** | Warm + Photographic + Human | Coral accent, Circular, warm tones, photo-first |
| **Discord** | Playful + Rounded + Colorful | Blurple,gg sans, rounded corners, colorful |
| **Spotify** | Bold + Dark + Green accent | Black/green, Circular, bold typography, gradient text |

### Content & Documentation

| Brand | Aesthetic | Key Characteristics |
|-------|-----------|-------------------|
| **Notion** | Editorial + Content-focused | Monochrome, Inter/Sentinel, content-first, minimal UI |
| **Medium** | Editorial + Typography-focused | Green accent, Austin Editorial, reading-optimized |
| **GitBook** | Clean + Minimal | Blue accent, Inter, documentation-focused |

### Premium & Luxury

| Brand | Aesthetic | Key Characteristics |
|-------|-----------|-------------------|
| **Apple** | Premium + Refined whitespace | Black/white, SF Pro, generous padding, minimalist |
| **Tesla** | Futuristic + Bold | Red/black, Gotham, bold, futuristic |
| **Sonos** | Minimal + Audio-focused | Black/white, Proxima Nova, clean, sophisticated |

## Example Workflows

### Scenario 1: Client Says "Make it Linear-like"

```
1. → Read Linear DESIGN.md
2. → Extract: Dark mode default, Inter/Geist fonts, purple accent, subtle animations
3. → Apply to project:
   - Use dark mode as base (with client's brand color instead of purple)
   - Keep Inter/Geist font pairing
   - Add subtle hover transitions
   - Maintain generous whitespace
4. → Result: Linear's sophistication + client's brand identity
```

### Scenario 2: "We're a Fintech Startup"

```
1. → Browse Fintech brands: Stripe, PayPal, Square
2. → Extract common patterns:
   - Blue/blurple accents (trust-building)
   - Clean, minimal layouts (professionalism)
   - Inter-based typography (readability)
   - Generous whitespace (clarity)
3. → Apply with startup's brand colors
4. → Result: Familiar fintech patterns + unique brand identity
```

### Scenario 3: "Need a Dashboard Design"

```
1. → Reference dashboard-heavy brands: Linear, GitHub, Vercel
2. → Extract dashboard patterns:
   - Dense data grids (GitHub)
   - Sidebar navigation (Linear)
   - Card-based metrics (Vercel)
3. → Combine based on project needs
4. → Result: Optimized dashboard for specific use case
```

## Important Notes

### Brand System Evolution
Brands evolve. The DESIGN.md file captures a **snapshot in time**:
- Always verify current brand website for changes
- Use principles, not exact implementation
- Brands test and iterate — learn from their evolution

### Accessibility Considerations
Popular brands sometimes sacrifice accessibility for aesthetics:
- Always verify WCAG compliance (contrast 4.5:1)
- Test with screen readers
- Keyboard navigation must work
- Don't copy inaccessible patterns

### Performance vs Aesthetics
Some brand choices prioritize aesthetics over performance:
- Heavy custom fonts → Consider `next/font` optimization
- Large hero images → Use `next/image` optimization
- Complex animations → Respect `prefers-reduced-motion`
- Custom icons → Consider tree-shakeable icon libraries

## Integration with Design Thinking

Brand systems are **concrete examples** of aesthetic directions:

| Aesthetic Direction | Brand Examples |
|---------------------|---------------|
| Brutally minimal | Vercel, Apple |
| Luxury/refined | Apple, Notion |
| Playful/toy-like | Discord, Figma |
| Industrial/utilitarian | GitHub, Linear |
| Editorial/magazine | Notion, Medium |
| Retro-futuristic | (None mainstream — create original) |

When you choose an aesthetic direction from `design-thinking.md`, reference 2-3 brands here to see how they implement it.

## Common Pitfalls

### Don't: Clone Entirely
- ❌ "Let's make it exactly like Stripe"
- ✅ "Stripe's gradient usage is inspiring — let's adapt that approach"

### Don't: Ignore Context
- ❌ "Linear uses dark mode, so we must too"
- ✅ "Linear's dark mode works for their developer audience — does ours prefer light?"

### Don't: Copy Anti-Patterns
- ❌ "Stripe does it, so it's correct"
- ✅ "Stripe's pattern works for their context — will it work for ours?"

### Don't: Over-Reference
- ❌ Constantly checking 5+ brands (paralysis by analysis)
- ✅ Pick 2-3 relevant brands, extract principles, move forward

## When NOT to Use Brand Systems

Skip brand reference when:

- **Highly original concept**: Breaking conventions intentionally
- **Niche industry**: No relevant brand examples exist
- **Strong existing brand**: Client has established design system
- **Simple utility tools**: Over-engineering for basic functionality

In these cases, use `design-thinking.md` framework directly without brand crutches.
