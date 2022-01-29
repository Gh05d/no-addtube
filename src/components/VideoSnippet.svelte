<script>
  import { push } from "svelte-spa-router";
  export let snippet;
  export let id;
</script>

<div class="snippet" on:click={() => push(`/videos/${id}`)} role="link">
  <img
    src={snippet.thumbnails?.high?.url || snippet.thumbnails?.standard?.url}
    alt={snippet.title}
    width={snippet.thumbnails?.high?.width || snippet.thumbnails?.standard?.width || 100}
    height={snippet.thumbnails?.high?.height ||
      snippet.thumbnails?.standard?.height ||
      200} />
  <h3>{snippet.title}</h3>
  <h4>{snippet.channelTitle}</h4>
</div>

<style lang="scss">
  .snippet {
    display: flex;
    flex-flow: column;
  }
</style>
