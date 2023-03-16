<script>
  import Papa from 'papaparse'

  let upload
  let output = ''

  const parse = () => {
    const file = upload.files[0]

    Papa.parse(file, {
      complete: function(results) {
        output = JSON.stringify(results.data)
      }
    })
  }
</script>

<div>
  <div>
    <input bind:this={upload} type="file" />
    <button on:click={parse}>Parse</button>
  </div>

  <textarea>{output}</textarea>
</div>
