<script lang="ts">
  import { tick } from 'svelte';
  import { contact } from '$lib/data/site';

  let name = '';
  let email = '';
  let message = '';
  let input: HTMLTextAreaElement;
  let state: 'idle' | 'processing' | 'sent' = 'idle';
  let logs: string[] = [];

  const sequence = [
    '> INIT COMMS...',
    '> ALLOCATING CHANNEL...',
    '> SERIALIZING MESSAGE...',
    '> ENCRYPTING PAYLOAD...',
    '> ESTABLISHING HANDSHAKE...',
    '> ROUTING TO RAEJUS.NODE...',
    '> DELIVERY CHANNEL: READY',
    '> HANDSHAKE: OK',
    '> PACKET TRANSMITTED'
  ];

  const sleep = (ms: number) => new Promise((r) => setTimeout(r, ms));

  async function transmit() {
    if (state === 'processing' || !message.trim()) {
      tick().then(() => input?.focus());
      return;
    }
    state = 'processing';
    logs = [];
    for (const line of sequence) {
      await sleep(140);
      logs = [...logs, line];
    }
    await sleep(240);
    state = 'sent';
  }

  function keydown(event: KeyboardEvent) {
    if (event.key === 'Enter' && !event.shiftKey) {
      event.preventDefault();
      transmit();
    }
  }

  function reset() {
    name = ''; email = ''; message = ''; logs = []; state = 'idle';
    tick().then(() => input?.focus());
  }
</script>

<svelte:head><title>CONTACT // RAEJUS</title></svelte:head>

<section class="contact page">
  <header class="head"><div><span class="kicker">06 // TERMINAL</span><h1>GET IN<br />TOUCH.</h1></div><span class="code">COMMS CHANNEL / LIVE</span></header>

  <section class="status-grid">
    <div><span>[ STATUS ]</span><strong>{contact.status}</strong></div>
    <div><span>[ LOC ]</span><strong>{contact.location}</strong></div>
    <div><span>[ COMMS ]</span><a href={`mailto:${contact.email}`}>{contact.email}</a></div>
    <div><span>[ PING ]</span><a href={`tel:${contact.phone.replaceAll('-','')}`}>{contact.phone}</a></div>
  </section>

  <section class:processing={state === 'processing'} class:sent={state === 'sent'} class="terminal">
    <header><span><i></i><i></i><i></i></span><b>RAEJUS.COM // SECURE INPUT CHANNEL</b><em>{state.toUpperCase()}</em></header>
    <div class="body">
      <div class="field"><label for="name">&gt; IDENT:</label><input id="name" bind:value={name} placeholder="YOUR NAME" disabled={state !== 'idle'} /></div>
      <div class="field"><label for="email">&gt; RETURN CHANNEL:</label><input id="email" type="email" bind:value={email} placeholder="YOUR EMAIL" disabled={state !== 'idle'} /></div>
      <div class="field"><label for="message">&gt; ENTER YOUR MESSAGE:</label><textarea bind:this={input} id="message" bind:value={message} on:keydown={keydown} rows="7" placeholder="TYPE DIRECTLY INTO THE TERMINAL..." disabled={state !== 'idle'}></textarea></div>
      {#if logs.length}<div class="logs">{#each logs as line}<div>{line}</div>{/each}</div>{/if}
      {#if state === 'sent'}
        <div class="success"><span>[ OK ]</span><strong>&gt; MESSAGE TRANSMITTED.</strong><small>CHANNEL CLOSED // AWAIT RESPONSE</small></div>
      {/if}
      <footer>
        {#if state === 'idle'}
          <button on:click={transmit}>TRANSMIT [ENTER]</button><span>SHIFT + ENTER → NEW LINE</span>
        {:else if state === 'processing'}
          <span class="processing-text">PROCESSING<span class="blink">_</span></span>
        {:else}
          <button on:click={reset}>NEW TRANSMISSION</button>
        {/if}
      </footer>
    </div>
  </section>

  <footer class="end"><span>RAEJUS // AVIATION // AEROSPACE // R&amp;D</span><span>[ SYSTEM END ]</span></footer>
</section>

<style>
  .head { min-height: 42vh; display: flex; flex-direction: column; justify-content: space-between; padding: clamp(1rem,4vw,4rem); border-bottom: var(--line); }
  .head h1 { margin-top: 1rem; font-family: var(--font-display); font-size: clamp(5rem,14vw,13rem); line-height: .78; letter-spacing: -.1em; }
  .code { font-size: .6rem; }
  .status-grid { display: grid; grid-template-columns: repeat(4,1fr); border-bottom: var(--line); }
  .status-grid > div { min-height: 120px; display: flex; flex-direction: column; justify-content: space-between; gap: 1rem; padding: 1rem; border-right: var(--line); }
  .status-grid > div:last-child { border-right: 0; }
  .status-grid span { opacity: .55; font-size: .55rem; }
  .status-grid strong, .status-grid a { font-size: .7rem; font-weight: 700; }
  .terminal { margin: 2rem; border: 2px solid var(--ink); background: #050505; color: var(--paper); box-shadow: 10px 10px 0 var(--accent); }
  .terminal > header { min-height: 44px; display: grid; grid-template-columns: 90px 1fr auto; align-items: center; gap: 1rem; padding: 0 .8rem; border-bottom: 1px solid var(--paper); font-size: .58rem; }
  .terminal > header > span { display: flex; gap: 4px; }
  .terminal > header i { width: 8px; height: 8px; border: 1px solid var(--paper); }
  .terminal > header em { font-style: normal; }
  .body { padding: clamp(1rem,4vw,3rem); }
  .field { display: grid; grid-template-columns: 220px 1fr; border-bottom: 1px solid #333; }
  .field label { padding: 1rem; color: var(--accent); font-size: .72rem; font-weight: 700; }
  .field input, .field textarea { border: 0; border-left: 1px solid #333; padding: 1rem; background: transparent; color: var(--paper); outline: 0; font-size: .85rem; }
  .field input:focus, .field textarea:focus { background: var(--accent); color: var(--ink); }
  .logs { padding: 1.2rem 1rem; border-bottom: 1px solid #333; color: #aaa; font-size: .7rem; line-height: 1.8; }
  .success { display: flex; flex-direction: column; gap: .6rem; padding: 1.5rem 1rem; background: var(--accent); color: var(--ink); border-bottom: var(--line); }
  .success strong { font-family: var(--font-display); font-size: clamp(2rem,6vw,5rem); line-height: .86; letter-spacing: -.07em; }
  .terminal footer { display: flex; justify-content: space-between; align-items: center; gap: 1rem; padding: 1rem; font-size: .58rem; }
  .terminal footer button { border: var(--line); background: var(--accent); color: var(--ink); padding: .75rem 1rem; font-weight: 700; }
  .processing-text { color: var(--accent); font-weight: 700; }
  .blink { animation: blink 650ms steps(2,end) infinite; }
  .end { display: flex; justify-content: space-between; gap: 1rem; padding: 1rem; border-top: var(--line); font-size: .55rem; }
  @media (max-width: 800px) { .status-grid { grid-template-columns: 1fr 1fr; } .status-grid > div:nth-child(2n) { border-right: 0; } .status-grid > div { border-bottom: var(--line); } .field { grid-template-columns: 1fr; } .field input, .field textarea { border-left: 0; border-top: 1px solid #333; } }
  @media (max-width: 560px) { .status-grid { grid-template-columns: 1fr; } .status-grid > div { border-right: 0 !important; } .terminal { margin: .7rem; box-shadow: 6px 6px 0 var(--accent); } .terminal > header { grid-template-columns: 65px 1fr; } .terminal > header em { display: none; } .end { flex-direction: column; } }
</style>
