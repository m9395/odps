---
keyword: [keyword, data type conversion, partitioned table, UNION ALL]
---

# MaxCompute SQL overview

This topic describes MaxCompute SQL keywords, data type conversions, partitioned tables, UNION ALL operations, and limits on MaxCompute SQL.

## Introduction

MaxCompute SQL is used for offline batch computing and computing scenarios that involve gigabytes, terabytes, or exabytes of data. After you submit a MaxCompute job, it is scheduled in dozens of seconds or several minutes. Therefore, MaxCompute is suitable for batch jobs that process large volumes of data. It is not suitable for frontend business systems that process several thousand or tens of thousands of transactions per second.

The MaxCompute SQL syntax is similar to the SQL syntax. The MaxCompute SQL syntax is a subset of the standard ANSI SQL-92 syntax and extends the standard syntax. MaxCompute projects lack many database features, such as transactions, primary key constraints, and indexes. Therefore, MaxCompute projects cannot be regarded as databases. The maximum size of each SQL statement supported by MaxCompute is 2 MB.

## Keywords

The keywords of SQL statements are reserved words in MaxCompute. If you use keywords to name tables, columns, or partitions, you must escape the keywords by using a pair of backticks \(````\). Otherwise, an error is returned. Reserved words are not case-sensitive. The following section lists common reserved words. For a complete list of reserved words, see [Reserved words and keywords](/intl.en-US/Development/SQL/Appendix/Reserved words and keywords.md).

```
%    &    &&    (    )    *    +  
 -    .    /    ;    <    <=    <>  
 =    >    >=    ?    ADD    ALL    ALTER  
 AND  AS    ASC    BETWEEN    BIGINT    BOOLEAN    BY  
 CASE CAST  COLUMN    COMMENT    CREATE    DESC    DISTINCT  
 DISTRIBUTE    DOUBLE    DROP    ELSE    FALSE    FROM    FULL  
 GROUP    IF    IN    INSERT    INTO    IS    JOIN  
 LEFT    LIFECYCLE    LIKE    LIMIT    MAPJOIN    NOT    NULL  
 ON    OR    ORDER    OUTER    OVERWRITE    PARTITION    RENAME  
 REPLACE    RIGHT    RLIKE    SELECT    SORT    STRING    TABLE  
 THEN    TOUCH    TRUE    UNION    VIEW    WHEN    WHERE
```

## Data type conversions

MaxCompute SQL supports two modes of data type conversions: explicit conversion and implicit conversion. For more information, see [Data type conversion](/intl.en-US/Development/SQL/Data type conversion.md).

-   An explicit conversion uses CAST to convert the data type of a value.
-   An implicit conversion is automatically performed by MaxCompute based on the context and a set of predefined rules. The implicit conversion scope includes various operators and built-in functions.

## Partitioned tables

MaxCompute SQL supports partitioned tables. You can use partitioned tables to improve SQL query performance and reduce your costs. For more information about partitions, see [Partition](/intl.en-US/Product Introduction/Definitions/Partition.md).

## UNION ALL

Data types of content in columns, the number of columns, and column names used in a UNION ALL operation must be the same. Otherwise, an error is returned.

## Limits

For limits on MaxCompute SQL, see [MaxCompute SQL limits](/intl.en-US/Development/SQL/MaxCompute SQL limits.md). For more information about DDL and DML statements that are not supported by MaxCompute, see [Differences with other SQL statements](/intl.en-US/Development/SQL/Differences with other SQL statements.md).

The following limits also apply:

-   For more information about the limits of scalar subqueries, see [Scalar subquery](/intl.en-US/Development/SQL/Select Operation/Subquery.md).
-   If you execute an INSERT statement to insert values, the values must be constants.
-   MaxCompute allows you to perform a UNION ALL or UNION operation on a maximum of 256 tables.
-   The size of small tables on which you perform a MAPJOIN operation cannot exceed 512 MB.
-   Due to the adjustment made by the International Organization for Standardization \(ISO\) on the UTC+8 time zone, the DATETIME data obtained by executing related SQL statements is different from the actual time in some periods. For the DATETIME data between the year 1900 and the year 1928, the time difference is 352 seconds. For the DATETIME data before the year 1900, the time difference is 9 seconds.

