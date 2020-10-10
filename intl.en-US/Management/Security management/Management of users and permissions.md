---
keyword: management of users and permissions
---

# Management of users and permissions

This topic describes how to manage users and permissions in both MaxCompute and DataWorks.

For more information about management of users and permissions, see [Manage users](/intl.en-US/Management/Configure security features/Manage users and permissions/Manage users.md).

## User management

|Item|MaxCompute|DataWorks|
|----|----------|---------|
|Operation|Add and manage users. Delete or lock ownerless accounts, inactive accounts, and accounts of resigned personnel. **Note:** Users added on DataWorks are assigned default roles.

|Add and manage users. Delete or lock ownerless accounts, inactive accounts, and accounts of resigned personnel. Strictly control the permissions of administrators and OAM roles.|
|Role|Project owner, Super\_Administrator, or Admin.|Project administrator.|
|View details|-   To query the users of a project, run the list users; command.
-   To query the permissions of a user, run the show grants for <username\>; command.

|To view the members and roles in a workspace and check the validity of the permissions of each member, log on to the DataWorks console and choose [Workspace Management](https://setting-cn-beijing.data.aliyun.com/#/) \> **User Management**.|
|Grant permissions|Members are only added to a MaxCompute project and do not have any permissions. Granting permissions needs to work with **object actions**, **role permissions**, and **label permissions**. When you manage permissions, you must ensure that the members have the appropriate permissions and revoke unnecessary permissions. You can also add Alibaba Cloud accounts and RAM users.

To add a user to a project, run the add user <username\>; command.

|To add members and assign roles, log on to the DataWorks console and choose [Workspace Management](https://setting-cn-beijing.data.aliyun.com/#/)**User Management**. **Note:**

-   You can add only RAM users under the project owner role as project members.
-   After you add a member and assign it a role, this member may be granted the default permissions of the role in MaxCompute. |
|Roll back settings|To remove a user from a project, run the remove user <username\>; command.|Revoke the permissions from members or roles in DataWorks. After you revoke the permissions from a member or role, the system automatically deletes the users and roles from MaxCompute.|

## Role management

|Item|MaxCompute|DataWorks|
|----|----------|---------|
|Operation|Create roles and grant permissions to those roles. Promptly delete the accounts of resigned or transferred personnel and revoke unnecessary resources and permissions from roles.

In addition to the default Admin role of a MaxCompute project, [other roles](/intl.en-US/Management/Security management/Permission relationships between MaxCompute and DataWorks.md) are created in DataWorks.

|Assign roles. Promptly change the roles of members that no longer belong to a project. Strictly control the assignment of project administrators and OAM roles.|
|Role|Project owner, Super\_Administrator, or Admin.|Project administrator.|
|View details|-   To query all the roles in a project, run the list roles; command.
-   To query the permissions of a role, run the describe role <role\_name\>; command.
-   To query the roles assigned to a user, run the show grants for <username\>; command.

You cannot query users who are assigned a specific role.

|To query the members who are assigned a specific role, log on to the DataWorks console and choose [Workspace Management](https://setting-cn-beijing.data.aliyun.com/#/) \> **User Management**.|
|Grant permissions|In addition to the default roles provided by MaxCompute, you can define other roles, customize role permissions, and assign roles to users.

1.  To create a role, run the create role <role\_name\>; command.
2.  To grant permissions to the role, run the grant actions on object to <role\_name\>; command.
3.  To grant the role to a user, run the GRANT <role\_name\> TO <full\_username\> ; command.

**Note:**

You can also log on to the DataWorks console and choose [Workspace Management](https://setting-cn-hangzhou.data.aliyun.com/) \> **Maxcompute Management** \> **Custom User Roles** to create MaxCompute roles, grant permissions to roles, and assign roles to members.

Roles created by using the CLI are not displayed on this page.

|You cannot define DataWorks roles. When you add a member to a DataWorks project, you can select the roles that you want to assign to the member to grant the permissions of those roles to that member.|
|Roll back settings|1.  To delete a user who is assigned a role, run the REVOKE <roleName\> FROM <full\_username\>; command.
2.  To revoke the permissions granted to the role, run the revoke <privList\> on <objType\> <objName\> from role <rolename\>; command.
3.  To delete the role, run the DROP ROLE <roleName\>; command.

**Note:** If you create a role on the page displayed after you choose [Workspace Management](https://setting-cn-hangzhou.data.aliyun.com/) \> **Maxcompute Management** \> **Custom User Roles** in DataWorks, you can roll the operation back on this page.

|DataWorks roles cannot be removed. You can remove only the roles of a member.|

## ACL-based permission assignment

|Item|Description|
|----|-----------|
|Operation|Revoke unnecessary action permissions on objects. If the permissions involve multiple operation objects and types, verify the objects and types first.|
|Role|Project owner, Super\_Administrator, or Admin.|
|View details|-   To query the permissions of a user, run the show grants for <username\>; command.
-   To query the permissions of the current user, run the show grants; command.
-   To query the permission list of an object, run the show acl for <objectName\> \[on type <objectType\>\]; command.
-   To query the permissions granted to a package, run the show acl for alipaydw.alipaydw\_for\_alisec\_app on type package; command. |
|Grant permissions|To grant action permissions on an object, run the grant actions on object to subject; command.

Expressions of subject, object, and action types:

-   Action: action\_item1, action\_item2, ....
-   Object: project project\_name,table schema\_name ,instance inst\_name ,function func\_name ,resource res\_name.
-   Subject: user full\_username ,role role\_name. |
|Roll back settings|To revoke action permissions on an object, run the revoke actions on object from subject; command.|

## Package-based permission assignment

|Item|Description|
|----|-----------|
|Operation|If projects that have [project protection](/intl.en-US/Management/Configure security features/Project data protection.md) enabled are not in the same trusted project group, permissions must be granted by using packages. Ensure that the package contains only the required resources, and is assigned the least required permissions.|
|Role|Project owner, Super\_Administrator, or Admin.|
|View details|-   To view the packages for the current project and the permissions included in these packages:
    -   To query the list of packages created and installed, run the show packages; command.
    -   To query detailed information about a package, run the describe package <pkgname\>; command.
-   To view the permissions granted to a user by using packages installed for a project, run the show acl for <project\_name.package\_name\> on type package; command. |
|Grant permissions|For a package creator: 1.  To create a package, run the create package <pkgname\>; command.
2.  To add resources you want to share to the package, run the add project\_object to package package\_name \[with privileges privileges\]; command. The project\_object expression is table table\_name ,instance inst\_name ,function func\_name ,resource res\_name.
3.  To authorize other projects to use the package, run the allow project <prjname\> to install package <pkgname\> \[using label<number\>\]; command.

For a package user: 1.  To install a package, run the install package <pkgname\>; command.
2.  To grant permissions on the package to a user or role, run the grant actions on package <pkgName\> to user <username\>; or grant actions on package <pkgName\> to role <role\_name\>; command. The project owner, Super\_Administrator, or Admin can grant permissions on the package to a user or role.

**Note:** When you grant permissions on the package to a user, you are not allowed to specify a label.


For more information about action permissions, see [Authorize users](/intl.en-US/Management/Configure security features/Manage users and permissions/Authorize users.md). Typically, to enable a user to access the resources in a package, you only need to grant the user READ permissions on the package. After the permissions are granted to access a table in the package, you must write the table name in the format of `Name of the project to which the table belongs. Table name`. |
|Roll back settings|1.  To revoke the permissions granted for other projects to use a package, run the disallow project <prjname\> to install package <pkgname\>; command.
2.  To delete the package, run the delete package <pkgname\>; command.
3.  To remove shared resources from the package, run the remove project\_object from package package\_name; command.

The project\_object expression is table table\_name ,instance inst\_name ,function func\_name ,resource res\_name.

4.  To revoke permissions on the package from a user or role, run the revoke actions on package <pkgName\> from user <username\>;revoke actions on package <pkgName\> from role <role\_name\>; command. |

## Label-based permission assignment

|Item|Description|
|----|-----------|
|Operation|Set labels for fields, tables, and packages that fall into the sensitivity levels of 0, 1, 2, 3, and 4 in MaxCompute to grant permissions to users as required.|
|Role|Project owner and Super\_Administrator.|
|View details|-   To check which sensitive data sets are accessible to a user, run the SHOW LABEL \[<level\>\] GRANTS \[FOR USER <username\>\]; command.
    -   If \[FOR USER <username\>\] is not specified, the sensitive data sets accessible to the current user are returned.
    -   If <level\> is not specified, the permissions on labels at all levels are returned.
    -   If <level\> is specified, only the permissions on the labels of the specified level are returned.
-   To check which users can access a table that contains sensitive data, run the SHOW LABEL \[<level\>\] GRANTS ON TABLE <tablename\>; command. The label-authorized permissions on a specific table are returned.
-   To query all the column-level and label-authorized permissions that a user has on a data table, run the SHOW LABEL \[<level\>\] GRANTS ON TABLE <tablename\> FOR USER <username\>; command. The [label-authorized permissions](/intl.en-US/Management/Configure security features/Column-level access control.md) of a specific user to access the columns of a specific table are returned. |
|Grant permissions|-   To grant a label for a table or field to a user, run the GRANT LABEL <number\> ON TABLE <tablename\> \[\(column\_list\)\] TO \[USER\|ROLE\] <name\> \[WITH EXP <days\>\]; command. The default value of days in \[WITH EXP <days\>\] is 180. Examples:
    -   If you run the GRANT LABEL 2 ON TABLE t1 TO USER alice WITH EXP 1; command, the user named alice is granted the permissions to access data whose sensitivity level is 2 or lower in the t1 table. These permissions remain valid for one day.
    -   If you run the GRANT LABEL 3 ON TABLE t1\(col1, col2\) TO USER alice WITH EXP 1; command, the user named alice is granted the permissions to access data whose sensitivity level is 3 or lower in columns 1 and 2 in the t1 table, namely, `t1(col1, col2)`. These permissions remain valid for one day.
-   To grant a project label to a user, run the SET LABEL <number\> TO USER <username\>; command.
-   To manage a label granted to a package installer for accessing sensitive resources in a package, run the ALLOW PROJECT <prjName\> TO INSTALL PACKAGE <pkgName\> \[USING LABEL <number\>\]; command. A package creator grants a label for accessing sensitive resources in a package to a package installer.
-   To grant permissions on a package to a user or role, run the grant actions on package <pkgName\> to user <username\>; or grant actions on package <pkgName\> to role <role\_name\>; command. When you grant permissions on a package to a user, you cannot specify a label. |
|Roll back settings|-   Revoke a label for a table or field from a user.
    -   To revoke a label, run the REVOKE LABEL ON TABLE <tablename\>\[\(column\_list\)\] FROM \[USER\|ROLE\] <name\>; command.

To revoke the label that allows the user named alice to access sensitive data in the t1 table, run the REVOKE LABEL ON TABLE t1 FROM USER alice; command.

    -   To delete expired permissions, run the CLEAR EXPIRED GRANTS; command.
-   To change a project label granted to a user, run the SET LABEL <number\> TO \[USER\|ROLE\] <name\>; command. In this command, the default value of number is 0.
-   To change a label granted to a package installer for accessing sensitive resources in a package, run the ALLOW PROJECT <prjName\> TO INSTALL PACKAGE <pkgName\> \[USING LABEL <number\>\]; command. In this command, the default value of number is 0.
-   To revoke permissions on a package from a user or role, run the revoke actions on package <pkgName\> from user <username\>; or revoke actions on package <pkgName\> from role <role\_name\>; command. |

