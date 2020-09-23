---
keyword: feature updates
---

# Feature updates

This topic describes the latest updates to MaxCompute V2.0 documentation. These updates allow you to understand the new features, syntax, and permissions in MaxCompute and help you improve project development efficiency.

## Updates in August 2020

|Date|Feature|Category|Description|Documentation|
|----|-------|--------|-----------|-------------|
|2020-08-07|Best practice to optimize costs|New practices|A best practice to optimize the computing, storage, data upload, and data download costs is added.|[Cost optimization](/intl.en-US/Best Practices/Cost optimization/Overview.md)|
|2020-08-06|Best practice to assign the Super\_Administrator role to a RAM user for a MaxCompute project|New practices|A best practice to assign the Super\_Administrator role to a RAM user for a MaxCompute project is added. The suggestions on how to manage members and permissions by using the Super\_Administrator role are provided.|[Set a RAM user as the super administrator for a MaxCompute project](/intl.en-US/Best Practices/Security management/Set a RAM user as the super administrator for a MaxCompute project.md)|
|2020-08-05|Best practices to segment Chinese text by using Jieba in a PyODPS node|New practices|The following best practices are added: 1. Segment Chinese text by using Jieba, an open source segmentation tool, and write the segmented words and phrases to a new table in a PyODPS node in DataWorks. 2. Use Jieba to segment Chinese text based on a custom dictionary referenced by a closure function.|[Use a PyODPS node to segment Chinese text based on Jieba]()|
|2020-08-05|Best practice to grant access to a specific UDF only to a specified user|New practices|A best practice to grant access to a specific resource, such as a table or a user defined function \(UDF\), only to a specified user is added.|[Grant access to a specific UDF to a specified user]()|
|2020-08-05|Best practice to migrate data from Oracle to MaxCompute|New practices|A best practice to use the data synchronization feature of DataWorks to migrate data from Oracle to MaxCompute is added.|[Migrate data from Oracle to MaxCompute]()|
|2020-08-05|Best practice to migrate data from a user-created MySQL database on an ECS instance to MaxCompute|New practices|A best practice to use exclusive resource groups for Data Integration to migrate data from a user-created MySQL database on an Elastic Compute Service \(ECS\) instance to MaxCompute is added.|[Migrate data from a user-created MySQL database on an ECS instance to MaxCompute]()|
|2020-08-05|`odps.text.option.use.quote` property supported by SERDEPROPERTIES|New description|The description of the odps.text.option.use.quote property is added. The odps.text.option.use.quote property specifies whether to recognize a double quotation mark \(`"`\) as the column delimiter in a CSV file.|[Access OSS data by using the built-in extractor](/intl.en-US/Development/External table/Access OSS data by using the built-in extractor.md)|

## Updates in July 2020

|Date|Feature|Category|Description|Documentation|
|----|-------|--------|-----------|-------------|
|2020-07-29|Best practice to migrate data from MaxCompute to Tablestore|New practices|A best practice to migrate data from MaxCompute to Tablestore is added.|[Migrate data from MaxCompute to Tablestore](/intl.en-US/Best Practices/Data migration/Migrate data from MaxCompute to Tablestore.md)|
|2020-07-29|Best practices to migrate data from MaxCompute to Object Storage Service \(OSS\)|New practices|A best practice to use the data synchronization feature of DataWorks to migrate data from MaxCompute to OSS is added.|[Migrate data from MaxCompute to OSS](/intl.en-US/Best Practices/Data migration/Migrate data from MaxCompute to OSS.md)|
|2020-07-24|Data encryption|New feature|The description of the data encryption feature is added. MaxCompute uses Key Management Service \(KMS\) to encrypt data for storage. This way, MaxCompute can provide static data protection to meet corporate governance and security compliance requirements.|[Data encryption](/intl.en-US/Management/Data encryption.md)|
|2020-07-23|Aggregate functions|New description|The description of aggregate functions `APPROX_DISTINCT`, `ANY_VALUE`, `ARG_MAX`, and `ARG_MIN` is added.|[Aggregate functions](/intl.en-US/Development/SQL/Builtin functions/Aggregate functions.md)|
|2020-07-23|New data types supported by Python UDFs|New description|The description of new data types supported by Python UDFs is added.|-   [Python 2 UDFs](/intl.en-US/Development/SQL/UDF/Python 2 UDFs.md)
-   [Python 3 UDFs](/intl.en-US/Development/SQL/UDF/Python 3 UDFs.md) |
|2020-07-23|SQL functions|New feature|The description of SQL functions that allow you to reference SQL UDFs in SQL scripts is added.|[SQL functions](/intl.en-US/Development/SQL/UDF/SQL functions.md)|
|2020-07-23|Code-embedded UDFs|New feature|The description of code-embedded UDFs is added. Code-embedded UDFs allow you to embed Java or Python code into SQL scripts.|-   [Code-embedded UDFs](/intl.en-US/Development/SQL/UDF/Code-embedded UDFs.md)
-   [Usage examples](/intl.en-US/Development/SQL/UDT/Usage examples.md) |
|2020-07-20|Log auditing|New feature|The description of the features, scenarios, scope, and fields of log auditing is added.|[Audit logs](/intl.en-US/Management/Audit logs.md)|
|2020-07-15|Best practice to use Tunnel to upload log data to MaxCompute|New practices|A best practice to use Tunnel to upload log data to MaxCompute is added.|[Use Tunnel to upload log data to MaxCompute](/intl.en-US/Best Practices/Data migration/Migrate log data to MaxCompute/Use Tunnel to upload log data to MaxCompute.md)|
|2020-07-15|Best practice to use DataHub to migrate log data to MaxCompute|New practices|A best practice to use DataHub to migrate log data to MaxCompute is added.|[Use DataHub to migrate log data to MaxCompute](/intl.en-US/Best Practices/Data migration/Migrate log data to MaxCompute/Use DataHub to migrate log data to MaxCompute.md)|
|2020-07-15|Best practices to use of DataWorks Data Integration to migrate log data to MaxCompute|New practices|A best practice to use DataWorks Data Integration to synchronize data collected by LogHub to MaxCompute is added.|[Use DataWorks Data Integration to migrate log data to MaxCompute](/intl.en-US/Best Practices/Data migration/Migrate log data to MaxCompute/Use DataWorks Data Integration to migrate log data to MaxCompute.md)|
|2020-07-08|CLONE TABLE|New feature|The description of the CLONE TABLE statement is added. MaxCompute supports the CLONE TABLE statement that allows you to clone data from one table to another. This statement facilitates both data migration and replication.|[CLONE TABLE](/intl.en-US/Development/SQL/CLONE TABLE.md)|

## Updates in June 2020

|Date|Feature|Category|Description|Documentation|
|----|-------|--------|-----------|-------------|
|2020-06-03|Tunnel OVERWRITE command|New description|The description of the Tunnel OVERWRITE command is added.|[Tunnel commands](/intl.en-US/Development/Data upload and download/Run Tunnel commands to upload and download data/Tunnel commands.md)|
|2020-06-01|Optimization of access to instances in a virtual private cloud \(VPC\) from Spark on MaxCompute|New description and examples|The following content is added:-   Limits on the VPC whitelist and regions
-   Examples of merged JSON text when Spark on MaxCompute is used to access different instances

|[Spark on MaxCompute accesses VPC-type instances](/intl.en-US/Development/Spark/Spark on MaxCompute accesses VPC-type instances.md)|
|2020-06-01|Policy-based access control and download control|New examples|Examples on how to use policy-based access control and permission revocation are added.|[Policy-based access control and download control](/intl.en-US/Management/Configure security features/Policy-based access control and download control.md)|

## Updates in January 2020

|Date|Feature|Category|Description|Documentation|
|----|-------|--------|-----------|-------------|
|2020-01-14|Improved SQL compatibility|New description|The execution rules of the `GET_IDCARD_AGE`, `CONCAT_WS`, and `LIKE` functions are modified.|-   [Other functions](/intl.en-US/Development/SQL/Builtin functions/Other functions.md)
-   [String functions](/intl.en-US/Development/SQL/Builtin functions/String functions.md)
-   [LIKE usage](/intl.en-US/Development/SQL/Appendix/LIKE usage.md) |
|2020-01-09|Parameter description|New description|The parameters in examples are described in detail.|[Project data protection](/intl.en-US/Management/Configure security features/Project data protection.md)|

## Updates in December 2019

|Date|Feature|Category|Description|Documentation|
|----|-------|--------|-----------|-------------|
|2019-12-25|Open source geospatial UDFs|New feature|The description of open source geospatial UDFs is added. You can register open source geospatial UDFs with MaxCompute and use them as open source Hive UDFs.|[Open source geospatial UDFs](/intl.en-US/Development/SQL/UDF/Open source geospatial UDFs.md)|

## Updates in November 2019

|Date|Feature|Category|Description|Documentation|
|----|-------|--------|-----------|-------------|
|2019-11-06|Description of whether MaxCompute supports partition pruning|New description|The description of whether MaxCompute supports partition pruning is added.|[Comparison of functions built in MaxCompute, MySQL, and Oracle](/intl.en-US/Development/SQL/Builtin functions/Comparison of functions built in MaxCompute, MySQL, and Oracle.md)|

## Updates in October 2019

|Date|Feature|Category|Description|Documentation|
|----|-------|--------|-----------|-------------|
|2019-10-09|New SQL syntax|New feature|-   The syntax to merge partitions is added.
-   The syntax that uses a pair of parentheses to specify the priorities of operations in JOIN and SETOP statements is added.
-   The JSON\_TUPLE built-in function is added.
-   The EXTRACT function is added.
-   Two flags are added.
-   The LIMIT and OFFSET clauses are supported.
-   Default values can be specified for columns in a table.
-   The NATURAL JOIN statement is supported.
-   New operators are supported.
-   The syntax to delete partitions is added.

|-   [Partition and column operations](/intl.en-US/Development/SQL/DDL SQL/Partition and column operations.md)
-   [JOIN](/intl.en-US/Development/SQL/Select Operation/JOIN.md)
-   [String functions](/intl.en-US/Development/SQL/Builtin functions/String functions.md)
-   [Date functions](/intl.en-US/Development/SQL/Builtin functions/Date functions.md)
-   [SELECT syntax](/intl.en-US/Development/SQL/Select Operation/SELECT syntax.md)
-   [SELECT syntax](/intl.en-US/Development/SQL/Select Operation/SELECT syntax.md)
-   [Table operations](/intl.en-US/Development/SQL/DDL SQL/Table operations.md)
-   [JOIN](/intl.en-US/Development/SQL/Select Operation/JOIN.md)
-   [Operators](/intl.en-US/Development/SQL/Operators.md)
-   [Partition and column operations](/intl.en-US/Development/SQL/DDL SQL/Partition and column operations.md) |

## Updates in July 2019

|Date|Feature|Category|Description|Documentation|
|----|-------|--------|-----------|-------------|
|2019-07-12|odps.sql.reshuffle.dynamicpt added in SET operations|New command|The command used to configure dynamic partitions is added. This command prevents excessive small files from being generated when dynamic partitions are split.|[SET operations](/intl.en-US/Development/Common commands/SET operations.md)|

## Updates in June 2019

|Date|Feature|Category|Description|Documentation|
|----|-------|--------|-----------|-------------|
|2019-06-17|Description of the VALUES statement|New description|The description of how to create data for simple business testing is added.|[VALUES](/intl.en-US/Development/SQL/Insert Operation/VALUES.md)|

