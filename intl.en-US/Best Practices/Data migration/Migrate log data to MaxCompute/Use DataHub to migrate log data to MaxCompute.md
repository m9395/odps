---
keyword: [migrate log data, DataHub]
---

# Use DataHub to migrate log data to MaxCompute

This topic describes how to use DataHub to migrate log data to MaxCompute.

The following permissions are granted to the account authorized to access MaxCompute:

-   CreateInstance permission on MaxCompute projects
-   Permissions to view, modify, and update MaxCompute tables

For more information, see [Authorize users](/intl.en-US/Management/Configure security features/Manage users and permissions/Authorize users.md).

DataHub is a platform that is designed to process streaming data. After data is uploaded to DataHub, the data is stored in a table for real-time processing. DataHub executes scheduled tasks within five minutes to synchronize the data to a MaxCompute table for offline computing.

To periodically archive streaming data in DataHub to MaxCompute, you only need to create and configure a DataConnector.

1.  On the odpscmd client, create a table that is used to store the data synchronized from DataHub. Example:

    ```
     CREATE TABLE test(f1 string, f2 string, f3 double) partitioned by (ds string);
    ```

2.  Create a project in the DataHub console.

    1.  Log on to the [DataHub console](https://datahub.console.aliyun.com/datahub). In the upper-left corner, select a region.

    2.  In the left-side navigation pane, click **Project Manager**.

    3.  In the upper-right corner of the **Project List** page, click **Create Project**.

    4.  In the **Create Project** dialog box, specify **Name** and **Comment**, and click **Create**.

3.  Create a topic.

    1.  On the **Project List** page, find the project for which you want to create a topic and click **View** in the Operate column.

    2.  In the upper-right corner of the project details page, click **Create Topic**. In the **Create Topic** dialog box, configure the parameters.

        ![Create Topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6411359951/p73849.png)

    3.  Click **Next Step** to complete topic configurations.

        **Note:**

        -   **Schema** corresponds to a MaxCompute table. The field names, data types, and field sequence specified by Schema must be consistent with those of the MaxCompute table. You can create a DataConnector only if the three conditions are met.
        -   You are allowed to migrate the topics of the **TUPLE** and **BLOB** types to MaxCompute tables.
        -   A maximum of 20 topics can be created by default. If you require more topics, submit a [ticket](https://workorder-intl.console.aliyun.com/).
        -   The owner of a DataHub topic or the Creator account has the permissions to manage a DataConnector. For example, you can create or delete a DataConnector.
4.  Write data to the newly created topic.

    1.  Click **View** in the Operate column of the newly created topic.

    2.  On the topic details page, click **Connector**.

    3.  In the **Create connector** dialog box, click **MaxCompute**, configure the parameters, and then click **Create**.

5.  View DataConnector details.

    1.  In the left-side navigation pane, click **Project Manager**.

    2.  On the **Project List** page, find the project that you want to view its DataConnector details and click **View** in the Operate column.

    3.  On the **Topic List** tab, find the topic of the project and click **View** in the Operate column.

    4.  On the topic details page, click the **Connector** tab to view the created DataConnector.

    5.  Click **View** to view DataConnector details.

        By default, DataHub migrates data to MaxCompute tables at five-minute intervals or when the amount of data reaches 60 MB. **Sync Offset** indicates the number of migrated data entries.

        ![DataConnector details](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7411359951/p73847.png)

6.  Execute the following statement to check whether the log data is migrated to MaxCompute:

    ```
    SELECT * FROM test;
    ```

    If the result shown in the following figure is displayed, the log data is migrated to MaxCompute.

    ![Test results](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7411359951/p73848.png)


