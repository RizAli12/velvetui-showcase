<script>
  let ripples = $state([]);
  let nextId = $state(0);

  /** @type {(e: PointerEvent) => void} */
  const addRipple = (e) => {
    const btn = /** @type {HTMLElement} */ (e.currentTarget);
    const rect = btn.getBoundingClientRect();
    const x = ((e.clientX - rect.left) / rect.width) * 100;
    const y = ((e.clientY - rect.top)  / rect.height) * 100;
    const id = nextId++;
    ripples = [...ripples, { id, x, y }];
    setTimeout(() => { ripples = ripples.filter(r => r.id !== id); }, 700);
  };
</script>

<div class="demo-area">
  <button class="ripple-btn" onpointerdown={addRipple}>
    {#each ripples as r (r.id)}
      <span
        class="ripple"
        style="left: {r.x}%; top: {r.y}%"
        aria-hidden="true"
      ></span>
    {/each}
    Click me
  </button>

  <button class="ripple-btn variant-ghost" onpointerdown={addRipple}>
    {#each ripples as r (r.id)}
      <span class="ripple ripple-ghost" style="left: {r.x}%; top: {r.y}%" aria-hidden="true"></span>
    {/each}
    Ghost variant
  </button>
</div>

<style>
  .demo-area {
    display: flex;
    gap: 2vw;
    align-items: center;
    flex-wrap: wrap;
    justify-content: center;
  }

  .ripple-btn {
    position: relative;
    overflow: hidden;
    padding: 1vw 2.5vw;
    font-family: var(--font-body);
    font-size: clamp(0.75rem, 0.9vw, 0.95rem);
    font-weight: 500;
    background: var(--gold);
    color: var(--bg);
    border-radius: var(--radius-pill);
    border: none;
    cursor: pointer;
    transition: background 0.25s ease, box-shadow 0.25s ease;
    box-shadow: 0 0 2vw rgba(212,167,44,0.2);
    user-select: none;
  }

  .ripple-btn:hover {
    background: var(--gold-bright);
    box-shadow: 0 0 3.5vw rgba(212,167,44,0.4);
  }

  .ripple-btn.variant-ghost {
    background: transparent;
    color: var(--gold-bright);
    border: 1px solid rgba(212,167,44,0.4);
    box-shadow: none;
  }

  .ripple-btn.variant-ghost:hover {
    background: rgba(212,167,44,0.07);
    box-shadow: 0 0 2vw rgba(212,167,44,0.15);
  }

  .ripple {
    position: absolute;
    width: 1px;
    height: 1px;
    background: rgba(255,255,255,0.55);
    border-radius: 50%;
    transform: scale(0);
    animation: rippleOut 0.65s var(--ease-expo) forwards;
    pointer-events: none;
  }

  .ripple-ghost {
    background: rgba(212,167,44,0.5);
  }
</style>
