<script>
  let wiping = $state(false);
  let wipeTimer = $state(null);

  const triggerWipe = () => {
    wiping = true;
    clearTimeout(wipeTimer);
    wipeTimer = setTimeout(() => { wiping = false; }, 600);
  };
</script>

<div class="demo-area">
  <!-- Outer wrapper absorbs the overflow so border layers can expand freely -->
  <div class="btn-wrap">
    <button class="animated-btn" class:wipe={wiping} onclick={triggerWipe}>
      <!-- Multi-layer animated borders -->
      <span class="border-layer b1" aria-hidden="true"></span>
      <span class="border-layer b2" aria-hidden="true"></span>
      <span class="border-layer b3" aria-hidden="true"></span>

      <!-- Wipe overlay -->
      <span class="wipe-overlay" aria-hidden="true"></span>

      <span class="btn-text">Animated Button</span>
    </button>
  </div>
  <p class="hint">Hover for border rings · Click for color wipe</p>
</div>

<style>
  .demo-area {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1.5vw;
  }

  /* Wrapper gives room for the scaling border layers */
  .btn-wrap {
    padding: 1.5vw;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .animated-btn {
    position: relative;
    padding: 1.1vw 3vw;
    background: var(--bg-raised);
    border-radius: var(--radius-pill);
    /* Resting border so button is always visible */
    border: 1px solid rgba(212,167,44,0.35);
    cursor: pointer;
    /* NO overflow:hidden — that was killing the border layers */
    isolation: isolate;
    transition: background 0.3s ease, border-color 0.3s ease;
  }

  .animated-btn:hover {
    border-color: transparent; /* borders taken over by layers on hover */
  }

  /* ── Border layers ── */
  .border-layer {
    position: absolute;
    inset: 0;
    border-radius: var(--radius-pill);
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.3s ease, transform 0.45s var(--ease-expo);
  }

  .b1 {
    border: 1px solid var(--gold);
    transform: scale(0.92);
  }
  .b2 {
    border: 1px solid rgba(212,167,44,0.55);
    transform: scale(0.96);
    transition-delay: 0.05s;
  }
  .b3 {
    border: 1px solid rgba(212,167,44,0.28);
    transform: scale(1);
    transition-delay: 0.1s;
  }

  .animated-btn:hover .b1 { opacity: 1; transform: scale(1.08); }
  .animated-btn:hover .b2 { opacity: 1; transform: scale(1.16); }
  .animated-btn:hover .b3 { opacity: 1; transform: scale(1.26); }

  /* ── Wipe overlay ── */
  .wipe-overlay {
    position: absolute;
    inset: 0;
    background: var(--gold);
    clip-path: inset(0 100% 0 0 round 99vw);
    pointer-events: none;
    border-radius: var(--radius-pill);
  }

  .animated-btn.wipe .wipe-overlay {
    animation: wipeRight 0.5s var(--ease-expo) forwards;
  }

  /* ── Text ── */
  .btn-text {
    position: relative;
    z-index: 1;
    font-family: var(--font-body);
    font-size: clamp(0.75rem, 0.95vw, 1rem);
    font-weight: 500;
    color: var(--gold-bright);
    transition: color 0.25s ease 0.08s;
    pointer-events: none;
    white-space: nowrap;
  }

  .animated-btn.wipe .btn-text { color: #08090c; }

  .hint {
    font-family: var(--font-mono);
    font-size: clamp(0.52rem, 0.65vw, 0.7rem);
    color: var(--text-dim);
    letter-spacing: 0.06em;
  }
</style>
