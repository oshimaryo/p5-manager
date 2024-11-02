<script>
  import { onMount } from 'svelte';
  import { push, location } from 'svelte-spa-router';

  let isActive = false;
  let loaded = false;
  let p5rc;

  const fetchData = async () => {
    const response = await fetch('/api/p5rc');
    return await response.json();
  };

  function handleToggle() {
    isActive = !isActive;
  }

  function handleRouteChange(to) {
    push(to);
    isActive = false;
  }

  onMount(async () => {
    try {
      p5rc = await fetchData();
      loaded = true;
    } catch (error) {
      consle.error(error);
    }
  });
</script>

<button class="toggle" on:click="{handleToggle}" class:-active="{isActive}">
  <img src="/assets/star.png" alt="star.png" />
</button>

<div class="sidebar {isActive ? '-active' : ''}">
  {#if loaded}
    <h2>{p5rc.collectionName}</h2>
    <ul>
      {#each p5rc.projects as project}
        <li>
          <span
            on:click="{() => handleRouteChange(`/${project}`)}"
            class="{project === $location.slice(1) ? '-active' : ''}">
            {project}
            <span>
              <a href="{`/${project}/index.html`}" target="_blank">↗</a>
            </span></span>
        </li>
      {/each}
    </ul>
  {/if}
</div>

<style>
  .sidebar {
    position: absolute;
    top: 0;
    left: -300px;
    width: 300px;
    height: 100%;
    padding: 0 10px 40px 40px;
    background-color: #f5f5f5;
    text-align: left;
    transition: all 0.5s;
    -webkit-transition: all 0.5s;
    z-index: 10;
    box-sizing: border-box;
    overflow-x: hidden;
    overflow-y: auto;

    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
      Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue',
      sans-serif;
  }

  .sidebar.-active {
    left: 0;
  }

  .toggle {
    position: fixed;
    top: 10px;
    left: 5px;
    background-color: transparent;
    border: none;
    transition: all 1s;
    -webkit-transition: all 1s;
    z-index: 11;
    cursor: pointer;
  }

  .toggle > img {
    width: 24px;
  }

  .toggle.-active {
    -ms-transform: rotate(144deg);
    -webkit-transform: rotate(144deg);
    transform: rotate(144deg);
  }

  .sidebar ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }

  .sidebar ul li {
    cursor: pointer;
    font-size: 14px;
    line-height: 1.5em;
  }

  .sidebar ul li span {
    word-break: break-word;
  }

  .sidebar ul li span.-active {
    color: #f07;
  }

  .sidebar ul li a {
    color: #333;
  }

  /* .highlight {
    color: #f07;
  } */
</style>
