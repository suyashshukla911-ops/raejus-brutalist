<script lang="ts">
  import '$lib/styles/global.css';
  import IndexNav from '$lib/components/IndexNav.svelte';
</script>

<svelte:head>
  <meta name="theme-color" content="#eae6d7" />
  <meta name="description" content="RAEJUS — personal insight across aviation, aerospace, unmanned systems, R&D, training and consulting." />
</svelte:head>

<IndexNav />
<div class="site-shell"><slot /></div>
