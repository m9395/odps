---
keyword: 去重
---

# PyODPS的去重

本文为您介绍如何进行PyODPS的去重。

请提前完成如下操作：

-   [开通MaxCompute](/intl.zh-CN/准备工作/开通MaxCompute.md)。
-   [开通DataWorks](https://common-buy.aliyun.com/)。
-   在DataWorks上完成业务流程创建，本例使用DataWorks简单模式。详情请参见[创建业务流程]()。

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

7.  在PyODPS节点输入代码实现数据去重。

    示例代码如下。

    ```
    from  odps.df import DataFrame
    iris = DataFrame(o.get_table('pyodps_iris'))
    print iris[['name']].distinct()
    print iris.distinct('name')
    print iris.distinct('name','sepallength').head(3)
    #您可以调用unique对Sequence进行去重操作，但是调用unique的Sequence不能用在列选择中
    print iris.name.unique()
    ```

8.  单击**运行**。

9.  在**运行日志**中查看运行结果。

    ![运行日志](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2490659951/p65193.png)

    完整的运行结果如下。

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


