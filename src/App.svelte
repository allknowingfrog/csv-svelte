<script>
  import icon from '/icon.svg'
  import Parser from './lib/Parser.svelte'
</script>

<main>
  <img src={icon} class="logo" alt="Logo" />
  <h1>CSV Parser</h1>

  <div class="card">
    <Parser />
  </div>
</main>

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
</style>
