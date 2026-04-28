---
name: aura-frontend-philosophy
description: Visual and UI philosophy (The 5 Pillars of Intentional UI). Agents must load this before writing or reviewing frontend code and styles.
---

# Aura Frontend Philosophy: The 5 Pillars of Intentional UI

**Applies to:** aura-implementer, aura-designer, aura-reviewer

**Scope:** Styling, layout, colors, typography, animations, component structure, and UI composition.

**Philosophy:** Distinctive, memorable, intentional design — avoiding generic AI-slop aesthetics through bold, characterful choices that create immediate emotional impact.

## The 5 Pillars

### 1. Typography with Character
- Fonts set the entire tone. Generic fonts create generic, forgettable interfaces.
- Avoid Inter, Roboto, Arial, and system-ui defaults. Choose distinctive, characterful typefaces.
- Pair dramatic display fonts with refined, readable body fonts.

### 2. Committed Color and Theme
- Timid palettes lack impact and feel algorithmically generated.
- Use bold, dominant colors with sharp accent contrasts. Avoid evenly-distributed rainbow gradients.
- Establish CSS variable systems early. Break away from the purple-gradient-on-white AI cliché.

### 3. Purposeful Motion
- Animation should delight, not distract. Scattered micro-interactions create noise.
- One well-orchestrated animation beats a dozen minor transitions. Focus on high-impact moments.
- Use CSS animations for HTML, a Motion library for React. Prioritize staggered reveals and surprising hover states.

### 4. Brave Spatial Composition
- Predictable layouts are forgettable. Safe spacing feels automated.
- Either generous negative space OR controlled density — not the middle ground.
- Embrace asymmetry, overlap, diagonal flow, and grid-breaking elements.

### 5. Atmosphere and Depth
- Flat solid backgrounds lack presence and feel unfinished.
- Layer visual richness through gradient meshes, noise textures, geometric patterns, and transparencies.
- Add dramatic shadows, decorative borders, grain overlays.

## Adherence Checklist

Before completing your task, verify:
- [ ] **Typography:** Did you avoid generic system fonts?
- [ ] **Color:** Are the color choices bold and intentional?
- [ ] **Motion:** Is there a primary, high-impact animation?
- [ ] **Space:** Does the layout feel designed rather than templated?
- [ ] **Depth:** Is there visual richness (textures, gradients, layering)?
