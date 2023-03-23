<script>
  import Papa from "papaparse"

  export let rows

  const csv = Papa.unparse(rows)
  const file = new Blob([csv], {type: "text/csv"})

  const downloadUrl = URL.createObjectURL(file)
</script>

<div>
  <a href={downloadUrl} download="download.csv">Download</a>
</div>
