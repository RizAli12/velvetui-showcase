<script>
  const { activeSection = $bindable("") } = $props();

  const sections = [
    { id: "ripple", label: "RippleButton", tag: "interaction" },
    { id: "morph", label: "MorphText", tag: "text" },
    { id: "magnetic", label: "MagneticButton", tag: "interaction" },
    { id: "flipcard", label: "FlipCard", tag: "layout" },
    { id: "animated", label: "AnimatedButton", tag: "interaction" },
    { id: "liquid", label: "LiquidCard", tag: "layout" },
  ];

  const tagColor = (tag) =>
    ({
      interaction: "var(--cyan)",
      layout: "var(--purple)",
      text: "var(--gold)",
    })[tag] ?? "var(--ice-muted)";
</script>

<aside class="sidebar">
  <div class="sidebar-top">
    <a href="/" class="brand">
      <span class="brand-v">V</span>
      <span class="brand-name">elvetUI</span>
    </a>
    <span class="brand-sub">Svelte 5 · Pure CSS</span>
  </div>

  <nav class="sidebar-nav">
    <span class="nav-group-label">Components</span>
    <ul>
      {#each sections as s}
        <li>
          <a
            href="#{s.id}"
            class="nav-item"
            class:active={activeSection === s.id}
          >
            <span class="nav-label">{s.label}</span>
            <span class="nav-tag" style="color: {tagColor(s.tag)}">{s.tag}</span
            >
          </a>
        </li>
      {/each}
    </ul>
  </nav>

  <div class="sidebar-footer">
    <a
      href="https://velvet-ui.vercel.app"
      target="_blank"
      rel="noopener noreferrer"
      class="buy-btn"
    >
      Get VelvetUI — $29 ↗
    </a>
    <a
      href="https://github.com/RizAli12"
      target="_blank"
      rel="noopener noreferrer"
      class="github-link"
    >
      GitHub
    </a>
  </div>
</aside>

<style>
  .sidebar {
    position: fixed;
    top: 0;
    left: 0;
    width: var(--sidebar-w);
    height: 100vh;
    background: var(--bg-surface);
    border-right: 1px solid var(--ice-dim);
    display: flex;
    flex-direction: column;
    padding: 2.5vw 1.5vw;
    z-index: 40;
    overflow-y: auto;
  }

  /* Brand */
  .sidebar-top {
    margin-bottom: 3vw;
    display: flex;
    flex-direction: column;
    gap: 0.3vw;
  }

  .brand {
    display: inline-flex;
    align-items: baseline;
    font-family: var(--font-display);
    font-size: clamp(1rem, 1.4vw, 1.5rem);
    font-weight: 700;
    line-height: 1;
  }

  .brand-v {
    background: linear-gradient(135deg, var(--gold), var(--gold-bright));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .brand-name {
    color: var(--text);
  }

  .brand-sub {
    font-family: var(--font-mono);
    font-size: clamp(0.5rem, 0.6vw, 0.65rem);
    color: var(--text-dim);
    letter-spacing: 0.08em;
  }

  /* Nav */
  .sidebar-nav {
    flex: 1;
  }

  .nav-group-label {
    font-family: var(--font-mono);
    font-size: clamp(0.5rem, 0.6vw, 0.65rem);
    text-transform: uppercase;
    letter-spacing: 0.18em;
    color: var(--text-dim);
    display: block;
    margin-bottom: 0.8vw;
  }

  ul {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 0.2vw;
  }

  .nav-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.6vw 0.8vw;
    border-radius: var(--radius-sm);
    transition:
      background 0.2s ease,
      color 0.2s ease;
    color: var(--text-muted);
  }

  .nav-item:hover {
    background: var(--bg-raised);
    color: var(--text);
  }

  .nav-item.active {
    background: rgba(212, 167, 44, 0.1);
    color: var(--gold-bright);
    border-left: 2px solid var(--gold);
    padding-left: calc(0.8vw - 2px);
  }

  .nav-label {
    font-family: var(--font-body);
    font-size: clamp(0.65rem, 0.8vw, 0.85rem);
    font-weight: 400;
  }

  .nav-tag {
    font-family: var(--font-mono);
    font-size: clamp(0.45rem, 0.55vw, 0.6rem);
    opacity: 0.7;
  }

  /* Footer */
  .sidebar-footer {
    display: flex;
    flex-direction: column;
    gap: 0.8vw;
    padding-top: 2vw;
    border-top: 1px solid var(--ice-dim);
  }

  .buy-btn {
    display: block;
    padding: 0.7vw 1vw;
    background: var(--gold);
    color: #08090c;
    font-family: var(--font-body);
    font-size: clamp(0.6rem, 0.75vw, 0.8rem);
    font-weight: 500;
    border-radius: var(--radius-sm);
    text-align: center;
    transition:
      background 0.25s ease,
      box-shadow 0.25s ease;
    box-shadow: 0 0 2vw rgba(212, 167, 44, 0.2);
  }

  .buy-btn:hover {
    background: var(--gold-pale);
    box-shadow: 0 0 3vw rgba(212, 167, 44, 0.4);
  }

  .github-link {
    font-family: var(--font-mono);
    font-size: clamp(0.55rem, 0.65vw, 0.7rem);
    color: var(--text-dim);
    text-align: center;
    transition: color 0.2s ease;
  }

  .github-link:hover {
    color: var(--cyan);
  }
</style>
