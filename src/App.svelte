<script>
  import Footer from "./Footer.svelte";
  import Header from "./Header.svelte";
  import Home from "./pages/Home.svelte";
  import Video from "./pages/Video.svelte";
  import Router from "svelte-spa-router";
  import Channel from "./pages/Channel.svelte";

  const routes = {
    "/": Home,
    "/videos/:id": Video,
    "/channels/:id": Channel,
  };
</script>

<svelte:head>
  <title>No Addtube</title>
</svelte:head>

<Header />
<Router {routes} />
<Footer />
