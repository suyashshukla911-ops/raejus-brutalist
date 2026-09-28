<script lang="ts">
  import { goto } from '$app/navigation';
  import { page } from '$app/state';
  import { tick } from 'svelte';
  import { spring } from 'svelte/motion';
  import { navItems } from '$lib/data/site';

  let open = false;
  let query = '';
  let input: HTMLInputElement;

  const palette = spring(0, { stiffness: 0.3, damping: 0.75 });

  const normalizePath = (path: string) => path.replace(/\/$/, '') || '/';

  $: filtered = navItems.filter((item) => {
    const q = query.trim().toLowerCase();
    if (!q) return true;
    return item.label.toLowerCase().includes(q) || item.path.includes(q) || item.number === q;
  });

  function openPalette() {
    open = true;
    palette.set(1);
    tick().then(() => input?.focus());
  }

  function closePalette() {
    open = false;
    query = '';
    palette.set(0);
  }

  function run(path: string) {
    closePalette();
    goto(path);
  }

  function keydown(event: KeyboardEvent) {
    const mod = event.ctrlKey || event.metaKey;
    if (mod && event.key.toLowerCase() === 'k') {
      event.preventDefault();
      open ? closePalette() : openPalette();
    }
    if (event.key === 'Escape' && open) closePalette();
    if (event.key === 'Enter' && open && query.trim()) {
      const match = filtered[0];
      if (match) run(match.path);
    }
  }
</script>

<svelte:window on:keydown={keydown} />

<aside class="index-nav">
  <button class="brand" on:click={() => run('/')} aria-label="RAEJUS home">
    <strong>RJ</strong>
    <span>INDEX</span>
  </button>

  <nav>
    {#each navItems as item}
      <button class:active={normalizePath(page.url.pathname) === normalizePath(item.path)} class="nav-item" on:click={() => run(item.path)}>
        <span>{item.number}</span>
        <b>{item.label}</b>
      </button>
    {/each}
  </nav>

  <button class="cmd" on:click={openPalette}>
    <b>⌘K</b>
    <span>COMMAND</span>
  </button>

  <div class="status"><i></i> ONLINE</div>
</aside>

{#if open}
  <div class="backdrop" role="presentation" on:click={(e) => e.currentTarget === e.target && closePalette()}>
    <section class="palette" style={`opacity:${$palette}; transform: translateY(${(1 - $palette) * -18}px) scale(${0.96 + $palette * 0.04})`} role="dialog" aria-modal="true">
      <div class="palette-head">
        <div>
          <span class="kicker">RAEJUS.SYS // NAVIGATION TERMINAL</span>
          <h2>ENTER COMMAND</h2>
        </div>
        <button on:click={closePalette}>ESC</button>
      </div>
      <div class="terminal-row">
        <span>&gt;</span>
        <input bind:this={input} bind:value={query} placeholder="/contact" autocomplete="off" spellcheck="false" />
      </div>
      <div class="results">
        {#each filtered as item}
          <button on:click={() => run(item.path)}>
            <span>{item.number}</span><b>{item.label}</b><code>{item.path}</code>
          </button>
        {/each}
        {#if filtered.length === 0}<div class="empty">ERROR // COMMAND NOT FOUND</div>{/if}
      </div>
      <footer>ENTER → EXECUTE &nbsp;&nbsp; ESC → ABORT &nbsp;&nbsp; CTRL/CMD + K → TOGGLE</footer>
    </section>
  </div>
{/if}

<style>
  .index-nav { position: fixed; z-index: 1000; inset: 0 auto 0 0; width: var(--sidebar-width); display: flex; flex-direction: column; border-right: var(--line); background: var(--paper); }
  .brand { min-height: 90px; display: flex; flex-direction: column; justify-content: space-between; align-items: flex-start; padding: .7rem; border: 0; border-bottom: var(--line); background: transparent; color: var(--ink); }
  .brand strong { font-family: var(--font-display); font-size: 2rem; letter-spacing: -.08em; }
  .brand span { font-size: .5rem; font-weight: 700; }
  nav { flex: 1; overflow: auto; }
  .nav-item { width: 100%; min-height: 66px; display: flex; align-items: flex-start; justify-content: space-between; padding: .6rem; border: 0; border-bottom: var(--line); background: transparent; color: var(--ink); text-align: left; }
  .nav-item:hover { background: var(--accent); }
  .nav-item.active { background: var(--ink); color: var(--paper); }
  .nav-item span { font-size: .58rem; font-weight: 700; }
  .nav-item b { writing-mode: vertical-rl; transform: rotate(180deg); font-size: .58rem; letter-spacing: .08em; }
  .cmd { min-height: 66px; display: flex; flex-direction: column; justify-content: center; align-items: center; gap: .2rem; border: 0; border-top: var(--line); border-bottom: var(--line); background: transparent; }
  .cmd:hover { background: var(--accent); }
  .cmd b { font-size: .65rem; }
  .cmd span, .status { font-size: .45rem; font-weight: 700; }
  .status { min-height: 40px; display: flex; justify-content: center; align-items: center; gap: .3rem; }
  .status i { width: 7px; height: 7px; border: 1px solid var(--ink); background: var(--accent); }

  .backdrop { position: fixed; z-index: 9999; inset: 0; display: grid; place-items: center; padding: 1.5rem; background: rgba(0,0,0,.72); backdrop-filter: blur(10px); }
  .palette { width: min(820px, 100%); border: 2px solid var(--paper); background: var(--ink); color: var(--paper); box-shadow: 10px 10px 0 var(--accent); }
  .palette-head { display: flex; justify-content: space-between; gap: 1rem; padding: 1rem; border-bottom: 1px solid var(--paper); }
  .palette-head h2 { margin: .4rem 0 0; font-family: var(--font-display); font-size: clamp(2.1rem, 7vw, 4.2rem); line-height: .88; letter-spacing: -.07em; }
  .palette-head button { border: 1px solid var(--paper); background: var(--ink); color: var(--paper); padding: .5rem .7rem; }
  .palette-head button:hover { background: var(--accent); color: var(--ink); }
  .terminal-row { display: grid; grid-template-columns: 30px 1fr; align-items: center; gap: .75rem; padding: 1rem; border-bottom: 1px solid #444; color: var(--accent); font-size: 1.1rem; }
  .terminal-row input { width: 100%; border: 0; outline: 0; background: transparent; color: var(--paper); font-size: 1.05rem; }
  .terminal-row input:focus { background: transparent; }
  .results { max-height: 360px; overflow: auto; }
  .results button { width: 100%; display: grid; grid-template-columns: 60px 1fr auto; gap: 1rem; padding: 1rem; border: 0; border-bottom: 1px solid #333; background: var(--ink); color: var(--paper); text-align: left; }
  .results button:hover { background: var(--accent); color: var(--ink); }
  .results code { opacity: .6; }
  .empty { padding: 1.2rem; color: var(--accent); font-size: .75rem; }
  .palette footer { padding: .7rem 1rem; border-top: 1px solid var(--paper); font-size: .55rem; }

  @media (max-width: 640px) {
    .index-nav { inset: 0 0 auto 0; width: 100%; height: 54px; flex-direction: row; }
    .brand { width: 58px; min-height: 54px; border-right: var(--line); border-bottom: 0; }
    .brand span, .status { display: none; }
    nav { display: flex; overflow-x: auto; }
    .nav-item { width: auto; min-width: 82px; min-height: 54px; flex-direction: row; align-items: center; border-right: var(--line); border-bottom: 0; }
    .nav-item b { writing-mode: horizontal-tb; transform: none; }
    .cmd { min-width: 60px; min-height: 54px; border: 0; border-left: var(--line); }
  }
</style>
