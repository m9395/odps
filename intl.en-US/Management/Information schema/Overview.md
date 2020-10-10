---
keyword: Information\_Schema
---

# Overview

This topic introduces the basic concepts, functions, and limits of Information Schema as the metadata service of MaxCompute.

MaxCompute Information Schema provides information such as project metadata and historical usage data. Fields and views that are specific to MaxCompute are added to ANSI SQL-92 Information Schema. MaxCompute provides a public project named Information\_Schema. You can query the metadata and historical usage data of your project by accessing the read-only views provided by this public project.

## Limits

-   Information Schema provides metadata views of the current project. Cross-project metadata access is not allowed. If you want to query and analyze the metadata of multiple projects, you must obtain the metadata of each project and integrate the metadata.
-   Quasi-real-time views are provided for metadata system tables. For applications that require high metadata timeliness, we recommend that you use the SDK or CLI to obtain the metadata of a specified object.
-   Metadata and task history data are stored in the Information\_Schema project. If you need to create a snapshot of the historical data or obtain historical task data of more than 14 days, you can back up Information\_Schema data to a project on a regular basis.

## Install the package and obtain access permissions.

Before you use the service, you must install the Information Schema package as a project owner and obtain the permission to access the project metadata. Use one of the following methods to install the package:

-   Run the following command on the MaxCompute client \(odpscmd\):

    ```
    odps@myproject1>install package information_schema.systables;
    ```

-   In DataWorks, choose **DataStudio** \> **Ad-Hoc Query**. Then, execute the following statement:

    ```
    install package information_schema.systables;
    ```


After the package is installed, you are authorized to query the metadata of the current project by using Information\_Schema. Data is stored in the Information\_Schema project. You do not have to pay for metadata storage.

Run the following command to view the list of views provided by the Information\_Schema project:

```
odps@myproject1> describe package information_schema.systables;
```

The following figure shows the query result.

![Snapshot of the result returned by the Information_Schema project](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1343948951/p63142.png)

## Query a metadata view

To query a metadata view, you must prefix the project name Information\_Schema to the view name, that is, information\_schema.view\_name.

For example, the project that you work on is myproject1. You run the following command to query the metadata of all tables in myproject1:

```
odps@myproject1>select * from information_schema.tables;
```

The Information\_Schema project also contains the task history view. This view allows you to query the task history of the current project. You can execute the following statement to query task history information by date:

```
odps@myproject1>select * from information_schema.tasks_history where ds='yyyymmdd' limit 100;
```

## Access authorization

The views provided by Information\_Schema contain user data at the project level. By default, the owner of a project can view the user data of this project. Other users or roles in the project must be granted permissions to view the data. For more information, see [MaxCompute package authorization method](/intl.en-US/Management/Configure security features/Resource share across project space/Package usage method.md).

Syntax of the statements to grant permissions to users or roles:

```
grant actions on package <pkgName> to user <username>;
grant actions on package <pkgName> to role <role_name>;
```

Example:

```
grant read on package information_schema.systables to user RAM$name@your_account.com:user01;
```

