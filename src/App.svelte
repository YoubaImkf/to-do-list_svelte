<script>
  import svelteLogo from './assets/svelte.svg'
  import viteLogo from '/vite.svg'
  import Task from './lib/Task.svelte'
  import List from './lib/List.svelte'
  import Router from 'svelte-spa-router'

</script>

<main>

  <div class="card">
    <List />
  </div>
  
</main>

<Router routes={{  
  '/list/:id': Task }} 
  />

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
</style>


