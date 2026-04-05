# VelvetUI Component Showcase

Live interactive demo of the VelvetUI Svelte 5 component library.

## Stack

- **Svelte 5** — `$state`, `$effect`, `$props`, `$bindable`, `{@render}`
- **SvelteKit 2** — Vercel adapter
- **Pure CSS** — zero utility frameworks
- **`vw` / `vh` / `clamp()`** — no `px` units
- **ES2025+** throughout

## Components demoed

| Component      | Technique                                              |
| -------------- | ------------------------------------------------------ |
| RippleButton   | `$state` array, pointer position math                  |
| MorphText      | `requestAnimationFrame` scramble, char-by-char reveal  |
| MagneticButton | `$effect` event wiring, mousemove tracking             |
| FlipCard       | CSS 3D, `backface-visibility`, single boolean `$state` |
| AnimatedButton | CSS clip-path wipe, multi-border expand                |
| LiquidCard     | `$effect` + CSS custom props for 3D tilt + glow        |

## Deploy

```bash
npm install
npm run dev    # localhost:5173
```

Push to GitHub → import at vercel.com (auto-detects SvelteKit).

## Project structure

```
src/
  routes/
    +layout.svelte       — root layout, fonts
    +page.svelte         — page shell, active section tracking
  lib/
    components/
      Sidebar.svelte     — fixed sidebar, bind:activeSection
      DemoShell.svelte   — preview/code tab switcher
    demos/
      RippleDemo.svelte
      MorphTextDemo.svelte
      MagneticDemo.svelte
      FlipCardDemo.svelte
      AnimatedButtonDemo.svelte
      LiquidCardDemo.svelte
  app.css                — tokens, reset, keyframes
```
