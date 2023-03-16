<script>
  import Papa from "papaparse"

  let uploader
  let downloadUrl

  const parseRow = row => {
    const childId = parseInt(row[1])
    const logDate = row[7]
    const hours = parseFloat(row[13])

    return childId && logDate && hours ? [childId, logDate, hours] : null
  }

  const groupRows = rows => {
    let groups = {}
    let childId, logDate, hours

    rows.forEach(row => {
      [childId, logDate, hours] = row

      if(!groups[childId]) {
        groups[childId] = {}
      }

      if(!groups[childId][logDate]) {
        groups[childId][logDate] = []
      }

      groups[childId][logDate].push(hours)
    })

    return groups
  }

  const calculateTotals = groups => {
    let rows = []
    let childId
    let dateGroups
    let logDate
    let total

    for(childId in groups) {
      dateGroups = groups[childId]

      for(logDate in dateGroups) {
        total = dateGroups[logDate].reduce((a, b) => a + b, 0)

        rows.push([childId, logDate, total])
      }
    }

    return rows
  }

  const parse = () => {
    const file = uploader.files[0]

    Papa.parse(file, {
      skipEmptyLines: true,
      complete: (results) => {
        const rows = results.data.map(row => parseRow(row)).filter(row => row)
        const groups = groupRows(rows)
        const totals = calculateTotals(groups)

        totals.unshift(["Child ID", "Date", "Hours"])

        const csv = Papa.unparse(totals)
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
