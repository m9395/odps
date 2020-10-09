---
keyword: [JDBC, SQL Workbench/J]
---

# Configure MaxCompute JDBC on SQL Workbench/J

This topic describes how to configure the MaxCompute JDBC driver on SQL Workbench/J. After the configuration, you can connect SQL Workbench/J to a MaxCompute project to execute SQL statements.

-   [SQL Workbench/J](http://sql-workbench.eu/downloads.html) is installed. SQL Workbench/J Build 125 \(2019-05-08\) is used in this topic.
-   The [MaxCompute JDBC driver](https://github.com/aliyun/aliyun-odps-jdbc/releases) is downloaded. The MaxCompute JDBC driver V3.0.1 is used in this topic.
-   Java 8 or later is installed.

SQL Workbench/J is a JDBC client tool. The procedure set out in this topic also can be used to configure the MaxCompute JDBC driver on other tools.

**Note:** If you encounter any issues when you configure the MaxCompute JDBC driver on other tools, submit [issues](https://github.com/aliyun/aliyun-odps-jdbc/issues).

1.  Start SQL Workbench/J and click **Manage Drivers**.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1532122061/p67872.png)

2.  On the page that appears, click the ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6710148951/p67874.png) icon.

3.  Set **Name** to MaxCompute.

4.  Click the ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6710148951/p67842.png) icon to upload the MaxCompute JDBC driver that you have downloaded. The **Classname** parameter is automatically set to com.aliyun.odps.jdbc.OdpsDriver.

5.  Click **OK** to return to the profile configuration page.

6.  Set **URL**, **Username**, and **Password**.

    ![**](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1532122061/p97283.png)

    -   **URL**: the URL for connecting to the specified MaxCompute project. The value must be in the jdbc:odps:<maxcompute\_endpoint\>?project=<maxcompute\_project\_name\> format, where:

        -   <maxcompute\_endpoint\>: the endpoint of MaxCompute in a specific region. For example, the public endpoint of MaxCompute in the China \(Hangzhou\) region is `http://service.cn-hangzhou.maxcompute.aliyun.com/api`. For more information about MaxCompute endpoints, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).
        -   <maxcompute\_project\_name\>: the name of the MaxCompute project.
        The following URL shows an example:

        ```
        jdbc:odps:http://service.cn-hangzhou.maxcompute.aliyun.com/api?project=test_project
        ```

    -   **Username**: the AccessKey ID of the account that has access to the specified MaxCompute project.
    -   **Password**: the AccessKey secret of the account that has access to the specified MaxCompute project.
7.  Click **OK**. On the SQL execution page that appears, you can execute MaxCompute SQL statements.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6710148951/p67849.png)


