---
keyword: [Tunnel, upload, download]
---

# Tunnel command usage

This topic describes the features of Tunnel commands and how to use Tunnel commands to upload or download data.

## Features

The [MaxCompute client](/intl.en-US/Tools and Downloads/Client.md) provides [Tunnel](/intl.en-US/Development/Data upload and download/Run Tunnel commands to upload and download data/Tunnel commands.md) commands that implement the features of the Dship tool. You can use Tunnel commands to upload or download data. This section describes the features of the following Tunnel commands:

-   UPLOAD: uploads local data to a MaxCompute table. You can upload files or level-1 directories to only one table or only one [partition](/intl.en-US/Development/SQL/DDL SQL/Partition and column operations.md) in a table each time. For a partitioned table, you must specify the partition to which you want to upload data. If the table has multiple levels of partitions, you must specify the last-level partition. For more information, see [Upload](/intl.en-US/Development/Data upload and download/Run Tunnel commands to upload and download data/Tunnel commands.md#section_uwr_pwf_vdb).
-   DOWNLOAD: downloads MaxCompute table data or the query results of a specific instance to a local directory. You can download data from only one table or partition to a single local file each time. For a partitioned table, you must specify the partition from which you want to download data. If the table has multiple levels of partitions, you must specify the last-level partition. For more information, see [Download](/intl.en-US/Development/Data upload and download/Run Tunnel commands to upload and download data/Tunnel commands.md).
-   RESUME: resumes the transfer of files or directories when your network is disconnected or the Tunnel service is faulty. You cannot use this command to resume data downloads. For more information, see [Resume](/intl.en-US/Development/Data upload and download/Run Tunnel commands to upload and download data/Tunnel commands.md).
-   SHOW: displays historical task information. For more information, see [Show](/intl.en-US/Development/Data upload and download/Run Tunnel commands to upload and download data/Tunnel commands.md).
-   PURGE: clears the session directory. Sessions from the last three days are cleared by default. For more information, see [Purge](/intl.en-US/Development/Data upload and download/Run Tunnel commands to upload and download data/Tunnel commands.md#section_l3y_51g_vdb).
-   HELP: queries help information. Command aliases are supported.

## Limits of Tunnel commands

-   Tunnel commands and Tunnel SDK are unavailable to external tables. You can run Tunnel commands to upload data to MaxCompute internal tables. Alternatively, you can use the Object Storage Service \(OSS\) SDK for Python to upload data to OSS and then map the data to external tables in MaxCompute. For more information about external tables, see [Overview of External tables](/intl.en-US/Development/External table/Overview of External tables.md).
-   You cannot use Tunnel commands to upload or download data of the ARRAY, MAP, or STRUCT type.
-   Each session of the Tunnel service has a 24-hour lifecycle on the server. Sessions are valid within 24 hours after they are created and can be shared among processes or threads. The block ID must be unique among these processes or threads.
-   If you download data by session, only data of the session created by using your Alibaba Cloud account can be downloaded by this account or its RAM users.

## Upload and download table data

1.  Execute the following statement to create a partitioned table named sale\_detail:

    ```
    CREATE TABLE IF NOT EXISTS sale_detail(
          shop_name     STRING,
          customer_id   STRING,
          total_price   DOUBLE)
    PARTITIONED BY (sale_date STRING,region STRING);
    ```

2.  Add a partition to the sale\_detail partitioned table.

    ```
    alter table sale_detail add partition (sale_date='201312', region='hangzhou');
    ```

3.  Prepare the data.txt file that contains the following content. Save the file in the d:\\data.txt directory.

    In this example, the third row of the data.txt file does not comply with the definition of the sale\_detail partitioned table. The sale\_detail partitioned table defines three columns, but the third row of this file contains only two columns.

    ```
    shopx,x_id,100
    shopy,y_id,200
    shopz,z_id
    ```

4.  Run the `UPLOAD` command to upload the data.txt file to the sale\_detail partitioned table.

    ```
    odps@ project_name>tunnel upload d:\data.txt sale_detail/sale_date=201312,region=hangzhou -s false
    Upload session: 20150610xxxxxxxxxxx70a002ec60c
    Start upload:d:\data.txt
    Total bytes:41   Split input to 1 blocks
    2015-06-10 16:39:22     upload block: '1'
    ERROR: column mismatch -,expected 3 columns, 2 columns found, please check data or delimiter
    ```

    Data upload fails because the data.txt file contains dirty data. The system returns the session ID and an error message.

5.  Execute the following statement to verify the data upload. Data upload fails because the data.txt file contains dirty data. As a result, the sale\_detail partitioned table is empty.

    ```
    odps@ project_name> select * from sale_detail where sale_date='201312';
    ID = 20150610xxxxxxxxxxxvc61z5
    +-----------+-------------+-------------+-----------+--------+
    | shop_name | customer_id | total_price | sale_date | region |
    +-----------+-------------+-------------+-----------+--------+
    +-----------+-------------+-------------+-----------+--------+
    ```

6.  Run the `SHOW` command to query the ID of the failed session.

    ```
    odps@ project_name>tunnel show  history;
    20150610xxxxxxxxxxx70a002ec60c  failed  'u --config-file /D:/console/conf/odps_config.ini --project odpstest_ay52c_ay52 --endpoint http://service.cn-shanghai.maxcompute.aliyun.com/api --id UlxxxxxxxxxxxrI1 --key 2m4r3WvTxxxxxxxxxx0InVke7UkvR d:\data.txt sale_detail/sale_date=201312,region=hangzhou -s false'
    ```

7.  Modify the data.txt file to comply with the schema of the sale\_detail partitioned table.

    ```
    shopx,x_id,100
    shopy,y_id,200
    ```

8.  Run the `RESUME` command to resume the data upload. In the command, 20150610xxxxxxxxxxx70a002ec60c is the ID of the failed session.

    ```
    odps@ project_name>tunnel resume 20150610xxxxxxxxxxx70a002ec60c --force;
    start resume
    20150610xxxxxxxxxxx70a002ec60c
    Upload session: 20150610xxxxxxxxxxx70a002ec60c
    Start upload:d:\data.txt
    Resume 1 blocks 
    2015-06-10 16:46:42     upload block: '1'
    2015-06-10 16:46:42     upload block complete, blockid=1
    upload complete, average speed is 0 KB/s
    OK
    ```

9.  Execute the following statement to verify the data upload:

    ```
    odps@ project_name>select * from sale_detail where sale_date='201312';
     ID = 20150610xxxxxxxxxxxa741z5
     +-----------+-------------+-------------+-----------+--------+
     | shop_name | customer_id | total_price | sale_date | region |
     +-----------+-------------+-------------+-----------+--------+
     | shopx     | x_id        | 100.0       | 201312    | hangzhou|
     | shopy     | y_id        | 200.0       | 201312    | hangzhou|
     +-----------+-------------+-------------+-----------+--------+
    ```

10. Run the `DOWNLOAD` command to download data from the sale\_detail partitioned table to the local result.txt file.

    ```
    $ ./tunnel download sale_detail/sale_date=201312,region=hangzhou result.txt;
        Download session: 20150610xxxxxxxxxxx70a002ed0b9
        Total records: 2
        2015-06-10 16:58:24     download records: 2
        2015-06-10 16:58:24     file size: 30 bytes
        OK
    ```

11. Check whether the result.txt file has the following data. If yes, the data download succeeded.

    ```
    shopx,x_id,100.0
    shopy,y_id,200.0
    ```


## Download data of an instance

-   Method 1: Run the Tunnel DOWNLOAD command to download the query results of a specific instance to a local file.
    1.  Execute the SELECT statement to query the sale\_detail partitioned table.

        ```
        odps@ odps_test_project>select * from sale_detail;
        ID = 20170724071705393ge3csfb8
        ... ...
        ```

    2.  Run the following command to download the query results to a local file:

        ```
        odps@ odps_test_project>tunnel download instance://20170724071705393ge3csfb8 result;
        2017-07-24 15:18:47  -  new session: 2017072415184785b6516400090ca8    total lines: 8
        2017-07-24 15:18:47  -  file [0]: [0, 8), result
        downloading 8 records into 1 file
        2017-07-24 15:18:47  -  file [0] start
        2017-07-24 15:18:48  -  file [0] OK. total: 44 bytes
        download OK
        ```

    3.  Run the following command to view the download result:

        ```
        cat result
        ```

-   Method 2: Configure the required parameter to download query results by using InstanceTunnel.

    After you set the `use_instance_tunnel` parameter to true on the MaxCompute client, you can use InstanceTunnel to download the query results that are returned by SELECT statements. This helps you download query results of any size at any time. You can use one of the following methods to enable this feature:

    -   Log on to the latest [client](/intl.en-US/Tools and Downloads/Client.md#). Find the odps\_config.ini file and make sure that use\_instance\_tunnel is set to true and `instance_tunnel_max_record` is set to 10000.

        ```
        # download sql results by instance tunnel
        use_instance_tunnel=true
        # the max records when download sql results by instance tunnel
        instance_tunnel_max_record=10000
        ```

        **Note:** The `instance_tunnel_max_record` parameter specifies the maximum number of SQL query result records that can be downloaded by using InstanceTunnel. If you do not specify this parameter, the number of query result records that can be downloaded is unlimited.

    -   Set `set console.sql.result.instancetunnel` to true.

        ```
        -- Enable InstanceTunnel.
        odps@ odps_test_tunnel_project>set console.sql.result.instancetunnel=true;
        OK
        
        -- Execute the SELECT statement.
        odps@ odps_test_tunnel_project>select * from wc_in;
        ID = 20170724081946458g14csfb8
        Log view:
        http://logview/xxxxx.....
        +------------+
        | key        |
        +------------+
        | slkdfj     |
        | hellp      |
        | apple      |
        | tea        |
        | peach      |
        | apple      |
        | tea        |
        | teaa       |
        +------------+
        A total of 8 records fetched by instance tunnel.
        ```

        **Note:** If InstanceTunnel is used, a message appears at the end of the output that indicates the total number of records downloaded after you execute a SELECT statement. In this example, a total of eight records are downloaded. You can set `set console.sql.result.instancetunnel` to false to disable this feature.


