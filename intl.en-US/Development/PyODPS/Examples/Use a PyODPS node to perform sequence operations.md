---
keyword: Sequence
---

# Use a PyODPS node to perform sequence operations

This topic describes how to use a PyODPS node to perform sequence operations.

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

7.  On the configuration tab of the PyODPS 2 node, enter the code of the node in the code editor. In this example, enter the following code:

    ```
    from odps import DataFrame
    iris = DataFrame(o.get_table('pyodps_iris'))
    
    # Obtain data from the specified column.
    print iris.sepallength.head(5)
    
    print iris['sepallength'].head(5)
    
    # View the data type of the column.
    print iris.sepallength.dtype
    
    # Change the data type of the column.
    iris.sepallength.astype('int')
    
    # Group values in the specified column and obtain the maximum value in each group.
    print iris.groupby('name').sepallength.max().head(5)
    
    print iris.sepallength.max()
    
    # Rename the specified column.
    print iris.sepalwidth.rename('speal_width').head(5)
    
    # Obtain the sum of the values of the specified columns, place the sum in a new column, and rename the new column.
    print (iris.sepallength + iris.sepalwidth).rename('sum_sepal').head(5)
    ```

8.  Click the Run icon in the toolbar.

9.  View the running result of the PyODPS 2 node on the **Run Log** tab.

    In this example, the following information appears on the Run Log tab:

    ```
    Executing user script with PyODPS 0.8.0
    Try to fetch data from tunnel
    
       sepallength
    0          4.9
    1          4.7
    2          4.6
    3          5.0
    4          5.4
    Try to fetch data from tunnel
       sepallength
    0          4.9
    1          4.7
    2          4.6
    3          5.0
    4          5.4
    FLOAT64
    Sql compiled:
    CREATE TABLE tmp_pyodps_ed78e3ba_f13c_4a49_812d_2790d57c25dd LIFECYCLE 1 AS
    SELECT MAX(t1.`sepallength`) AS `sepallength_max`
    FROM data_service_fr.`pyodps_iris` t1
    GROUP BY t1.`name`
    
       sepallength_max
    0              5.8
    1              7.0
    2              7.9
    Collection: ref_0
      odps.Table
        name: data_service_fr.`pyodps_iris`
        schema:
          sepallength           : double      # Sepal length (cm)
          sepalwidth            : double      # Sepal width (cm)
          petallength           : double      # Petal length (cm)
          petalwidth            : double      # Petal width (cm)
          name                  : string      # Type
    max = Max[float64]
      sepallength = Column[sequence(float64)] 'sepallength' from collection ref_0
    Try to fetch data from tunnel
       speal_width
    0          3.0
    1          3.2
    2          3.1
    3          3.6
    4          3.9
    Sql compiled:
    CREATE TABLE tmp_pyodps_28120275_8d0f_4683_8318_302fa21459ac LIFECYCLE 1 AS
    SELECT t1.`sepallength` + t1.`sepalwidth` AS `sum_sepal`
    FROM data_service_fr.`pyodps_iris` t1
    
       sum_sepal
    0        7.9
    1        7.9
    2        7.7
    3        8.6
    4        9.3
    2019-08-13 10:48:13 INFO =================================================================
    2019-08-13 10:48:13 INFO Exit code of the Shell command 0
    2019-08-13 10:48:13 INFO --- Invocation of Shell command completed ---
    2019-08-13 10:48:13 INFO Shell run successfully!
    ```

10. Use the same method to create and run another PyODPS node named PyExecute.

    On the configuration tab of the PyExecute node, enter the following code in the code editor:

    ```
    from odps import options
    from odps import DataFrame
    
    # View the logview of the running instance.
    options.verbose = True
    iris = DataFrame(o.get_table('pyodps_iris'))
    iris[iris.sepallength < 5].exclude('sepallength')[:5].execute()
    
    my_logs = []
    def my_loggers(x):
        my_logs.append(x)
    
    options.verbose_log = my_loggers
    
    iris[iris.sepallength < 5].exclude('sepallength')[:5].execute()
    
    print(my_logs)
    
    # Cache the intermediate collection.
    cached = iris[iris.sepalwidth < 3.5].cache()
    print cached.head(3)
    
    # Concurrently execute methods in asynchronous mode.
    from odps.df import Delay
    delay = Delay() # Create a Delay object.
    df = iris[iris.sepalwidth < 5].cache()  # The sum(), mean(), and max() methods depend on cache().
    future1 = df.sepalwidth.sum().execute(delay=delay) # This method immediately returns a future object without execution after it is called.
    future2 = df.sepalwidth.mean().execute(delay=delay)
    future3 = df.sepalwidth.max().execute(delay=delay)
    
    delay.execute(n_parallel=3)
    
    print future1.result()
    print future2.result()
    print future3.result()
    ```

    In this example, the following logs are recorded after the PyExecute node is run:

    ```
    Executing user script with PyODPS 0.8.0
    Sql compiled:
    CREATE TABLE tmp_pyodps_4a204590_0510_4e9c_823b_5b837a437840 LIFECYCLE 1 AS
    SELECT t1.`sepalwidth`, t1.`petallength`, t1.`petalwidth`, t1.`name`
    FROM data_service_fr.`pyodps_iris` t1
    WHERE t1.`sepallength` < 5
    LIMIT 5
    Instance ID: 20190813025233386g04djssa
      Log view: http://logview.odps.aliyun.com/logview/XXX
    ['Sql compiled:', 'CREATE TABLE tmp_pyodps_03b92c55_8442_4e61_8978_656495487b8a LIFECYCLE 1 AS \nSELECT t1.`sepalwidth`, t1.`petallength`, t1.`petalwidth`, t1.`name` \nFROM data_service_fr.`pyodps_iris` t1 \nWHERE t1.`sepallength` < 5 \nLIMIT 5', 'Instance ID: 20190813025236282gcsna5pr2', u'  
    Log view: http://logview.odps.aliyun.com/logview/?h=http://service.odps.aliyun.com/api&XXX
    
       sepallength  sepalwidth  petallength  petalwidth         name
    0          4.9         3.0          1.4         0.2  Iris-setosa
    1          4.7         3.2          1.3         0.2  Iris-setosa
    2          4.6         3.1          1.5         0.2  Iris-setosa
    
    454.6
    3.05100671141
    4.4
    2019-08-13 10:52:48 INFO =================================================================
    2019-08-13 10:52:48 INFO Exit code of the Shell command 0
    2019-08-13 10:52:48 INFO --- Invocation of Shell command completed ---
    2019-08-13 10:52:48 INFO Shell run successfully!
    2019-08-13 10:52:48 INFO Current task status: FINISH
    ```


