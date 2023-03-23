<script>
  import icon from '/icon.svg'
  import Parser from './lib/Parser.svelte'
  import Downloader from './lib/Downloader.svelte'
  import Database from './lib/Database.svelte'

  let rows

  const store = data => {
    console.log(data)

    rows = data
  }
</script>

<main>
  <img src={icon} class="logo" alt="Logo" />
  <h1>CSV Parser</h1>

  <div class="card">
    <Parser {store} />
    <Database />
    {#if rows}
      <Downloader {rows} />
    {/if}
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
