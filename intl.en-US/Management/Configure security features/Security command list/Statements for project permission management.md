---
keyword: project permission management
---

# Statements for project permission management

This topic describes common statements for project permission management, such as user management, role management, ACL-based authorization, and permission review.

## User management

|Statement|Description|
|:--------|:----------|
|list users|Allows you to view all users that are added to the project.|
|add user `<username>`|Allows you to add a user.|
|remove user `<username>`|Allows you to remove a user.|

## Role management

|Statement|Description|
|:--------|:----------|
|list roles|Allows you to view all created roles.|
|create role `<rolename>`|Allows you to create a role.|
|drop role `<rolename>`|Allows you to delete a role.|
|grant `<rolelist>` to `<username>`|Allows you to assign one or multiple roles to a user.|
|revoke `<rolelist>` from `<username>`|Allows you to revoke roles from a user.|

## ACL-based authorization

|Statement|Description|
|:--------|:----------|
|grant `<privList>` on `<objType>``<objName>` to user `<username>`|Allows you to authorize a user.|
|grant `<privList>` on `<objType>``<objName>` to role `<rolename>`|Allows you to authorize a role.|
|revoke `<privList>` on `<objType>``<objName>` from user `<username>`|Allows you to revoke permissions from a user.|
|revoke `<privList>` on `<objType>``<objName>` from role `<rolename>`|Allows you to revoke permissions from a role.|

## Permission review

|Statement|Description|
|:--------|:----------|
|whoami|Allows you to view information about a user.|
|show grants \[for `<username>`\] \[on type `<objectType>`\]|Allows you to view permissions and roles of a user.|
|show acl for `<objectName>` \[on type `<objectType>`\]|Allows you to view the authorization information of an object.|
|describe role `<roleName>`|Allows you to view the authorization and assignment information of a role.|

