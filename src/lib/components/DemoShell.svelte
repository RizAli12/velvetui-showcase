<script>
  let { id, title, description, code, children } = $props();
  let tab = $state('preview');
  let copied = $state(false);
  let copyTimer = $state(null);

  const copyCode = async () => {
    await navigator.clipboard.writeText(code);
    copied = true;
    clearTimeout(copyTimer);
    copyTimer = setTimeout(() => { copied = false; }, 2000);
  };
</script>

<section {id} class="demo-shell">

  <!-- Header -->
  <div class="demo-header">
    <div class="demo-meta">
      <h2 class="demo-title">{title}</h2>
      <p class="demo-desc">{description}</p>
    </div>
    <div class="demo-tabs">
      <button
        class="tab-btn"
        class:active={tab === 'preview'}
        onclick={() => { tab = 'preview'; }}
      >
        Preview
      </button>
      <button
        class="tab-btn"
        class:active={tab === 'code'}
        onclick={() => { tab = 'code'; }}
      >
        Code
      </button>
    </div>
  </div>

  <!-- Preview panel -->
  {#if tab === 'preview'}
    <div class="preview-panel">
      {@render children()}
    </div>
  {/if}

  <!-- Code panel -->
  {#if tab === 'code'}
    <div class="code-panel">
      <button class="copy-btn" onclick={copyCode} type="button">
        {copied ? '✓ Copied' : 'Copy'}
      </button>
      <pre class="code-block"><code>{code}</code></pre>
    </div>
  {/if}

</section>

<style>
  .demo-shell {
    margin-bottom: 6vw;
    animation: fadeUp 0.5s var(--ease-expo) both;
  }

  /* Header */
  .demo-header {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    gap: 2vw;
    margin-bottom: 1.5vw;
  }

  .demo-meta { flex: 1; }

  .demo-title {
    font-family: var(--font-display);
    font-size: clamp(1.1rem, 1.6vw, 1.8rem);
    font-weight: 700;
    color: var(--text);
    margin-bottom: 0.4vw;
  }

  .demo-desc {
    font-family: var(--font-body);
    font-size: clamp(0.65rem, 0.82vw, 0.88rem);
    font-weight: 300;
    color: var(--text-muted);
    line-height: 1.6;
  }

  /* Tabs */
  .demo-tabs {
    display: flex;
    gap: 0.2vw;
    background: var(--bg-raised);
    border-radius: var(--radius-sm);
    padding: 0.25vw;
    flex-shrink: 0;
  }

  .tab-btn {
    padding: 0.4vw 1vw;
    font-family: var(--font-mono);
    font-size: clamp(0.55rem, 0.68vw, 0.72rem);
    color: var(--text-muted);
    border-radius: calc(var(--radius-sm) - 0.15vw);
    transition: background 0.2s ease, color 0.2s ease;
  }

  .tab-btn.active {
    background: var(--bg-overlay);
    color: var(--gold-bright);
  }

  /* Preview */
  .preview-panel {
    min-height: 18vw;
    background: var(--bg-raised);
    border: 1px solid var(--ice-dim);
    border-radius: var(--radius-md);
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 4vw;
    position: relative;
    overflow: hidden;
  }

  .preview-panel::before {
    content: '';
    position: absolute;
    inset: 0;
    background-image:
      linear-gradient(var(--ice-dim) 1px, transparent 1px),
      linear-gradient(90deg, var(--ice-dim) 1px, transparent 1px);
    background-size: 3vw 3vw;
    opacity: 0.35;
    pointer-events: none;
  }

  /* Code */
  .code-panel {
    position: relative;
    border-radius: var(--radius-md);
    border: 1px solid var(--ice-dim);
    background: var(--bg-code);
    overflow: hidden;
  }

  .copy-btn {
    position: absolute;
    top: 0.8vw;
    right: 0.8vw;
    padding: 0.3vw 0.8vw;
    font-family: var(--font-mono);
    font-size: clamp(0.5rem, 0.62vw, 0.66rem);
    color: var(--text-muted);
    background: var(--bg-raised);
    border: 1px solid var(--ice-dim);
    border-radius: var(--radius-sm);
    transition: color 0.2s ease, border-color 0.2s ease;
    z-index: 2;
  }

  .copy-btn:hover { color: var(--gold-bright); border-color: var(--gold-dim); }

  .code-block {
    padding: 2vw 2vw 2vw 1.5vw;
    overflow-x: auto;
    font-family: var(--font-mono);
    font-size: clamp(0.6rem, 0.75vw, 0.8rem);
    line-height: 1.7;
    color: var(--ice);
    white-space: pre;
  }

  code {
    font-family: var(--font-mono);
  }
</style>
