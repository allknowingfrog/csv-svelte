<script>
  import Papa from "papaparse"

  import initSqlJs from "sql.js"
  import sqlJsWasm from "../../node_modules/sql.js/dist/sql-wasm.wasm?url"

  initSqlJs({
    locateFile: file => sqlJsWasm
  }).then(SQL => {
    const db = new SQL.Database()

    console.log(db.exec("SELECT 1 + 1;")[0].values[0][0])
  })

  let uploader
  let downloadUrl

  const parse = () => {
    const file = uploader.files[0]

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
    <input bind:this={uploader} type="file" />
    <button on:click={parse}>Parse</button>
  </div>

  {#if downloadUrl}
    <a href={downloadUrl} download="download.csv">Download</a>
  {/if}
</div>
