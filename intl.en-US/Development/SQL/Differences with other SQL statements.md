---
keyword: [DDL statements, DML statements, SCRIPTING statements]
---

# Differences with other SQL statements

This topic lists SQL statements that are supported by MaxCompute by comparing MaxCompute SQL statements with SQL statements of Hive, MySQL, Oracle, and SQL Server.

## Support for DDL statements

|Statement|MaxCompute|Hive|MySQL|Oracle|SQL Server|
|:--------|:---------|:---|:----|:-----|:---------|
|CREATE TABLE—PRIMARY KEY|N|N|Y|Y|Y|
|CREATE TABLE—NOT NULL|Y|N|Y|Y|Y|
|CREATE TABLE—CLUSTER BY|Y|Y|N|Y|Y|
|CREATE TABLE—EXTERNAL TABLE|Y \(OSS, OTS, TDDL\)|Y|N|Y|N|
|CREATE TABLE—TEMPORARY TABLE|N|Y|Y|Y|Y \(with the \# prefix\)|
|INDEX—CREATE INDEX|N|Y|Y|Y|Y|
|VIRTUAL COLUMN|N|N|N|Y|Y|

## Support for DML statements

|Statement|MaxCompute|Hive|MySQL|Oracle|SQL Server|
|:--------|:---------|:---|:----|:-----|:---------|
|CTE|Y|Y|Y|Y|Y|
|SELECT—recursive CTE|N|N|N|Y|Y|
|SELECT—GROUP BY ROLL UP|Y|Y|Y|Y|Y|
|SELECT—GROUP BY CUBE|Y|Y|N|Y|Y|
|SELECT—GROUPING SET|Y|Y|N|Y|Y|
|SELECT—IMPLICIT JOIN|Y|Y|N|Y|Y|
|SELECT—PIVOT|N|N|N|Y|Y|
|SEMI JOIN|Y|Y|Y|N|N|
|SELEC TRANSFROM|Y|Y|N|N|N|
|SELECT—correlated subquery|Y|Y|Y|Y|Y|
|ORDER BY NULLS FIRST/LAST|N|Y|Y|Y|Y|
|LATERAL VIEW|Y|Y|N|Y|Y \(CROSS APPLY keyword\)|
|SET OPERATOR—UNION \(distinct\)|Y|Y|Y|Y|Y|
|SET OPERATOR—INTERSECT|Y|N|N|Y|Y|
|SET OPERATOR—MINUS/EXCEPT|Y|N|N|Y|Y \(keyword EXCEPT\)|
|INSERT INTO ... VALUES|Y|Y|Y|Y|Y|
|INSERT INTO \(ColumnList\)|Y|Y|Y|Y|Y|
|UPDATE … WHERE|N|Y|Y|Y|Y|
|UPDATE … ORDER BY LIMIT|N|N|Y|N|Y|
|DELETE … WHERE|N|Y|Y|Y|Y|
|DELETE … ORDER BY LIMIT|N|N|Y|N|N|
|ANALYTIC—reusable WINDOWING CLUSUE|Y|Y|N|N|N|
|ANALYTIC—CURRENT ROW|Y|Y|N|Y|Y|
|ANALYTIC—UNBOUNDED|Y|N|Y|Y|Y|
|ANALYTIC—RANGE ...|N|Y|N|Y|Y|
|WHILE DO|N|N|Y|Y|Y|

## Support for SCRIPTING statements

|Statement|MaxCompute|Hive|MySQL|Oracle|SQL Server|
|---------|----------|----|-----|------|----------|
|TABLE VARIABLE|Y|Y|Y|Y|Y|
|SCALER VARIABLE|Y|Y|Y|Y|Y|
|ERROR HANDLING—RAISE ERROR|N|N|Y|Y|Y|
|ERROR HANDLING—TRY CATCH|N|N|N|Y|Y|
|FLOW CONTROL—LOOP|N|N|Y|Y|Y|
|CURSOR|N|N|Y|Y|Y|

