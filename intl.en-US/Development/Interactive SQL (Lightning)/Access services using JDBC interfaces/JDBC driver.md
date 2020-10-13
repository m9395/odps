---
keyword: JDBC driver
---

# JDBC driver

MaxCompute provides JDBC interfaces that are fully compatible with the PostgreSQL protocol. You can connect SQL Client tools to the MaxCompute Lightning service by using JDBC interfaces.

MaxCompute Lightning can be accessed by using JDBC drivers downloaded from the PostgreSQL official website or other drivers optimized for MaxCompute Lightning.

The MaxCompute Lightning query engine is based on PostgreSQL 8.2 and allows you to query the existing MaxCompute tables by using a SELECT statement. For more information, see [Queries](https://www.postgresql.org/docs/8.2/static/queries.html) and [Functions and Operators](https://www.postgresql.org/docs/8.2/static/functions.html) in PostgreSQL Documentation.

-   [JDBC](https://jdbc.postgresql.org/) driver downloaded from the PostgreSQL official website

    Many client tools integrate the PostgreSQL database driver by default. You can directly use their built-in driver. If the PostgreSQL database driver is not integrated, you can download it from the client website. If you use the SQL Workbench/J client, you can select the PostgreSQL official driver to create a JDBC connection.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6092659951/p11216.jpg)

-   [JDBC driver](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/89778/cn_zh/1535960228920/MaxComputeLightningJDBC.jar) optimized for MaxCompute Lightning

    Save the downloaded MaxCompute Lightning JDBC driver as the MaxComputeLightningJDBC.jar file. If you use the SQL Workbench/J client, add the MaxCompute Lightning JDBC driver in the **Manage drivers** window, as shown in the following figure.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6092659951/p11243.jpg)

    When you create a connection, select the MaxCompute Lightning JDBC driver that you just added from the driver list.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6092659951/p11244.jpg)


