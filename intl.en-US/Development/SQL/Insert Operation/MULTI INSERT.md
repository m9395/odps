---
keyword: MULTI INSERT
---

# MULTI INSERT

This topic describes how to use one SQL statement to insert data into different destination tables or partitions to generate multiple outputs.

**Syntax**

```
FROM from_statement
INSERT OVERWRITE | INTO TABLE tablename1 [PARTITION (partcol1=val1, partcol2=val2 ...)]
select_statement1 [FROM from_statement]
[INSERT OVERWRITE | INTO TABLE tablename2 [PARTITION (partcol1=val3, partcol2=val4 ...)]
select_statement2 [FROM from_statement]]
```

**Description**

This statement is used to insert data into different destination tables or partitions to generate multiple outputs.

**Note:**

-   One SQL statement can generate up to 255 outputs. If the number of outputs exceeds 255, an error is returned.
-   In a `MULTI INSERT` task:
    -   Destination partitions must be unique in a partitioned table.
    -   Non-partitioned tables must be unique.
-   The `INSERT OVERWRITE` and `INSERT INTO` operations cannot be performed simultaneously on different partitions of a table. Otherwise, an error is returned.

**Parameters**

-   from\_statement: the FROM clause used to indicate the data source. For example, the value can be a source table name.
-   PARTITION \(partcol1=val1, partcol2=val2 ...\): the name of the partition into which you want to insert data. The value must be a constant. It cannot be an expression, such as a function.
-   tablename1 and tablename2: the names of the destination tables into which you want to insert data.
-   select\_statement: the SELECT clause used to query the data you want to insert from the source table.

**Example**

-   Insert the sale records of the China region in 2010 and 2011 from the sale\_detail table into the sale\_detail\_multi table.

    ```
    -- Create a destination table named sale_detail_multi.
    create table sale_detail_multi like sale_detail;
    
    -- Insert data from sale_detail into sale_detail_multi.
    set odps.sql.allow.fullscan=true; // Enable the full table scan function, which is only valid for this session.
    from sale_detail
    insert overwrite table sale_detail_multi partition (sale_date='2010', region='china' ) 
    select shop_name, customer_id, total_price 
    insert overwrite table sale_detail_multi partition (sale_date='2011', region='china' )
    select shop_name, customer_id, total_price ;
    ```

-   If a partition appears multiple times, an error is returned.

    ```
    from sale_detail
    insert overwrite table sale_detail_multi partition (sale_date='2010', region='china' )
    select shop_name, customer_id, total_price
    insert overwrite table sale_detail_multi partition (sale_date='2010', region='china' )
    select shop_name, customer_id, total_price;
    ```

-   If the INSERT OVERWRITE and INSERT INTO operations are performed simultaneously on different partitions of a table, an error is returned.

    ```
    from sale_detail
    insert overwrite table sale_detail_multi partition (sale_date='2010', region='china' )
    select shop_name, customer_id, total_price
    insert into table sale_detail_multi partition (sale_date='2011', region='china' )
    select shop_name, customer_id, total_price;
    ```


