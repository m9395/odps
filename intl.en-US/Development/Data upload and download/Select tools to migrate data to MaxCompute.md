---
keyword: [migrate Hadoop data, synchronize data from a database, collect logs]
---

# Select tools to migrate data to MaxCompute

MaxCompute provides a variety of tools for you to upload data to or download data from MaxCompute. You can use the tools to migrate data to MaxCompute in different scenarios. This topic describes how to select data transmission tools in three typical scenarios.

## Migrate Hadoop data

You can use MaxCompute Migration Assist \(MMA\), Sqoop, or DataWorks to migrate Hadoop data.

-   If you use DataWorks, DataX is required.
-   If you use Sqoop, a MapReduce job is executed on the original Hadoop cluster to transmit data to MaxCompute in a distributed manner. For more information, see [Apache Sqoop](http://sqoop.apache.org/).

## Synchronize data from a database

To synchronize data from a database to MaxCompute, you must select a tool based on the database type and synchronization policy.

-   Use DataWorks for offline batch synchronization. DataWorks supports a wide range of database types, which include MySQL, SQL Server, and PostgreSQL. For more information, see [Batch Sync node](). You can also perform instance-related operations based on [Create a batch synchronization node]().
-   Use the OGG plug-in for real-time synchronization of data in an Oracle database.
-   Use Data Transmission Service \(DTS\) for real-time synchronization of data in an ApsaraDB for RDS database.

## Collect logs

You can use tools such as Flume, Fluentd, and Logstash to collect logs.

