<script lang="ts">
  import { achievements, credentials, focusDomains, profileStats, timeline } from '$lib/data/profile';
  let held = false;
</script>

<svelte:head><title>PROFILE // RAEJUS</title></svelte:head>

<section class="profile page">
  <header class="header panel-black">
    <div><span class="kicker">02 // DOSSIER</span><h1>MY<br />PROFILE</h1></div>
    <div class="stamp">CAPT. RAEJUS JOB<br />AVIATION / RPAS / R&amp;D</div>
  </header>

  <div class="stats">
    {#each profileStats as stat}
      <div class="stat panel"><small>{stat[0]}</small><strong>{stat[1]}</strong><span>{stat[2]}</span></div>
    {/each}
  </div>

  <div class="content">
    <article class="bio panel">
      <span class="kicker">/PROFESSIONAL PROFILE</span>
      <p class="lead">With over 25 years of experience as a Licensed Commercial Pilot, NTRO / DGCA certified UAV Instructor, qualified in Airport &amp; Aviation Operations and Disaster Management, the profile is built around practical aviation expertise, project execution, specialist instruction and unmanned systems.</p>
      <div class="domain-grid">
        {#each focusDomains as domain, i}<div><small>DOMAIN {String(i+1).padStart(2,'0')}</small><strong>{domain}</strong></div>{/each}
      </div>
      <div class="dense-copy">
        <p>Experience spans government aviation, aerospace R&amp;D, RPAS/UAV integration, airport operations, safety management, ATC, flight simulation, specialist curriculum development, business planning and project support.</p>
        <p>Hands-on areas include lighter-than-air systems, composites, electric propulsion, telemetry, avionics and navigation, payloads, flight safety, mission planning and aircraft production logistics.</p>
      </div>
    </article>

    <aside class="portrait panel">
      <div class="asset-missing" class:held>
        <div class="asset-grid"></div>
        <strong>PORTRAIT<br />ASSET</strong>
        <span>DROP ORIGINAL RAEJUS IMAGE INTO<br />static/media/existing-site/profile/</span>
      </div>
      <button on:mousedown={() => held = true} on:mouseup={() => held = false} on:mouseleave={() => held = false}>HOLD TO REVEAL COLOUR</button>
    </aside>

    <article class="achievements panel">
      <span class="kicker">/ACHIEVEMENTS</span>
      <div class="achievement-list">
        {#each achievements as item, i}<div class="achievement"><span>{String(i+1).padStart(2,'0')}</span><p>{item}</p></div>{/each}
      </div>
    </article>

    <article class="credentials panel">
      <span class="kicker">/CREDENTIAL REGISTER</span>
      <div class="cred-list">{#each credentials as cred}<div>{cred}</div>{/each}</div>
    </article>
  </div>

  <section class="timeline panel">
    <header><span class="kicker">/PROFESSIONAL TIMELINE</span><strong>1994 → PRESENT</strong></header>
    {#each timeline as row}
      <article class="timeline-row">
        <div class="year">{row[0]}</div><div class="role">{row[1]}</div><p>{row[2]}</p>
      </article>
    {/each}
  </section>
</section>

<style>
  .profile { background: var(--paper); }
  .header { min-height: 45vh; display: grid; grid-template-columns: 1fr 250px; align-items: end; gap: 1rem; padding: clamp(1rem,4vw,4rem); }
  .header h1 { margin-top: 1rem; font-family: var(--font-display); font-size: clamp(5rem, 15vw, 14rem); line-height: .8; letter-spacing: -.1em; }
  .stamp { border-left: 1px solid var(--paper); padding-left: 1rem; font-size: .6rem; line-height: 1.7; }
  .stats { display: grid; grid-template-columns: repeat(3, 1fr); border-bottom: var(--line); }
  .stat { min-height: 160px; display: flex; flex-direction: column; justify-content: space-between; padding: 1rem; border-right: 0; }
  .stat:not(:last-child) { border-right: var(--line); }
  .stat small { font-size: .55rem; }
  .stat strong { font-family: var(--font-display); font-size: clamp(3rem,7vw,7rem); line-height: .8; letter-spacing: -.08em; }
  .stat span { font-size: .55rem; font-weight: 700; }
  .content { display: grid; grid-template-columns: minmax(0,1.3fr) minmax(300px,.7fr); grid-template-areas: 'bio portrait' 'achievements credentials'; }
  .bio { grid-area: bio; padding: clamp(1rem,3vw,3rem); border-left: var(--line); }
  .lead { margin-top: 1rem; max-width: 900px; font-family: var(--font-display); font-size: clamp(1.5rem,3vw,2.8rem); line-height: .98; letter-spacing: -.05em; }
  .domain-grid { display: grid; grid-template-columns: repeat(2,1fr); margin-top: 2rem; border: var(--line); }
  .domain-grid div { min-height: 105px; padding: .8rem; border-right: var(--line); border-bottom: var(--line); }
  .domain-grid div:nth-child(2n) { border-right: 0; }
  .domain-grid div:nth-last-child(-n+2) { border-bottom: 0; }
  .domain-grid small { display: block; font-size: .48rem; opacity: .55; }
  .domain-grid strong { display: block; margin-top: .8rem; font-size: .75rem; }
  .dense-copy { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; margin-top: 2rem; font-size: .8rem; }
  .portrait { grid-area: portrait; display: flex; flex-direction: column; padding: 1rem; }
  .asset-missing { min-height: 520px; position: relative; display: grid; place-items: center; overflow: hidden; background: #7b7667; color: var(--paper); filter: grayscale(1); transition: filter 0ms linear; }
  .asset-missing.held { filter: grayscale(0) saturate(1.8); box-shadow: 5px 5px 0 var(--accent); }
  .asset-grid { position: absolute; inset: 0; background-image: linear-gradient(rgba(255,255,255,.12) 1px, transparent 1px), linear-gradient(90deg, rgba(255,255,255,.12) 1px, transparent 1px); background-size: 22px 22px; }
  .asset-missing strong { position: relative; font-family: var(--font-display); font-size: clamp(2.5rem, 6vw, 5rem); line-height: .82; letter-spacing: -.07em; }
  .asset-missing span { position: absolute; bottom: 1rem; left: 1rem; font-size: .5rem; }
  .portrait button { margin-top: 1rem; border: var(--line); background: var(--ink); color: var(--paper); padding: .8rem; }
  .portrait button:hover { background: var(--accent); color: var(--ink); }
  .achievements { grid-area: achievements; padding: 1rem; border-left: var(--line); }
  .achievement-list { margin-top: 1rem; }
  .achievement { display: grid; grid-template-columns: 50px 1fr; gap: 1rem; padding: 1rem 0; border-top: var(--line); }
  .achievement span { font-weight: 700; }
  .achievement p { margin: 0; font-size: .8rem; }
  .credentials { grid-area: credentials; padding: 1rem; }
  .cred-list { margin-top: 1rem; border-top: var(--line); }
  .cred-list div { padding: .7rem 0; border-bottom: var(--line); font-size: .66rem; }
  .timeline { padding: 0 1rem 1rem; border-left: var(--line); }
  .timeline > header { display: flex; justify-content: space-between; gap: 1rem; padding: 1rem 0; border-bottom: var(--line); }
  .timeline > header strong { font-family: var(--font-display); letter-spacing: -.04em; }
  .timeline-row { display: grid; grid-template-columns: 150px 250px 1fr; gap: 1rem; padding: 1rem 0; border-bottom: var(--line); }
  .year, .role { font-size: .65rem; font-weight: 700; }
  .timeline-row p { margin: 0; font-size: .72rem; }
  @media (max-width: 850px) {
    .header { grid-template-columns: 1fr; }
    .stamp { border-left: 0; border-top: 1px solid var(--paper); padding: 1rem 0 0; }
    .content { grid-template-columns: 1fr; grid-template-areas: 'bio' 'portrait' 'achievements' 'credentials'; }
    .portrait { border-left: var(--line); }
    .stats { grid-template-columns: 1fr; }
    .stat { border-right: 0 !important; border-bottom: var(--line); }
    .timeline-row { grid-template-columns: 1fr; gap: .4rem; }
  }
  @media (max-width: 560px) {
    .domain-grid, .dense-copy { grid-template-columns: 1fr; }
    .domain-grid div { border-right: 0; }
    .domain-grid div:nth-last-child(-n+2) { border-bottom: var(--line); }
    .domain-grid div:last-child { border-bottom: 0; }
  }
</style>
