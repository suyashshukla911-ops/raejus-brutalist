<script lang="ts">
  import ArchiveTable from '$lib/components/ArchiveTable.svelte';
</script>

<svelte:head><title>ARCHIVE // RAEJUS</title></svelte:head>

<section class="page"><header class="head panel-black"><span class="kicker">ARCHIVE // COMPOSITE INDEX</span><h1>THE<br />DATABASE.</h1></header><ArchiveTable /></section>

<style>
  .head { min-height: 38vh; padding: clamp(1rem,4vw,4rem); display: flex; flex-direction: column; justify-content: space-between; }
  .head h1 { font-family: var(--font-display); font-size: clamp(5rem,14vw,13rem); line-height: .77; letter-spacing: -.1em; }
</style>
