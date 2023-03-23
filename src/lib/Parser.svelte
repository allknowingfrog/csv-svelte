<script>
  import Papa from "papaparse"

  export let store

  let uploader

  const parse = () => {
    const file = uploader.files[0]

    Papa.parse(file, {
      skipEmptyLines: true,
      complete: results => store(results.data)
    })
  }
</script>

<div>
  <div>
    <input bind:this={uploader} type="file" />
    <button on:click={parse}>Parse</button>
  </div>
</div>
