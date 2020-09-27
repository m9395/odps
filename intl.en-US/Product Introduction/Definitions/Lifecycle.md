---
keyword: [lifecycle, reclaim a table]
---

# Lifecycle

This topic describes the lifecycle of a MaxCompute table.

The lifecycle of a MaxCompute table or a partition starts from the time of the last modification and ends if the table or partition remains unchanged within a specific period. After the lifecycle ends, MaxCompute automatically reclaims the table or partition. This specific period is the lifecycle of a table or its partition.

-   The value of a lifecycle is a positive integer. Unit: days.
-   If the data in a non-partitioned table remains unchanged within the lifecycle of the table, MaxCompute automatically executes a statement similar to DROP TABLE to reclaim the table. The lifecycle of a non-partitioned table starts from the time of the last modification, which is specified by LastDataModifiedTime.
-   Partitions of a table can be separately reclaimed. MaxCompute automatically reclaims the partitions whose data remains unchanged within the lifecycle. The lifecycle of a partition starts from the time of the last modification, which is specified by LastDataModifiedTime. Unlike non-partitioned tables, a partitioned table is not deleted even though all of its partitions have been reclaimed.

    **Note:**

    -   A lifecycle-based table scan is performed at a scheduled time each day to scan all the partitions. A partition can be reclaimed only when the period after the time of the last modification, which is specified by LastDataModifiedTime, exceeds the lifecycle.

        Assume that the lifecycle of a partitioned table is one day and data in one of its partitions was last modified at 15:00 on February 17, 2020. If MaxCompute scans this table before 15:00 of February 18, 2020, the table partition is not reclaimed because the period after the time of the last modification is less than its one-day lifecycle. If MaxCompute scans this table on February 19, 2020, the table partition is reclaimed because the period after the time of the last modification, which is specified by LastDataModifiedTime, exceeds its one-day lifecycle.

    -   The lifecycle feature allows MaxCompute to periodically reclaim a table or partition. However, whether MaxCompute immediately reclaims a table or partition when the period after the time of the last modification exceeds the lifecycle of the table or partition depends on whether the system is busy. Therefore, MaxCompute cannot ensure to reclaim a table or partition immediately when the period after the time of the last modification exceeds the lifecycle.
-   You can specify a lifecycle for tables, but not for partitions. The lifecycle specified for a partitioned table applies to all partitions of the partitioned table. You can specify a lifecycle when you create a table.
-   If no lifecycle is specified, the table or its partitions cannot be automatically reclaimed by MaxCompute based on the lifecycle.

For more information about how to specify and modify the lifecycle of a table or how to modify `LastDataModifiedTime` of a table, see [Table operations](/intl.en-US/Development/SQL/DDL SQL/Table operations.md).

