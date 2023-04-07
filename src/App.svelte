<script>
  import Papa from "papaparse"

  import initSqlJs from "sql.js"
  import sqlJsWasm from "../node_modules/sql.js/dist/sql-wasm.wasm?url"

  import icon from '/icon.svg'

  import Uploader from './lib/Uploader.svelte'
  import Downloader from './lib/Downloader.svelte'

  let csv

  const parseRow = row => {
    const childId = parseInt(row[1])
    const firstName = row[3]
    const lastName = row[4]
    const logDate = row[7]
    const hours = parseFloat(row[13])

    const name = lastName + ", " + firstName

    return childId && logDate && hours ? [name, logDate, hours] : null
  }

  const load = rows => {
    initSqlJs({
      locateFile: file => sqlJsWasm
    }).then(SQL => {
      const db = new SQL.Database()

      db.run("CREATE TABLE logs (name INTEGER, log_date TEXT, hours REAL);")

      const stmt = db.prepare("INSERT INTO logs VALUES (?, ?, ?);")

      let values

      rows.forEach(row => {
        values = parseRow(row)

        if (values) {
          stmt.run(values)
        }
      })

      const result = db.exec(`
        SELECT name, SUM(days) AS days, SUM(low_hours + high_hours) AS hours
        FROM (
          SELECT name, log_date,
            IIF(hours >= 6, 1, 0) AS days,
            IIF(hours < 6, CEIL(hours * 4) / 4.0, 0) AS low_hours,
            IIF(hours > 10, CEIL((hours - 10) * 4) / 4.0, 0) AS high_hours
          FROM (
            SELECT name, log_date, SUM(hours) hours
            FROM logs GROUP BY name, log_date
          )
        ) GROUP BY name;
      `)

      let data = result[0].values

      data.unshift(["Child", "Days", "Hours"])

      csv = Papa.unparse(data)
    })
  }

  const store = file => {
    Papa.parse(file, {
      skipEmptyLines: true,
      complete: results => load(results.data)
    })
  }
</script>

<main>
  <img src={icon} class="logo" alt="Logo" />
  <h1>Attendance Calculator</h1>

  <div class="card">
    <Uploader {store} />
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
