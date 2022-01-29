<script>
  import axios from "axios";
  import VideoSnippet from "../components/VideoSnippet.svelte";
  import VideoList from "../components/VideoList.svelte";
  import ChannelSnippet from "../components/ChannelSnippet.svelte";

  let video = "";
  let channel = "";
  let error;
  let videos = [];
  let channels = [];

  // List of categories: https://gist.github.com/dgp/1b24bf2961521bd75d6c
  const videoCategories = [
    { id: 10, label: "Music", country: "us" },
    { id: 20, label: "Gaming", country: "us" },
    { id: 28, label: "Science & Technology", country: "us" },
    { id: 1, label: "Film & Animation", country: "us" },
    { id: 25, label: "News & Politics", country: "de" },
  ];

  function debounce(fn, wait = 300) {
    let timeout;

    return function debounced(...args) {
      clearTimeout(timeout);

      timeout = setTimeout(() => {
        timeout = null;

        fn.apply(this, args);
      }, wait);
    };
  }

  async function searchVideo() {
    if (video?.length > 2) {
      try {
        const { data } = await axios.get("https://www.googleapis.com/youtube/v3/search", {
          params: {
            q: video,
            type: "video",
            key: YOUTUBE_API,
            part: "snippet",
            maxResults: 10,
          },
        });

        videos = data.items;
        error = null;
      } catch (err) {
        error = err;
      }
    }
  }

  async function searchChannel() {
    if (channel?.length > 2) {
      try {
        const { data } = await axios.get("https://www.googleapis.com/youtube/v3/search", {
          params: {
            q: channel,
            type: "channel",
            key: YOUTUBE_API,
            part: "snippet",
            maxResults: 10,
          },
        });

        channels = data.items;

        error = null;
      } catch (err) {
        error = err;
      }
    }
  }

  async function getPopular(category, country) {
    const { data } = await axios.get("https://www.googleapis.com/youtube/v3/videos", {
      params: {
        part: "snippet",
        chart: "mostPopular",
        key: YOUTUBE_API,
        regionCode: country,
        videoCategoryId: category,
        maxResults: 15,
      },
    });

    return data;
  }
</script>

<main>
  <input placeholder="Search for a video" bind:value={video} />
  <button class="search-button" on:click={searchVideo}>Search</button>

  <input placeholder="Search for a channel" bind:value={channel} />
  <button class="search-button" on:click={searchChannel}>Search</button>

  {#if error}
    <div>Sorry, something went wrong</div>
  {:else}
    {#if videos.length}
      <ul class="videos">
        {#each videos as video}
          <li><VideoSnippet snippet={video.snippet} id={video.id.videoId} /></li>
        {/each}
      </ul>
    {/if}

    {#if channels.length}
      <ul class="videos">
        {#each channels as channel}
          <li><ChannelSnippet snippet={channel.snippet} id={channel.id.channelId} /></li>
        {/each}
      </ul>
    {/if}
  {/if}

  {#each videoCategories as { id, label, country }}
    <h2>Hot Videos for {label}</h2>
    <VideoList search={() => getPopular(id, country)} />
  {/each}
</main>

<style lang="scss">
  main {
    padding: 1em;
    flex-grow: 1;
  }

  h2 {
    margin: 1rem 0;
  }

  .search-button {
    outline: none;
    border: 0;
    padding: 14px;
    font-size: 1.2rem;
    background-color: lightgreen;
  }

  .videos {
    flex-flow: row wrap;

    li {
      flex: 1 0 480px;
      min-width: 0;
    }
  }
</style>
