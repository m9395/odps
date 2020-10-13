---
keyword: process OSS data stored in open source formats
---

# Process OSS data stored in open source formats

This topic describes how to use the MaxCompute unstructured framework to process Object Storage Service \(OSS\) data that is stored in open source formats. The formats include ORC, PARQUET, SEQUENCEFILE, RCFILE, AVRO, and TEXTFILE.

You can create, search, configure, and process external tables in the DataWorks console. You can also query and analyze data in external tables. For more information, see [External table]().

For data that is stored in open source formats, the unstructured framework calls the implementation method provided by an open source community to parse the data. The method can be seamlessly integrated with MaxCompute to read and parse data stored in open source formats.

**Note:** Before you process the OSS data stored in open source formats, you must use STS to authorize access to OSS first. For more information, see [STS authorization](/intl.en-US/Development/External table/STS authorization.md).

## Syntax to create an external table

The MaxCompute unstructured framework uses external tables to access data stored in various formats. The following code describes the syntax that is used to create an external table. Then, the table can be used to access OSS data that is stored in open source formats.

```
DROP TABLE [IF EXISTS] <external_table>;
CREATE EXTERNAL TABLE [IF NOT EXISTS] <external_table>
(<column schemas>)
[PARTITIONED BY (partition column schemas)]
[ROW FORMAT SERDE '<serde class>'
  [WITH SERDEPROPERTIES ('odps.properties.rolearn'='${roleran}' [,'name2'='value2',...])]
]
STORED AS <file format>
LOCATION 'oss://${endpoint}/${bucket}/${userfilePath}/';
```

The syntax is similar to that in Hive. Note the following points:

-   column schemas: defines the columns of the external table. The column definition must be the same as the definition of data stored in OSS.
-   ROW FORMAT SERDE: This clause is required only when you use special formats, such as TEXTFILE.
-   WITH SERDEPROPERTIES: If STS authorization is performed on OSS, this clause is required to set the odps.properties.rolearn property. The property is the Alibaba Cloud Resource Name \(ARN\) of the RAM role. You can configure STORED AS <file format\> and <serde class\> to specify the file format. The ORC format is used in the following example:

    ```
    CREATE EXTERNAL TABLE [IF NOT EXISTS] <external_table>
    (<column schemas>)
    [PARTITIONED BY (partition column schemas)]
    ROW FORMAT SERDE 'org.apache.hadoop.hive.ql.io.orc.OrcSerde'
    WITH SERDEPROPERTIES ('odps.properties.rolearn'='${roleran}'
    STORED AS ORC
    LOCATION 'oss://${endpoint}/${bucket}/${userfilePath}/'
    ```

    **Note:** If STS authorization is not performed, you do not need to set the odps.properties.rolearn property. You only need to specify the `AccessKey ID` and `AccessKey secret` for the Location clause in plaintext.

-   STORED AS: It is unique for reading data that is stored in open source formats. It is different from the STORED BY clause that is used to [create a standard unstructured external table](/intl.en-US/Development/External table/Access OSS data by using the built-in extractor.md).

    STORED AS is followed by the file format, such as ORC, PARQUET, RCFILE, SEQUENCEFILE, or TEXTFILE. In the STORED AS clause, the size of a file cannot be greater than 3 GB. If the size of a file exceeds 3 GB, split the file.

    Mappings between file formats and SerDe classes:

    -   SEQUENCEFILE: org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe
    -   TEXTFILE: org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe
    -   RCFILE: org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe
    -   ORC: org.apache.hadoop.hive.ql.io.orc.OrcSerde
    -   ORCFILE: rg.apache.hadoop.hive.ql.io.orc.OrcSerde
    -   PARQUET: org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe
    -   AVRO: org.apache.hadoop.hive.serde2.avro.AvroSerDe
-   LOCATION: If the OSS service is associated, configure the AccessKey ID and AccessKey secret in plaintext. Example:

    ```
    LOCATION 'oss://${accessKeyId}:${accessKeySecret}@${endpoint}/${bucket}/${userPath}/'
    ```

    You cannot use external tables to access OSS data by using the public endpoint of OSS.


## Create an external table based on a PARQUET object in an OSS bucket

Assume that some objects are stored in an OSS directory, these objects are stored in the PARQUET format, and each object contains 16 columns: four BIGINT-type columns, four DOUBLE-type columns, and eight STRING-type columns. The following DDL statement is used to create a table:

```
CREATE EXTERNAL TABLE tpch_lineitem_parquet
(
  l_orderkey bigint,
  l_partkey bigint,
  l_suppkey bigint,
  l_linenumber bigint,
  l_quantity double,
  l_extendedprice double,
  l_discount double,
  l_tax double,
  l_returnflag string,
  l_linestatus string,
  l_shipdate string,
  l_commitdate string,
  l_receiptdate string,
  l_shipinstruct string,
  l_shipmode string,
  l_comment string
)
STORED AS PARQUET
LOCATION 'oss://${accessKeyId}:${accessKeySecret}@oss-cn-hangzhou-zmf.aliyuncs.com/bucket/parquet_data/';
```

Data stored in the PARQUET format is not compressed by default. If you want to compress such data in MaxCompute, set `set odps.sql.hive.compatible` to true. The format of data after compression varies with the value of the TBLPROPERTIES parameter.

-   `'mcfed.parquet.compression'='SNAPPY'`: The format is SNAPPY.
-   `'mcfed.parquet.compression'='GZIP'`. The format is GZIP.

## Create an external table based on a TEXTFILE object in an OSS bucket

-   If the data is in the JSON format and stored as TEXTFILE files in multiple directories in OSS and the file storage and naming comply with the same rules, you can use a MaxCompute partitioned table to associate the data. The following DDL statement is used to create a partitioned table:

    ```
    CREATE EXTERNAL TABLE tpch_lineitem_textfile
    (
      l_orderkey bigint,
      l_partkey bigint,
      l_suppkey bigint,
      l_linenumber bigint,
      l_quantity double,
      l_extendedprice double,
      l_discount double,
      l_tax double,
      l_returnflag string,
      l_linestatus string,
      l_shipdate string,
      l_commitdate string,
      l_receiptdate string,
      l_shipinstruct string,
      l_shipmode string,
      l_comment string
    )
    PARTITIONED BY (ds string)
    ROW FORMAT serde 'org.apache.hive.hcatalog.data.JsonSerDe'
    STORED AS TEXTFILE
    LOCATION 'oss://${accessKeyId}:${accessKeySecret}@oss-cn-hangzhou-zmf.aliyuncs.com/bucket/text_data/';
    ```

-   The subdirectories under the OSS table directory are organized by partition name. Example:

    ```
    oss://${accessKeyId}:${accessKeySecret}@oss-cn-hangzhou-zmf.aliyuncs.com/bucket/text_data/ds=20170102/'
    oss://${accessKeyId}:${accessKeySecret}@oss-cn-hangzhou-zmf.aliyuncs.com/bucket/text_data/ds=20170103/'
    ...
    ```

    In this case, execute the following DDL statements to add partitions:

    ```
    ALTER TABLE tpch_lineitem_textfile ADD PARTITION(ds="20170102");
    ALTER TABLE tpch_lineitem_textfile ADD PARTITION(ds="20170103");
    ```

-   The subdirectories under the OSS table directory are not organized by partition name, or not in the table directory. Example:

    ```
    oss://${accessKeyId}:${accessKeySecret}@oss-cn-hangzhou-zmf.aliyuncs.com/bucket/text_data_20170102/;
    oss://${accessKeyId}:${accessKeySecret}@oss-cn-hangzhou-zmf.aliyuncs.com/bucket/text_data_20170103/;
    ...
    ```

    In this case, execute the following DDL statements to add partitions:

    ```
    ALTER TABLE tpch_lineitem_textfile ADD PARTITION(ds="20170102")
    LOCATION 'oss://${accessKeyId}:${accessKeySecret}@oss-cn-hangzhou-zmf.aliyuncs.com/bucket/text_data_20170102/';
    ALTER TABLE tpch_lineitem_textfile ADD PARTITION(ds="20170103")
    LOCATION 'oss://${accessKeyId}:${accessKeySecret}@oss-cn-hangzhou-zmf.aliyuncs.com/bucket/text_data_20170103/';
    ...
    ```

-   You cannot customize the `ROW FORMAT` clause when you create TEXT data tables. The following code describes the default value of the `ROW FORMAT` clause in the DDL statement:

    ```
    FIELDS TERMINATED BY : '\001'
    ESCAPED BY : '\'
    COLLECTION ITEMS TERMINATED BY : '\002'
    MAP KEYS TERMINATED BY : '\003'
    LINES TERMINATED BY : '\n'
    NULL DEFINED AS : '\N'
    ```


## Create an external table based on a CSV object in an OSS bucket

The following DDL statement is used to create an external table:

```
CREATE EXTERNAL TABLE [IF NOT EXISTS] 
(<column schemas>)
[PARTITIONED BY (partition column schemas)]
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
  WITH SERDEPROPERTIES
    ('separatorChar'=',', 'quoteChar'='"', 'escapeChar'='\\')
STORED AS TEXTFILE
LOCATION 'oss://${endpoint}/${bucket}/${userfilePath}/';
```

The following code describes the fields and their default values for the WITH SERDEPROPERTIES clause in the DDL statement:

```
separatorChar:','
quoteChar:'"'
escapeChar:'\'
```

**Note:** Hive OpenCSVSerde supports only the STRING type. OpenCSVSerde is not a built-in SerDe. To execute the DML statement, you must set `set odps.sql.hive.compatible` to true.

## Create an external table based on a JSON object in an OSS bucket

The following DDL statement that supports the WITH SERDEPROPERTIES clause is used to create an external table:

```
CREATE EXTERNAL TABLE [IF NOT EXISTS] 
(<column schemas>)
[PARTITIONED BY (partition column schemas)]
ROW FORMAT SERDE 'org.apache.hive.hcatalog.data.JsonSerDe'
STORED AS TEXTFILE
LOCATION 'oss://${endpoint}/${bucket}/${userfilePath}/';
```

## Create an external table based on an ORC object in an OSS bucket

The following DDL statement is used to create an external table:

```
CREATE EXTERNAL TABLE [IF NOT EXISTS] 
(<column schemas>)
[PARTITIONED BY (partition column schemas)]
STORED AS ORC
LOCATION 'oss://${endpoint}/${bucket}/${userfilePath}/';
```

## Create an external table based on an AVRO object in an OSS bucket

The following DDL statement is used to create an external table:

```
CREATE EXTERNAL TABLE [IF NOT EXISTS] 
(<column schemas>)
[PARTITIONED BY (partition column schemas)]
STORED AS AVRO
LOCATION 'oss://${endpoint}/${bucket}/${userfilePath}/';
```

## Create an external table based on a SEQUENCEFILE object in an OSS bucket

The following DDL statement is used to create an external table:

```
CREATE EXTERNAL TABLE [IF NOT EXISTS] 
(<column schemas>)
[PARTITIONED BY (partition column schemas)]
STORED AS SEQUENCEFILE
LOCATION 'oss://${endpoint}/${bucket}/${userfilePath}/';
```

## Read and process OSS data stored in open source formats

The preceding DDL statements show that you only need to modify the value after STORED AS to create external tables for different object formats. This section describes how to use the tpch\_lineitem\_parquet external table that is created based on a PARQUET object. To use external tables that are created based on objects in different formats, you only need to set STORED AS to PARQUET, ORC, TEXTFILE, or RCFILE.

-   Read and process OSS data stored in open source formats

    After you create an external table and associate it with specific data, you can manage the created table as a standard MaxCompute table.

    ```
    SELECT l_returnflag, l_linestatus,
    SUM(l_extendedprice*(1-l_discount)) AS sum_disc_price,
    AVG(l_quantity) AS avg_qty,
    COUNT(*) AS count_order
    FROM tpch_lineitem_parquet
    WHERE l_shipdate <= '1998-09-02'
    GROUP BY l_returnflag, l_linestatus;
    ```

    tpch\_lineitem\_parquet is used as an internal table. However, the MaxCompute internal computing engine directly reads PARQUET data from OSS for processing.

    `odps.sql.hive.compatible` is set to false if you use the `ROW FORMAT` and `STORED AS` clauses to create the tpch\_lineitem\_textfile external partitioned table. To properly read data, you must set `set odps.sql.hive.compatible` to true. Otherwise, an error is returned.

    ```
    SELECT * FROM tpch_lineitem_textfile LIMIT 1;
    FAILED: ODPS-0123131:User defined function exception - Traceback:
    com.aliyun.odps.udf.UDFException: java.lang.ClassNotFoundException: com.aliyun.odps.hive.wrapper.HiveStorageHandlerWrapper
    // Add the following flag that is compatible with Hive:
    set odps.sql.hive.compatible=true;
    SELECT * FROM tpch_lineitem_textfile LIMIT 1;
    +------------+------------+------------+--------------+------------+-----------------+------------+------------+--------------+--------------+------------+--------------+---------------+----------------+------------+-----------+
    | l_orderkey | l_partkey  | l_suppkey  | l_linenumber | l_quantity | l_extendedprice | l_discount | l_tax      | l_returnflag | l_linestatus | l_shipdate | l_commitdate | l_receiptdate | l_shipinstruct | l_shipmode | l_comment |
    +------------+------------+------------+--------------+------------+-----------------+------------+------------+--------------+--------------+------------+--------------+---------------+----------------+------------+-----------+
    | 5640000001 | 174458698  | 9458733    | 1            | 14.0       | 23071.58        | 0.08       | 0.06       | N            | O            | 1998-01-26 | 1997-11-16   | 1998-02-18    | TAKE BACK RETURN | SHIP       | cuses nag silently. quick |
    +------------+------------+------------+--------------+------------+-----------------+------------+------------+--------------+--------------+------------+--------------+---------------+----------------+------------+-----------+
    ```

    **Note:**

    -   If you use an external table to read data, each data read operation triggers I/O operations on OSS data, and MaxCompute performance optimization for internal storage cannot take effect. As a result, the data reading performance may deteriorate. If you require repeated data computing or high computing efficiency, we recommend that you import data into MaxCompute for computing.
    -   If complex data types are used in SQL statements, such as CREATE, SELECT, and INSERT, you must set `set odps.sql.type.system.odps2` to true before the statements. Then, commit the statements for execution. For more information, see [Date types](/intl.en-US/Development/Data types/Data type editions.md).
-   Import data stored in open source formats into MaxCompute for computing

    Create a MaxCompute internal table named tpch\_lineitem\_internal that has the same schema as the external table. Import OSS data stored in open source formats into the newly created internal table. Save the data in the internal storage format.

    ```
    CREATE TABLE tpch_lineitem_internal LIKE tpch_lineitem_parquet;
    INSERT OVERWRITE TABLE tpch_lineitem_internal;
    SELECT * FROM tpch_lineitem_parquet;
    ```

    On the internal table, execute complex query statements based on the external table to improve computing performance.

    ```
    SELECT l_returnflag, l_linestatus,
    SUM(l_extendedprice*(1-l_discount)) AS sum_disc_price,
    AVG(l_quantity) AS avg_qty,
    COUNT(*) AS count_order
    FROM tpch_lineitem_internal
    WHERE l_shipdate <= '1998-09-02'
    GROUP BY l_returnflag, l_linestatus;
    ```


## FAQ

**Error**: `Inline data exceeds the maximum allowed size`.

**Cause**: OSS storage limits the size of each object. If the size of an object exceeds 3 GB, an error is reported.

**Solution**: Adjust the values of the following flags. The flags control the volume of data that each reducer can write into OSS storage. You can change the values of the flags to ensure that the size of an object stored in OSS does not exceed 3 GB.

```
set odps.sql.mapper.split.size=256; # Adjust the volume of table data that is read by each mapper. Unit: MB.
set odps.sql.reducer.instances=100; # Adjust the number of reducers in the execution plan.
```

