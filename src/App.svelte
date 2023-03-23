<script>
  import Papa from "papaparse"

  import icon from '/icon.svg'

  import Uploader from './lib/Uploader.svelte'
  import Downloader from './lib/Downloader.svelte'

  import Database from './lib/Database.svelte'

  let rows
  let csv

  const store = file => {
    Papa.parse(file, {
      skipEmptyLines: true,
      complete: results => {
        rows = results.data
      }
    })
  }

  const generate = () => {
    csv = Papa.unparse(rows)
  }
</script>

<main>
  <img src={icon} class="logo" alt="Logo" />
  <h1>CSV Parser</h1>

  <Database />

  <div class="card">
    <Uploader {store} />
    {#if rows}
      <button on:click={generate}>Generate</button>
    {/if}
    {#if csv}
      <Downloader {csv} />
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
