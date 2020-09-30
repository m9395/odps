---
keyword: Sequence
---

# PyODPS的Sequence及执行操作

本文为您介绍如何进行PyODPS的Sequence及执行操作。

请提前完成如下操作：

-   [开通MaxCompute](/intl.zh-CN/准备工作/开通MaxCompute.md)。
-   [开通DataWorks](https://common-buy.aliyun.com/)。
-   在DataWorks上完成创建业务流程，本例使用DataWorks简单模式。详情请参见[创建业务流程]()。

1.  创建表并导入数据。

    1.  下载[鸢尾花](http://archive.ics.uci.edu/ml/machine-learning-databases/iris/)数据集iris.data，重命名为iris.csv。

    2.  创建表pyodps\_iris并上传数据集iris.csv。操作方法请参见[建表并上传数据]()。

        建表语句如下。

        ```
        CREATE TABLE if not exists pyodps_iris
        (
        sepallength  DOUBLE comment '片长度(cm)',
        sepalwidth   DOUBLE comment '片宽度(cm)',
        petallength  DOUBLE comment '瓣长度(cm)',
        petalwidth   DOUBLE comment '瓣宽度(cm)',
        name         STRING comment '种类'
        );
        ```

2.  登录[DataWorks控制台](https://workbench.data.aliyun.com/console)。

3.  在左侧导航栏上单击**工作空间列表**。

4.  单击工作空间后的**进入数据开发**。

5.  在数据开发页面，右键单击已经创建的业务流程，选择**新建** \> **MaxCompute** \> **PyODPS 2**。

6.  在**新建节点**对话框，输入**节点名称**，并单击**提交**。

7.  进入**PyODPS**节点编辑框，输入示例代码。示例代码如下。

    ```
    from odps import DataFrame
    iris = DataFrame(o.get_table('pyodps_iris'))
    
    #获取列。
    print iris.sepallength.head(5)
    
    print iris['sepallength'].head(5)
    
    #查看列的类型。
    print iris.sepallength.dtype
    
    #修改列的类型。
    iris.sepallength.astype('int')
    
    #计算。
    print iris.groupby('name').sepallength.max().head(5)
    
    print iris.sepallength.max()
    
    #重命名列。
    print iris.sepalwidth.rename('speal_width').head(5)
    
    #简单的列变化。
    print (iris.sepallength + iris.sepalwidth).rename('sum_sepal').head(5)
    ```

8.  单击**运行**。

9.  在**运行日志**中查看结果。

    结果如下。

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
          sepallength           : double      # 片长度(cm)
          sepalwidth            : double      # 片宽度(cm)
          petallength           : double      # 瓣长度(cm)
          petalwidth            : double      # 瓣宽度(cm)
          name                  : string      # 种类
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

10. 按照如上方法，新建并运行Pyodps节点PyExecute。

    PyExecute节点示例代码如下。

    ```
    from odps import options
    from odps import DataFrame
    
    #查看运行时的instance的logview。
    options.verbose = True
    iris = DataFrame(o.get_table('pyodps_iris'))
    iris[iris.sepallength < 5].exclude('sepallength')[:5].execute()
    
    my_logs = []
    def my_loggers(x):
        my_logs.append(x)
    
    options.verbose_log = my_loggers
    
    iris[iris.sepallength < 5].exclude('sepallength')[:5].execute()
    
    print(my_logs)
    
    #缓存中间Collection结果。
    cached = iris[iris.sepalwidth < 3.5].cache()
    print cached.head(3)
    
    #异步和并行执行。
    from odps.df import Delay
    delay = Delay() #创建Delay对象。
    df = iris[iris.sepalwidth < 5].cache()  #有一个共同的依赖。
    future1 = df.sepalwidth.sum().execute(delay=delay) #立即返回future对象，此时并没有执行。
    future2 = df.sepalwidth.mean().execute(delay=delay)
    future3 = df.sepalwidth.max().execute(delay=delay)
    
    delay.execute(n_parallel=3)
    
    print future1.result()
    print future2.result()
    print future3.result()
    ```

    运行结果如下。

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


