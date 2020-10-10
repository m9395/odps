---
keyword: [add a resource, remove a resource, view a resource list, download resources]
---

# Resource operations

This topic describes how to use common commands to manage resources on the MaxCompute client.

You can also search and upload resources by using the visualized online data development tools offered in DataWorks. For more information, see [Resource]().

## Add a resource

**Syntax**

```
add file <local_file> [as alias] [comment 'cmt'][-f];
add archive <local_file> [as alias] [comment 'cmt'][-f];
add table <table_name> [partition (spec)] [as alias] [comment 'cmt'][-f];
add <PY | JAR> <localfile[.py |.jar]> [COMMENT 'cmt'][-f];
```

**Description**

Adds a resource to MaxCompute.

**Parameters**

-   file/archive/table/jar/py: specifies the resource type. For more information, see [Resource](/intl.en-US/Product Introduction/Definitions/Resource.md).
-   local\_file: specifies the path of the local file. The file name is used as the resource name, which uniquely identifies a resource.
-   table\_name: specifies the table name in MaxCompute. External tables cannot be added as resources.
-   \[partition \(spec\)\]: If the resource you want to add is a partitioned table, MaxCompute considers only a partition as a resource, not the entire partitioned table.
-   alias: specifies a resource name. If this parameter is not specified, the file name is used as a resource name by default. Jar and Python resources do not support this parameter.
-   \[comment'cmt'\]: adds a comment to a resource.
-   \[-f\]: overwrites the original resource that has the same name as the current resource. If this parameter is not specified and a duplicate resource name exists, the operation fails.

**Example**

```
-- Add a partitioned table resource named sale.res to MaxCompute.
odps@ odps_public_dev>add table sale_detail partition (ds='20150602') as sale.res comment 'sale detail on 20150602' -f;
OK: Resource 'sale.res' have been updated.
```

**Note:** The size of each resource file cannot exceed 500 MB. The resource size referenced by a single SQL or MapReduce task cannot exceed 2,048 MB. For more limits, see [Limits](/intl.en-US/Development/MapReduce/Limits.md).

## View a resource list

**Syntax**

```
LIST RESOURCES;
```

**Description**

Views all resources in the current project.

**Example**

```
odps@ $project_name>list resources;
Resource Name      Comment      Last Modified Time        Type
1234.txt                        2014-02-27 07:07:56       file
mapred.jar                      2014-02-27 07:07:57       jar
```

## Download resources

**Syntax**

```
GET RESOURCE <resource_name> <path>;
```

**Description**

Downloads MaxCompute resources to your local device. The resource type must be FILE, JAR, ARCHIVE, or PY.

**Parameters**

-   resource\_name: specifies the name of the resource you want to download.
-   path: specifies the path used to save a resource to your local device.

**Example**

```
odps@ $project_name>get resource odps-udf-examples.jar d:\;
OK
```

## Remove a resource

**Syntax**

```
DROP RESOURCE <resource_name>; 
```

**Description**

Removes an existing resource.

**Parameters**

resource\_name: specifies the name of the resource you want to remove. The resource name is specified when you create the resource.

