<script lang="ts">
  const books = [
    { title: 'GETTING AIRBORNE', subtitle: 'A Practical insight into project & operations management', code: 'BOOK-01', date: 'AVAILABLE / ORIGINAL SITE' },
    { title: 'MULTIROTOR DRONE', subtitle: 'RPAS Operators Training Manual — Flight Training Manual / Technical Category', code: 'BOOK-02', date: 'LAUNCHED TITLE / SEP 2020' }
  ];
</script>

<svelte:head><title>BOOKS // RAEJUS</title></svelte:head>

<section class="books page">
  <header class="header panel-black"><span class="kicker">05 // THE BOOK</span><h1>GET<br />AIRBORNE.</h1><span class="code">AUTHOR / CAPT. RAEJUS JOB</span></header>
  <div class="book-grid">
    {#each books as book, i}
      <article class="book panel">
        <div class="cover hard-offset accent-offset"><span>{book.code}</span><strong>{i === 0 ? 'GETTING\nAIRBORNE' : 'MULTIROTOR\nDRONE'}</strong><small>RAEJUS / ARCHIVE</small></div>
        <div class="book-copy"><span class="kicker">{book.date}</span><h2>{book.title}</h2><p>{book.subtitle}</p><button>OPEN ARCHIVE ENTRY →</button></div>
      </article>
    {/each}
  </div>
</section>

<style>
  .header { position: relative; min-height: 46vh; padding: clamp(1rem,4vw,4rem); display: flex; flex-direction: column; justify-content: space-between; }
  .header h1 { font-family: var(--font-display); font-size: clamp(5rem,16vw,15rem); line-height: .77; letter-spacing: -.1em; }
  .code { font-size: .58rem; }
  .book-grid { display: grid; grid-template-columns: repeat(2,1fr); border-bottom: var(--line); }
  .book { min-height: 620px; display: grid; grid-template-columns: minmax(240px,.8fr) minmax(0,1.2fr); }
  .book + .book { border-left: 0; }
  .cover { margin: 2rem; display: flex; flex-direction: column; justify-content: space-between; padding: 1rem; min-height: 460px; background: var(--ink); color: var(--paper); }
  .cover span { font-size: .6rem; color: var(--accent); }
  .cover strong { white-space: pre-line; font-family: var(--font-display); font-size: clamp(2.5rem,5vw,5rem); line-height: .8; letter-spacing: -.08em; }
  .cover small { font-size: .5rem; }
  .book-copy { display: flex; flex-direction: column; justify-content: center; gap: 1rem; padding: 1.5rem; border-left: var(--line); }
  .book-copy h2 { font-family: var(--font-display); font-size: clamp(2rem,4vw,4rem); line-height: .88; }
  .book-copy p { max-width: 420px; font-size: .8rem; }
  .book-copy button { width: fit-content; border: var(--line); background: var(--ink); color: var(--paper); padding: .8rem; }
  .book-copy button:hover { background: var(--accent); color: var(--ink); }
  @media (max-width: 800px) { .book-grid { grid-template-columns: 1fr; } .book { border-left: 0; border-top: var(--line); } }
  @media (max-width: 560px) { .book { grid-template-columns: 1fr; } .book-copy { border-left: 0; border-top: var(--line); } }
</style>
