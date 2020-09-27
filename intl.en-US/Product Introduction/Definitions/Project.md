---
keyword: [project, user isolation, access control]
---

# Project

A project is a basic organizational unit of MaxCompute. A project in MaxCompute is similar to a database or schema in a traditional database management system. Projects are used to isolate users and manage access requests. A project contains multiple objects, such as tables, resources, functions, and instances.

You can have permissions to manage multiple projects. You can access objects of another project from your project after relevant security authorization. For more information, see [Resource sharing across projects based on package](/intl.en-US/Management/Configure security features/Resource share across project space/Resource sharing across projects based on package.md).

You can run the use project; command to enter a project. The following command shows an example:

```
-- Enter a project named my_project.
use my_project;
```

After you run the preceding command, you can enter a project named my\_project and manage objects such as tables, resources, functions, and instances in the project. The use project; command is provided by the MaxCompute client. For more information about other commands provided by MaxCompute, see [Common MaxCompute commands](/intl.en-US/Development/Common commands/List of common commands.md).

**Note:** A project in MaxCompute is associated with a workspace in DataWorks. For more information, see [Basic mode and standard mode]().

