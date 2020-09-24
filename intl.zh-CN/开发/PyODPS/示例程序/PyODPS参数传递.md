---
keyword: 参数传递
---

# PyODPS参数传递

本文为您介绍如何在DataWorks中进行PyODPS参数的传递。

请提前完成如下操作：

-   [开通MaxCompute](/intl.zh-CN/准备工作/开通MaxCompute.md)。
-   [开通DataWorks](https://common-buy.aliyun.com/)。
-   在DataWorks上完成创建业务流程，本例使用DataWorks简单模式。详情请参见[创建业务流程]()。

1.  准备测试数据。

    1.  创建表并上传数据。操作方法请参见[建表并上传数据]()。

        表结构以及源数据信息如下。

        -   分区表user\_detail建表语句如下。

            ```
            create table if not exists user_detail
            (
            userid    BIGINT comment '用户id',
            job       STRING comment '工作类型',
            education STRING comment '教育程度'
            ) comment '用户信息表'
            partitioned by (dt STRING comment '日期',region STRING comment '地区');
            ```

        -   源数据表user\_detail\_ods建表语句如下。

            ```
            create table if not exists user_detail_ods
            (
              userid    BIGINT comment '用户id',
              job       STRING comment '工作类型',
              education STRING comment '教育程度',
              dt STRING comment '日期',
              region STRING comment '地区'
            );
            ```

        -   测试数据保存为user\_detail.txt文件。将此文件上传至表user\_detail\_ods中。

            ```
            0001,互联网,本科,20190715,beijing
            0002,教育,大专,20190716,beijing
            0003,金融,硕士,20190715,shandong
            0004,互联网,硕士,20190715,beijing
            ```

    2.  右键单击业务流程，选择**新建** \> **ODPS SQL**。

    3.  输入**节点名称**，并单击**提交**。

    4.  在ODPS SQL节点中输入如下代码。

        ```
        insert overwrite table user_detail partition (dt,region)
        select userid,job,education,dt,region from user_detail_ods;
        ```

    5.  单击**运行**，将数据插入到分区表user\_detail中。

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7390659951/p72612.png)

2.  使用PyODPS传递参数。

    1.  登录[DataWorks控制台](https://workbench.data.aliyun.com/console)。

    2.  在左侧导航栏上单击**工作空间列表**。

    3.  单击工作空间后的**进入数据开发**。

    4.  在数据开发页面，右键单击已经创建的业务流程，选择**新建** \> **MaxCompute** \> **PyODPS 2**。

    5.  输入**节点名称**，单击**提交**。

    6.  在PyODPS 2节点中输入如下代码实现参数传递。

        ```
        import sys
        reload(sys)
        print('dt=' + args['dt'])
        #修改系统默认编码。
        sys.setdefaultencoding('utf8')
        #获取表。
        t = o.get_table('user_detail')
        #接受传入的分区参数。
        with t.open_reader(partition='dt=' + args['dt'] + ',region=beijing') as reader1:
            count = reader1.count
        print("查询分区表数据：")
        for record in reader1:
            print record[0],record[1],record[2]
        ```

    7.  单击**高级运行（带参数运行）**。

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6390659951/p72614.png)

    8.  在**参数**对话框填写配置参数，单击**确定**。

        配置参数说明如下：

        -   **调度资源组**：选择**默认资源组**。
        -   **dt**：设置为dt=20190715。
    9.  在**运行日志**中查看运行结果。

        ![运行日志](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5390659951/p52360.jpg)


