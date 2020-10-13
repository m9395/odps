---
keyword: [partition, column]
---

# Partition and column operations

This topic describes how to execute statements to add or delete partitions and add or modify columns in MaxCompute tables.

## Add a partition

**Syntax**

```
ALTER TABLE table_name ADD [IF NOT EXISTS] PARTITION partition_spec;
partition_spec:(partition_col1 = partition_col_value1, partition_col2 = partiton_col_value2, ...)
```

**Description**

This statement is used to add a partition to an existing table. This statement can add only a partition instead of a partition field.

**Note:**

-   You can create a maximum of 60,000 partitions in a single table in MaxCompute.
-   To add a partition to a table that has multi-level partitions, you must specify all partition column values.

**Parameters**

-   table\_name: the name of the destination table to which you want to add a partition.
-   IF NOT EXISTS: If IF NOT EXISTS is not specified and a partition with an identical name already exists, an error is returned.
-   partition\_spec: the name of the new partition. Partition names must be in lowercase.

**Examples**

```
-- Add a partition to the sale_detail table to store the sales records of Hangzhou in December 2013.
alter table sale_detail add if not exists partition (sale_date='201312', region='hangzhou');

-- Add a partition to the sale_detail table to store the sales records of Shanghai in December 2013.
alter table sale_detail add if not exists partition (sale_date='201312', region='shanghai');

-- Add a partition to the sale_detail table to store the sales records on October 11, 2011. The addition fails and an error is returned.
alter table sale_detail add if not exists partition(sale_date='20111011');

-- Add a partition to the sale_detail table to store the sales records of Shanghai. The addition fails and an error is returned.
alter table sale_detail add if not exists partition(region='shanghai');
```

## Drop a partition

**Syntax**

```
ALTER TABLE table_name DROP [IF EXISTS] PARTITION partition_spec;
partition_spec:(partition_col1 = partition_col_value1, partition_col2 = partiton_col_value2, ...)
```

**Parameters**

-   table\_name: the name of the destination table to which you want to drop a partition.
-   IF EXISTS: If IF EXISTS is not specified and the partition does not exist, an error is returned.
-   partition\_spec: the name of the partition you want to delete. Partition names must be in lowercase.

**Example**

```
-- Delete the sales records of Hangzhou in December 2013 from the sale_detail table.
alter table sale_detail drop if exists partition(sale_date='201312',region='hangzhou'); 
```

## Add columns

**Syntax**

-   Add columns to a table

    ```
    ALTER TABLE table_name ADD COLUMNS (col_name1 type1,col_name2 type2...) ;
    ```

-   Add columns and comments at the same time

    ```
    ALTER TABLE table_name ADD COLUMNS (col_name1 type1 comment 'XXX',col_name2 type2 comment 'XXX');
    ```


**Parameters**

-   table\_name: the name of the destination table to which you want to add a column. You cannot specify the order of a new column in the table. The new column is added as the last column by default.
-   col\_name, type ,comment: the name, data type, and comment of the column you want to add.

## Modify column name

**Syntax**

```
ALTER TABLE table_name CHANGE COLUMN old_col_name RENAME TO new_col_name;
```

**Parameters**

-   table\_name: the name of the table whose column name you want to modify.
-   old\_col\_name: the name of the column you want to modify. The column specified by `old_col_name` must already exist in the table.
-   new\_col\_name: the name of the new column. The table does not contain a column named `new_col_name`.

## Modify the comment of a column

**Syntax**

```
ALTER TABLE table_name CHANGE COLUMN col_name COMMENT comment_string;
```

**Parameters**

-   table\_name: the name of the table for which you want to modify the column comment.
-   col\_name: the name of the column for which you want to modify comment. The column specified by `col_name`.must already exist in the table.
-   comment\_string: the information about the new comment. The maximum size of a comment is 1024 bytes.

## Modify column names and column comments simultaneously

**Syntax**

```
ALTER TABLE table_name CHANGE COLUMN old_col_name new_col_name column_type COMMENT column_comment;
```

**Parameters**

-   table\_name: the name of the table for which you want to modify the column names and comments.
-   old\_col\_name: the name of the column for which you want to modify the comment. The column specified by `old_col_name` must already exist in the table.
-   new\_col\_name: the new column name. The table does not contain a column named `new_col_name`.
-   column\_type: the data type of the column.
-   column\_comment: the information about the new comment. The maximum size of a comment is 1024 bytes.

## Modify LastDataModifiedTime of a table or partition

MaxCompute SQL allows you to execute the `TOUCH` statement to modify `LastDataModifiedTime` of a partition. This statement modifies `LastDataModifiedTime` of a partition to the current time. In this case, MaxCompute considers that the table and partition data have changed, and the new lifecycle of the table starts from the last update time.

**Syntax**

```
ALTER TABLE table_name TOUCH PARTITION(partition_col='partition_col_value', ...) ;
```

**Parameters**

-   table\_name: the name of the table whose LastDataModifiedTime you want to modify. If the table does not exist, an error is returned.
-   partition\_col='partition\_col\_value': the name and column value of the partition whose LastDataModifiedTime you want to modify. If the specified partition and its column value do not exist, an error is returned.

## Modify the partition column value

MaxCompute SQL allows you to execute the `RENAME` statement to change the partition column values of a table.

**Syntax**

```
ALTER TABLE table_name PARTITION (partition_col1 = partition_col_value1, partition_col2 = partiton_col_value2, ...) 
RENAME TO PARTITION (partition_col1 = partition_col_newvalue1, partition_col2 = partiton_col_newvalue2, ...) ;
```

**Parameters**

-   table\_name: the name of the table whose partition column value you want to change. If the table does not exist, an error is returned.
-   partition\_col = partition\_col\_value: the name and column value of the partition in a table.
-   partition\_col = partition\_col\_newvalue: the name and new column value of the partition in a table.

    **Note:**

    -   This statement can only change partition column values instead of partition column names.
    -   To change one or more partition column values in a table that has multi-level partitions, you must specify the partition column values of partitions at each level.

## Merge partitions

MaxCompute SQL allows you to execute the `MERGE PARTITION` statement to merge multiple partitions of a table into one partition. This statement deletes the dimension information about the merged partitions and transfers data to the specified partition.

-   **Syntax**

    ```
    ALTER TABLE <tableName> MERGE [IF EXISTS] PARTITION(<predicate>) [, PARTITION(<predicate2>) ...] OVERWRITE PARTITION(<fullPartitionSpec>) [PURGE];
    ```

-   **Syntax description**
    -   If the partition does not exist and `IF EXISTS` is not specified, an error is returned.
    -   If no partitions meet merge conditions after `IF EXISTS` is specified, a new partition is not generated.
    -   If the source data is modified by executing the INSERT, RENAME, and DROP statements concurrently, an error is returned even if `IF EXISTS` is specified.
    -   You cannot merge partitions of an external table or a table shard. If you merge partitions of a clustered table, the clustered attribute of the table is removed.
    -   You can merge a maximum of 4,000 partitions at a time.
-   **Example**

    ```
    -- Partitions and data of the table:
    odps@ jet_zwz>list partitions intpstringstringstring;
    
    ds=20181101/hh=00/mm=00
    ds=20181101/hh=00/mm=10
    ds=20181101/hh=10/mm=00
    ds=20181101/hh=10/mm=10
    
    OK
    odps@ jet_zwz>read intpstringstringstring;
    +------------+------------+------------+------------+
    | value      | ds         | hh         | mm         |
    +------------+------------+------------+------------+
    | 1          | 20181101   | 00         | 00         |
    | 1          | 20181101   | 00         | 10         |
    | 1          | 20181101   | 10         | 00         |
    | 1          | 20181101   | 10         | 10         |
    +------------+------------+------------+------------+
    
    -- Merge all partitions that meet the hh='00' condition into the ds='20181101', hh='00', mm='00' partition:
    odps@ jet_zwz>alter table intpstringstringstring merge partition(hh='00') overwrite partition(ds='20181101', hh='00', mm='00');
    
    ID = 20190404025755844g80qwa7a
    OK
    
    -- View the merged partitions.
    odps@ jet_zwz>list partitions intpstringstringstring;
    
    ds=20181101/hh=00/mm=00
    ds=20181101/hh=10/mm=00
    ds=20181101/hh=10/mm=10
    
    OK
    
    -- Data in two partitions that meet the hh='00' condition is merged into the ds='20181101', hh='00', mm='00' partition.
    odps@ jet_zwz>read intpstringstringstring;
    +------------+------------+------------+------------+
    | value      | ds         | hh         | mm         |
    +------------+------------+------------+------------+
    | 1          | 20181101   | 00         | 00         |
    | 1          | 20181101   | 00         | 00         |
    | 1          | 20181101   | 10         | 00         |
    | 1          | 20181101   | 10         | 10         |
    +------------+------------+------------+------------+
                            
    ```

    When you merge partitions, you can specify multiple predicate conditions. For example, you can execute the following statement to merge all the remaining partitions to the ds=20181101/hh=00/mm=00 partition:

    ```
    odps@ jet_zwz>alter table intpstringstringstring merge if exists partition(ds='20181101', hh='00', mm='00'), partition(ds='20181101', hh='10', mm='00'),  partition(ds='20181101', hh='10', mm='10') overwrite partition(ds='20181101', hh='00', mm='00') purge;
    
    ID = 20190404034632854g431sqzt2
    OK
    
    odps@ jet_zwz>show partitions intpstringstringstring;
    
    ds=20181101/hh=00/mm=00
    
    OK
    ```


