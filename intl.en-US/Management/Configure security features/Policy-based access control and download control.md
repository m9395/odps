---
keyword: [policy-based access control, download control]
---

# Policy-based access control and download control

This topic describes how to use the policy-based access control and download control features.

## Policy-based access control by using the GRANT statement

GRANT and REVOKE syntax:

```
GRANT [privileges] ON <objectType> <objectName> to role <rolename> privilegeproperties("policy" = "true", "allow"="[true|false]", "conditions"= "acs:SourceIp in ('192.168.0.0/16','172.12.0.0/16') and 'odps:InstanceId'='aaaaaa'");
REVOKE  [privileges] ON <objectType> <objectName> from role <rolename> privilegeproperties ("policy" = "true", "allow"="[true|false]");
```

Description:

-   You can use policy-based access control to grant permissions only to roles.
-   privileges: the type of the action, such as read or write. For more information, see [Authorize users](/intl.en-US/Management/Configure security features/Manage users and permissions/Authorize users.md).
-   objectType: the type of the object, such as a project or table. For more information, see [Authorize users](/intl.en-US/Management/Configure security features/Manage users and permissions/Authorize users.md).
-   objectName: the name of the project.
-   rolename: the name of the role.
-   The `{"policy" = "true"}` field in the privilegeproperties parameter indicates that policy-based access control is used.
-   You can specify `allow` or `deny` in the privilegeproperties parameter to allow or deny the specified action. In most cases, `deny` has a higher priority. If an action is both `allowed` and `denied` for the same role, the settings of `deny` takes effect.
-   The `{"allow"="[true|false]"}` field in the privilegeproperties parameter specifies whether to allow the specified action. The `{"deny"="[true|false]"}` field specifies whether to deny the specified action.
-   You can execute a `REVOKE` statement only when the allow, objectName, and rolename parameters in the statement match the information of a granted permission.
-   Do not mix up the concepts: `deny` and REVOKE. They are independent of each other. In a statement, `deny` specifies whether to deny an action for a role. The REVOKE statement is used to revoke a permission that has been granted to a role. The permission may be about an `allowed` or a `denied` action.

Examples

-   Example 1

    Grant the aliyun\_test role the read-only permission on the dataworks\_test project. After authorization, the aliyun\_test role can view information about the dataworks\_test project.

    ```
    GRANT READ ON PROJECT dataworks_test to role aliyun_test privilegeproperties("policy" = "true", "allow"="true");
    ```

-   Example 2

    Grant the aliyun\_test role the read-only permission on all tables in a MaxCompute project. After authorization, the aliyun\_test role can view all tables in the project.

    ```
    GRANT Select ON TABLE * to role aliyun_test privilegeproperties("policy" = "true", "allow"="true");
    ```

-   Example 3

    Prohibit the aliyun\_test role from deleting tables from a MaxCompute project.

    ```
    GRANT DROP ON TABLE * to role aliyun_test privilegeproperties("policy" = "true", "allow"="false");
    ```

-   Example 4

    Revoke the preceding prohibition operation to allow the aliyun\_test role to delete tables from a MaxCompute project.

    ```
    REVOKE DROP ON TABLE * from role aliyun_test privilegeproperties ("policy" = "true", "allow"="false");
    ```


## Download control

To control Tunnel-based data downloads, download control is provided for the permission model. Download permissions are required for you to download data by using Tunnel.

Syntax:

```
GRANT DOWNLOAD ON <objectType> <objectName> to [role|user] <name>;
```

Description:

-   Only the project owner or a user who is assigned the Super\_Administrator role can grant download permissions.
-   Download permissions can be granted only on tables.

