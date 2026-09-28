<script lang="ts">
  import { spring } from 'svelte/motion';

  const alphabet = '!@#$%^&*+-=<>/\\01ZXCVBNM';
  const targets = ['AVIATION', 'AEROSPACE', 'UAV / RPAS', 'R&D'];
  let display = [...targets];
  let scrambleTimer: ReturnType<typeof setInterval> | undefined;
  const posterScale = spring(1, { stiffness: 0.32, damping: 0.62 });

  function randomise(text: string) {
    return text.split('').map((char) => char === ' ' || char === '/' ? char : alphabet[Math.floor(Math.random() * alphabet.length)]).join('');
  }

  function scramble() {
    if (scrambleTimer) clearInterval(scrambleTimer);
    let frame = 0;
    scrambleTimer = setInterval(() => {
      display = targets.map((word, i) => word.split('').map((char, j) => {
        const settle = frame > 9 + i * 4 + j * 2;
        return settle ? char : randomise(char)[0];
      }).join(''));
      frame += 1;
      if (frame > 25) {
        clearInterval(scrambleTimer);
        display = [...targets];
        posterScale.set(1);
      }
    }, 45);
  }

  function hover() {
    posterScale.set(1.015);
    scramble();
  }

  scramble();
</script>

<svelte:head><title>RAEJUS // INDEX</title></svelte:head>

<section class="home page" role="presentation" on:mouseenter={hover}>
  <header class="topline">
    <span class="kicker">RAEJUS.COM // PERSONAL INSIGHT</span>
    <span class="mono">FILE 01 / HOME / 2026</span>
  </header>

  <main class="poster grid-paper">
    <div class="side-note mono">AVIATION / AEROSPACE / UNMANNED / RESEARCH</div>
    <div class="headline" style={`transform: scale(${$posterScale})`}>
      {#each display as line, index}
        <span class:accent={index === 2}>{line}</span>
      {/each}
    </div>
    <div class="poster-meta">
      <div><span>IDENTITY</span><strong>CAPT. RAEJUS JOB</strong></div>
      <div><span>DISCIPLINES</span><strong>OPERATIONS / R&D / TRAINING</strong></div>
      <div><span>MODE</span><strong>CONSULT / MENTOR / BUILD</strong></div>
    </div>
  </main>

  <div class="marquee">
    <div class="marquee-track">
      {#each Array(2) as _}
        <span class="marquee-item">MENTORING //</span>
        <span class="marquee-item">INCUBATION //</span>
        <span class="marquee-item">RESEARCH &amp; DEVELOPMENT //</span>
        <span class="marquee-item">CONSULTING //</span>
        <span class="marquee-item">MANAGEMENT //</span>
      {/each}
    </div>
  </div>
</section>

<style>
  .home { background: var(--paper); }
  .topline { min-height: 52px; display: flex; align-items: center; justify-content: space-between; gap: 1rem; padding: 0 1rem; border-bottom: var(--line); }
  .topline .mono { font-size: .58rem; }
  .poster { min-height: calc(100vh - 100px); display: grid; grid-template-columns: 52px minmax(0,1fr) 240px; grid-template-areas: 'side headline meta'; position: relative; overflow: hidden; border-bottom: var(--line); }
  .side-note { grid-area: side; writing-mode: vertical-rl; transform: rotate(180deg); display: flex; align-items: center; justify-content: center; padding: .5rem; border-right: var(--line); font-size: .55rem; letter-spacing: .08em; }
  .headline { grid-area: headline; align-self: center; display: flex; flex-direction: column; padding: 3vw 2vw; transform-origin: left center; }
  .headline span { display: block; font-family: var(--font-display); font-size: clamp(4rem, 12vw, 12rem); font-weight: 700; line-height: .82; letter-spacing: -.095em; text-transform: uppercase; animation: glitch 110ms steps(2,end) 2; }
  .headline span.accent { color: var(--ink); text-decoration: underline; text-decoration-thickness: .04em; text-underline-offset: .05em; }
  .poster-meta { grid-area: meta; display: flex; flex-direction: column; border-left: var(--line); align-self: stretch; }
  .poster-meta > div { flex: 1; display: flex; flex-direction: column; justify-content: space-between; padding: .9rem; border-bottom: var(--line); }
  .poster-meta > div:last-child { border-bottom: 0; }
  .poster-meta span { font-size: .55rem; opacity: .6; }
  .poster-meta strong { font-family: var(--font-display); font-size: 1rem; line-height: 1; letter-spacing: -.03em; }

  @media (max-width: 800px) {
    .poster { grid-template-columns: 40px 1fr; grid-template-areas: 'side headline' 'side meta'; }
    .poster-meta { grid-area: meta; border-left: 0; border-top: var(--line); flex-direction: row; min-height: 150px; }
    .poster-meta > div { border-right: var(--line); border-bottom: 0; }
    .poster-meta > div:last-child { border-right: 0; }
  }
  @media (max-width: 620px) {
    .topline { padding: 0 .6rem; }
    .topline .mono { display: none; }
    .poster { display: block; min-height: calc(100vh - 106px); }
    .side-note { position: absolute; inset: 0 auto 0 0; width: 34px; border-right: var(--line); }
    .headline { padding: 20vh 2rem 1.5rem 48px; }
    .headline span { font-size: clamp(3.2rem, 17vw, 7rem); }
    .poster-meta { margin-left: 34px; border-left: 0; }
  }
</style>
