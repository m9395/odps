---
keyword: CTE
---

# Common table expressions

MaxCompute supports SQL-compliant common table expressions \(CTEs\) to improve the readability and execution efficiency of SQL statements.

**Syntax**

```
WITH 
     cte_name AS
    (
        cte_query
    )
    [,cte_name2  AS 
     (
     cte_query2
     )
    ,......]
```

**Parameters**

-   cte\_name: the name of the CTE. It must be unique in the `WITH` clause. The cte\_name identifier in any position of the query indicates the CTE.
-   cte\_query: a `SELECT` statement. Its result set is used to populate the CTE.

**Example**

```
INSERT OVERWRITE TABLE srcp PARTITION (p='abc')
SELECT * FROM (
    SELECT a.key, b.value
    FROM (
        SELECT * FROM src WHERE key IS NOT NULL    ) a
    JOIN (
        SELECT * FROM src2 WHERE value > 0    ) b
    ON a.key = b.key
) c
UNION ALL
SELECT * FROM (
    SELECT a.key, b.value
    FROM (
        SELECT * FROM src WHERE key IS NOT NULL    ) a
    LEFT OUTER JOIN (
        SELECT * FROM src3 WHERE value > 0    ) b
    ON a.key = b.key AND b.key IS NOT NULL
)d;
```

A `JOIN` clause is written on both sides of `UNION`. The query criteria of the left tables in the `JOIN` clauses are the same. You must repeat these statements if you want to write subqueries.

The preceding statements can be rewritten by using the CTE.

```
with 
  a as (select * from src where key is not null),
  b as (select  * from src2 where value>0),
  c as (select * from src3 where value>0),
  d as (select a.key,b.value from a join b on a.key=b.key),
  e as (select a.key,c.value from a left outer join c on a.key=c.key and c.key is not null)
insert overwrite table srcp partition (p='abc')
select * from d union all select * from e;
```

The subquery that corresponds to `a` needs to be rewritten only once, and then can be reused subsequently. You can specify multiple subqueries that can be repeatedly used like variables in the entire statement in the `WITH` clause of the CTE. Except subquery reuse, subqueries do not have to be repeatedly nested.

