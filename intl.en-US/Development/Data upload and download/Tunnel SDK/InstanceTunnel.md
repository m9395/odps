---
keyword: InstanceTunnel
---

# InstanceTunnel

This topic describes how to use InstanceTunnel to download the execution results of a SELECT statement.

## Description

The following code snippet describes InstanceTunnel. For more information, visit [Java-sdk-doc](https://www.javadoc.io/doc/com.aliyun.odps/odps-sdk-core/0.31.3-public).

```
public class InstanceTunnel{
 public DownloadSession createDownloadSession(String projectName, String instanceID);
 public DownloadSession createDownloadSession(String projectName, String instanceID, boolean limitEnabled);
 public DownloadSession getDownloadSession(String projectName, String id);
 }
```

Parameters:

-   projectName: the name of a project.
-   instanceID: the ID of an instance.

## Limits

Although InstanceTunnel provides an easy way to obtain instance execution results, it is subject to the following permission limits to ensure data security:

-   If the number of records does not exceed 10,000, all users who have read permissions on the specific instance can use InstanceTunnel to download the data. The same rule applies to data query by calling a RESTful API.
-   If the number of records exceeds 10,000, only users who have read permissions on all the source tables in the SQL query statement can use InstanceTunnel to download the data.

