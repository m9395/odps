---
keyword: [Super\_Administrator, Admin]
---

# Management role permissions

After a project is created in MaxCompute, this project can be managed by the project owner and two default roles \(Super\_Administrator and Admin\). This topic describes the three roles that are used to manage a project created in MaxCompute.

## Background information

The following roles have permissions to manage a project created in MaxCompute:

-   Project owner: has all permissions on a project.
-   Super\_Administrator: has permissions on all types of resources in a project and all management permissions. This is a built-in administrator role.
-   Admin: has permissions on all types of resources in a project and some basic management permissions. This is also a built-in administrator role.

## Permissions

The following table describes the permissions of these roles.

|Permission type|Object|Action|Description|Project owner|Super\_Administrator|Admin|
|---------------|------|------|-----------|-------------|--------------------|-----|
|Security configuration|project|SetSecurityConfiguration|Configure the security information of a project.|Yes|Yes|N/A|
|project|GetSecurityConfiguration|Query the security configuration of a project.|Yes|Yes|Yes|
|Management of a protected project|project|AddTrustedProject|Add a protected project.|Yes|Yes|N/A|
|project|RemoveTrustedProject|Remove a protected project.|Yes|Yes|N/A|
|project|ListTrustedProjects|List protected projects.|Yes|Yes|Yes|
|User management|project|AddUser|Add a user.|Yes|Yes|Yes|
|project|RemoveUser|Remove a user.|Yes|Yes|Yes|
|project|ListUsers|List users.|Yes|Yes|Yes|
|project|ListUserRoles|List the roles assigned to a user.|Yes|Yes|Yes|
|Role management|project|CreateRole|Create a role.|Yes|Yes|Yes|
|project|DescribeRole|Query a role.|Yes|Yes|Yes|
|project|AlterRole|Modify properties of a role.|Yes|Yes|Yes|
|project|DropRole|Delete a role.|Yes|Yes|Yes|
|project|ListRoles|List roles.|Yes|Yes|Yes|
|Role authorization|role|GrantRole|Grant a role to a user.|Yes|Yes|Yes|
|role|RevokeRole|Revoke a role from a user.|Yes|Yes|Yes|
|role|ListRolePrincipals|List the roles assigned to a user.|Yes|Yes|Yes|
|Package management|project|CreatePackage|Create a package.|Yes|Yes|N/A|
|project|ShowPackages|List packages.|Yes|Yes|N/A|
|package|DescribePackage|Query a package.|Yes|Yes|Yes|
|package|DropPackage|Delete a package.|Yes|Yes|N/A|
|package|InstallPackage|Install a package.|Yes|Yes|Yes|
|package|UninstallPackage|Uninstall a package.|Yes|Yes|Yes|
|package|AllowInstallPackage|Allow other projects to use a package.|Yes|Yes|N/A|
|package|DisallowInstallPackage|Disallow other projects to use a package.|Yes|Yes|N/A|
|package|AddPackageResource|Add resources to a package.|Yes|Yes|N/A|
|package|RemovePackageResource|Remove resources from a package.|Yes|Yes|N/A|
|Label grant control|table|GrantLabel|Grant a label.|Yes|Yes|Yes|
|table|RevokeLabel|Revoke a label.|Yes|Yes|Yes|
|table|ShowLabelGrants|Query label grants.|Yes|Yes|Yes|
|table|SetDataLabel|Set labels for users and roles.|Yes|Yes|Yes|
|Clearance of expired permissions|project|ClearExpiredGrants|Clear expired permissions.|Yes|Yes|Yes|

