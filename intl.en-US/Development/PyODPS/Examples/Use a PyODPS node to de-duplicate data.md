---
keyword: de-duplicate data
---

# Use a PyODPS node to de-duplicate data

This topic describes how to use a PyODPS node to de-duplicate data.

The following operations are completed:

-   [Activate MaxCompute](/intl.en-US/Prepare/Activate MaxCompute.md)
-   [Activate DataWorks](https://common-buy.aliyun.com/)
-   Create a workflow in the DataWorks console. In this example, create a workflow in a DataWorks workspace in basic mode. For more information, see [Create a workflow]().

1.  Prepare test data.

    1.  Download the [iris](http://t.cn/Rf8GeUq) dataset named iris.data and rename it iris.csv.

    2.  Create a table named pyodps\_iris and import the data in the iris.csv dataset to the table. For more information, see [Create tables and import data]().

        In this example, execute the following statement to create the pyodps\_iris table:

        ```
        CREATE TABLE if not exists pyodps_iris
        (
        sepallength  DOUBLE comment 'sepal length (cm)',
        sepalwidth   DOUBLE comment 'sepal width (cm)',
        petallength  DOUBLE comment 'petal length (cm)',
        petalwidth   DOUBLE comment 'petal width (cm)',
        name         STRING comment 'type'
        );
        ```

2.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console).

3.  In the left-side navigation pane, click **Workspaces**.

4.  On the Workspaces page, find the target workspace and click **Data Analytics** in the Actions column.

5.  On the Data Development tab, right-click the target workflow in the Business process section and choose**New** \> **MaxCompute** \> **PyODPS 2**.

6.  In the **New node** dialog box, set the **Node name** parameter and click **Submit**.

7.  On the configuration tab of the PyODPS 2 node, enter the code of the node in the code editor.

    In this example, enter the following code:

    ```
    from  odps.df import DataFrame
    iris = DataFrame(o.get_table('pyodps_iris'))
    print iris[['name']].distinct()
    print iris.distinct('name')
    print iris.distinct('name','sepallength').head(3)
    # You can call the unique method to de-duplicate data in the specified sequence. The sequence whose data is de-duplicated by using the unique method cannot be selected as a column.
    print iris.name.unique()
    ```

8.  Click the Run icon in the toolbar.

9.  View the running result of the PyODPS 2 node on the **Run Log** tab.

    ![Run Log](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1977441061/p65193.png)

    In this example, the following information appears on the Run Log tab:

    ```
    Sql compiled:
    CREATE TABLE tmp_pyodps_ed85ebd5_d678_44dd_9ece_bff1822376f6 LIFECYCLE 1 AS
    SELECT DISTINCT t1.`name`
    FROM WB_BestPractice_dev.`pyodps_iris` t1
    Instance ID: 2019101006391142g2cp5692
    
    
                  name
    0      Iris-setosa
    1  Iris-versicolor
    2   Iris-virginica
    Sql compiled:
    CREATE TABLE tmp_pyodps_8ce6128f_9c6f_45af_b9de_c73ce9d5ba51 LIFECYCLE 1 AS
    SELECT DISTINCT t1.`name`
    FROM WB_BestPractice_dev.`pyodps_iris` t1
    
    Instance ID: 20191010063915987gmuws592
    
    
                  name
    0      Iris-setosa
    1  Iris-versicolor
    2   Iris-virginica
    Sql compiled:
    CREATE TABLE tmp_pyodps_a3dc338e_0fea_4d5f_847c_79fb19ec1c72 LIFECYCLE 1 AS
    SELECT DISTINCT t1.`name`, t1.`sepallength`
    FROM WB_BestPractice_dev.`pyodps_iris` t1
    
    Instance ID: 2019101006392210gj056292
    
    
              name  sepallength
    0  Iris-setosa          4.3
    1  Iris-setosa          4.4
    2  Iris-setosa          4.5
    Sql compiled:
    CREATE TABLE tmp_pyodps_bc0917bb_f10c_426b_9b75_47e94478382a LIFECYCLE 1 AS
    SELECT t2.`name`
    FROM (
      SELECT DISTINCT t1.`name`
      FROM WB_BestPractice_dev.`pyodps_iris` t1
    ) t2
    Instance ID: 20191010063927189g9fsz192
    
                  name
    0      Iris-setosa
    1  Iris-versicolor
    2   Iris-virginica
    ```


