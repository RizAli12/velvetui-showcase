<script>
  import '../app.css';
  let { children } = $props();
</script>

<svelte:head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>VelvetUI — Svelte 5 Component Showcase</title>
  <meta name="description" content="Live demos of VelvetUI — a dark-luxury Svelte 5 component library. Zero dependencies, pure CSS, rune-driven." />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
  <link
    href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=Inter:wght@300;400;500&family=Fira+Code:wght@400;500&display=swap"
    rel="stylesheet"
  />
</svelte:head>

{@render children()}
