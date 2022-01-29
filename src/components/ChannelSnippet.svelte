<script>
  import { push } from "svelte-spa-router";
  export let snippet;
  export let id;
</script>

<div class="snippet" on:click={() => push(`/channels/${id}`)} role="link">
  <img
    src={snippet.thumbnails?.high?.url || snippet.thumbnails?.standard?.url}
    alt={snippet.title}
    width={200}
    height={200} />
  <h3>{snippet.channelTitle}</h3>
  <h4>{snippet.description}</h4>
</div>

<style lang="scss">
  .snippet {
    display: flex;
    flex-flow: column;
  }
</style>
