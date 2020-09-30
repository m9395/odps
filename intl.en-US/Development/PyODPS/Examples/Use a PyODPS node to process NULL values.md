---
keyword: process NULL values
---

# Use a PyODPS node to process NULL values

This topic describes how to use a PyODPS node to process NULL values.

-   [Activate MaxCompute](/intl.en-US/Prepare/Activate MaxCompute.md).
-   [Activate DataWorks](https://common-buy.aliyun.com/?commodityCode=dide_create_post#/buy).
-   Create a DataWorks on the workflow. This example uses DataWorks simple mode. For more information, see [create a workflow]().

1.  Prepare test data.

    1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console).

    2.  Create a table and import data to the table. For more information, see [Create tables and import data]().

        Execute the following statement to create a table named pytable2:

        ```
        CREATE TABLE `pytable2` (
          `id` string,
          `name` string,
          `f1` double,
          `f2` double,
          `f3` double,
          `f4` double
        ) ;
        ```

        Import the data in the pytable2.txt file to the pytable2 table. The pytable2.txt file contains the following data:

        ```
        0, name1, 1.0, NaN, 3.0, 4.0
        1, name1, 2.0, NaN, NaN, 1.0
        2, name1, 3.0, 4.0, 1.0, NaN
        3, name1, NaN, 1.0, 2.0, 3.0
        4, name1, 1.0, NaN, 3.0, 4.0
        5, name1, 1.0, 2.0, 3.0, 4.0
        6, name1, NaN, NaN, NaN, NaN
        ```

2.  In the left-side navigation pane, click **Workspaces**.

3.  On the Workspaces page, find the target workspace and click **Data Analytics** in the Actions column.

4.  On the Data Development tab, right-click the target workflow in the Business process section and choose**New** \> **MaxCompute** \> **PyODPS 2**.

5.  In the **New node** dialog box, set the **Node name** parameter and click **Submit**.

6.  On the configuration tab of the PyODPS 2 node, enter the code of the node in the code editor.

    In this example, enter the following code:

    ```
    df2 = DataFrame(o.get_table('pytable2'))
    # Call the dropna method to delete the rows that contain NULL values.
    print df2.dropna(subset=['f1','f2','f3','f4']).head()
    
    # If you do not want to delete the rows that also contain non-NULL values, specify how='all' for the dropna method.
    print df2.dropna(how='all', subset=['f1','f2','f3','f4']).head()
    print df2.dropna(thresh=3, subset=['f1', 'f2', 'f3', 'f4']).head()
    
    # Call the fillna method to replace NULL values with a specified constant or values in an existing column.
    print df2.fillna(100, subset=['f1','f2','f3','f4']).head()
    
    # Replace NULL values with the values in an existing column.
    print df2.fillna(df2.f2, subset=['f1','f2','f3','f4']).head()
    
    # Replace each NULL value with the value in the same column of the previous row.
    print df2.fillna(method='bfill', subset=['f1', 'f2', 'f3', 'f4']).head()
    
    # Replace each NULL value with the value in the same column of the following row.
    print df2.fillna(method='ffill', subset=['f1', 'f2', 'f3', 'f4']).head()
    ```

7.  Click the Run icon in the toolbar.

    ![Run node.png](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5408441061/p65170.png)

8.  View the running result of the PyODPS 2 node on the **Run Log** tab.

    ![Run Log.png](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9908441061/p65237.png)

    In this example, the following information appears on the Run Log tab:

    ```
    Sql compiled:
    CREATE TABLE tmp_pyodps_d0c7d8c2_be38_4d48_b0eb_e89bae5bde01 LIFECYCLE 1 AS
    SELECT *
    FROM WB_BestPractice_dev.`pytable2` t1
    WHERE (((IF(t1.`f1` IS NOT NULL, 1, 0) + IF(t1.`f2` IS NOT NULL, 1, 0)) + IF(t1.`f3` IS NOT NULL, 1, 0)) + IF(t1.`f4` IS NOT NULL, 1, 0)) >= 4
    Instance ID: 20191010071154980g2hic292
    
      id   name   f1   f2   f3   f4
    0  0  name1  1.0  NaN  3.0  4.0
    1  1  name1  2.0  NaN  NaN  1.0
    2  2  name1  3.0  4.0  1.0  NaN
    3  3  name1  NaN  1.0  2.0  3.0
    4  4  name1  1.0  NaN  3.0  4.0
    5  5  name1  1.0  2.0  3.0  4.0
    6  6  name1  NaN  NaN  NaN  NaN
    Sql compiled:
    CREATE TABLE tmp_pyodps_49b46768_f589_48f6_be8a_b7139f31f6f2 LIFECYCLE 1 AS
    SELECT *
    FROM WB_BestPractice_dev.`pytable2` t1
    WHERE (((IF(t1.`f1` IS NOT NULL, 1, 0) + IF(t1.`f2` IS NOT NULL, 1, 0)) + IF(t1.`f3` IS NOT NULL, 1, 0)) + IF(t1.`f4` IS NOT NULL, 1, 0)) >= 1
    
    Instance ID: 20191010071159759g0dk9592
    
    
      id   name   f1   f2   f3   f4
    0  0  name1  1.0  NaN  3.0  4.0
    1  1  name1  2.0  NaN  NaN  1.0
    2  2  name1  3.0  4.0  1.0  NaN
    3  3  name1  NaN  1.0  2.0  3.0
    4  4  name1  1.0  NaN  3.0  4.0
    5  5  name1  1.0  2.0  3.0  4.0
    6  6  name1  NaN  NaN  NaN  NaN
    Sql compiled:
    CREATE TABLE tmp_pyodps_7f941800_1539_415b_9257_283ebeb893a6 LIFECYCLE 1 AS
    SELECT *
    FROM WB_BestPractice_dev.`pytable2` t1
    WHERE (((IF(t1.`f1` IS NOT NULL, 1, 0) + IF(t1.`f2` IS NOT NULL, 1, 0)) + IF(t1.`f3` IS NOT NULL, 1, 0)) + IF(t1.`f4` IS NOT NULL, 1, 0)) >= 3
    Instance ID: 20191010071204544giyswx7
    
    0  0  name1  1.0  NaN  3.0  4.0
    1  1  name1  2.0  NaN  NaN  1.0
    2  2  name1  3.0  4.0  1.0  NaN
    3  3  name1  NaN  1.0  2.0  3.0
    4  4  name1  1.0  NaN  3.0  4.0
    5  5  name1  1.0  2.0  3.0  4.0
    6  6  name1  NaN  NaN  NaN  NaN
    Sql compiled:
    CREATE TABLE tmp_pyodps_16d6ea6d_5195_4e4c_8346_644a395852f7 LIFECYCLE 1 AS
    SELECT t1.`id`, t1.`name`, IF(t1.`f1` IS NULL, 100, t1.`f1`) AS `f1`, IF(t1.`f2` IS NULL, 100, t1.`f2`) AS `f2`, IF(t1.`f3` IS NULL, 100, t1.`f3`) AS `f3`, IF(t1.`f4` IS NULL, 100, t1.`f4`) AS `f4`
    FROM WB_BestPractice_dev.`pytable2` t1
    Instance ID: 20191010071209190gyl56292
    
      id   name   f1   f2   f3   f4
    0  0  name1  1.0  NaN  3.0  4.0
    1  1  name1  2.0  NaN  NaN  1.0
    2  2  name1  3.0  4.0  1.0  NaN
    3  3  name1  NaN  1.0  2.0  3.0
    4  4  name1  1.0  NaN  3.0  4.0
    5  5  name1  1.0  2.0  3.0  4.0
    6  6  name1  NaN  NaN  NaN  NaN
    Sql compiled:
    CREATE TABLE tmp_pyodps_40755ebd_2d2a_482e_b360_3f3da0d5422c LIFECYCLE 1 AS
    SELECT t1.`id`, t1.`name`, IF(t1.`f1` IS NULL, t1.`f2`, t1.`f1`) AS `f1`, IF(t1.`f2` IS NULL, t1.`f2`, t1.`f2`) AS `f2`, IF(t1.`f3` IS NULL, t1.`f2`, t1.`f3`) AS `f3`, IF(t1.`f4` IS NULL, t1.`f2`, t1.`f4`) AS `f4`
    FROM WB_BestPractice_dev.`pytable2` t1
    
    Instance ID: 20191010071213970gbp66792
    
    
      id   name   f1   f2   f3   f4
    0  0  name1  1.0  NaN  3.0  4.0
    1  1  name1  2.0  NaN  NaN  1.0
    2  2  name1  3.0  4.0  1.0  NaN
    3  3  name1  NaN  1.0  2.0  3.0
    4  4  name1  1.0  NaN  3.0  4.0
    5  5  name1  1.0  2.0  3.0  4.0
    6  6  name1  NaN  NaN  NaN  NaN
    Sql compiled:
    CREATE TABLE tmp_pyodps_d39fcce1_d8a9_4cc2_8aff_2ed1e9c6bb1b LIFECYCLE 1 AS
    SELECT pyodps_udf_1570691538_d9441c59_c666_4a5d_8154_67d8bc8c24ad(t1.`id`, t1.`name`, t1.`f1`, t1.`f2`, t1.`f3`, t1.`f4`) AS (`id`, `name`, `f1`, `f2`, `f3`, `f4`)
    FROM WB_BestPractice_dev.`pytable2` t1
    Instance ID: 20191010071219627goqv9292
    
    
      id   name   f1   f2   f3   f4
    0  0  name1  1.0  3.0  3.0  4.0
    1  1  name1  2.0  1.0  1.0  1.0
    2  2  name1  3.0  4.0  1.0  NaN
    3  3  name1  1.0  1.0  2.0  3.0
    4  4  name1  1.0  3.0  3.0  4.0
    5  5  name1  1.0  2.0  3.0  4.0
    6  6  name1  NaN  NaN  NaN  NaN
    Sql compiled:
    CREATE TABLE tmp_pyodps_3f190cf0_f9fb_4e06_a942_ab31c0241cd3 LIFECYCLE 1 AS
    SELECT pyodps_udf_1570691566_0330848b_82d3_411c_88e1_cbbcc6adb9c1(t1.`id`, t1.`name`, t1.`f1`, t1.`f2`, t1.`f3`, t1.`f4`) AS (`id`, `name`, `f1`, `f2`, `f3`, `f4`)
    FROM WB_BestPractice_dev.`pytable2` t1
    Instance ID: 20191010071247729gt776792
    
      id   name   f1   f2   f3   f4
    0  0  name1  1.0  1.0  3.0  4.0
    1  1  name1  2.0  2.0  2.0  1.0
    2  2  name1  3.0  4.0  1.0  1.0
    3  3  name1  NaN  1.0  2.0  3.0
    4  4  name1  1.0  1.0  3.0  4.0
    5  5  name1  1.0  2.0  3.0  4.0
    6  6  name1  NaN  NaN  NaN  NaN
    ```


