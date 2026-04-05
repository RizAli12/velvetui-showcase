<script>
  import Sidebar from "$lib/components/Sidebar.svelte";
  import DemoShell from "$lib/components/DemoShell.svelte";
  import RippleDemo from "$lib/demos/RippleDemo.svelte";
  import MagneticDemo from "$lib/demos/MagneticDemo.svelte";
  import FlipCardDemo from "$lib/demos/FlipCardDemo.svelte";
  import AnimatedButtonDemo from "$lib/demos/AnimatedButtonDemo.svelte";
  import LiquidCardDemo from "$lib/demos/LiquidCardDemo.svelte";
  import MorphTextDemo from '$lib/demos/MorphTextDemo.svelte';

  let activeSection = $state("ripple");

  // Track active section via IntersectionObserver
  $effect(() => {
    const targets = document.querySelectorAll("section[id]");
    const observer = new IntersectionObserver(
      (entries) => {
        for (const entry of entries) {
          if (entry.isIntersecting) {
            activeSection = entry.target.id;
            break;
          }
        }
      },
      { rootMargin: "-35% 0px -55% 0px", threshold: 0 },
    );
    targets.forEach((t) => observer.observe(t));
    return () => observer.disconnect();
  });

  // ── Code snippets ──
  const codes = {
    ripple: `<script>
  let ripples = $state([]);
  let nextId = $state(0);

  const addRipple = (e) => {
    const rect = e.currentTarget.getBoundingClientRect();
    const x = ((e.clientX - rect.left) / rect.width)  * 100;
    const y = ((e.clientY - rect.top)  / rect.height) * 100;
    const id = nextId++;
    ripples = [...ripples, { id, x, y }];
    setTimeout(() => {
      ripples = ripples.filter(r => r.id !== id);
    }, 700);
  };
<\/script>

<button class="ripple-btn" onpointerdown={addRipple}>
  {#each ripples as r (r.id)}
    <span class="ripple" style="left:{r.x}%;top:{r.y}%"/>
  {/each}
  Click me
</button>`,

morph: `<script>
const GLYPHS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789@#$%&*';
const scramble = (target, onUpdate, onDone) => {
const len = target.length;
const frames = Math.round(len * 6);
let frame = 0, raf = null;
const tick = () => {
  const revealed = Math.floor((frame / frames) * len);
  const chars = target.split('').map((ch, i) =>
    i < revealed || ch === ' '
      ? ch
      : GLYPHS[Math.floor(Math.random() * GLYPHS.length)]
  );
  onUpdate(chars);
  frame++;
  if (frame <= frames) raf = requestAnimationFrame(tick);
  else { onUpdate(target.split('')); onDone?.(); }
};

raf = requestAnimationFrame(tick);
return () => cancelAnimationFrame(raf);
};
const WORDS = ['DESIGN', 'ANIMATE', 'DEPLOY', 'DELIGHT'];
let wordIndex = $state(0);
let chars     = $state(WORDS[0].split(''));
let active    = $state(false);
let cancel    = null;
const next = () => {
if (active) return;
active = true;
wordIndex = (wordIndex + 1) % WORDS.length;
cancel?.();
cancel = scramble(
WORDS[wordIndex],
(c) => { chars = c; },
()  => { active = false; }
);
};
$effect(() => {
const id = setInterval(next, 2400);
return () => { clearInterval(id); cancel?.(); };
});
<\/script>
<button class="morph-head" onclick={next}>
  {#each chars as ch, i (i)}
    <span class="glyph" class:scrambling={active}>{ch}</span>
  {/each}
  <span class="cursor">_</span>
</button>`,

    magnetic: `<script>
  let btnEl   = $state(null);
  let tx      = $state(0);
  let ty      = $state(0);
  let hovered = $state(false);
  const STRENGTH = 0.35;

  $effect(() => {
    if (!btnEl) return;
    const onMove = (e) => {
      if (!hovered) return;
      const { left, top, width, height } =
        btnEl.getBoundingClientRect();
      tx = (e.clientX - left - width  / 2) * STRENGTH;
      ty = (e.clientY - top  - height / 2) * STRENGTH;
    };
    const onEnter = () => { hovered = true; };
    const onLeave = () => { hovered = false; tx = 0; ty = 0; };
    btnEl.addEventListener('mouseenter', onEnter);
    btnEl.addEventListener('mouseleave', onLeave);
    window.addEventListener('mousemove', onMove);
    return () => {
      btnEl?.removeEventListener('mouseenter', onEnter);
      btnEl?.removeEventListener('mouseleave', onLeave);
      window.removeEventListener('mousemove', onMove);
    };
  });
<\/script>

<button
  bind:this={btnEl}
  style="transform: translate({tx}px, {ty}px)"
>
  Hover me
</button>`,

    flipcard: `<script>
  let flipped = $state(false);
<\/script>

<button
  class="flip-card"
  class:flipped
  onclick={() => { flipped = !flipped; }}
>
  <div class="card-face card-front">Front</div>
  <div class="card-face card-back">Back</div>
</button>

<style>
  .flip-card { perspective: 80vw; }

  .card-face {
    backface-visibility: hidden;
    transition: transform 0.7s cubic-bezier(0.16,1,0.3,1);
  }
  .card-front { transform: rotateY(0deg); }
  .card-back  { transform: rotateY(-180deg); }

  .flipped .card-front { transform: rotateY(180deg); }
  .flipped .card-back  { transform: rotateY(0deg); }
</style>`,

    animated: `<script>
  let wiping = $state(false);
  let timer  = $state(null);

  const triggerWipe = () => {
    wiping = true;
    clearTimeout(timer);
    timer = setTimeout(() => { wiping = false; }, 600);
  };
<\/script>

<button class="animated-btn" class:wipe={wiping} onclick={triggerWipe}>
  <span class="b1" /><span class="b2" /><span class="b3" />
  <span class="wipe-overlay" />
  <span class="btn-text">Animated Button</span>
</button>`,

    liquid: `<script>
  let cardEl = $state(null);
  let rx = $state(0);
  let ry = $state(0);

  $effect(() => {
    if (!cardEl) return;
    const onMove = (e) => {
      const { left, top, width, height } =
        cardEl.getBoundingClientRect();
      ry =  ((e.clientX - left) / width  - 0.5) * 28;
      rx = -((e.clientY - top)  / height - 0.5) * 28;
    };
    const reset = () => { rx = 0; ry = 0; };
    cardEl.addEventListener('mousemove', onMove);
    cardEl.addEventListener('mouseleave', reset);
    return () => {
      cardEl?.removeEventListener('mousemove', onMove);
      cardEl?.removeEventListener('mouseleave', reset);
    };
  });
<\/script>

<div
  bind:this={cardEl}
  style="transform: rotateX({rx}deg) rotateY({ry}deg)"
>
  LiquidCard
</div>`,
  };

  const demos = [
    {
      id: "ripple",
      title: "RippleButton",
      description:
        "Pointer-position ripple effect driven by $state. Supports multiple concurrent ripples — each gets a unique id and auto-cleans after animation ends.",
      code: codes.ripple,
      component: RippleDemo,
    },
    {
  id: 'morph',
  title: 'MorphText',
  description:
    'Character-scramble morpher that reveals text left-to-right using requestAnimationFrame. Gold → cyan color shift during scramble, two variants: headline and inline tag.',
  code: codes.morph,
  component: MorphTextDemo,
},
    {
      id: "magnetic",
      title: "MagneticButton",
      description:
        "Cursor-tracking magnetic attraction using $effect for event wiring. The label moves at 50% of the button's offset for a layered parallax feel.",
      code: codes.magnetic,
      component: MagneticDemo,
    },
    {
      id: "flipcard",
      title: "FlipCard",
      description:
        "3D CSS flip card with glassmorphism surfaces. Pure CSS perspective + backface-visibility — a single boolean $state drives both faces.",
      code: codes.flipcard,
      component: FlipCardDemo,
    },
    {
      id: "animated",
      title: "AnimatedButton",
      description:
        "Multi-ring expanding border on hover, plus a clip-path color-wipe on click. All driven by CSS transitions; $state just toggles the class.",
      code: codes.animated,
      component: AnimatedButtonDemo,
    },

    {
      id: "liquid",
      title: "LiquidCard",
      description:
        "Real-time 3D tilt driven by mousemove → $state → inline style binding. A radial gradient glow follows the cursor position via CSS custom properties.",
      code: codes.liquid,
      component: LiquidCardDemo,
    },
  ];
</script>

<div class="layout">
  <Sidebar bind:activeSection />

  <main class="content">
    <!-- Page header -->
    <header class="page-header">
      <div class="header-inner">
        <h1 class="page-title">
          <span class="title-velvet">VelvetUI</span>
          <span class="title-components"> Component Showcase</span>
        </h1>
        <p class="page-sub">
          6 production-ready Svelte 5 components. Rune-driven state, pure CSS
          animations, zero dependencies. Live demos + source code.
        </p>
        <div class="header-badges">
          {#each ["Svelte 5 runes", "Pure CSS", "vw / clamp()", "Zero deps", "ES2025"] as badge}
            <span class="badge">{badge}</span>
          {/each}
        </div>
      </div>
    </header>

    <!-- Divider -->
    <div class="divider" aria-hidden="true"></div>

    <!-- Demos -->
    <div class="demos-list">
      {#each demos as demo}
        <DemoShell
          id={demo.id}
          title={demo.title}
          description={demo.description}
          code={demo.code}
        >
          <demo.component />
        </DemoShell>
      {/each}
    </div>
  </main>
</div>

<style>
  .layout {
    display: flex;
    min-height: 100vh;
  }

  /* ── Main content ── */
  .content {
    margin-left: var(--sidebar-w);
    flex: 1;
    padding: 4vw 5vw 8vw;
    min-width: 0;
  }

  /* ── Page header ── */
  .page-header {
    padding-bottom: 3vw;
    animation: fadeUp 0.6s var(--ease-expo) both;
  }

  .header-inner {
    display: flex;
    flex-direction: column;
    gap: 1vw;
  }

  .page-title {
    font-family: var(--font-display);
    font-size: clamp(1.8rem, 3.5vw, 4rem);
    font-weight: 800;
    line-height: 1.1;
  }

  .title-velvet {
    background: linear-gradient(
      90deg,
      var(--gold-dim),
      var(--gold-pale),
      var(--gold-bright),
      var(--gold-pale),
      var(--gold-dim)
    );
    background-size: 200% auto;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    animation: shimmer 2.4s linear infinite;
  }

  .title-components {
    color: var(--text);
  }

  .page-sub {
    font-family: var(--font-body);
    font-size: clamp(0.75rem, 0.95vw, 1rem);
    font-weight: 300;
    line-height: 1.7;
    color: var(--text-muted);
    max-width: 52vw;
  }

  .header-badges {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5vw;
    margin-top: 0.5vw;
  }

  .badge {
    padding: 0.25vw 0.8vw;
    font-family: var(--font-mono);
    font-size: clamp(0.5rem, 0.62vw, 0.66rem);
    color: var(--cyan-dim);
    border: 1px solid rgba(79, 196, 207, 0.18);
    border-radius: var(--radius-pill);
    background: rgba(79, 196, 207, 0.05);
  }

  .divider {
    height: 1px;
    background: linear-gradient(90deg, var(--ice-dim), transparent);
    margin-bottom: 4vw;
  }

  .demos-list {
    display: flex;
    flex-direction: column;
  }
</style>
