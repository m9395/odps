---
keyword: [resource sharing, package]
---

# Statements for package-based resource sharing

This topic describes the statements used for package-based resource sharing.

## Resource sharing

|Statement|Description|
|---------|-----------|
|create package `<pkgName>`|Allows you to create a package.|
|delete package `<pkgName>`|Allows you to delete a package.|
|add `<objType><objName>` to package `<pkgName>` \[with privileges privs\]|Allows you to add resources you want to share to a package.|
|remove `<objType><objName>` from package `<pkgName>`|Allows you to remove shared resources from a package.|
|allow project `<prjName>` to install package `<pkgName>` \[using label `<num>`\]|Allows you to grant a project to use your package.|
|disallow project `<prjName>` to install package `<pkgName>`|Allows you to revoke a project from your package.|

## Resource usage

|Statement|Description|
|:--------|:----------|
|install package `<pkgName>`|Allows you to install a package.|
|uninstall package `<pkgName>`|Allows you to uninstall a package.|

## Package information query

|Statement|Description|
|:--------|:----------|
|show packages|Allows you to view all of the created and installed packages.|
|describe package `<pkgName>`|Allows you to view the details of a package.|

