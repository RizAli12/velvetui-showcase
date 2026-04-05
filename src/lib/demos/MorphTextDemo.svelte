<script>
  const GLYPHS = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789@#$%&*";

  /**
   * Scramble `target` string, revealing characters left-to-right.
   * Calls `onUpdate(chars[])` on every frame, `onDone()` when settled.
   */
  const scramble = (target, onUpdate, onDone) => {
    const len = target.length;
    const frames = Math.round(len * 6); // total animation frames
    let frame = 0;
    let raf = null;

    const tick = () => {
      const progress = frame / frames; // 0 → 1
      const revealed = Math.floor(progress * len); // chars locked in

      const chars = target.split("").map((ch, i) => {
        if (i < revealed) return ch; // settled
        if (ch === " ") return " "; // preserve spaces
        return GLYPHS[Math.floor(Math.random() * GLYPHS.length)];
      });

      onUpdate(chars);
      frame++;

      if (frame <= frames) {
        raf = requestAnimationFrame(tick);
      } else {
        onUpdate(target.split(""));
        onDone?.();
      }
    };

    raf = requestAnimationFrame(tick);
    return () => cancelAnimationFrame(raf);
  };

  // ── Headline morpher ──────────────────────────────────────────────
  const WORDS = ["DESIGN", "ANIMATE", "DEPLOY", "DELIGHT"];

  let wordIndex = $state(0);
  let headChars = $state(WORDS[0].split(""));
  let headActive = $state(false);
  let cancelHead = null;

  const nextWord = () => {
    if (headActive) return;
    headActive = true;
    wordIndex = (wordIndex + 1) % WORDS.length;
    cancelHead?.();
    cancelHead = scramble(
      WORDS[wordIndex],
      (chars) => {
        headChars = chars;
      },
      () => {
        headActive = false;
      },
    );
  };

  // Auto-cycle every 2.4 s
  $effect(() => {
    const id = setInterval(nextWord, 2400);
    return () => {
      clearInterval(id);
      cancelHead?.();
    };
  });

  // ── Tag morpher ───────────────────────────────────────────────────
  const TAGS = [
    "frontend dev",
    "svelte 5",
    "ui motion",
    "pure CSS",
    "zero deps",
  ];

  let tagIndex = $state(0);
  let tagChars = $state(TAGS[0].split(""));
  let tagActive = $state(false);
  let cancelTag = null;

  const nextTag = () => {
    if (tagActive) return;
    tagActive = true;
    tagIndex = (tagIndex + 1) % TAGS.length;
    cancelTag?.();
    cancelTag = scramble(
      TAGS[tagIndex],
      (chars) => {
        tagChars = chars;
      },
      () => {
        tagActive = false;
      },
    );
  };

  $effect(() => {
    const id = setInterval(nextTag, 1800);
    return () => {
      clearInterval(id);
      cancelTag?.();
    };
  });
</script>

<div class="demo-area">
  <!-- Headline variant -->
  <div class="block">
    <span class="block-label">headline</span>
    <button class="morph-head" onclick={nextWord} title="Click to morph">
      {#each headChars as ch, i (i)}
        <span
          class="glyph"
          class:space={ch === " "}
          class:scrambling={headActive}>{ch}</span
        >
      {/each}
      <span class="cursor" aria-hidden="true">_</span>
    </button>
    <p class="hint">auto-cycles · click to skip</p>
  </div>

  <!-- Tag variant -->
  <div class="block">
    <span class="block-label">inline tag</span>
    <div class="tag-line">
      <span class="tag-prefix">i build</span>
      <button class="morph-tag" onclick={nextTag} title="Click to morph">
        {#each tagChars as ch, i (i)}
          <span
            class="glyph glyph-sm"
            class:space={ch === " "}
            class:scrambling={tagActive}>{ch}</span
          >
        {/each}
        <span class="cursor cursor-sm" aria-hidden="true">_</span>
      </button>
      <span class="tag-suffix">things</span>
    </div>
    <p class="hint">auto-cycles · click to skip</p>
  </div>
</div>

<style>
  .demo-area {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 3.5vw;
    padding: 1vw 0;
  }

  .block {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.8vw;
  }

  .block-label {
    font-family: var(--font-mono);
    font-size: clamp(0.45rem, 0.58vw, 0.62rem);
    color: var(--text-dim);
    text-transform: uppercase;
    letter-spacing: 0.18em;
  }

  /* ── Headline ── */
  .morph-head {
    display: inline-flex;
    align-items: baseline;
    gap: 0;
    font-family: var(--font-display);
    font-size: clamp(2rem, 4.5vw, 5rem);
    font-weight: 800;
    line-height: 1;
    letter-spacing: -0.01em;
    color: var(--text);
    background: none;
    border: none;
    cursor: pointer;
    padding: 0;
  }

  /* ── Tag line ── */
  .tag-line {
    display: inline-flex;
    align-items: baseline;
    gap: 0.6vw;
    font-family: var(--font-body);
    font-size: clamp(0.9rem, 1.4vw, 1.6rem);
    font-weight: 300;
    color: var(--text-muted);
  }

  .tag-prefix,
  .tag-suffix {
    white-space: nowrap;
  }

  .morph-tag {
    display: inline-flex;
    align-items: baseline;
    background: none;
    border: none;
    cursor: pointer;
    padding: 0;
    font: inherit;
    font-weight: 600;
  }

  /* ── Individual glyph ── */
  .glyph {
    display: inline-block;
    font-family: var(--font-mono);
    font-weight: 700;
    /* gradient: settled = gold, scrambling = cyan */
    background: linear-gradient(135deg, var(--gold-dim), var(--gold-bright));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    transition: background 0.1s ease;
    min-width: 0.55em;
    text-align: center;
  }

  .glyph.scrambling {
    background: linear-gradient(135deg, var(--cyan-dim), var(--cyan));
    -webkit-background-clip: text;
    background-clip: text;
  }

  .glyph.space {
    min-width: 0.3em;
    background: none;
    -webkit-text-fill-color: transparent;
  }

  .glyph-sm {
    font-size: clamp(0.9rem, 1.4vw, 1.6rem);
    font-weight: 600;
    min-width: 0.6em;
  }

  /* ── Blinking cursor ── */
  .cursor {
    font-family: var(--font-mono);
    font-size: clamp(2rem, 4.5vw, 5rem);
    font-weight: 300;
    color: var(--gold);
    opacity: 0.7;
    animation: blink 1.1s step-end infinite;
    line-height: 1;
    margin-left: 0.05em;
  }

  .cursor-sm {
    font-size: clamp(0.9rem, 1.4vw, 1.6rem);
  }

  @keyframes blink {
    0%,
    100% {
      opacity: 0.7;
    }
    50% {
      opacity: 0;
    }
  }

  .hint {
    font-family: var(--font-mono);
    font-size: clamp(0.48rem, 0.6vw, 0.65rem);
    color: var(--text-dim);
    letter-spacing: 0.06em;
  }
</style>
