---
keyword: Information\_Schema
---

# Information\_Schema概述

本文介绍了MaxCompute的元数据服务Information\_Schema服务的基本概念、操作使用以及使用限制。

MaxCompute的Information Schema提供了项目元数据及使用历史数据等信息。在ANSI SQL-92的Information Schema基础上，添加了面向MaxCompute服务特有的字段及视图。MaxCompute提供了名称为Information\_Schema的公共项目，通过访问该公共项目提供的只读视图，可以查询到用户项目的元数据信息及使用历史信息。

## 使用限制

-   Information Schema提供的是当前项目的元数据视图，不支持跨项目的元数据访问。如果需要对多个项目的元数据进行统一查询、分析，需要分别获取各个项目中的元数据并整合在一起进行跨项目元数据分析。
-   元数据系统表目前提供准实时视图，对元数据时效性要求较高的应用，建议使用SDK/CLI直接获取指定对象的元数据。
-   元数据及作业历史数据保存在Information\_Schema空间下，如果需要对历史数据进行快照备份或获得超过14天的作业历史，您可以定期将Information Schema的数据备份到指定项目空间。

## 安装Package获得访问权限

开始使用前，需要以项目空间所有者（Project Owner）身份安装Information Schema的权限包，获得访问本项目元数据的权限。安装方式有如下两种：

-   在MaxCompute客户端（odpscmd）中执行如下命令。

    ```
    odps@myproject1>install package information_schema.systables;
    ```

-   在DataWorks中的**数据开发** \> **临时查询**中执行如下语句。

    ```
    install package information_schema.systables;
    ```


Package安装成功后，当前操作所在项目即获得了通过Information\_Schema查询本项目相关元数据的权限。数据保存在Information\_Schema项目内，无需为元数据存储付费。

执行如下命令，可以查看Information\_Schema所提供的视图列表。

```
odps@myproject1> describe package information_schema.systables;
```

查询结果如下图。

![information_schema 截图返回结果](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1123398951/p63142.png)

## 查询元数据视图

查询元数据视图时，需要在视图名称前指定项目空间Information\_Schema，即information\_schema.view\_name。

例如，您登录访问的当前项目为myproject1，在myproject1中，执行如下命令查询当前myproject1中所有表的元数据信息。

```
odps@myproject1>select * from information_schema.tables;
```

Information Schema同时也包含了作业历史视图，可以查询到当前项目内的作业历史信息。使用时可添加日期分区进行过滤，请参见如下命令。

```
odps@myproject1>select * from information_schema.tasks_history where ds=‘yyyymmdd' limit 100;
```

## 访问授权

Information Schema的视图包含了项目级别的所有用户数据，默认项目空间所有者可以查看。如果项目内其他用户或角色需要查看，需要进行授权，请参见[MaxCompute Package授权方法](/intl.zh-CN/管理/安全管理详解/跨项目空间的资源分享/Package的使用方法.md)。

语法如下。

```
grant actions on package <pkgName> to user <username>;
grant actions on package <pkgName> to role <role_name>;
```

示例如下。

```
grant read on package information_schema.systables to user RAM$name@your_account.com:user01；
```

