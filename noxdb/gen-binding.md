There is a general concencus to never trust user input. In embedded SQL, you would use host parameters (binding parameters / parameter markers) to get around this.

---

When using the SQL APIs available in noxDB, there is a way to make use of Db2 parameter marker functionality. noxDB allows a different type of parameter markers in SQL statements.

```
sql   = (
  'Select PRODKEY, PRODID, PRICE ' +
  'from product ' +
  'where prodKey = $prodKey ' +
  'fetch first 1 row only'
);
```

The parameter marker in this SQL statement is `$prodKey`. The SQL APIs have parameters which allow the user to pass in either a JSON document pointer or string object, with nodes that match the parameter markers.

```
Row = json_sqlResultRow(sql:'{prodKey : 250}');
Result = json_asJsonText(Row);
```

---

If you wanted to use parameter markers for a result set instead of a single row, `json_sqlResultSet` has the markers parameter too.

```
sql = (
  'Select PRODKEY, PRODID, PRICE ' +
  'from product ' +
  'where MANUID = $manuID '
);

ResultSetPtr = json_sqlResultSet(
  sql:    // The sql stmt
  1:                             // from row number
  JSON_ALLROWS:                  // Max number of rows to fetch
  JSON_META+JSON_TOTALROWS:      // return a object of rows and row count
  '{manuID : "SAMSUNG"}'
);

Result = json_asJsonText(ResultSetPtr);
```

---

There are plenty more SQL APIs available in noxDB and you can find the [documentation for them here](/noxdb/docs/jsonsql).
