<script>
  import Router from 'svelte-spa-router';
  import Sidebar from './Sidebar.svelte';
  import Welcome from './Welcome.svelte';
  import Sketch from './Sketch.svelte';

  const routes = {
    '/': Welcome,
    '/:id': Sketch,
  };
</script>

<div class="app">
  <Sidebar />
  <Router routes="{routes}" />
</div>

<style>
  :root {
    --cBackground: #fff;
  }

  /* OSの設定がダークモード時のスタイル */
  @media (prefers-color-scheme: dark) {
    :root {
      --cBackground: #000;
    }
  }
  .app {
    height: 100%;
    /* background-color: black; */
    background-color: var(--cBackground);
  }
</style>
