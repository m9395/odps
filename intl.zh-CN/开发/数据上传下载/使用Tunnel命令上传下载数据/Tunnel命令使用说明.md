---
keyword: [Tunnel, 上传, 下载]
---

# Tunnel命令使用说明

本文为您介绍Tunnel命令的功能和如何通过Tunnel命令上传下载数据。

## Tunnel命令功能

您可以通过[客户端](/intl.zh-CN/工具及下载/客户端.md)提供的[Tunnel](/intl.zh-CN/开发/数据上传下载/使用Tunnel命令上传下载数据/Tunnel命令参考.md)命令实现原有Dship工具的功能。Tunnel命令主要用于数据的上传和下载等，其功能如下：

-   Upload：上传本地数据至MaxCompute表中。支持文件或目录（指一级目录）的上传，每一次上传只支持数据上传到一张表或表的一个[分区](/intl.zh-CN/开发/SQL及函数/DDL语句/分区和列操作.md)。分区表一定要指定上传的分区，多级分区一定要指定到末级分区。更多信息请参见[Upload](/intl.zh-CN/开发/数据上传下载/使用Tunnel命令上传下载数据/Tunnel命令参考.md#section_uwr_pwf_vdb) 。
-   Download：下载MaxCompute表或指定Instance执行结果至本地。只支持下载到单个文件，每一次下载只支持下载一张表或一个分区到一个文件。分区表一定要指定下载的分区，多级分区一定要指定到末级分区。更多信息请参见[Download](/intl.zh-CN/开发/数据上传下载/使用Tunnel命令上传下载数据/Tunnel命令参考.md)。
-   Resume：因为网络或Tunnel服务的原因造成上传出错，可以通过`Resume`命令对文件或目录进行续传。可以继续上一次的数据上传操作，但Resume命令暂时不支持下载操作。更多信息请参见[Resume](/intl.zh-CN/开发/数据上传下载/使用Tunnel命令上传下载数据/Tunnel命令参考.md)。
-   Show：显示历史任务信息。更多信息请参见[Show](/intl.zh-CN/开发/数据上传下载/使用Tunnel命令上传下载数据/Tunnel命令参考.md) 。
-   Purge：清理session目录，默认清理3天内的日志。更多信息请参见[Purge](/intl.zh-CN/开发/数据上传下载/使用Tunnel命令上传下载数据/Tunnel命令参考.md#section_l3y_51g_vdb)。
-   help：获取帮助信息，每个命令和选择支持短命令格式。

## Tunnel上传下载限制

-   Tunnel功能及Tunnel SDK当前不支持外部表操作。您可以通过Tunnel直接上传数据到MaxCompute内部表，或者是通过OSS Python SDK上传到OSS后，在MaxCompute使用外部表做映射。关于外部表详情请参见[外部表概述](/intl.zh-CN/开发/外部表/外部表概述.md)。
-   Tunnel命令不支持上传下载ARRAY、MAP和STRUCT类型的数据。
-   每个Tunnel的Session在服务端的生命周期为24小时，创建后24小时内均可使用，也可以跨进程/线程共享使用，但是必须保证同一个BlockId没有重复使用。
-   如果您根据Session下载，请注意只有主账号创建的Session，可以被主账号及所属子账号下载。

## 上传下载表数据

1.  执行如下语句创建分区表sale\_detail。

    ```
    CREATE TABLE IF NOT EXISTS sale_detail(
          shop_name     STRING,
          customer_id   STRING,
          total_price   DOUBLE)
    PARTITIONED BY (sale_date STRING,region STRING);
    ```

2.  为分区表sale\_detail添加分区。

    ```
    alter table sale_detail add partition (sale_date='201312', region='hangzhou');
    ```

3.  准备需要上传的数据文件data.txt，内容如下所示，保存路径为d:\\data.txt。

    这份文件的第三行数据与sale\_detail的表定义不符。sale\_detail定义了三列，但数据只有两列。

    ```
    shopx,x_id,100
    shopy,y_id,200
    shopz,z_id
    ```

4.  使用`upload`命令上传数据文件data.txt至分区表sale\_detail。

    ```
    odps@ project_name>tunnel upload d:\data.txt sale_detail/sale_date=201312,region=hangzhou -s false
    Upload session: 20150610xxxxxxxxxxx70a002ec60c
    Start upload:d:\data.txt
    Total bytes:41   Split input to 1 blocks
    2015-06-10 16:39:22     upload block: '1'
    ERROR: column mismatch -,expected 3 columns, 2 columns found, please check data or delimiter
    ```

    由于data.txt中有脏数据，数据导入失败，并给出session ID及错误提示信息。

5.  使用如下语句进行数据验证。由于有脏数据，数据导入失败，表中无数据。

    ```
    odps@ project_name> select * from sale_detail where sale_date='201312';
    ID = 20150610xxxxxxxxxxxvc61z5
    +-----------+-------------+-------------+-----------+--------+
    | shop_name | customer_id | total_price | sale_date | region |
    +-----------+-------------+-------------+-----------+--------+
    +-----------+-------------+-------------+-----------+--------+
    ```

6.  使用`show`命令查询失败上传的session ID。

    ```
    odps@ project_name>tunnel show  history;
    20150610xxxxxxxxxxx70a002ec60c  failed  'u --config-file /D:/console/conf/odps_config.ini --project odpstest_ay52c_ay52 --endpoint http://service.cn-shanghai.maxcompute.aliyun.com/api --id UlxxxxxxxxxxxrI1 --key 2m4r3WvTxxxxxxxxxx0InVke7UkvR d:\data.txt sale_detail/sale_date=201312,region=hangzhou -s false'
    ```

7.  修改示例数据data.txt文件为如下内容，符合sale\_detail的表结构。

    ```
    shopx,x_id,100
    shopy,y_id,200
    ```

8.  执行`resume`命令修复执行上传数据。其中，20150610xxxxxxxxxxx70a002ec60c为上传失败的session ID。

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

9.  执行如下语句进行数据验证，上传成功。

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

10. 执行`download`命令将表sale\_detail上的数据下载至本地result.txt。

    ```
    $ ./tunnel download sale_detail/sale_date=201312,region=hangzhou result.txt;
        Download session: 20150610xxxxxxxxxxx70a002ed0b9
        Total records: 2
        2015-06-10 16:58:24     download records: 2
        2015-06-10 16:58:24     file size: 30 bytes
        OK
    ```

11. 验证result.txt的文件内容，如下所示，下载成功。

    ```
    shopx,x_id,100.0
    shopy,y_id,200.0
    ```


## 下载Instance数据

-   方式一：使用tunnel download命令将特定Instance的执行结果下载到本地文件。
    1.  执行SELECT语句查询表sale\_detail。

        ```
        odps@ odps_test_project>select * from sale_detail;
        ID = 20170724071705393ge3csfb8
        ... ...
        ```

    2.  执行如下Tunnel命令下载执行结果到本地文件。

        ```
        odps@ odps_test_project>tunnel download instance://20170724071705393ge3csfb8 result;
        2017-07-24 15:18:47  -  new session: 2017072415184785b6516400090ca8    total lines: 8
        2017-07-24 15:18:47  -  file [0]: [0, 8), result
        downloading 8 records into 1 file
        2017-07-24 15:18:47  -  file [0] start
        2017-07-24 15:18:48  -  file [0] OK. total: 44 bytes
        download OK
        ```

    3.  执行如下命令查看结果。

        ```
        cat result
        ```

-   方式二：通过配置参数使SQL查询默认采用InstanceTunnel方式输出执行结果。

    在MaxCompute客户端中打开`use_instance_tunnel`选项后，执行的SELECT查询会默认使用InstanceTunnel来下载结果，从而避免在MaxCompute平台获取SQL查询结果时所遇到的获取数据超时和获取数据量受限的问题。打开该配置有以下两种方法：

    -   在最新版本的[客户端](/intl.zh-CN/工具及下载/客户端.md#)中，odps\_config.ini文件里已经默认打开此选项，并且`instance_tunnel_max_record`被默认设置成了10000。

        ```
        # download sql results by instance tunnel
        use_instance_tunnel=true
        # the max records when download sql results by instance tunnel
        instance_tunnel_max_record=10000
        ```

        **说明：** `instance_tunnel_max_record`用来设置通过InstanceTunnel下载SQL查询结果的条数。若不设置该项，则下载条数不受限制。

    -   将`set console.sql.result.instancetunnel`设置为true来开启此功能。

        ```
        --打开Instance tunnel选项。
        odps@ odps_test_tunnel_project>set console.sql.result.instancetunnel=true;
        OK
        
        --运行select query。
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

        **说明：** 如果用InstanceTunnel的方式来输出SELECT查询结果，系统在最后一行会打印一条提示，此示例中Instance的执行结果共有8条数据。同理，您可以将`set console.sql.result.instancetunnel`设置为false来关闭此功能。


