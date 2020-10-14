---
keyword: [copy table data, CLONE TABLE]
---

# CLONE TABLE

This topic describes how to use the CLONE TABLE statement to copy data from one table to another. CLONE TABLE improves the efficiency of data migration.

## Limits

-   The schema of the destination table must be compatible with that of the source table.
-   The CLONE TABLE operation supports both partitioned and non-partitioned tables.
-   If a destination table is created before data copy, data in a maximum of 10,000 partitions can be copied at a time.
-   If a destination table is not created before data copy, the number of partitions from which you can copy data at a time is unlimited. This guarantees atomicity.
-   You cannot perform the CLONE TABLE operation for more than seven times in the same non-partitioned table or in the same partition of a partitioned table.

## Syntax

```
CLONE TABLE <[src_project_name.]src_table_name> [PARTITION(spec), ...]
 TO <[dest_project_name.]desc_table_name> [IF EXISTS (OVERWRITE | IGNORE)] ;
```

## Description

This statement is used to copy data from src\_table\_name to desc\_table\_name.

**Note:** After you copy data to a destination table, we recommend that you verify the data to ensure data accuracy. For example, you can run the `select count` command to view the number of rows in a table or run the `desc` command to view the table size.

## Parameters

-   src\_table\_name: the name of the source table.
-   src\_project\_name: the name of the project to which the source table belongs. If this parameter is not specified, the current project name is used by default.
-   desc\_table\_name: the name of the destination table.
    -   If a destination table is not created before data copy, a table is created by using the CREATE TABLE LIKE statement when you perform the CLONE TABLE operation.
    -   If a destination table is created before data copy and IF EXISTS OVERWRITE is used, data in the corresponding partitions of the destination table is overwritten.
    -   If a destination table is created before data copy and IF EXISTS IGNORE is used, existing partitions in the table are skipped and data in these partitions is not overwritten.
-   dest\_project\_name: the name of the project to which the destination table belongs. If this parameter is not specified, the current project name is used by default.

## Example

Assume that partitioned table srcpart\_copy and non-partitioned table src\_copy are source tables. The following example shows the table metadata.

```
odps@ multi>read srcpart_copy;
+------------+------------+------------+------------+
| key        | value      | ds         | hr         |
+------------+------------+------------+------------+
| 1          | ok49       | 2008-04-09 | 11         |
| 1          | ok48       | 2008-04-08 | 12         |
+------------+------------+------------+------------+
odps@ multi>read src_copy;
+------------+------------+
| key        | value      |
+------------+------------+
| 1          | ok         |
+------------+------------+
```

-   Copy all data from src\_copy to destination table src\_clone.

    ```
    odps@ multi>clone table src_copy to src_clone;
    ID = 2019102303024544g2540cdv2
    OK
    //Query information in src_clone after data copy and verify the data.
    odps@ multi>select * from src_clone;
    ```

-   Copy data from a specific partition of srcpart\_copy to srcpart\_clone.

    ```
    odps@ multi>clone table srcpart_copy partition(ds="2008-04-09", hr='11') to srcpart_clone IF EXISTS OVERWRITE;
    ID = 20191023030534986g4540cdv2
    OK
    //Query information in srcpart_clone after data copy and verify the data.
    odps@ multi>select * from srcpart_clone;
    ```

-   Copy all data from srcpart\_copy to srcpart\_clone and skip the partitions that already exist in the destination table.

    ```
    odps@ multi>clone table srcpart_copy to srcpart_clone IF EXISTS IGNORE;
    ID = 20191023030619196g5540cdv2
    OK
    //Query information in srcpart_clone after data copy and verify the data.
    odps@ multi>select * from srcpart_clone;
    ```

-   Copy all data from srcpart\_copy to destination table srcpart\_clone2.

    ```
    odps@ multi>clone table srcpart_copy to srcpart_clone2;
    ID = 20191023030825186g6540cdv2
    OK
    //Query information in srcpart_clone2 after data copy and verify the data.
    odps@ multi>select * from srcpart_clone2;
    ```


