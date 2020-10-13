---
keyword: Python SDK
---

# Overview

This topic introduces Python on MaxCompute \(PyODPS\) and its common usage.

## Background information

PyODPS is the MaxCompute SDK for Python. PyODPS supports the DataFrame framework and basic operations on MaxCompute objects. You can use PyODPS to analyze data in MaxCompute.

PyODPS supports Python 2.6 and later, and Python 3.

Read the following documentation to get familiar with PyODPS:

-   For more information about PyODPS, see [PyODPS: ODPS Python SDK and data analysis framework](http://pyodps.readthedocs.org/)and [PyODPS-related articles](https://yq.aliyun.com/album/19).
-   For more information about how to download PyODPS, visit [GitHub](https://github.com/aliyun/aliyun-odps-python-sdk).
-   For more information about how to install PyODPS, see [PyODPS installation instructions](/intl.en-US/Development/PyODPS/Installation guide and limits.md).
-   For more information about how to develop PyODPS, see [PyODPS developer guide](/intl.en-US/Development/PyODPS/Platform instructions/Use PyODPS in DataWorks.md).

If you have the interest to help build the PyODPS ecosystem, you can perform the following operations:

-   Cooperate in writing [PyODPS documentation](http://pyodps.readthedocs.io/zh_CN/latest/?spm=a2c4e.11153959.blogcont138752.16.5bec51d32BpKgB).
-   Develop PyODPS code at [GitHub](https://github.com/aliyun/aliyun-odps-python-sdk?spm=a2c4e.11153959.blogcont138752.17.5bec51d3IMNtLJ).
-   Join the DingTalk group for technical communication. To join, find and enter group number 11701793.

## Initialization

Before you use PyODPS, run the following command to initialize a connection to MaxCompute by using your Alibaba Cloud account:

```
from odps import ODPS
odps = ODPS('**your-access-id**', '**your-secret-access-key**', '**your-default-project**',endpoint='**your-end-point**')
```

Parameter description:

-   your-access-id: the AccessKey ID of your Alibaba Cloud account.
-   your-secret-access-key: the AccessKey secret of your Alibaba Cloud account.
-   your-default-project: the name of the project.
-   your-end-point: the endpoint of the region where your MaxCompute project resides. For more information, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).

After you initialize the connection, you can use PyODPS to manage MaxCompute objects, such as tables, resources, and functions.

## Method description

The following table lists the available basic methods that PyODPS provides for you to use on MaxCompute objects.

|Item|Method|Description|
|----|------|-----------|
|Project|get\_project\(project\_name\)|Obtains a project.|
|exist\_project\(project\_name\)|Checks whether a project exists.|
|Table|list\_tables\(\)|Lists all tables in a project.|
|exist\_table\(table\_name\)|Checks whether a table exists.|
|get\_table\(table\_name,project=project\_name\)|Obtains a specified table in the current project or another specified project.|
|create\_table\(\)|Creates a table.|
|read\_table\(\)|Reads data from a table.|
|write\_table\(\)|Writes data to a table.|
|delete\_table\(\)|Deletes a table.|
|Table partition|exist\_partition\(\)|Checks whether a partition exists.|
|get\_partition\(\)|Obtains a partition.|
|create\_partition\(\)|Creates a partition.|
|delete\_partition\(\)|Deletes a partition.|
|SQL|execute\_sql\(\)/run\_sql\(\)|Executes an SQL statement.|
|open\_reader\(\)|Reads execution results.|
|Instance|list\_instances\(\)|Lists all instances in a project.|
|exist\_instance\(\)|Checks whether an instance exists.|
|get\_instance\(\)|Obtains an instance.|
|stop\_instance\(\)|Terminates an instance.|
|Resource|create\_resource\(\)|Creates a resource.|
|open\_resource\(\)|Opens a resource.|
|get\_resource\(\)|Obtains a resource.|
|list\_resources\(\)|Lists all resources.|
|exist\_resource\(\)|Checks whether a resource exists.|
|delete\_resource\(\)|Deletes a resource.|
|Function|create\_function\(\)|Creates a function.|
|delete\_function\(\)|Deletes a function.|
|Data upload or download channel|create\_upload\_session\(\)|Creates a session that uploads data.|
|create\_download\_session\(\)|Creates a session that downloads data.|

