<script>
  import Papa from 'papaparse'

  let upload
  let downloadUrl

  const parse = () => {
    const file = upload.files[0]

    Papa.parse(file, {
      skipEmptyLines: true,
      complete: (results) => {
        console.log(results.data)

        const csv = Papa.unparse(results.data)
        const file = new Blob([csv], {type: "text/csv"})
        downloadUrl = URL.createObjectURL(file)
      }
    })
  }
</script>

<div>
  <div>
    <input bind:this={upload} type="file" />
    <button on:click={parse}>Parse</button>
  </div>

  {#if downloadUrl}
    <a href={downloadUrl} download="download.csv">Download</a>
  {/if}
</div>
