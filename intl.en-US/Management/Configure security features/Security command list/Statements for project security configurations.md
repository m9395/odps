---
keyword: project security configurations
---

# Statements for project security configurations

This topic describes authentication configurations and common statements for data protection in project security configurations.

## Authentication configurations

|Statement|Description|
|:--------|:----------|
|show SecurityConfiguration|Allows you to view the security configurations of a project.|
|set CheckPermissionUsingACL=true/false|Allows you to enable or disable ACL-based authorization.|
|set CheckPermissionUsingPolicy=true/false|Allows you to enable or disable policy-based authorization.|
|set ObjectCreatorHasAccessPermission=true/false|Allows you to grant/revoke default access permissions to/from object creators.|
|set ObjectCreatorHasGrantPermission=true/false|Allows you to grant/revoke default authorization permissions to/from object creators.|

## Data protection

|Statement|Description|
|:--------|:----------|
|set ProjectProtection=false|Allows you to disable project protection.|
|list TrustedProjects|Allows you to view the list of trusted projects.|
|add TrustedProject `<projectName>`|Allows you to add a project to trusted projects.|
|remove TrustedProject `<projectName>`|Allows you to remove a project from trusted projects.|

