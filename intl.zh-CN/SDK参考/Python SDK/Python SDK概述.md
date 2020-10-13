---
keyword: Python SDK
---

# Python SDK概述

本文为您介绍Python SDK及其常见方法。

## 背景信息

PyODPS是MaxCompute的Python SDK，提供DataFrame框架和MaxCompute对象的基本操作方法。您可以通过MaxCompute轻松地分析数据。

PyODPS支持的底层Python版本为Python2（2.6以上版本）和Python3。

获取详细PyODPS信息的方式如下：

-   了解PyODPS：[PyODPS文档](http://pyodps.readthedocs.org/)和[PyODPS云栖社区专辑](https://yq.aliyun.com/album/19)。
-   下载odps-python-sdk：[Github](https://github.com/aliyun/aliyun-odps-python-sdk)。
-   安装PyODPS：[PyODPS安装指南](/intl.zh-CN/开发/PyODPS/安装指南及使用限制.md)。
-   开发PyODPS：[PyODPS开发指南](/intl.zh-CN/开发/PyODPS/工具平台使用指南/在DataWorks上使用PyODPS.md)。

您也可以通过如下方式参与PyODPS的生态开发：

-   编写PyODPS文档：[PyODPS](http://pyodps.readthedocs.io/zh_CN/latest/?spm=a2c4e.11153959.blogcont138752.16.5bec51d32BpKgB)。
-   开发PyODPS代码：[代码](https://github.com/aliyun/aliyun-odps-python-sdk?spm=a2c4e.11153959.blogcont138752.17.5bec51d3IMNtLJ)。
-   技术交流：加入钉钉技术交流群11701793。

## 初始化入口

在使用PyODPS前，您需要用阿里云账号初始化一个MaxCompute的入口，执行命令如下。

```
from odps import ODPS
odps = ODPS('**your-access-id**', '**your-secret-access-key**', '**your-default-project**',endpoint='**your-end-point**')
```

参数说明：

-   your-access-id：账号的AccessKey ID。
-   your-secret-access-key：账号的AccessKey Secret。
-   your-default-project：使用的项目空间名称。
-   your-end-point：MaxCompute服务所在区域的Endpoint。详情请参见[配置Endpoint](/intl.zh-CN/准备工作/配置Endpoint.md)。

完成上述操作即可对对象（例如，表、资源和函数）进行操作。

## 方法说明

PyODPS提供MaxCompute对象的基本操作方法，详情如下。

|操作类型|方法名称|方法说明|
|----|----|----|
|项目空间|get\_project\(project\_name\)|获取项目空间。|
|exist\_project\(project\_name\)|判断某个项目空间是否存在。|
|表|list\_tables\(\)|列出项目空间下的所有表。|
|exist\_table\(table\_name\)|判断表是否存在。|
|get\_table\(table\_name，project=project\_name\)|获取指定表。允许跨项目获取表。|
|create\_table\(\)|创建表。|
|read\_table\(\)|读取表数据。|
|write\_table\(\)|写入表数据。|
|delete\_table\(\)|删除已经存在的表。|
|表分区|exist\_partition\(\)|判断分区是否存在。|
|get\_partition\(\)|获取分区。|
|create\_partition\(\)|创建分区。|
|delete\_partition\(\)|删除分区。|
|SQL|execute\_sql\(\)/run\_sql\(\)|执行SQL语句。|
|open\_reader\(\)|读取执行结果。|
|任务实例|list\_instances\(\)|获取项目空间下的所有Instance。|
|exist\_instance\(\)|判断Instance是否存在。|
|get\_instance\(\)|获取Instance。|
|stop\_instance\(\)|停止Instance。|
|资源|create\_resource\(\)|创建资源。|
|open\_resource\(\)|打开资源。|
|get\_resource\(\)|获取资源。|
|list\_resources\(\)|列出所有资源。|
|exist\_resource\(\)|判断资源是否存在。|
|delete\_resource\(\)|删除资源。|
|函数|create\_function\(\)|创建函数。|
|delete\_function\(\)|删除函数。|
|数据上传下载通道|create\_upload\_session\(\)|创建上传数据会话。|
|create\_download\_session\(\)|创建下载数据会话。|

