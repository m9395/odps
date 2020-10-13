---
keyword: [INSERT OVERWRITE, INSERT INTO]
---

# INSERT OVERWRITE and INSERT INTO

This topic describes how to use the INSERT OVERWRITE and INSERT INTO statements to update table data.

## Descriptions

**Syntax**

```
INSERT OVERWRITE|INTO TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)] [(col1,col2 ...)]
select_statement
FROM from_statement;
```

**Description**

If MaxCompute SQL is used to process data, `INSERT OVERWRITE and INSERT INTO` are used to save calculation results to a destination table.

-   INSERT INTO: inserts data into a table or into a partition of a table. However, you cannot use `INSERT INTO` to insert data into a hash clustering table. If you need to insert a small amount of test data, you can use this statement with [VALUES](/intl.en-US/Development/SQL/Insert Operation/VALUES.md).
-   INSERT OVERWRITE: clears the original data from a table, and then inserts data into the table or its partition. If you use `INSERT OVERWRITE`, you cannot specify the columns into which data is inserted. If you need to specify the columns, use `INSERT INTO` instead.

    **Note:**

    -   The syntax of `INSERT` statements in MaxCompute differs from that of the commonly used `INSERT` statements in MySQL or Oracle. To execute `INSERT OVERWRITE or INSERT INTO` in MaxCompute, you must add keyword `TABLE` before `TABLENAME` in the statement.
    -   If you repeat the `INSERT OVERWRITE` operation on a partition, the volume of the data queried by using `DESCRIBE` may vary. The reason is that the logic used to split a file changes after you use `SELECT` to extract the data from a partition of a table and then use `INSERT OVERWRITE` to insert the data into the same partition. The total data length remains unchanged after the `INSERT OVERWRITE` operation. As a result, the storage fees remain unchanged.

**Parameters**

-   tablename: the name of the destination table into which you want to insert data.
-   PARTITION \(partcol1=val1, partcol2=val2 ...\): the name of the partition into which you want to insert data. The value must be a constant. It cannot be an expression, such as a function.
-   select\_statement: the SELECT clause used to query the data you want to insert from the source table.

    **Note:**

    -   The mappings between the source and destination tables depend on the column sequence in the SELECT clause, instead of the mappings of column names between tables.
    -   If you insert data into a partition, partitioning columns cannot exist in the SELECT clause.
-   from\_statement: the FROM clause used to indicate a data source, such as a source table name.

**Examples**

-   Calculate the amount of sales of different regions listed in the `sale_detail` table and then insert the obtained data into the `sale_detail_insert` table.

    ```
    -- Create the sale_detail_insert destination table.
    create table sale_detail_insert like sale_detail;
    
    -- Add a partition to the destination table.
    alter table sale_detail_insert add partition(sale_date='2013', region='china');
    
    -- Extract the data from sale_detail and then insert it into sale_detail_insert.
    insert overwrite table sale_detail_insert partition (sale_date='2013', region='china')
      select shop_name, customer_id,total_price from sale_detail;
    ```

-   The mappings between the source and destination tables depend on the column sequence in the `SELECT` clause, instead of the mappings of column names between tables. Sample statements:

    ```
    insert overwrite table sale_detail_insert partition (sale_date='2013', region='china')
        select customer_id, shop_name, total_price from sale_detail;                      
    ```

    If you create the `sale_detail_insert` table, the column sequence is defined as `shop_name string, customer_id string, and then total_price bigint`. However, you insert the data from `sale_detail` to `sale_detail_insert` based on the sequence of `customer_id, shop_name, and then total_price`. As a result, the data in the `sale_detail.customer_id` column is inserted into the `sale_detail_insert.shop_name` column, and the data in the `sale_detail.shop_name` column is inserted into the `sale_detail_insert.customer_id` column.

-   If you insert data into a partition, partitioning columns cannot be included in the `SELECT` clause. In the following example, an error is returned because `sale_date,region` is a partitioning column that cannot be included in an INSERT statement for a static partition.

    ```
    insert overwrite table sale_detail_insert partition (sale_date='2013', region='china')
       select shop_name, customer_id, total_price, sale_date, region  from sale_detail;
    ```

-   `partition` is not an expression but a constant. The following example shows an incorrect usage.

    ```
    insert overwrite table sale_detail_insert partition (sale_date=datepart('2016-09-18 01:10:00', 'yyyy') , region='china')
       select shop_name, customer_id, total_price from sale_detail;
    ```


## Precautions for dynamic partitions

To update table data to a dynamic partition, note the following points:

-   If you perform the `insert into partition` operation but the specified partition does not exist, a partition is automatically created.
-   If you perform multiple `insert into partition` operations at the same time but the specified partitions do not exist, only one partition is automatically created.
-   If concurrent `insert into partition` operations are performed, you must create a partition in advance to avoid issues caused by concurrent operations.

For more information about dynamic partitions, see [Insert data in dynamic partition mode \(DYNAMIC PARTITION\)](/intl.en-US/Development/SQL/Insert Operation/Insert data in dynamic partition mode (DYNAMIC PARTITION).md).

