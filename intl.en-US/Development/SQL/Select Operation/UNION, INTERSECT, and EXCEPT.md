---
keyword: [UNION, INTERSECT, EXCEPT]
---

# UNION, INTERSECT, and EXCEPT

This topic describes the SQL statements that use the UNION, INTERSECT, and EXCEPT set operators, such as UNION, UNION ALL, UNION DISTINCT, INTERSECT, INTERSECT ALL, INTERSECT DISTINCT, EXCEPT, EXCEPT ALL, and EXCEPT DISTINCT.

## Syntax

```
select_statement UNION ALL select_statement;
select_statement UNION [DISTINCT] select_statement;
select_statement INTERSECT ALL select_statement;
select_statement INTERSECT [DISTINCT] select_statement;
select_statement EXCEPT ALL select_statement;
select_statement EXCEPT [DISTINCT] select_statement;
select_statement MINUS ALL select_statement;
select_statement MINUS [DISTINCT] select_statement;
```

## Description

-   UNION

    Usage: returns the union of two datasets. UNION combines the two datasets into one dataset.

    -   If UNION ALL is used, all records of the two datasets are returned.

        Example:

        ```
        SELECT * FROM VALUES (1, 2), (1, 2), (3, 4) t(a, b) 
        UNION ALL 
        SELECT * FROM VALUES (1, 2), (1, 4) t(a, b);
        ```

        Result:

        ```
        +------------+------------+
        | a          | b          |
        +------------+------------+
        | 1          | 2          |
        | 1          | 4          |
        | 1          | 2          |
        | 1          | 2          |
        | 3          | 4          |
        +------------+------------+
        ```

    -   If multiple UNION ALL clauses exist, enclose the UNION ALL clauses in parentheses \(\( \)\) to specify the order of precedence.

        ```
        SELECT * FROM src UNION ALL (SELECT * FROM src2 UNION ALL SELECT * FROM src3);
        ```

    -   If UNION is used, duplicate records are not included in the returned records. The usage of UNION is similar to that of UNION DISTINCT.

        Example:

        ```
        SELECT * FROM VALUES (1, 2), (1, 2), (3, 4) t(a, b) 
        UNION 
        SELECT * FROM VALUES (1, 2), (1, 4) t(a, b);
        -- Equivalent to the following statement:
        SELECT DISTINCT * FROM (<the result of UNION ALL>)t;
        ```

        Result:

        ```
        +------------+------------+
        | a          | b          |
        +------------+------------+
        | 1          | 2          |
        | 1          | 4          |
        | 3          | 4          |
        +------------+------------+
        ```

    -   Assume that a UNION statement is followed by a `CLUSTER BY`, a `DISTRIBUTE BY`, a `SORT BY`, an `ORDER BY`, or a `LIMIT` clause. If `set odps.sql.type.system.odps2` is set to false, the clause works only on the result of the `SELECT` statement after the UNION operator. If `set odps.sql.type.system.odps2` is set to true, the clause works on the result of the UNION operation. Example:

        ```
        set odps.sql.type.system.odps2=true;
        SELECT explode(array(3, 1)) AS (a) UNION ALL SELECT explode(array(0, 4, 2)) AS (a) ORDER BY a LIMIT 3;
        ```

        Result:

        ```
        +------+
        | a    |
        +------+
        | 0    |
        | 1    |
        | 2    |
        +------+
        ```

-   INTERSECT

    Usage: returns the intersection of two datasets, that is, data contained in both datasets.

    Examples

    -   Example of an `INTERSECT ALL` statement:

        ```
        SELECT * FROM VALUES (1, 2), (1, 2), (3, 4), (5, 6) t(a, b) 
        INTERSECT ALL 
        SELECT * FROM VALUES (1, 2), (1, 2), (3, 4), (5, 7) t(a, b);
        ```

        Result:

        ```
        +------------+------------+
        | a          | b          |
        +------------+------------+
        | 1          | 2          |
        | 1          | 2          |
        | 3          | 4          |
        +------------+------------+
        ```

    -   Example of an `INTERSECT DISTINCT` statement:

        ```
        SELECT * FROM VALUES (1, 2), (1, 2), (3, 4), (5, 6) t(a, b) 
        INTERSECT DISTINCT
        SELECT * FROM VALUES (1, 2), (1, 2), (3, 4), (5, 7) t(a, b);
        ```

        Result: The returned result is equivalent to that returned by executing the `SELECT DISTINCT * FROM (< result of INTERSECT ALL >) t;` statement.

        ```
        +------------+------------+
        | a          | b          |
        +------------+------------+
        | 1          | 2          |
        | 3          | 4          |
        +------------+------------+
        ```

-   EXCEPT

    Usage: returns distinct values from the first dataset that are not contained in the second dataset.

    Examples

    -   Example of an `EXCEPT ALL` statement

        ```
        SELECT * FROM VALUES (1, 2), (1, 2), (3, 4), (3, 4), (5, 6), (7, 8) t(a, b) 
        EXCEPT ALL 
        SELECT * FROM VALUES (3, 4), (5, 6), (5, 6), (9, 10) t(a, b);
        ```

        Result:

        ```
        +------------+------------+
        | a          | b          |
        +------------+------------+
        | 1          | 2          |
        | 1          | 2          |
        | 3          | 4          |
        | 7          | 8          |
        +------------+------------+
        ```

    -   Example of an `EXCEPT DISTINCT` statement:

        ```
        SELECT * FROM VALUES (1, 2), (1, 2), (3, 4), (3, 4), (5, 6), (7, 8) t(a, b) 
        EXCEPT
        SELECT * FROM VALUES (3, 4), (5, 6), (5, 6), (9, 10) t(a, b);
        ```

        Result: The returned result is similar to that returned by executing the `SELECT DISTINCT * FROM left_branch EXCEPT ALL SELECT DISTINCT * FROM right_branch;` statement.

        ```
        +------------+------------+
        | a          | b          |
        +------------+------------+
        | 1          | 2          |
        | 7          | 8          |
        +------------+------------+
        ```

-   MINUS

    Usage: The usage of MINUS is similar to that of `EXCEPT`.


## Remarks

-   The sequence of data returned in a set operation may not be ordered.
-   The left and right branches in a set operation must have the same number of columns. In addition, if data types in the left and right branches are not the same, the data types may be implicitly converted. For compatibility reasons, MaxCompute disables the implicit conversion between data of the STRING type and data of other types in set operations.
-   MaxCompute allows a maximum of 256 branches in a set operation. An error is reported if the number of branches exceeds 256.

