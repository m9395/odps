---
keyword: [enter a project, view projects, specify project properties, configure IP address whitelist]
---

# Project operations

This topic describes how to enter a project, view projects, specify project properties, and configure IP address whitelists for projects.

## Enter a project

-   Syntax

    ```
    use <project_name>;
    ```

-   Description
    -   You can run this command to enter a specific project. You must have access permissions on the project. After you enter the project, you can manage all objects in the project.
    -   If the project that you specify does not exist or you do not have access permissions on the project, an error is returned.
-   Examples
    -   Enter a specific project.

        ```
        -- Enter the my_project_test project. The user has access permissions on this project.
        odps @ my_project>use my_project_test;
        ```

    -   Access an object in a specific project.

        ```
        -- Query the test_src table in the my_project_test project.
        odps @ my_project_test>SELECT * FROM test_src;
        ```

        MaxCompute automatically searches for the table in the my\_project\_test project. If this table exists, data in the table is returned. If this table does not exist, an error is returned.

    -   Access an object in another project. To achieve this, you must specify the project name.

        ```
        -- Access the test_src table in the my_project2 project from the my_project_test project.
        odps @ my_project_test>select * from my_project2.test_src;
        ```

        **Note:**

        -   All the preceding commands must be run on the odpscmd client. All command keywords, project names, table names, and column names in MaxCompute are not case-sensitive.
        -   Each workspace that you create in the DataWorks console is a MaxCompute project.
        -   You cannot run commands to create or delete projects in MaxCompute. You can manage projects in the DataWorks console. For more information, see [Workspaces]().

## View projects

-   Syntax

    ```
    list projects;
    ```

    **Note:** You can run this command only on odpscmd 0.30.2 or later.

-   Description

    You can run this command by using your Alibaba Cloud account to view the projects that are created by the account.


## Specify project properties

-   Syntax

    ```
    setproject <KEY>=<VALUE>;
    ```

-   Description
    -   You can run this command to specify project properties in the `<KEY>=<VALUE>` format for the current project. For example, to enable a full table scan, run the following command:

        ```
        setproject odps.sql.allow.fullscan=true;
        ```

        **Note:** The command that is used to specify project properties takes effect within five minutes. Check the result five minutes after the command succeeds.

    -   If you do not specify any properties in the `<KEY>=<VALUE>` format in the command, the property settings of the current project are displayed.

        ```
        -- View the project properties that are configured by running the setproject command.
        setproject;
        ```

        The following table describes details about project properties.

        |Property|Role|Description|Valid value|
        |:-------|:---|:----------|:----------|
        |odps.sql.allow.fullscan|Project Owner|Specifies whether to allow a full table scan.|        -   True
        -   False |
        |odps.table.drop.ignorenonexistent|All users|Specifies whether to report an error if the table that you want to delete does not exist.|        -   True: No error is reported.
        -   False: An error is reported. |
        |odps.table.lifecycle|Project Owner|        -   Optional: The lifecycle clause is optional in a table creation statement. If you do not set the lifecycle for a table, the table does not expire.
        -   Mandatory: The lifecycle clause is required in a table creation statement.
        -   Inherit: If you do not set a lifecycle for a table when you create the table, the value of odps.table.lifecycle.value is used as the lifecycle of the table.
|        -   Optional
        -   Mandatory
        -   Inherit |
        |odps.table.lifecycle.value|Project Owner|The lifecycle of the table. Unit: days.|1 to 37231 \(default value\)|
        |odps.security.ip.whitelist|Project Owner|The whitelist of IP addresses that are authorized to access the project over the classic network.|A list of IP addresses separated with commas \(,\)|
        |odps.security.vpc.whitelist|Project Owner|The whitelist of IP addresses that are authorized to access the project in a specific virtual private cloud \(VPC\).|A list of records that consist of region IDs, VPC IDs, and IP addresses. Records are separated with commas \(,\).|
        |odps.instance.remain.days|Project Owner|The retention period for instance information, in days.|3 to 30|
        |READ\_TABLE\_MAX\_ROW|Project Owner|The maximum number of data entries that can be returned by a SELECT statement.|1 to 10000|
        |odps.sql.type.system.odps2|Project Owner|Specifies whether to enable the MaxCompute V2.0 data type edition.|        -   True
        -   False |
        |odps.sql.hive.compatible|Project Owner|Specifies whether to enable the Hive-compatible data type edition. MaxCompute supports Hive syntax, such as `inputRecordReader`, `outputRecordReader`, and `Serde`, only after the Hive-compatible data type edition is enabled.|        -   True
        -   False |

        **Note:** Enabling the MaxCompute V2.0 data type edition has the following impacts:

        -   Some implicit type conversions are disabled. For example, if the data type is converted from STRING to BIGINT, from STRING to DATETIME, from DOUBLE to BIGINT, from DECIMAL to DOUBLE, or from DECIMAL to BIGINT, precision may be reduced, or errors may occur. You can use the `CAST` function to forcibly convert the data type.
        -   The constant type changes. A single integer constant, such as 123, falls into the BIGINT type in the original data type edition, but it falls into the INT type in the MaxCompute V2.0 data type edition.
        -   User-defined function \(UDF\) resolution results may change. Assume that a UDF contains two overloads: BIGINT and INT. After UDF resolution, the BIGINT overload is obtained in the original data type edition, but the INT overload may be obtained in the MaxCompute V2.0 data type edition.

## Configure IP address whitelists

MaxCompute allows you to configure the whitelists of IP addresses that are authorized to access projects over the classic network or a VPC. The odps.security.ip.whitelist parameter specifies the IP address whitelist for the classic network. The odps.security.vpc.whitelist parameter specifies the IP address whitelist for a VPC.

-   Syntax
    -   If you configure the IP address whitelist only for the classic network, access requests over the classic network are limited, and access requests over a VPC are prohibited. Configuration command:

        ```
        setproject odps.security.ip.whitelist=IP Address odps.security.vpc.whitelist=\N;
        ```

        When you configure the IP address whitelist for the classic network, add the IP address of the device on which the MaxCompute client is installed to the whitelist. Otherwise, your access request is denied.

        ![Classic network configuration check](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6190711061/p166338.png)

    -   If you configure the IP address whitelist only for a VPC, access requests over the VPC are limited, and access requests over the classic network are prohibited. Configuration command:

        ```
        setproject odps.security.ip.whitelist=\N odps.security.vpc.whitelist=RegionID_VPCID[IP Address];
        ```

    -   If you configure the IP address whitelists for both the classic network and VPC, access requests over the classic network and VPC are limited. Configuration command:

        ```
        setproject odps.security.ip.whitelist=IP Address odps.security.vpc.whitelist=RegionID_VPCID[IP Address];
        ```

    -   You can modify the configuration of an IP address whitelist. Configuration commands:

        ```
        -- Modify the configuration of an IP address whitelist for the classic network.
        setproject odps.security.ip.whitelist=IP Address;
        -- Modify the configuration of an IP address whitelist for a VPC.
        setproject odps.security.vpc.whitelist=RegionID_VPCID[IP Address];
        ```

    -   You can disable the IP address whitelist feature.

        ```
        setproject odps.security.ip.whitelist= odps.security.vpc.whitelist= ;
        ```

    -   Description

        -   After an IP address whitelist is configured for a project, only IP addresses in the whitelist, such as the outbound IP addresses of the odpscmd client or SDK, can be used to access the project.
        -   The IP address whitelist takes effect five minutes after it is configured.
        **Note:** If you are blocked from a project due to misoperations, [submit a ticket](https://workorder-intl.console.aliyun.com/) to Alibaba Cloud for technical support.

-   Parameters
    -   IP Address: a list of IP addresses. For a VPC, the value is a list of private IP addresses in the VPC.

        You can specify IP addresses in the following formats in an IP address whitelist and separate the IP addresses with commas \(,\):

        -   IPv4 or IPv6 addresses. Example: 192.168.0.0 or 2001:db8::
        -   IP addresses with subnet masks. Example: 172.12.0.0/16 or 2001:db8::/32
        -   CIDR blocks. Example: 192.168.10.0-192.168.255.255 or 2001:db8:1:1:1:1:1:1-2001:db8:4:4:4:4:4:4
    -   ReigonID: the ID of the region to which the VPC belongs.

        |Region|Region ID|
        |------|---------|
        |China \(Zhangjiakou-Beijing Winter Olympics\)|cn-zhangjiakou|
        |China \(Beijing\)|cn-beijing|
        |China \(Shenzhen\)|cn-shenzhen|
        |China \(Chengdu\)|cn-chengdu|
        |China \(Shanghai\)|cn-shanghai|
        |China \(Hangzhou\)|cn-hangzhou|
        |Shanghai Tower|cn|
        |China \(Hong Kong\)|cn-hongkong|
        |Singapore \(Singapore\)|ap-southeast-1|
        |Australia \(Sydney\)|ap-southeast-2|
        |Malaysia \(Kuala Lumpur\)|ap-southeast-3|
        |Indonesia \(Jakarta\)|ap-southeast-5|
        |Japan \(Tokyo\)|ap-northeast-1|
        |Germany \(Frankfurt\)|eu-central-1|
        |US \(Silicon Valley\)|us-west-1|
        |US \(Virginia\)|us-east-1|
        |India \(Mumbai\)|ap-south-1|
        |UAE \(Dubai\)|me-east-1|
        |UK \(London\)|eu-west-1|

    -   VPCID: the ID of the VPC.
        -   If this is your first time to configure an IP address whitelist for a VPC, log on to the [MaxCompute client](/intl.en-US/Tools and Downloads/Client.md) and run the following command to obtain the VPC ID:

            ```
            whoami;
            ```

            The following information is returned.

            ![VPC ID](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1243948951/p65998.png)

            **Note:** This command can be used only if the version of the MaxCompute client is V0.31.2 or later.

        -   If you want to add an IP address to an established whitelist for a VPC, obtain the region ID from the error message returned when you use the IP address to access MaxCompute for the first time. The error message returned because the new IP address is not authorized.

            ![Error message](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1243948951/p66073.png)

-   Examples

    For more information, see [Configure an IP address whitelist](/intl.en-US/Management/Configure security features/Quick Start/Configure an IP address whitelist.md).


