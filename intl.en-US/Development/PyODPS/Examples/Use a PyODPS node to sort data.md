---
keyword: sort data
---

# Use a PyODPS node to sort data

This topic describes how to use a PyODPS node to sort data.

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

7.  On the configuration tab of the PyODPS 2 node, enter the following code in the code editor:

    ```
    from  odps.df import DataFrame
    
    iris = DataFrame(o.get_table('pyodps_iris'))
    
    # Sort data.
    
    print iris.sort('sepalwidth').head(5)
    
    # You can use one of the following methods to sort data in descending order:
    
    # Set the ascending parameter to False.
    print iris.sort('sepalwidth',ascending=False).head(5)
    
    # Use a hyphen (-) as the sorting parameter.
    print iris.sort(-iris.sepalwidth).head(5)
    
    # Sort multiple fields.
    print iris.sort(['sepalwidth','petallength']).head(5)
    
    # To sort multiple fields in different orders, you can set the ascending parameter to a list of BOOLEAN values. The number of values in the list must be the same as the number of fields to be sorted.
    print iris.sort(['sepalwidth','petallength'],ascending=[True,False]).head(5)
    print iris.sort(['sepalwidth',-iris.petallength]).head(5)
    ```

8.  Click the Run icon in the toolbar.

9.  View the running result of the PyODPS 2 node on the **Run Log** tab.

    ![Run Log](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3477441061/p65198.png)

    In this example, the following information appears on the Run Log tab:

    ```
    Sql compiled:
    CREATE TABLE tmp_pyodps_d1b06785_dc18_4288_ad34_de860de1be08 LIFECYCLE 1 AS
    SELECT *
    FROM WB_BestPractice_dev.`pyodps_iris` t1
    ORDER BY sepalwidth
    LIMIT 10000
    Instance ID: 20191010061554817gwml0lim
    
       sepallength  sepalwidth  petallength  petalwidth             name
    0          5.0         2.0          3.5         1.0  Iris-versicolor
    1          6.0         2.2          5.0         1.5   Iris-virginica
    2          6.2         2.2          4.5         1.5  Iris-versicolor
    3          6.0         2.2          4.0         1.0  Iris-versicolor
    4          5.5         2.3          4.0         1.3  Iris-versicolor
    Sql compiled:
    CREATE TABLE tmp_pyodps_3cb90bb2_fb95_43fb_ae84_f2b5a27d72dc LIFECYCLE 1 AS
    SELECT *
    FROM WB_BestPractice_dev.`pyodps_iris` t1
    ORDER BY sepalwidth DESC
    LIMIT 10000
    Instance ID: 20191010061601287gs086792
    
    
       sepallength  sepalwidth  petallength  petalwidth         name
    0          5.7         4.4          1.5         0.4  Iris-setosa
    1          5.5         4.2          1.4         0.2  Iris-setosa
    2          5.2         4.1          1.5         0.1  Iris-setosa
    3          5.8         4.0          1.2         0.2  Iris-setosa
    4          5.4         3.9          1.3         0.4  Iris-setosa
    Sql compiled:
    CREATE TABLE tmp_pyodps_97b080bb_e014_48e8_a310_4b45fcd6a2ed LIFECYCLE 1 AS
    SELECT *
    FROM WB_BestPractice_dev.`pyodps_iris` t1
    ORDER BY sepalwidth DESC
    LIMIT 10000
    Instance ID: 20191010061606927g6emz192
    
       sepallength  sepalwidth  petallength  petalwidth         name
    0          5.7         4.4          1.5         0.4  Iris-setosa
    1          5.5         4.2          1.4         0.2  Iris-setosa
    2          5.2         4.1          1.5         0.1  Iris-setosa
    3          5.8         4.0          1.2         0.2  Iris-setosa
    4          5.4         3.9          1.3         0.4  Iris-setosa
    Sql compiled:
    CREATE TABLE tmp_pyodps_6fe37b6e_6705_4052_b733_211eb9bd16ac LIFECYCLE 1 AS
    SELECT *
    FROM WB_BestPractice_dev.`pyodps_iris` t1
    ORDER BY sepalwidth, petallength
    LIMIT 10000
    Instance ID: 20191010061611714gn586792
    
       sepallength  sepalwidth  petallength  petalwidth             name
    0          5.0         2.0          3.5         1.0  Iris-versicolor
    1          6.0         2.2          4.0         1.0  Iris-versicolor
    2          6.2         2.2          4.5         1.5  Iris-versicolor
    3          6.0         2.2          5.0         1.5   Iris-virginica
    4          4.5         2.3          1.3         0.3      Iris-setosa
    Sql compiled:
    CREATE TABLE tmp_pyodps_a52c805c_94a1_4a75_a6af_4fc9ed06ae68 LIFECYCLE 1 AS
    SELECT *
    FROM WB_BestPractice_dev.`pyodps_iris` t1
    ORDER BY sepalwidth, petallength DESC
    LIMIT 10000
    Instance ID: 20191010061616553gw3m9592
    
       sepallength  sepalwidth  petallength  petalwidth             name
    0          5.0         2.0          3.5         1.0  Iris-versicolor
    1          6.0         2.2          5.0         1.5   Iris-virginica
    2          6.2         2.2          4.5         1.5  Iris-versicolor
    3          6.0         2.2          4.0         1.0  Iris-versicolor
    4          6.3         2.3          4.4         1.3  Iris-versicolor
    Sql compiled:
    CREATE TABLE tmp_pyodps_aac5538e_9b40_4078_b3c6_852b99c663c1 LIFECYCLE 1 AS
    SELECT *
    FROM WB_BestPractice_dev.`pyodps_iris` t1
    ORDER BY sepalwidth, petallength DESC
    LIMIT 10000
    Instance ID: 20191010061621329gvmkc292
    
    
       sepallength  sepalwidth  petallength  petalwidth             name
    0          5.0         2.0          3.5         1.0  Iris-versicolor
    1          6.0         2.2          5.0         1.5   Iris-virginica
    2          6.2         2.2          4.5         1.5  Iris-versicolor
    3          6.0         2.2          4.0         1.0  Iris-versicolor
    4          6.3         2.3          4.4         1.3  Iris-versicolor
    ```


