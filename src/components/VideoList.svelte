<script>
  import { onMount } from "svelte";
  import Snippet from "./VideoSnippet.svelte";

  export let search;
  let videos = [];
  let loading = true;
  let error = null;

  onMount(() => {
    (async function init() {
      try {
        videos = await search();
      } catch (err) {
        console.error(err);
        error = err;
      } finally {
        loading = false;
      }
    })();
  });
</script>

{#if loading}
  <div>Loading...</div>
{:else if error}
  <div>Sorry, something went wrong</div>
{:else}
  <ul class="videos">
    {#each videos.items as video}
      <li><Snippet snippet={video.snippet} id={video.id} /></li>
    {/each}
  </ul>
{/if}

<style lang="scss">
  .videos {
    flex-flow: row wrap;

    li {
      flex: 1 0 480px;
      min-width: 0;
    }
  }
</style>
