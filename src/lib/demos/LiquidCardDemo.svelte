<script>
  let cardEl = $state(null);
  let rx = $state(0);
  let ry = $state(0);
  let glowX = $state(50);
  let glowY = $state(50);
  let active = $state(false);

  const MAX_TILT = 14;

  $effect(() => {
    if (!cardEl) return;

    const onMove = (/** @type {MouseEvent} */ e) => {
      const rect = cardEl.getBoundingClientRect();
      const xPct = (e.clientX - rect.left) / rect.width;
      const yPct = (e.clientY - rect.top)  / rect.height;
      ry =  (xPct - 0.5) * MAX_TILT * 2;
      rx = -(yPct - 0.5) * MAX_TILT * 2;
      glowX = xPct * 100;
      glowY = yPct * 100;
    };

    const onEnter = () => { active = true; };
    const onLeave = () => { active = false; rx = 0; ry = 0; glowX = 50; glowY = 50; };

    cardEl.addEventListener('mouseenter', onEnter);
    cardEl.addEventListener('mouseleave', onLeave);
    cardEl.addEventListener('mousemove', onMove);

    return () => {
      cardEl?.removeEventListener('mouseenter', onEnter);
      cardEl?.removeEventListener('mouseleave', onLeave);
      cardEl?.removeEventListener('mousemove', onMove);
    };
  });
</script>

<div class="demo-area">
  <div
    bind:this={cardEl}
    class="liquid-card"
    class:active
    style="
      --rx: {rx}deg;
      --ry: {ry}deg;
      --gx: {glowX}%;
      --gy: {glowY}%;
    "
  >
    <!-- Glow highlight -->
    <div class="card-glow" aria-hidden="true"></div>

    <div class="card-content">
      <span class="card-eyebrow">Spring physics</span>
      <h3 class="card-heading">LiquidCard</h3>
      <p class="card-body">
        Pointer-reactive tilt with CSS custom property interpolation.
        Move your cursor over this card.
      </p>
      <div class="card-footer">
        <span class="tag">$effect</span>
        <span class="tag">CSS vars</span>
        <span class="tag">Pointer API</span>
      </div>
    </div>
  </div>
</div>

<style>
  .demo-area {
    display: flex;
    align-items: center;
    justify-content: center;
    perspective: 80vw;
    padding: 2vw;
  }

  .liquid-card {
    width: clamp(16rem, 26vw, 30rem);
    position: relative;
    border-radius: var(--radius-lg);
    border: 1px solid var(--ice-dim);
    background: linear-gradient(145deg, var(--bg-overlay), var(--bg-raised));
    transform: rotateX(0deg) rotateY(0deg);
    transition:
      transform 0.08s var(--ease-expo),
      box-shadow 0.3s ease,
      border-color 0.3s ease;
    will-change: transform;
    cursor: default;
    overflow: hidden;
  }

  .liquid-card.active {
    transform: rotateX(var(--rx)) rotateY(var(--ry));
    border-color: rgba(155,127,234,0.3);
    box-shadow:
      0 2vw 4vw rgba(0,0,0,0.5),
      0 0 3vw rgba(155,127,234,0.12);
  }

  /* Radial glow that follows cursor */
  .card-glow {
    position: absolute;
    inset: 0;
    background: radial-gradient(
      circle at var(--gx) var(--gy),
      rgba(155,127,234,0.12) 0%,
      transparent 55%
    );
    opacity: 0;
    transition: opacity 0.3s ease;
    pointer-events: none;
  }

  .liquid-card.active .card-glow { opacity: 1; }

  /* Content */
  .card-content {
    position: relative;
    padding: 2.5vw;
    display: flex;
    flex-direction: column;
    gap: 0.8vw;
    z-index: 1;
  }

  .card-eyebrow {
    font-family: var(--font-mono);
    font-size: clamp(0.5rem, 0.62vw, 0.66rem);
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: var(--purple-dim);
  }

  .card-heading {
    font-family: var(--font-display);
    font-size: clamp(1.1rem, 1.6vw, 1.8rem);
    font-weight: 700;
    color: var(--text);
  }

  .card-body {
    font-family: var(--font-body);
    font-size: clamp(0.65rem, 0.82vw, 0.88rem);
    font-weight: 300;
    line-height: 1.65;
    color: var(--text-muted);
  }

  .card-footer {
    display: flex;
    gap: 0.5vw;
    flex-wrap: wrap;
    margin-top: 0.5vw;
  }

  .tag {
    padding: 0.2vw 0.7vw;
    font-family: var(--font-mono);
    font-size: clamp(0.48rem, 0.6vw, 0.64rem);
    color: var(--purple-dim);
    border: 1px solid rgba(155,127,234,0.2);
    border-radius: var(--radius-pill);
    background: rgba(155,127,234,0.06);
  }
</style>
