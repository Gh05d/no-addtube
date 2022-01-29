<script>
  import { location } from "svelte-spa-router";

  const [, id] = $location.split("/videos/");
</script>

<iframe
  title="Youtube video"
  allowfullscreen
  class="video"
  src={`https://www.youtube.com/embed/${id}`} />

<style lang="scss">
  iframe {
    width: 100%;
    height: 100%;
  }
</style>
