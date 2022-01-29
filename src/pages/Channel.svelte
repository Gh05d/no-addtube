<script>
  import axios from "axios";
  import { location } from "svelte-spa-router";
  import VideoList from "../components/VideoList.svelte";

  const [, id] = $location.split("/channels/");

  async function searchVideos() {
    const { data } = await axios.get("https://www.googleapis.com/youtube/v3/search", {
      params: {
        key: YOUTUBE_API,
        channelId: id,
        type: "video",
        part: "snippet",
        order: "date",
        maxResults: 30,
      },
    });

    return data;
  }
</script>

<VideoList search={() => searchVideos()} />

<style lang="scss">
</style>
