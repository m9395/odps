---
keyword: common SQL error
---

# Common SQL errors

This topic describes common SQL errors and the conditions that trigger them.

|Error message|Trigger condition|
|:------------|:----------------|
|ODPS-0110011:Authorization exception|The error message returned because you are unauthorized.|
|ODPS-0110021:Invalid parameters|The error message returned because the parameters are invalid.|
|ODPS-0110031:Invalid object type|The error message returned because the object type is invalid.|
|ODPS-0110041:Invalid meta operation - AlreadyExistsException\(message:Partition already exists, existed values\)|The error message returned because MaxCompute does not have the lock mechanism on the table. This error is caused by competition for the metadata. Multiple read/write operations on a partition at the same time are prone to this type of error. If the lock mechanism is unavailable in MaxCompute, we recommend that you do not perform multiple operations on a table at the same time.|
|ODPS-0110061: Failed to run ddltask - AlreadyExistsException\(message:Partition already exists, existed values:\)|The error message returned because MaxCompute does not have the lock mechanism on the table. This error is caused by competition for the metadata. Multiple read/write operations on a partition at the same time are prone to this type of error. If the lock mechanism is unavailable in MaxCompute, we recommend that you do not perform multiple operations on a table at the same time.|
|ODPS-0110061:Failed to run ddltask - SimpleLock conflict failure, add partition is already on-going|The error message returned because you have added multiple partitions at a time. MaxCompute only runs the first command to add a partition and ignores the subsequent commands.|
|ODPS-0110071:OTS initialization exception|The error message returned because Tablestore initialization is abnormal.|
|ODPS-0110081:OTS transaction exception|The error message returned because Tablestore transaction is abnormal.|
|ODPS-0110091:OTS filtering exception|The error message returned because conditional filtering of Tablestore is abnormal.|
|ODPS-01100101:OTS processing exception|The error message returned because Tablestore execution is abnormal.|
|ODPS-01100111:OTS invalid data object|The error message returned because Tablestore data is invalid.|
|ODPS-0110131:StorageDescriptor compression exception|The error message returned because the storage descriptor compression is abnormal.|
|ODPS-0110141:Data version exception|The error message returned because the data version is abnormal.|
|ODPS-0110999:Critical! Internal error happened in commit operation and rollback failed, possible breach of atomicity|The error message returned because the rollback has failed.|
|ODPS-0120011:Authorization exception|The error message returned because you are unauthorized.|
|ODPS-0120021:the delimitor must be the same in wm\_concat|The error message returned because the delimiters in a group are different.|
|ODPS-0120031:Instance has been cancelled|The error message returned because the instance is canceled.|
|ODPS-0121011:Invalid regular expression pattern|The error message returned when the regular expression processing function of built-in functions has received an unrecognizable regular expression.|
|ODPS-0121021:Regexec call failed|The error message returned because regular expression matching has failed.|
|ODPS-0120031:Instance has been cancelled|The error message returned because the instance is canceled.|
|ODPS-0121035:Illegal implicit type cast|The error message returned because the data type conversion is invalid. This error is usually caused by violations of implicit conversion rules.|
|ODPS-0121045:Unsupported return type|The error message returned because the type of the return value is not supported.|
|ODPS-0121055:Empty argument value|The error message returned because the parameter value is an empty string or null.|
|ODPS-0121065:Argument value out of range|The error message returned because the parameter value is invalid.|
|ODPS-0121075:Invalid number of arguments|The error message returned because the number of parameters is invalid.|
|ODPS-0121081:Illegal argument type|The error message returned because the basic type of the parameter is invalid.|
|ODPS-0121095:Invalid arguments|The error message returned because other parameter errors have occurred.|
|ODPS-0121105:Constant argument value expected|The error message returned because you have specified column names instead of the required constants.|
|ODPS-0121115:Column reference expected|The error message returned because you have specified constants instead of the required column names.|
|ODPS-0121125:Unsupported function or operation|The error message returned because the UDF or operation is not supported.|
|ODPS-0121135:Malloc memory failed|The error message returned because memory allocation is abnormal.|
|ODPS-0121145:Data overflow|The error message returned because of a data overflow. The data type of the value is invalid. Data overflow is usually caused by an aggregate function, such as SUM.|
|ODPS-0123019:Distributed file operation exception|The error message returned because the read or write operation on the disk is abnormal.|
|ODPS-0123023:Unsupported reduce type|The error message returned because the Reduce type is not supported.|
|ODPS-0123031:Partition exception|The error message returned because a partition exception has occurred.|
|ODPS-0123043:buffer overflow|The error message returned because of a cache overflow.|
|ODPS-0123055:Script exception|The error message returned because the script is abnormal.|
|ODPS-0123065:Join exception|The error message returned because the JOIN operation is abnormal.|
|ODPS-0123075:Hash exception|The error message returned because a hash exception has occurred.|
|ODPS-0123081:Invalid datetime string|The error message returned because the DATETIME string is abnormal.|
|ODPS-0123091:Illegal type cast|The error message returned because the data type conversion is invalid. This error is caused by invalid explicit conversions.|
|ODPS-0123105:Job got killed|The error message returned because the job has been stopped.|
|ODPS-0123111:Format string does not match datetime string|The error message returned because the format string does not match the DATETIME string. The format of the date manually entered in the SQL statement does not meet the MaxCompute format requirements, or DATETIME-related built-in functions are improperly used.|
|ODPS-0123121:Mapjoin exception|The error message returned because a MAPJOIN exception has occurred. In most cases, this error occurs because the size of the small table on which the MAPJOIN operation is performed exceeds the upper limit of 512 MB.|
|ODPS-0123131:User defined function exception|The error message returned because the UDF is abnormal.|
|ODPS-0123141:Exstore exception|The error message returned because extreme storage is abnormal.|
|ODPS-0130013:Authorization exception|The error message returned because you are unauthorized and the security check has failed.|
|ODPS-0130025:Failed to I/O|The error message returned because an I/O exception has occurred.|
|ODPS-0130031:Failed to drop table|The error message returned because the source table does not exist when you try to delete the table.|
|ODPS-0130041:Statistics exception|The error message returned because a statistics exception has occurred.|
|ODPS-0130051:Exception in sub query|The error message returned because a subquery exception has occurred.|
|ODPS-0130061:Invalid table|The error message returned because the table is invalid.|
|ODPS-0130071:Semantic analysis exception - Invalid table alias or column reference|The error message returned because the column name is invalid and the required column has not been found.|
|ODPS-0130071:Semantic analysis exception - Invalid column reference|The error message returned because the column reference is invalid and the required column has not been found.|
|ODPS-0130071:Semantic analysis exception - Expression not in GROUP BY key|The error message returned because the columns read by the SELECT clause are different from those by the GROUP BY clause.|
|ODPS-0130071:Semantic analysis exception - Partition not found|The error message returned because the partition specified by the partitioning column value has not been found.|
|ODPS-0130071:Semantic analysis exception - SELECT DISTINCT and GROUP BY can not be in the same query|The error message returned because DISTINCT and GROUP BY cannot be used in the same SELECT clause.|
|ODPS-0130071:Semantic analysis exception - Cannot insert into target table because column number/types are different|The error message returned because the number or type of the columns in the source table is different from that in the destination table when you try to insert data into the destination table.|
|ODPS-0130071:Semantic analysis exception - physical plan generation failed: java.lang.RuntimeException: Table\(xxxx\) is full scan with all partitions, please specify partition predicates.|The error message returned because a full scan has been disabled for a partitioned table of the project to which the table belongs. To address this issue, you must specify the partition conditions. To scan a full table by using the current SQL statement, you can add the `set odps.sql.allow.fullscan=true` statement to the current SQL statement and submit the new statement. A full table scan increases data inputs and costs.|
|ODPS-0130071:Semantic analysis exception - xxxx type is not enabled in current mode|The error message returned because the function for using new data types is disabled. To use new data types, such as TINYINT, SMALLINT, INT, FLOAT, VARCHAR, TIMESTAMP, and BINARY, you must set the parameter for using the new data types to true. -   Session level: `set odps.sql.type.system.odps2=true;`

This statement must be submitted with the SQL statement. This setting is valid only for the current session.

-   Project level: `setproject odps.sql.type.system.odps2=true;` |
|ODPS-0130081:Invalid UDF reference|The error message returned because the UDF signature is invalid.|
|ODPS-0130091:Invalid parameters|The error message returned because the UDF parameters are invalid.|
|ODPS-0130101:Ambiguous data type|The error message returned because the data type is invalid.|
|ODPS-0130111:Subquery partition pruning exception|The error message returned because dynamic partition optimization in the subquery in the IN conditional statements is abnormal.|
|ODPS-0130121:Invalid argument type|The error message returned because the parameter type is invalid. In most cases, this error occurs because an invalid parameter type has been received by built-in functions.|
|ODPS-0130131:Table not found|The error message returned because the table you want to manage does not exist when you execute DDL or DML statements.|
|ODPS-0130141:Illegal implicit type cast|The error message returned because implicit conversion is not supported.|
|ODPS-0130151:Illegal data type|The error message returned because the data type is invalid.|
|ODPS-0130161:Parse exception|The error message returned because syntax parsing has failed.|
|ODPS-0130171:Creating view exception|The error message returned because an exception has occurred during view creation.|
|ODPS-0130181:Window function exception|The error message returned because the window function is abnormal.|
|ODPS-0130191:Invalid column or partition key|The error message returned because the column or partition key is invalid.|
|ODPS-0130201:View not found|The error message returned because the view does not exist.|
|ODPS-0130211:Table or view already exists|The error message returned because the table or view already exists.|
|ODPS-0130221:Invalid number of arguments|The error message returned because the number of parameters is invalid.|
|ODPS-0130231:Invalid view|The error message returned because the view status is invalid.|
|ODPS-0130241:Illegal union operation|The error message returned because the UNION operation is invalid. In most cases, this error occurs because the numbers or types of columns on the two sides of the UNION clause are inconsistent.|
|ODPS-0130252:Cartesian product is not allowed|The error message returned because Cartesian products are not supported. MaxCompute does not support non-equi joins for ON conditions in JOIN clauses.|
|ODPS-0130261:Invalid schema|The error message returned because the metadata is invalid.|
|ODPS-0130271:Partition does not exist|The error message returned because the partition does not exist.|
|ODPS-0140011:Illegal type cast|The error message returned because the explicit conversion is not supported.|
|ODPS-0140021:Illegal implicit type cast|The error message returned because implicit conversion is not supported.|
|ODPS-0140031:Invalid column reference|The error message returned because the column name or table name reference is invalid.|
|ODPS-0140041:Invalid UDF reference|The error message returned because the UDF does not exist.|
|ODPS-0140051:Invalid function|The error message returned because the function is invalid.|
|ODPS-0140061:Invalid parameters|The error message returned because the input parameters are invalid.|
|ODPS-0140071:Unsupported operator|The error message returned because the operator is not supported.|
|ODPS-0140081:Unsupported join type|The error message returned because the left table in the LEFT OUTER JOIN operation is a small table but the right table in the operation is a large table, or the left table is the RIGHT OUTER JOIN operation is a large table but the right table in the operation is a small table.|
|ODPS-0140091:Unsupported stage type|The error message returned because the execution plan type is not supported.|
|ODPS-0140105:Invalid multiple I/O|The error message returned because multiple outputs have caused a conflict.|
|ODPS-0140111:Unsupported col type in EXTRACT now|The error message returned because EXTRACT does not support the column type.|
|ODPS-0140133:Invalid structure|The error message returned because the data structure cannot be identified.|
|ODPS-0140151:Can not do topologic sort, the stages is not a DAG|The error message returned because an error has occurred in the sorting algorithm.|
|ODPS-0140171:Sandbox violation exception|The error message returned because the security sandbox model is abnormal.|
|ODPS-0140181:Sql plan exception|The error message returned because an SQL job cannot generate an execution plan. If this error occurs, try to submit the job again. If the fault persists, [submit a ticket](https://workorder-intl.console.aliyun.com/).|
|ODPS-0123049: buffer overflow|The error message returned because an out-of-memory error has occurred. You must check whether a data issue exists. For example, a large amount of data with duplicate keys exists in the JOIN operation.|
|ODPS-0140178: Internal system failure|The error message returned because an exception has occurred in the system. You can retry to solve the problem.|
|ODPS-0420061: Invalid parameter in HTTP request - Fetched data is larger than the rendering limitation. Please try to reduce your limit size or column number|The error message returned because a large amount of data is displayed. This may be caused by a large field content.|

