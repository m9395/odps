---
keyword: manage permissions by using a RAM user
---

# Manage permissions by using a RAM user

This topic describes how to manage permissions by using a RAM user.

## Scenarios

An enterprise has purchased multiple Alibaba Cloud services such as MaxCompute. All the services share one Alibaba Cloud account. MaxCompute users are not responsible for the management of this Alibaba Cloud account. They manage permissions on MaxCompute projects by using RAM users. For example, a MaxCompute user can run the `add user` command to add a RAM user and run the `grant xx on project/table` command to authorize the RAM user.

## Background information

-   By default, the owner of a MaxCompute project must be an Alibaba Cloud account, and only the project owner can manage permissions on the MaxCompute project.
-   After you activate MaxCompute by using a RAM user and create a project, the project owner is still the Alibaba Cloud account.
-   In DataWorks, a RAM user is granted a project administrator or security administrator role. A RAM user only has the operation permissions on DataWorks workspaces, but does not have the permissions to manage MaxCompute projects. For more information, see [Permission relationships between MaxCompute and DataWorks](/intl.en-US/Management/Security management/Permission relationships between MaxCompute and DataWorks.md).

## Procedure

Specify a RAM user to manage the permissions on MaxCompute. Grant the RAM user the Super\_Administrator or Admin roles by using the Alibaba Cloud account.

```
-- For example, the Alibaba Cloud account is bob@aliyun.com, and the RAM user used for routine permission management is Allen.
-- Grant Allen the Admin role.
grant admin TO ram$bob@aliyun.com:Allen; 
-- Grant Allen the Super_Administrator role.
grant Super_Administrator TO ram$bob@aliyun.com:Allen; 
```

**Note:** The Admin role can manage routine permissions, but cannot manage all permissions as the project owner. Only the project owner has the permissions to grant roles to RAM users.

