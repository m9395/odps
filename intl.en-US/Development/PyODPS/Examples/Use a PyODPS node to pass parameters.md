---
keyword: pass parameters
---

# Use a PyODPS node to pass parameters

This topic describes how to use a PyODPS node to pass parameters.

The following operations are completed:

-   [Activate MaxCompute](/intl.en-US/Prepare/Activate MaxCompute.md)
-   [Activate DataWorks](https://common-buy.aliyun.com/)
-   Create a workflow in the DataWorks console. In this example, create a workflow in a DataWorks workspace in basic mode. For more information, see [Create a workflow]().

1.  Prepare test data.

    1.  Create a partitioned table and a source table, and import data to the source table. For more information, see [Create tables and import data]().

        In this example, use the following table creation statements and source data.

        -   Execute the following statement to create a partitioned table named user\_detail:

            ```
            create table if not exists user_detail
            (
            userid    BIGINT comment 'user ID',
            job       STRING comment 'job type',
            education STRING comment 'education level'
            ) comment 'user information table'
            partitioned by (dt STRING comment 'date',region STRING comment 'region');
            ```

        -   Execute the following statement to create a source table named user\_detail\_ods:

            ```
            create table if not exists user_detail_ods
            (
              userid    BIGINT comment 'user ID',
              job       STRING comment 'job type',
              education STRING comment 'education level',
              dt STRING comment 'date',
              region STRING comment 'region'
            );
            ```

        -   Create a source data file named user\_detail.txt and save the following data to the file. Import the data to the user\_detail\_ods table.

            ```
            0001,Internet,bachelor,20190715,beijing
            0002,education,junior college,20190716,beijing
            0003,finance,master,20190715,shandong
            0004,Internet,master,20190715,beijing
            ```

    2.  Log on to the DataWorks console and click Workspaces in the left-side navigation pane. On the Workspaces page, find the target workspace and click Data Analytics in the Actions column. On the Data Development tab, right-click the target workflow in the Business process section and choose **New** \> **MaxCompute** \> ODPS SQL.

    3.  In the New node dialog box, set the **Node name** parameter and click **Submit**.

    4.  On the configuration tab of the ODPS SQL node, enter the following code in the code editor:

        ```
        insert overwrite table user_detail partition (dt,region)
        select userid,job,education,dt,region from user_detail_ods;
        ```

    5.  Click the Run icon in the toolbar to insert the data from the **user\_detail\_ods** table to the user\_detail table.

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6357290061/p72612.png)

2.  Use a PyODPS node to pass parameters.

    1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console).

    2.  In the left-side navigation pane, click **Workspaces**.

    3.  On the Workspaces page, find the target workspace and click **Data Analytics** in the Actions column.

    4.  On the Data Development tab, right-click the target workflow in the Business process section and choose **New** \> **MaxCompute** \> **PyODPS 2**.

    5.  In the New node dialog box, set the **Node name** parameter and click **Submit**.

    6.  On the configuration tab of the PyODPS 2 node, enter the following code in the code editor:

        ```
        import sys
        reload(sys)
        print('dt=' + args['dt'])
        # Set UTF-8 as the default encoding format.
        sys.setdefaultencoding('utf8')
        # Obtain the user_detail table.
        t = o.get_table('user_detail')
        # Receive the partition field that is passed.
        with t.open_reader(partition='dt=' + args['dt'] + ',region=beijing') as reader1:
            count = reader1.count
        print("Query data in the partitioned table:")
        for record in reader1:
            print record[0],record[1],record[2]
        ```

    7.  Click **Advanced run \(run with parameters\)** in the toolbar.

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6357290061/p72614.png)

    8.  In the **Parameters** dialog box, set the parameters and click **Confirm**.

        Parameter description:

        -   **Scheduling Resource Group**: Set this parameter to **Common scheduler resource group**.
        -   **dt**: Set this parameter to dt=20190715.
    9.  View the running result of the PyODPS 2 node on the **Run Log** tab.

        ![Run Log](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6397290061/p52360.jpg)


