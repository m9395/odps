---
keyword: [JDBC, SQL Workbench/J]
---

# Configure MaxCompute JDBC on SQL Workbench/J

This topic describes how to configure MaxCompute JDBC on SQL Workbench/J.

-   Download and install SQL Workbench/J. Click [SQL Workbench/J](http://sql-workbench.eu/downloads.html) to download SQL Workbench/J. SQL Workbench/J Build 125 \(2019-05-08\) is used in this topic.
-   Download the MaxCompute JDBC driver. Click [MaxCompute JDBC driver](https://github.com/aliyun/aliyun-odps-jdbc/releases) to download the MaxCompute JDBC driver. MaxCompute JDBC driver V3.0.1 is used in this topic.
-   Install Java 8 or later.

SQL Workbench/J is a typical general-purpose JDBC client tool. The procedure set out in this topic also can be used for configurations in other tools.

**Note:** For any problems that occur when you use other tools to configure MaxCompute JDBC, report them to [Issues](https://github.com/aliyun/aliyun-odps-jdbc/issues).

1.  Start SQL Workbench/J and click **Manage Drivers** to go to the corresponding page.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5710148951/p69933.png)

2.  Click ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6710148951/p67874.png)to create a new JDBC driver.

3.  Set **Name** to MaxCompute.

4.  Click ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6710148951/p67842.png) to upload the MaxCompute JDBC driver that you have downloaded. **Classname** will be automatically set to com.aliyun.odps.jdbc.OdpsDriver.

5.  Click **OK** and you return to the profile configuration page.

6.  Set **URL**, **Username**, and **Password**.

    -   URL: must be in the jdbc:odps:<maxcompute\_endpoint\>? project=<maxcompute\_project\_name\> format.
        -   `<maxcompute_endpoint>` is the MaxCompute endpoint of your region. Example: `http://service.odps.aliyun.com/api`. For more information about MaxCompute endpoints, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).
        -   `<maxcompute_project_name>` is the name of your MaxCompute project.
    -   Username: the AccessKey ID of the account used to create the project.
    -   Password: the AccessKey secret of the account used to create the project.
    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6710148951/p67876.png)

7.  Click **OK** and you return to the SQL execution page. On this page, you can execute MaxCompute SQL statements.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6710148951/p67849.png)


