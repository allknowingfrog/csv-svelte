<script>
  import initSqlJs from "sql.js"
  import sqlJsWasm from "../../node_modules/sql.js/dist/sql-wasm.wasm?url"

  let output

  initSqlJs({
    locateFile: file => sqlJsWasm
  }).then(SQL => {
    const db = new SQL.Database()

    output = db.exec("SELECT 1 + 1;")[0].values[0][0]
  })
</script>

<div>
  Database: 1 + 1 = {output}
</div>
