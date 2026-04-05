<script>
  let btnEl = $state(null);
  let tx = $state(0);
  let ty = $state(0);
  let isHovered = $state(false);

  const STRENGTH = 0.35;

  $effect(() => {
    if (!btnEl) return;

    const onMove = (/** @type {MouseEvent} */ e) => {
      if (!isHovered) return;
      const rect = btnEl.getBoundingClientRect();
      const cx = rect.left + rect.width  / 2;
      const cy = rect.top  + rect.height / 2;
      tx = (e.clientX - cx) * STRENGTH;
      ty = (e.clientY - cy) * STRENGTH;
    };

    const onEnter = () => { isHovered = true; };
    const onLeave = () => { isHovered = false; tx = 0; ty = 0; };

    btnEl.addEventListener('mouseenter', onEnter);
    btnEl.addEventListener('mouseleave', onLeave);
    window.addEventListener('mousemove', onMove);

    return () => {
      btnEl?.removeEventListener('mouseenter', onEnter);
      btnEl?.removeEventListener('mouseleave', onLeave);
      window.removeEventListener('mousemove', onMove);
    };
  });
</script>

<div class="demo-area">
  <button
    bind:this={btnEl}
    class="magnetic-btn"
    class:hovered={isHovered}
    style="transform: translate({tx}px, {ty}px)"
  >
    <span
      class="magnetic-label"
      style="transform: translate({tx * 0.5}px, {ty * 0.5}px)"
    >
      Hover me
    </span>
  </button>

  <p class="hint">Move your cursor over the button</p>
</div>

<style>
  .demo-area {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2vw;
  }

  .magnetic-btn {
    position: relative;
    padding: 1.2vw 3vw;
    background: var(--bg-overlay);
    border: 1px solid rgba(79,196,207,0.3);
    border-radius: var(--radius-pill);
    transition:
      transform 0.12s var(--ease-expo),
      border-color 0.25s ease,
      box-shadow 0.25s ease,
      background 0.25s ease;
    will-change: transform;
    overflow: hidden;
  }

  .magnetic-btn.hovered {
    border-color: var(--cyan);
    box-shadow: 0 0 3vw var(--cyan-glow);
    background: rgba(79,196,207,0.05);
  }

  .magnetic-btn::before {
    content: '';
    position: absolute;
    inset: -50%;
    background: radial-gradient(circle, rgba(79,196,207,0.08) 0%, transparent 60%);
    opacity: 0;
    transition: opacity 0.3s ease;
    pointer-events: none;
  }

  .magnetic-btn.hovered::before { opacity: 1; }

  .magnetic-label {
    font-family: var(--font-body);
    font-size: clamp(0.8rem, 1vw, 1.05rem);
    font-weight: 500;
    color: var(--cyan);
    display: block;
    transition: transform 0.08s var(--ease-expo);
    white-space: nowrap;
  }

  .hint {
    font-family: var(--font-mono);
    font-size: clamp(0.55rem, 0.68vw, 0.72rem);
    color: var(--text-dim);
    letter-spacing: 0.06em;
  }
</style>
