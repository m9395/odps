---
keyword: [Policy权限控制, Download权限控制]
---

# Policy和Download权限控制

本文为您介绍如何授权或撤销Policy和Download权限。

## Policy权限控制（GRANT方式）

Policy授权和撤销语法格式如下。

```
GRANT [privileges] ON <objectType> <objectName> to role <rolename> privilegeproperties("policy" = "true", "allow"="[true|false]", "conditions"= "acs:SourceIp in ('192.168.0.0/16','172.12.0.0/16') and 'odps:InstanceId'='aaaaaa'");
REVOKE  [privileges] ON <objectType> <objectName> from role <rolename> privilegeproperties ("policy" = "true", "allow"="[true|false]");
```

语法说明：

-   Policy只支持授权给角色（Role），不支持授权给用户（User）。
-   privileges表示操作类型（Action）。例如读或写，详情请参见[授权](/intl.zh-CN/管理/安全管理详解/用户及授权管理/授权.md)。
-   objectType表示客体类型（Object）。例如项目或表，详情请参见[授权](/intl.zh-CN/管理/安全管理详解/用户及授权管理/授权.md)。
-   objectName表示客体名称。
-   rolename表示角色名称。
-   privilegeproperties中的`{"policy" = "true"}`表示授权方式为Policy授权。
-   privilegeproperties的授权效力包括允许操作（`allow`）和拒绝操作（`deny`）。通常，`deny`具有更高效力。如果同时赋予`allow`和`deny`权限，`deny`会优先生效，`allow`不生效。
-   privilegeproperties中的`{"allow"="[true|false]"}`表示白名单形式授权。黑名单形式授权为`{"deny"="[true|false]"}`。
-   撤销授权（`REVOKE`）只有在allow、objectName和rolename三个参数同时匹配某一角色的权限信息时才会生效。
-   拒绝操作（`deny`）和撤销授权（Revoke）是完全独立的两个概念。拒绝操作（`deny`）可以为某个角色授予某种操作权限。撤销授权（Revoke）是撤销已授予某个角色的某种操作权限，可以撤销角色被赋予的`allow`或`deny`权限。

示例如下：

-   示例一

    为aliyun\_test角色授予dataworks\_test项目的只读权限。授权后，aliyun\_test角色即可查看dataworks\_test项目的信息。

    ```
    GRANT READ ON PROJECT dataworks_test to role aliyun_test privilegeproperties("policy" = "true", "allow"="true");
    ```

-   示例二

    为aliyun\_test角色授予MaxCompute项目中所有表的只读权限。授权后，aliyun\_test角色即可查看MaxCompute项目的所有表。

    ```
    GRANT Select ON TABLE * to role aliyun_test privilegeproperties("policy" = "true", "allow"="true");
    ```

-   示例三

    为aliyun\_test角色授予禁止删除MaxCompute项目中所有表的权限。授权后，aliyun\_test角色无法删除MaxCompute项目的所有表。

    ```
    GRANT DROP ON TABLE * to role aliyun_test privilegeproperties("policy" = "true", "allow"="false");
    ```

-   示例四

    为aliyun\_test角色撤销禁止删除MaxCompute项目中所有表的权限。撤销授权后，aliyun\_test角色可以删除MaxCompute项目的所有表。

    ```
    REVOKE DROP ON TABLE * from role aliyun_test privilegeproperties ("policy" = "true", "allow"="false");
    ```


## Download权限控制

为管控使用Tunnel下载数据的行为，权限模型新增Download权限控制。使用Tunnel下载数据时，您需要拥有Download权限。

语法格式如下。

```
GRANT DOWNLOAD ON <objectType> <objectName> to [role|user] <name>；
```

语法说明：

-   Download权限安全等级较高，需要项目所属者（Project Owner）或拥有Super\_Administrator角色的用户才可以执行Download授权。
-   只支持对表执行Download授权。

