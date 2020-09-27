---
keyword: terms
---

# Terms

This topic describes the terms used in MaxCompute.

## A

-   AccessKey

    An AccessKey pair is the credential for accessing Alibaba Cloud APIs. An AccessKey pair consists of an AccessKey ID and AccessKey secret. After you create an Alibaba Cloud account on the Alibaba Cloud official website, an AccessKey pair is generated on the [AccessKey Management](https://ak-console.aliyun.com/#/) page. AccessKey pairs are used to identify users and verify the signature of requests for accessing Alibaba Cloud services such as MaxCompute. You must keep your AccessKey secret confidential.

-   security

    The MaxCompute multi-tenant data security system includes user authentication, user and authorization management of projects, resource sharing across projects, and data protection of projects. For more information about MaxCompute security operations, see [Target users](/intl.en-US/Management/Configure security features/Target users.md).


## C

-   console

    The MaxCompute console is a client tool running on Windows or Linux. The MaxCompute console allows you to submit commands to complete operations, such as project management operations, DDL operations, and DML operations. For more information about tool installation and common parameters, see [Client](/intl.en-US/Tools and Downloads/Client.md).


## D

-   data type

    Data types are the types of data in the columns of a MaxCompute table. For more information about the data types supported by MaxCompute, see [Date types](/intl.en-US/Development/Data types/Data type editions.md).

-   DDL

    Data Definition Language \(DDL\) is a syntax for creating and modifying database objects. For example, it provides the statements for creating tables and views. For more information about the MaxCompute DDL syntax, see [Table operations](/intl.en-US/Development/SQL/DDL SQL/Table operations.md).

-   DML

    Data Manipulation Language \(DML\) is a syntax that includes statements such as INSERT. For more information about the MaxCompute DML syntax, see [INSERT OVERWRITE and INSERT INTO](/intl.en-US/Development/SQL/Insert Operation/INSERT OVERWRITE and INSERT INTO.md).


## J

-   Job Scheduler

    Job Scheduler is a module used for resource management and task scheduling in the kernel of the Apsara distributed operating system. Job Scheduler also provides a basic programming framework for application development. Job Scheduler serves as the underlying task scheduling module of MaxCompute.


## I

-   instance

    An instance is an actually running job, which is similar to a job in Hadoop. For more information, see [Instance](/intl.en-US/Product Introduction/Definitions/Instance.md).


## M

-   MapReduce

    MapReduce is a programming model for processing data. MapReduce is used for parallel operations on large data sets. You can use the Java API provided by MapReduce to write MapReduce programs and process MaxCompute data. The idea of MapReduce is to classify data processing methods as Map and Reduce. The Map method is used for the mapping of data and the Reduce method is used for the combination of data.

    Before the Map operation, the input data must be sliced into data blocks of equal size. Each data block is processed as the input to a single Map worker node. This way, multiple Map worker nodes can work simultaneously. Each Map worker node processes an input data block and generates the intermediate result to a Reduce worker node. The Reduce worker node combines the outputs of multiple Map worker nodes to obtain the final result. For more information, see [MapReduce](/intl.en-US/Development/MapReduce/Summary/MapReduce.md).


## O

-   ODPS

    ODPS is the original name of MaxCompute.


## P

-   partition

    A partition is a division of a table based on the partition key, which consists of one or more partition key columns. Partitions are used to divide the data stored in a table. If a table is not partitioned, data in the table is stored in the directory that stores the table. If a table is partitioned, each partition corresponds to a subdirectory in the directory that stores the table. In this case, data is stored in separate subdirectories. For more information about partitions, see [Partition](/intl.en-US/Product Introduction/Definitions/Partition.md).

-   project

    A project is a basic organizational unit of MaxCompute. A project in MaxCompute is similar to a database or schema in a traditional database management system. Projects are used to isolate users and manage access requests. For more information, see [Project](/intl.en-US/Product Introduction/Definitions/Project.md).


## R

-   role

    A role is a concept in MaxCompute security features. A role can be considered as a set of users with the same permissions. One user can have multiple roles, and multiple users can belong to the same role. After you authorize a role, all users assigned this role are granted the same permissions. For more information about role management, see [Manage roles](/intl.en-US/Management/Configure security features/Manage users and permissions/Manage roles.md).

-   resource

    Resources in MaxCompute provides resource dependencies for MaxCompute operations. You must have the required resources for implementing user defined functions \(UDFs\) and MapReduce operations in MaxCompute. For more information, see [Resource](/intl.en-US/Product Introduction/Definitions/Resource.md).


## S

-   SDK

    A Software Development Kit \(SDK\) is a collection of development tools used by software engineers to build application software for specific software packages, software instances, software frameworks, hardware platforms, operating systems, or document packages. MaxCompute supports Java SDK and Python SDK. For more information, see[Java SDK](/intl.en-US/SDK Reference/Java SDK/Java SDK.md) and [Python SDK](/intl.en-US/SDK Reference/Python SDK/SDK for Python.md).

-   authorization

    A project administrator or project owner grants you permissions to perform specific operations in MaxCompute. For example, you can read, write, and view objects such as tables, tasks, and resources with specified permissions granted. For more information about how to grant permissions, see [Manage users](/intl.en-US/Management/Configure security features/Manage users and permissions/Manage users.md).

-   sandbox

    A sandbox is an isolated environment to restrict program actions based on security policies. A sandbox serves as a security mechanism to isolate Java code execution in a separate environment and restrict malicious code from accessing local system resources. This prevents damage to the local system. MaxCompute MapReduce and UDF programs are restricted by a Java sandbox when they are run in a distributed environment. For more information, see [Java Sandbox](/intl.en-US/Development/MapReduce/Java SDK/Java Sandbox.md).


## T

-   table

    A table is a data storage unit in MaxCompute. For more information, see [Table](/intl.en-US/Product Introduction/Definitions/Table.md).

-   Tunnel

    MaxCompute Tunnel is a data channel in MaxCompute that provides high-concurrency offline data upload and download services. You can use MaxCompute Tunnel to upload data in batches to MaxCompute or download data in batches to your local device. For more information about related commands, see [Tunnel commands](/intl.en-US/Development/Data upload and download/Run Tunnel commands to upload and download data/Tunnel commands.md) and [Tunnel overview](/intl.en-US/Development/Data upload and download/Tunnel SDK/MaxCompute Tunnel overview.md).


## U

-   UDF

    In a broad sense, UDFs refer to all user defined functions: user-defined scalar functions, user-defined aggregate functions \(UDAFs\), and user-defined table-valued functions \(UDTFs\). For more information about UDFs of Java programming APIs provided by MaxCompute, see [Overview](/intl.en-US/Development/SQL/UDF/Overview.md).

    In a narrow sense, UDFs refer to only user-defined scalar functions. The relationship between the input and output is one-to-one mapping, which indicates that one value is returned each time a UDF reads one row of data.

-   UDAF

    UDAFs have a multiple-to-one mapping relationship between the input and output, which indicates that multiple rows are aggregated to one row. UDAFs can be used with the GROUP BY clause of SQL statements. For more information, see [UDAF](/intl.en-US/Development/SQL/UDF/Java UDF.md).

-   UDTF

    UDTFs are used in scenarios where multiple rows of data are returned after each function invocation. UDTFs are the only user defined functions that return multiple rows. For more information, see [UDTF](/intl.en-US/Development/SQL/UDF/Java UDF.md).


