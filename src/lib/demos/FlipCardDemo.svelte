<script>
  let flipped = $state(false);
</script>

<div class="demo-area">
  <button
    class="flip-card"
    class:flipped
    onclick={() => { flipped = !flipped; }}
    aria-label="Flip card"
  >
    <!-- Front -->
    <div class="card-face card-front" aria-hidden={flipped}>
      <span class="face-icon">◈</span>
      <span class="face-title">VelvetUI</span>
      <span class="face-sub">Click to flip</span>
    </div>
    <!-- Back -->
    <div class="card-face card-back" aria-hidden={!flipped}>
      <span class="face-icon back-icon">✦</span>
      <span class="face-title">Svelte 5</span>
      <span class="face-sub">Pure CSS · Zero deps</span>
    </div>
  </button>

  <p class="hint">Click the card to flip</p>
</div>

<style>
  .demo-area {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2vw;
  }

  .flip-card {
    width: clamp(10rem, 16vw, 18rem);
    height: clamp(12rem, 20vw, 22rem);
    perspective: 80vw;
    cursor: pointer;
    background: none;
    border: none;
    position: relative;
  }

  .card-face {
    position: absolute;
    inset: 0;
    border-radius: var(--radius-lg);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 1vw;
    backface-visibility: hidden;
    -webkit-backface-visibility: hidden;
    transition: transform 0.7s var(--ease-expo);
    padding: 2vw;
  }

  /* Front */
  .card-front {
    background: linear-gradient(145deg, var(--bg-overlay), var(--bg-raised));
    border: 1px solid rgba(212,167,44,0.2);
    box-shadow: 0 0 4vw rgba(212,167,44,0.08), inset 0 1px 0 rgba(212,167,44,0.15);
    transform: rotateY(0deg);
  }

  .flipped .card-front { transform: rotateY(180deg); }

  /* Back */
  .card-back {
    background: linear-gradient(145deg, rgba(79,196,207,0.08), var(--bg-raised));
    border: 1px solid rgba(79,196,207,0.25);
    box-shadow: 0 0 4vw rgba(79,196,207,0.1), inset 0 1px 0 rgba(79,196,207,0.15);
    transform: rotateY(-180deg);
  }

  .flipped .card-back { transform: rotateY(0deg); }

  /* Face content */
  .face-icon {
    font-size: clamp(1.8rem, 3vw, 3.2rem);
    color: var(--gold);
    opacity: 0.8;
  }

  .back-icon { color: var(--cyan); }

  .face-title {
    font-family: var(--font-display);
    font-size: clamp(1rem, 1.5vw, 1.6rem);
    font-weight: 700;
    color: var(--text);
  }

  .face-sub {
    font-family: var(--font-mono);
    font-size: clamp(0.55rem, 0.7vw, 0.75rem);
    color: var(--text-muted);
    letter-spacing: 0.06em;
  }

  .hint {
    font-family: var(--font-mono);
    font-size: clamp(0.55rem, 0.68vw, 0.72rem);
    color: var(--text-dim);
    letter-spacing: 0.06em;
  }
</style>
