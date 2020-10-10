---
keyword: check permissions
---

# Check permissions

MaxCompute allows you to view multiple permissions, including the permissions of users or roles, and authorization lists of specified objects.

## Permission description

MaxCompute uses the markup characters A, C, D, and G when it shows the permissions of users or roles. Meanings of these markup characters:

-   A: Access allowed.
-   D: Access denied.
-   C: Access granted with conditions. It appears only in a policy authorization system.
-   G: Access granted with conditions. Permissions can be granted to objects.

Sample script:

```
odps@test_project> show grants for aliyun$odpstest1@aliyun.com;
[roles]
dev
Authorization Type: ACL
[role/dev]
A       projects/test_project/tables/t1: Select
[user/odpstest1@aliyun.com]
A       projects/test_project: CreateTable | CreateInstance | CreateFunction | List
A       projects/test_project/tables/t1: Describe | Select
Authorization Type: Policy
[role/dev]
AC      projects/test_project/tables/test_*: Describe
DC      projects/test_project/tables/alifinance_*: Select
[user/odpstest1@aliyun.com]
A       projects/test_project: Create* | List
AC      projects/test_project/tables/alipay_*: Describe | Select
Authorization Type: ObjectCreator
AG      projects/test_project/tables/t6: All
AG      projects/test_project/tables/t7: All
```

## View permissions of a specified user

-   To view permissions of the current user account, run the following command:

    ```
    show grants; 
    ```

-   To view permissions of the Alibaba Cloud account, run the following command:

    ```
    show grants for <username>; 
    ```

    For example, you can run the following command to view the permissions of the Alibaba Cloud account bob@aliyun.com in the current project:

    ```
    show grants for ALIYUN$bob@aliyun.com;
    ```

-   To view permissions of a RAM user, run the following command:

    ```
    show grants for RAM$Alibaba Cloud account:RAM user;
    ```

    For example, you can run the following command to view the permissions of the RAM user RAM$bob@aliyun.com:Alice in the current project:

    ```
    show grants for RAM$bob@aliyun.com:Alice;
    ```


## View permissions of a specified role

To view the access permissions granted to a specified role, run the following command:

```
describe role ; 
```

**Note:** describe role displays only ACL information of projects and tables. ACL information of other objects such as functions, resources, instances, and jobs is not displayed.

## View the authorization list of a specified object

To view the user and role authorization list of a specified object, run the following command:

```
show acl for <objectName> [on type <objectType>]; 
```

**Note:** When `[on type <objectType>]` is omitted, the default object type is Table.

