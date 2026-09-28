<script lang="ts">
  import ArchiveTable from '$lib/components/ArchiveTable.svelte';
</script>

<svelte:head><title>MEDIA // RAEJUS</title></svelte:head>

<section class="archive page">
  <header class="header panel-black"><div><span class="kicker">04 // ARCHIVE</span><h1>MEDIA<br />/ CONTENT</h1></div><span class="code">CATALOGUE 01—03</span></header>
  <div class="intro panel"><p>The legacy Media section remains represented as an archive surface. Original images/videos should be mounted in <code>static/media/existing-site/</code> without recompression or visual edits.</p></div>
  <ArchiveTable />
</section>

<style>
  .header { min-height: 42vh; display: flex; align-items: end; justify-content: space-between; gap: 1rem; padding: clamp(1rem,4vw,4rem); }
  .header h1 { margin-top: .8rem; font-family: var(--font-display); font-size: clamp(4.5rem,12vw,12rem); line-height: .8; letter-spacing: -.1em; }
  .code { font-size: .58rem; }
  .intro { padding: 1rem; border-left: var(--line); font-size: .7rem; }
  .intro code { background: var(--accent); padding: .1rem .25rem; }
</style>
