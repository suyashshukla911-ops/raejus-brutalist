<script lang="ts">
  import { archiveItems } from '$lib/data/site';
  let activeMedia = '';
  let x = 0;
  let y = 0;

  function move(event: MouseEvent, media: string) {
    activeMedia = media;
    x = event.clientX + 18;
    y = event.clientY + 18;
  }
</script>

<div class="archive-wrap">
  <table class="archive-table">
    <thead><tr><th>ID</th><th>TYPE</th><th>TITLE</th><th>YEAR</th><th>CLASS</th></tr></thead>
    <tbody>
      {#each archiveItems as item}
        <tr on:mousemove={(e) => move(e, item.media)} on:mouseleave={() => activeMedia = ''}>
          <td>{item.id}</td><td>{item.type}</td><td><strong>{item.title}</strong></td><td>{item.year}</td><td>{item.tag}</td>
        </tr>
      {/each}
    </tbody>
  </table>

  {#if activeMedia}
    <div class="media-follow" style={`left:${x}px; top:${y}px`}>
      <img src={activeMedia} alt="Archive preview" />
    </div>
  {/if}
</div>

<style>
  .archive-wrap { position: relative; }
  .archive-table { width: 100%; border-collapse: collapse; font-family: var(--font-mono); }
  th, td { border: var(--line); padding: .75rem; text-align: left; vertical-align: top; }
  th { background: var(--ink); color: var(--paper); font-size: .58rem; }
  td { font-size: .68rem; }
  tbody tr { transition: transform 90ms steps(2,end); }
  tbody tr:hover { background: var(--accent); transform: translateX(4px); }
  .media-follow { position: fixed; z-index: 8000; width: 280px; height: 340px; pointer-events: none; border: 2px solid var(--ink); background: var(--paper); box-shadow: 7px 7px 0 var(--ink); overflow: hidden; }
  .media-follow img { width: 100%; height: 100%; object-fit: cover; filter: grayscale(1) contrast(1.15); }
</style>
