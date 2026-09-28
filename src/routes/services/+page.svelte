<script lang="ts">
  import { spring } from 'svelte/motion';
  import { services } from '$lib/data/services';

  type Node = { id: string; x: number; y: number; };
  let nodes: Node[] = services.map((s) => ({ id: s.id, x: s.x, y: s.y }));
  let dragging = false;
  let lastX = 0;
  let lastY = 0;
  let zoom = spring(1, { stiffness: .25, damping: .75 });
  let expanded = '';

  const serviceById = (id: string) => services.find((s) => s.id === id)!;

  function startPan(event: PointerEvent) {
    if ((event.target as HTMLElement).closest('.node')) return;
    dragging = true;
    lastX = event.clientX;
    lastY = event.clientY;
    (event.currentTarget as HTMLElement).setPointerCapture(event.pointerId);
  }
  function movePan(event: PointerEvent) {
    if (!dragging) return;
    const dx = event.clientX - lastX;
    const dy = event.clientY - lastY;
    lastX = event.clientX;
    lastY = event.clientY;
    nodes = nodes.map((node) => ({ ...node, x: Math.max(2, Math.min(88, node.x + dx / 8)), y: Math.max(2, Math.min(82, node.y + dy / 8)) }));
  }
  function endPan() { dragging = false; }

  function wheel(event: WheelEvent) {
    event.preventDefault();
    const next = Math.max(.7, Math.min(1.35, $zoom + (event.deltaY > 0 ? -.08 : .08)));
    zoom.set(next);
  }
</script>

<svelte:head><title>SERVICES // RAEJUS</title></svelte:head>

<section class="services page">
  <header class="services-header panel">
    <div><span class="kicker">03 // INFINITE CANVAS</span><h1>SERVICES<br />/ DECODED</h1></div>
    <div class="instructions"><span>DRAG FIELD → PAN</span><span>CLICK NODE → EXPAND</span><span>WHEEL → ZOOM</span></div>
  </header>

  <div class="canvas-wrap grid-paper" on:pointerdown={startPan} on:pointermove={movePan} on:pointerup={endPan} on:pointercancel={endPan} on:wheel={wheel}>
    <div class="canvas" style={`transform: scale(${$zoom})`}>
      <div class="crosshair crosshair-a">01</div>
      <div class="crosshair crosshair-b">02</div>
      {#each nodes as node}
        {@const service = serviceById(node.id)}
        <button class:expanded={expanded === service.id} class="node" style={`left:${node.x}%; top:${node.y}%`} on:pointerdown={(e) => e.stopPropagation()} on:click={() => expanded = service.id}>
          <span class="node-code">{service.code}</span>
          <strong>{service.title}</strong>
          <small>{service.strap}</small>
          <em>{service.bullets.length} MODULES</em>
        </button>
      {/each}
    </div>
  </div>

  {#if expanded}
    {@const service = serviceById(expanded)}
    <div class="expand-overlay" role="presentation" on:click={(e) => e.currentTarget === e.target && (expanded = '')}>
      <section class="expand-card">
        <header><span>{service.code}</span><button on:click={() => expanded = ''}>CLOSE ×</button></header>
        <h2>{service.title}</h2>
        <p>{service.strap}</p>
        <div class="module-grid">
          {#each service.bullets as bullet, i}<div><small>{String(i+1).padStart(2,'0')}</small><strong>{bullet}</strong></div>{/each}
        </div>
      </section>
    </div>
  {/if}
</section>

<style>
  .services { overflow: hidden; }
  .services-header { min-height: 220px; display: grid; grid-template-columns: 1fr 280px; align-items: end; padding: clamp(1rem,3vw,3rem); }
  .services-header h1 { margin-top: .8rem; font-family: var(--font-display); font-size: clamp(4rem,9vw,9rem); line-height: .82; letter-spacing: -.09em; }
  .instructions { display: flex; flex-direction: column; gap: .45rem; font-size: .58rem; text-align: right; }
  .canvas-wrap { position: relative; height: calc(100vh - 220px); min-height: 560px; overflow: hidden; touch-action: none; cursor: grab; border-bottom: var(--line); }
  .canvas-wrap:active { cursor: grabbing; }
  .canvas { position: absolute; inset: -20% -10%; width: 120%; height: 140%; transform-origin: 50% 50%; }
  .node { position: absolute; width: 260px; min-height: 180px; display: flex; flex-direction: column; align-items: flex-start; gap: .4rem; padding: 1rem; border: 2px solid var(--ink); background: var(--paper); color: var(--ink); box-shadow: 8px 8px 0 var(--ink); transform: translate(-50%, -50%); text-align: left; cursor: pointer; }
  .node:hover { background: var(--accent); transform: translate(-50%, -52%); }
  .node strong { margin-top: .6rem; font-family: var(--font-display); font-size: clamp(1.8rem,3vw,2.8rem); line-height: .86; letter-spacing: -.06em; }
  .node small { margin-top: auto; font-size: .58rem; font-weight: 700; }
  .node em { font-style: normal; font-size: .5rem; opacity: .55; }
  .node.expanded { outline: 4px solid var(--accent); }
  .node-code { font-size: .58rem; font-weight: 700; }
  .crosshair { position: absolute; font-size: .55rem; font-weight: 700; opacity: .45; }
  .crosshair-a { left: 45%; top: 25%; }
  .crosshair-b { left: 76%; top: 70%; }
  .expand-overlay { position: fixed; z-index: 4000; inset: 0; display: grid; place-items: center; padding: 1.5rem; background: rgba(0,0,0,.76); backdrop-filter: blur(8px); }
  .expand-card { width: min(1050px,100%); max-height: 90vh; overflow: auto; border: 2px solid var(--paper); background: var(--paper); color: var(--ink); box-shadow: 12px 12px 0 var(--accent); }
  .expand-card header { display: flex; justify-content: space-between; padding: 1rem; border-bottom: var(--line); font-size: .65rem; font-weight: 700; }
  .expand-card header button { padding: .45rem .6rem; border: var(--line); background: var(--paper); }
  .expand-card h2 { padding: 1rem; padding-bottom: 0; font-family: var(--font-display); font-size: clamp(3.5rem,9vw,8rem); line-height: .8; letter-spacing: -.09em; }
  .expand-card > p { padding: 1rem; font-size: .65rem; }
  .module-grid { display: grid; grid-template-columns: repeat(3,1fr); border-top: var(--line); }
  .module-grid div { min-height: 140px; padding: .9rem; border-right: var(--line); border-bottom: var(--line); }
  .module-grid div:nth-child(3n) { border-right: 0; }
  .module-grid small { display: block; font-size: .5rem; opacity: .55; }
  .module-grid strong { display: block; margin-top: 1rem; font-size: .72rem; }
  @media (max-width: 760px) {
    .services-header { grid-template-columns: 1fr; gap: 1rem; }
    .instructions { text-align: left; }
    .canvas-wrap { height: calc(100vh - 300px); }
    .node { width: 225px; }
    .module-grid { grid-template-columns: 1fr 1fr; }
    .module-grid div:nth-child(3n) { border-right: var(--line); }
    .module-grid div:nth-child(2n) { border-right: 0; }
  }
  @media (max-width: 500px) { .node { width: 195px; min-height: 150px; } .module-grid { grid-template-columns: 1fr; } .module-grid div { border-right: 0 !important; } }
</style>
