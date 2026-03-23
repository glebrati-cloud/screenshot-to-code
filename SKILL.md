---
name: screenshot-to-code
description: >
  Convert any screenshot, mockup, or design image into clean, production-ready code.
  Use this skill whenever the user shares a screenshot, UI mockup, Figma export, wireframe,
  or any visual reference and wants it turned into working code. Also trigger when the user says
  things like "build this", "code this design", "replicate this UI", "make this page",
  "convert this to HTML/React/Vue/Tailwind", or pastes an image with an implied request to
  recreate it. Works for full pages, individual components, dashboards, landing pages, forms,
  navbars, cards -- anything visual.
---

# Screenshot to Code

Transform any visual reference into clean, responsive, production-ready code.

## Core Philosophy

Users don't want "roughly similar" -- they want code that looks **identical** to their reference.
Every border-radius, shadow, and spacing decision matters. But the code must also be clean enough
for a developer to actually maintain it.

## Workflow

### Step 1: Deep Visual Analysis

Before writing any code, study the image systematically:

**Layout**
- Identify the grid system (12-col grid? Custom?)
- Map the component hierarchy: page > sections > components > elements
- Note flexbox vs grid patterns
- Identify fixed/sticky elements

**Design Tokens**
Extract a complete design system from the image:

```
Colors:
  --primary:     #___  (brand color, CTAs, links)
  --secondary:   #___  (accents, secondary actions)
  --bg:          #___  (page background)
  --surface:     #___  (card/panel backgrounds)
  --text:        #___  (primary text)
  --text-muted:  #___  (secondary text, placeholders)
  --border:      #___  (dividers, outlines)
  --accent:      #___  (highlights, badges, notifications)

Spacing: Identify the base unit (4px? 8px?) and the scale
Typography: Font family hints, size scale, weight usage
Radius: Identify border-radius patterns (sharp? rounded? pill?)
Shadows: Note shadow depth levels used
```

**Components**
List every distinct UI component visible:
- Navigation (type, items, active states)
- Content areas (cards, lists, tables, text blocks)
- Interactive elements (buttons, inputs, dropdowns, toggles)
- Feedback elements (alerts, badges, tooltips, progress bars)
- Media (images, icons, avatars, charts)

**Responsive Clues**
- Is this clearly a desktop view? Mobile? Tablet?
- What would logically stack, collapse, or hide at smaller sizes?
- Are there sidebar patterns that should become drawers on mobile?

### Step 2: Choose Output Format

Default based on context, or ask the user:

| What you see | Default output |
|---|---|
| Simple static page / landing page | Single HTML file (HTML + CSS + JS) |
| UI component / widget | React + Tailwind CSS (.jsx) |
| Full app with multiple components | React + Tailwind with component files |
| Email template | HTML with inline CSS |
| Dashboard with data | React + Tailwind + Recharts placeholders |

Supported stacks:
- **HTML/CSS/JS** -- Zero dependencies, works everywhere
- **React + Tailwind** -- Industry standard, prefer for components
- **Vue 3** -- Composition API with script setup
- **Svelte** -- Minimal boilerplate
- **React + CSS Modules** -- When Tailwind is not wanted

### Step 3: Generate Production Code

**Pixel Accuracy Rules**
- Colors must be exact hex values extracted from the image -- never approximate
- Spacing must be precise -- if it looks like 12px gap, use 12px not 0.75rem
- Border-radius, shadows, opacity: match exactly what you see
- Typography: match size, weight, line-height, and letter-spacing
- Icons: identify the icon library (Lucide, Heroicons, etc.) and use the correct icons

**Code Quality Rules**
- Semantic HTML: nav, main, section, article, aside, footer
- Meaningful names: .pricing-card__price, not .div-3
- CSS custom properties for design tokens (colors, spacing, radius)
- Component decomposition: if a UI element appears 2+ times, it is a component
- Comments only for non-obvious layout decisions

**Responsive Rules**
- Mobile-first: base styles for mobile, @media queries for larger screens
- At minimum: mobile (< 768px) and desktop (>= 768px)
- Navigation: hamburger menu with toggle on mobile
- Grid layouts: reduce columns on smaller screens
- Touch targets: minimum 44px on mobile

**Accessibility Rules**
- All images need descriptive alt text
- Form inputs must have associated label elements
- Interactive elements must be keyboard-navigable
- Color contrast must meet WCAG AA (4.5:1 for text)
- ARIA labels for icon-only buttons
- Focus visible styles must not be removed

### Step 4: Deliver

- Save as a working file the user can open/import immediately
- HTML files should render correctly when opened in a browser
- React components should be valid, importable, and self-contained
- Note any assumptions made about ambiguous parts of the design
- Proactively offer to refine specific areas

## Pattern Library

### Navigation
```
Desktop: Logo | Nav Links (with active indicator) | CTA Button
Mobile: Logo | Hamburger > Slide-out or dropdown menu
Sticky: position: sticky; top: 0; z-index: 50; backdrop-filter: blur;
```

### Hero Sections
```
Full-width container with constrained content (max-width: 1200px)
Headline (text-5xl/6xl) > Subtitle (text-xl, muted) > CTA group
Background: gradient | image with overlay | solid with pattern
```

### Card Grids
```
CSS Grid: grid-template-columns: repeat(auto-fill, minmax(300px, 1fr))
Card: surface bg + subtle shadow + radius + padding + hover:shadow-lg transition
Responsive: 3 cols > 2 cols > 1 col
```

### Dashboards
```
Layout: Fixed sidebar (240px) + scrollable main content
Sidebar: Logo + nav groups with icons + user profile at bottom
Main: Header bar + stat cards row + chart area + table
Mobile: Sidebar becomes a drawer with overlay
```

### Forms
```
Stack labels above inputs (not inline)
Input states: default > focus (ring) > error (red border + message) > disabled (opacity)
Submit button: full-width on mobile, auto-width on desktop
```

## Edge Cases

- **Blurry screenshots**: Do your best, note areas where you had to guess
- **Partial screenshots**: Build what is visible, use sensible defaults for cut-off areas
- **Dark mode only**: Build dark mode, add a note about light mode adaptation
- **Non-English text**: Preserve the original text in the code, use UTF-8
- **Complex animations**: Implement static version first, add CSS transitions for hover/focus
- **Charts/graphs**: Use placeholder data with the correct visual structure
