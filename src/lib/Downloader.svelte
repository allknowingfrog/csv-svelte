<script>
  export let csv

  const file = new Blob([csv], {type: "text/csv"})

  const downloadUrl = URL.createObjectURL(file)
</script>

<div>
  <a href={downloadUrl} download="attendance.csv">Download</a>
</div>
