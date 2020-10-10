---
keyword: project data protection
---

# Project data protection

This topic describes the mechanism used for project data protection and how to export data after project data protection is enabled.

## Background information

Some enterprises have high data security requirements. They take various measures to prevent leaks of sensitive data. For example, their employees can perform their jobs only in the workplace, and are not allowed to take work materials out of the office. All USB ports on office computers are disabled.

As a MaxCompute project administrator, you may also encounter similar situations where users are not allowed to transfer data out of a project.

If user Alice has access permissions on both Project1 and Project2, as shown in the following figure, Alice may transfer sensitive data from Project1 to Project2.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5243948951/p2795.png)

If Alice has the SELECT permission on myprj.table1 and the CREATE TABLE permission on Project2, Alice can transfer data from Project1 to Project2 by using the following SQL statement:

```
create table prj2.table2 as select * from myprj.table1;
```

To control the outflow of data in a project, MaxCompute offers a data protection mechanism.

## Data protection

Users who are authorized to access multiple projects can perform cross-project data access operations to transfer data. If a project stores highly-sensitive data, the administrator must configure the project protection mechanism.

To enable project data protection, run the following command in the project:

```
set projectProtection=true;
```

Command description:

-   The default value of the ProjectProtection parameter is false.
-   After the ProjectProtection parameter is set to true, only the inflow of data is allowed.
-   Cross-project data access operations fail because they violate the project protection rule.
-   Project protection controls data flows, but not data access. Data flow control is effective only if users can access their desired data.

## Data outflow after project protection is enabled

After project protection is enabled for a project, MaxCompute provides two methods for data outflow.

-   Set an exception policy
    -   Method

        When a project owner enables project protection, the owner can set an exception policy by running the following command:

        ```
        SET ProjectProtection=true WITH EXCEPTION <policyFile>
        ```

        Example of policyFile:

        The following code snippet shows that the Alibaba Cloud account Alice@aliyun.com can export data out of the alipay project when this account is used to perform the SELECT operation on the alipay.table\_test table in an SQL task.

        ```
            {
            "Version": "1",
            "Statement":
            [{
                "Effect":"Allow",
                "Principal":"ALIYUN$Alice@aliyun.com",
                "Action":["odps:Select"],
                "Resource":"acs:odps:*:projects/alipay/tables/table_test",
                "Condition":{
                    "StringEquals": {
                        "odps:TaskType":["DT", "SQL"]
                    }
                }
            }]
            }
        ```

        **Note:** `odps:TaskType` includes DT, SQL, and MapReduce. DT refers to tunnels \([Batch data tunnel](/intl.en-US/Development/Data upload and download/Tunnel SDK/MaxCompute Tunnel overview.md)\), which includes the encapsulation of Tunnel SDK, such as DataWorks data integration and open source DataX.

        The exception policy is not a common authorization method. If Alice does not have the SELECT permission on the alipay.table\_test table, Alice cannot export data even if the preceding exception policy is configured.

        Even though an exception policy shares the same syntax as a policy, Authorization based on an exception policy is different from that based on a policy. An exception policy describes an exception of the project protection mechanism. Any access request that conforms to the exception policy is ignored by the project protection rule.

        You can run the following command to check whether exceptions exist:

        ```
        show SecurityConfiguration;
        ```

    -   This method may cause data leaks due to a time-of-check to time-of-use \(TOCTOU\) bug, which is also known as the race condition:
        -   Problem description:
            1.  \[TOC stage\] User A submits an application to the project owner to export table t1. After the project owner verifies that table t1 does not contain sensitive data, the owner configures an exception policy to authorize user A to export table t1.
            2.  Before the TOU stage starts, a malicious user writes sensitive data to table t1.
            3.  \[TOU stage\] User A exports table t1. However, table t1 exported by user A is not the same as that authorized by the project owner.
        -   Solution:

            To prevent this issue, we recommend that the project owner must ensure that no other users \(including the administrator\) can update the table \(UPDATE\) or create a table with the same name \(DROP + CREATE TABLE\) after the owner receives the application to export a table. In the preceding example, we recommend that the project owner create a snapshot of t1 in the TOC stage, and then use this snapshot to set the exception policy. Additionally, no other users are granted the admin role.

-   Configure a trusted project

    If the current project is protected, data can be exported to its trusted project. This export does not violate the project protection rule. If multiple projects are mutually configured as trusted projects, they form a trusted project group. Data can flow only within this project group.

    Commands to manage trusted projects:

    -   To query all trusted projects of the current project, run the following command:

        ```
         list trustedprojects;                  
        ```

    -   To add a trusted project to the current project, run the following command:

        ```
         add trustedproject <projectname>;               
        ```

    -   To remove a trusted project from the current project, run the following command:

        ```
         remove trustedproject <projectname>;            
        ```

    **Note:**

    In MaxCompute, package-based resource sharing across projects described in [Resource sharing across projects based on package](/intl.en-US/Management/Configure security features/Resource share across project space/Resource sharing across projects based on package.md) and project protection are independent mechanisms that take effect at the same time, but their functions are mutually exclusive.

    Resource sharing takes precedence over project protection. If a data object is made accessible to users from other projects by using resource sharing, the object is not subject to project protection rules.


## Best practices

To control the outflow of data, you must set `ProjectProtection` to true and verify the following configurations:

-   No trusted projects are added. If a trusted project is added, you must assess potential risks.
-   No data sharing packages are used. If a data sharing package is used, you must ensure that the package does not contain sensitive data.

