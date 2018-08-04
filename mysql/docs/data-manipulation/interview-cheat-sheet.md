# Interview Cheat Sheet

* [SELECT](#select) <br>
* [WHERE](#where) <br>
* [Aliases](#aliases) <br>
* [ORDER BY](#order-by) <br>
* [LIMIT](#limit) <br>
* [UNION / JOIN Summary](./union-join-summary.md) <br>
* [GROUP BY And HAVING](#group-by-and-having) <br>
* [SubQuery](#subquery) <br>
* [INSERT](#insert) <br>
* [UPDATE](#update) <br>
* [DELETE](#delete) <br>
* [REPLACE](#replace) <br>

### SELECT
* You can `SELECT` all columns by `*` symbol.
* Or you can `SELECT` by columns names.
* You can filter the result by `DISTINCT` to remove the duplication.

### WHERE
We use MySQL `WHERE` clause with `SELECT`, `UPDATE` and `DELETE` statements to filter rows in the result set.

You can use following filters with `WHERE` clause:
* `AND`
* `OR`
* `IN / NOT IN`
* `EXISTS / NOT EXISTS`
* `BETWEEN`
* `LIKE`
* `IS NULL / IS NOT NULL `

### Aliases
Aliases is so important when use `JOIN` clause or to clear columns names.

You can create Aliases into:
* columns
* tables

And use them with:
* `JOIN`
* `GROUP BY`
* `HAVING`
* `ORDER BY`

### ORDER BY
The `ORDER BY` clause allows you to:
* Sort a result set by a single column or multiple columns.
* Sort a result set by different columns in ascending or descending order.

Also you can use **expressions** and **custom sort** with `FIELD` function.

### LIMIT
`LIMIT` clause parameters:

* The `offset` specifies the offset of the first row to return. The offset of the first row is 0, not 1.
* The `count` specifies the maximum number of rows to return.

### GROUP BY And HAVING
**GROUP BY**

Use MySQL `GROUP BY` to group rows into subgroups based on values of columns [with aggregation function] or expressions.

MySQL also allows you to sort the groups in ascending or descending orders while the standard SQL does not.

**HAVING**

The `HAVING` clause is used in the `SELECT` statement to specify filter conditions for a group of rows or aggregates.

The `HAVING` clause is often used with the `GROUP BY` clause to filter groups based on a specified condition. If the `GROUP BY` clause is omitted, **the `HAVING` clause behaves like the `WHERE` clause**.

**NOTE**

Use `HAVING` only to filter results return by `GROUP BY` statement, because `HAVING` executing before send back the query results to the client, While `WHERE` executing before `SELECT` statement, for that it is so important to run the query faster because the filtering happened from the beginning.

### SubQuery
A MySQL `subquery` is a query nested within another query such as `SELECT`, `INSERT`, `UPDATE` or `DELETE`. In addition, a MySQL `subquery` can be nested inside another `subquery`.

A MySQL `subquery` is called an `inner query` while the query that contains the `subquery` is called an `outer query`. A subquery can be used anywhere that expression is used and must be closed in parentheses.

**Correlated SubQuery**

You can use the `subquery` as standalone query, or you can **uses the data** from the `outer query` inside the `inner query`, in this case it called `correlated subquery`.

**You can use SubQuery with:**
* `SELECT`
* `FROM`
* `WHERE` / `HAVING`
    * `IN`
    * `EXISTS`
    * `ANY`
    * `ALL`

**EXISTS Clause vs IN Clause**

* Use `EXISTS` when the **subquery results is large**, and `IN` when the subquery results is very small or with **static list**, because `EXISTS` is faster then `IN` when deal with `SubQuery`.
* When `EXISTS` checks value contains `NULL` it returns `TRUE`, While `IN` returns `FALSE`, So use `EXISTS` when you want count `NULL` values, Or add `NOT NULL` in the `SubQuery`.

**SubQuery vs JOIN**

Use `JOIN` when:
* You want to combining (show) data from two tables side by side.

Use `SubQuery` when:
* You want to combining just a filtered column from other table or the same table.
* You want to filter current table data dependent on another table, in this case you will use SubQuery with `WHERE`, `IN`, `EXISTS`, `ANY` or `ALL`.
* You want to select `FROM` virtual table.

**Please notes** that `JOIN` faster then `SubQuery`, but `SubQuery` more readable from `JOIN`, **AND AFTER A LOT OF SEARCHING** I think you need to use `SubQuery` in **simple / straight forward queries**, and `JOIN` with **complex one**.

### INSERT
The MySQL `INSERT` statement allows you to insert one or more rows into a table.

```sql
INSERT INTO table(column_1,column_2 ...)
VALUES ('value_1','value_2'...),
       ('value_1','value_2'...),
       ('value_1','value_2'...),
```

In MySQL, you can specify the values for the `INSERT` statement from a `SELECT` statement.

Also you can use `ON DUPLICATE KEY UPDATE`

```sql
INSERT INTO devices(name)
VALUES ('Printer')
ON DUPLICATE KEY UPDATE name = 'Keyboard'
                        AND column_1 = VALUES(column_1) + 1; # <= to reuse column value
```

You can returns the **first id** generated by your batch of rows by `LAST_INSERT_ID()` function.

### UPDATE
`UPDATE` statement use to update existing data in a table. We can use the `UPDATE` statement to change column values of a single row, a group of rows, or all rows in a table.

MySQL supports two modifiers in the UPDATE statement.

* The `LOW_PRIORITY` modifier instructs the `UPDATE` statement to delay the update until there is no connection reading data from the table. The `LOW_PRIORITY` takes effect for the storage engines that use **table-level locking only**, for example, `MyISAM`, `MERGE`, `MEMORY`.
* The `IGNORE` modifier enables the `UPDATE` statement to continue updating rows even if errors occurred. The rows that cause errors such as **duplicate-key conflicts** are not updated.

You can `UPDATE` from `SELECT`. Also you can `UPDATE` / filter data form multiple tables using `UPDATE JOIN`.

### DELETE
`DELETE` statement use to delete existing data in a table. We can use the `DELETE` statement to delete a single row, a group of rows, or all rows in a table.

You can use `LIMIT` with `DELETE`, Also you can `DELETE` / filter data form multiple tables using `DELETE JOIN`.

You can use `ON DELETE CASCADE` to delete child rows when parent row deleted.

### REPLACE
The MySQL `REPLACE` statement is a MySQL extension to the standard SQL. The MySQL `REPLACE` statement works as follows:

* If the new row already does not exist, the MySQL `REPLACE` statement inserts a new row.
* If the new row already exist, the `REPLACE` statement deletes the old row first and then inserts a new row. In some cases, the `REPLACE` statement updates the existing row only.

To determine whether the new row already exists in the table, MySQL uses `PRIMARY KEY` or `UNIQUE KEY` index. If the table does not have one of these indexes, the `REPLACE` statement is equivalent to the `INSERT` statement.

You can use `REPLACE` statement with returned date from `SELECT` statement.