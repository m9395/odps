---
keyword: 采样
---

# PyODPS的采样

本文为您介绍如何进行PyODPS的采样。

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

7.  在PyODPS节点输入采样代码。

    示例代码如下。

    ```
    #采样
    from  odps.df import DataFrame
    iris = DataFrame(o.get_table('pyodps_iris'))
    #按份数采样
    print iris.sample(parts=10).head(5) #分为10，分默认取第0份
    print iris.sample(parts=10,i=0).head(5) #手动指定取第0份
    print iris.sample(parts=10,i=[2,5]).head(5)  #分成10份，取第2和第5份
    print iris.sample(parts=10,columns=['name','sepalwidth']).head(5) #根据name和sepalwidth的值做采样
    
    #按比例、条数采样
    print iris.sample(n=100).head()  #选取100条数据
    
    print iris.sample(frac=0.3).head() #采样30%的数据
    
    #按权重列采样
    print iris.sample(n=100,weights='sepallength').head()
    
    print iris.sample(n=100,weights='sepalwidth',replace=True).head()
    
    #分层采样
    print iris.sample(strata='name',n={'Iris-setosa' : 10,'Iris-versicolor' : 10}).head()
    print iris.sample(strata='name',frac={'Iris-setosa': 0.5,'Iris-versicolor': 0.4}).head()
    ```

8.  单击**运行**。

    ![运行节点.png](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3490659951/p65204.png)

9.  在**运行日志**中查看运行结果。

    完整的运行结果如下。

    ```
    Collection: ref_0
      odps.Table
        name: WB_BestPractice_dev.`pyodps_iris`
        schema:
          sepallength           : double      # 片长度(cm)
          sepalwidth            : double      # 片宽度(cm)
          petallength           : double      # 瓣长度(cm)
          petalwidth            : double      # 瓣宽度(cm)
          name                  : string      # 种类
    Sample[collection]
      _input:
      _parts:
        Scalar[int8]
          10
      _i:
      _replace:
        Scalar[boolean]
          False
    Collection: ref_0
      odps.Table
        name: WB_BestPractice_dev.`pyodps_iris`
        schema:
          sepallength           : double      # 片长度(cm)
          sepalwidth            : double      # 片宽度(cm)
          petallength           : double      # 瓣长度(cm)
          petalwidth            : double      # 瓣宽度(cm)
          name                  : string      # 种类
    Sample[collection]
      _input:
      _parts:
        Scalar[int8]
          10
      _i:
      _replace:
        Scalar[boolean]
          False
    Collection: ref_0
      odps.Table
        name: WB_BestPractice_dev.`pyodps_iris`
        schema:
          sepallength           : double      # 片长度(cm)
          sepalwidth            : double      # 片宽度(cm)
          petallength           : double      # 瓣长度(cm)
          petalwidth            : double      # 瓣宽度(cm)
          name                  : string      # 种类
    Sample[collection]
      _input:
      _parts:
        Scalar[int8]
          10
      _i:
      _replace:
        Scalar[boolean]
          False
    Collection: ref_0
      odps.Table
        name: WB_BestPractice_dev.`pyodps_iris`
        schema:
          sepallength           : double      # 片长度(cm)
          sepalwidth            : double      # 片宽度(cm)
          petallength           : double      # 瓣长度(cm)
          petalwidth            : double      # 瓣宽度(cm)
          name                  : string      # 种类
    Sample[collection]
      _input:
      _parts:
        Scalar[int8]
          10
      _i:
      _sampled_fields:
        name = Column[sequence(string)] 'name' from collection ref_0
        sepalwidth = Column[sequence(float64)] 'sepalwidth' from collection ref_0
      _replace:
        Scalar[boolean]
          False
    Executing RandomSample...
    Command: PAI -name RandomSample -project algo_public -Dreplace="false" -Dlifecycle="1" -DoutputTableName="tmp_pyodps_1570690014_69f3d75d_9537_4c9c_87ea_a5f6ad8d2e07" -DsampleSize="100" -DinputTableName="WB_BestPractice_dev.pyodps_iris";
    Instance ID: 20191010064654985g6co9592
    
    Sub Instance: create_output (20191010064700688g5cbn62m_71ee0561_bcc4_4147_b849_f74688353fb6)
    
    Sub Instance: without_replacement (20191010064703694g9cbn62m_93a8a15b_ffd1_4afe_8928_19f28455a15c)
    
    
    Try to fetch data from tunnel
    
        sepallength  sepalwidth  petallength  petalwidth            name
    0           5.1         3.5          1.4         0.2     Iris-setosa
    1           4.9         3.0          1.4         0.2     Iris-setosa
    2           4.7         3.2          1.3         0.2     Iris-setosa
    3           4.6         3.1          1.5         0.2     Iris-setosa
    4           5.0         3.6          1.4         0.2     Iris-setosa
    5           4.6         3.4          1.4         0.3     Iris-setosa
    6           4.4         2.9          1.4         0.2     Iris-setosa
    7           4.9         3.1          1.5         0.1     Iris-setosa
    8           4.8         3.4          1.6         0.2     Iris-setosa
    9           4.8         3.0          1.4         0.1     Iris-setosa
    10          4.3         3.0          1.1         0.1     Iris-setosa
    11          5.1         3.5          1.4         0.3     Iris-setosa
    12          5.7         3.8          1.7         0.3     Iris-setosa
    13          5.1         3.8          1.5         0.3     Iris-setosa
    14          5.1         3.7          1.5         0.4     Iris-setosa
    15          4.6         3.6          1.0         0.2     Iris-setosa
    16          5.1         3.3          1.7         0.5     Iris-setosa
    17          4.8         3.4          1.9         0.2     Iris-setosa
    18          5.0         3.4          1.6         0.4     Iris-setosa
    19          5.2         3.5          1.5         0.2     Iris-setosa
    20          5.2         3.4          1.4         0.2     Iris-setosa
    21          4.8         3.1          1.6         0.2     Iris-setosa
    22          5.2         4.1          1.5         0.1     Iris-setosa
    23          5.5         4.2          1.4         0.2     Iris-setosa
    24          4.9         3.1          1.5         0.1     Iris-setosa
    25          5.0         3.2          1.2         0.2     Iris-setosa
    26          4.4         3.0          1.3         0.2     Iris-setosa
    27          5.1         3.4          1.5         0.2     Iris-setosa
    28          5.0         3.5          1.3         0.3     Iris-setosa
    29          4.5         2.3          1.3         0.3     Iris-setosa
    ..          ...         ...          ...         ...             ...
    70          7.1         3.0          5.9         2.1  Iris-virginica
    71          7.6         3.0          6.6         2.1  Iris-virginica
    72          7.3         2.9          6.3         1.8  Iris-virginica
    73          7.2         3.6          6.1         2.5  Iris-virginica
    74          6.5         3.2          5.1         2.0  Iris-virginica
    75          6.8         3.0          5.5         2.1  Iris-virginica
    76          5.8         2.8          5.1         2.4  Iris-virginica
    77          7.7         3.8          6.7         2.2  Iris-virginica
    78          7.7         2.6          6.9         2.3  Iris-virginica
    79          7.7         2.8          6.7         2.0  Iris-virginica
    80          6.3         2.7          4.9         1.8  Iris-virginica
    81          6.7         3.3          5.7         2.1  Iris-virginica
    82          6.2         2.8          4.8         1.8  Iris-virginica
    83          6.1         3.0          4.9         1.8  Iris-virginica
    84          6.4         2.8          5.6         2.1  Iris-virginica
    85          7.2         3.0          5.8         1.6  Iris-virginica
    86          7.4         2.8          6.1         1.9  Iris-virginica
    87          7.9         3.8          6.4         2.0  Iris-virginica
    88          6.3         2.8          5.1         1.5  Iris-virginica
    89          6.3         3.4          5.6         2.4  Iris-virginica
    90          6.4         3.1          5.5         1.8  Iris-virginica
    91          6.0         3.0          4.8         1.8  Iris-virginica
    92          6.9         3.1          5.4         2.1  Iris-virginica
    93          6.9         3.1          5.1         2.3  Iris-virginica
    94          5.8         2.7          5.1         1.9  Iris-virginica
    95          6.8         3.2          5.9         2.3  Iris-virginica
    96          6.7         3.3          5.7         2.5  Iris-virginica
    97          6.3         2.5          5.0         1.9  Iris-virginica
    98          6.2         3.4          5.4         2.3  Iris-virginica
    99          5.9         3.0          5.1         1.8  Iris-virginica
    [100 rows x 5 columns]
    Executing RandomSample...
    Command: PAI -name RandomSample -project algo_public -Dreplace="false" -DsampleRatio="0.3" -DoutputTableName="tmp_pyodps_1570690039_e1867332_72ea_4656_928d_3bd6e31d87c7" -Dlifecycle="1" -DinputTableName="WB_BestPractice_dev.pyodps_iris";
    Instance ID: 20191010064720117gmpms38
    
    Sub Instance: create_output (20191010064725740grcbn62m_b338a671_6047_4360_8792_41d2e748e41f)
    
    Sub Instance: without_replacement (20191010064728747gtcbn62m_6c9914da_d5c3_4336_b076_163edb1bf48a)
    
    Try to fetch data from tunnel
        sepallength  sepalwidth  petallength  petalwidth             name
    0           5.1         3.5          1.4         0.2      Iris-setosa
    1           4.7         3.2          1.3         0.2      Iris-setosa
    2           4.6         3.1          1.5         0.2      Iris-setosa
    3           4.8         3.4          1.6         0.2      Iris-setosa
    4           5.7         4.4          1.5         0.4      Iris-setosa
    5           5.1         3.5          1.4         0.3      Iris-setosa
    6           5.0         3.4          1.6         0.4      Iris-setosa
    7           5.2         3.4          1.4         0.2      Iris-setosa
    8           4.9         3.1          1.5         0.1      Iris-setosa
    9           5.5         3.5          1.3         0.2      Iris-setosa
    10          4.4         3.2          1.3         0.2      Iris-setosa
    11          5.0         3.3          1.4         0.2      Iris-setosa
    12          5.7         2.8          4.5         1.3  Iris-versicolor
    13          5.2         2.7          3.9         1.4  Iris-versicolor
    14          5.0         2.0          3.5         1.0  Iris-versicolor
    15          5.6         2.9          3.6         1.3  Iris-versicolor
    16          5.8         2.7          4.1         1.0  Iris-versicolor
    17          6.1         2.8          4.0         1.3  Iris-versicolor
    18          6.6         3.0          4.4         1.4  Iris-versicolor
    19          6.8         2.8          4.8         1.4  Iris-versicolor
    20          6.0         2.9          4.5         1.5  Iris-versicolor
    21          5.4         3.0          4.5         1.5  Iris-versicolor
    22          5.7         2.9          4.2         1.3  Iris-versicolor
    23          5.7         2.8          4.1         1.3  Iris-versicolor
    24          6.3         2.9          5.6         1.8   Iris-virginica
    25          4.9         2.5          4.5         1.7   Iris-virginica
    26          6.7         2.5          5.8         1.8   Iris-virginica
    27          6.4         2.7          5.3         1.9   Iris-virginica
    28          6.8         3.0          5.5         2.1   Iris-virginica
    29          5.7         2.5          5.0         2.0   Iris-virginica
    30          5.8         2.8          5.1         2.4   Iris-virginica
    31          6.5         3.0          5.5         1.8   Iris-virginica
    32          6.0         2.2          5.0         1.5   Iris-virginica
    33          6.3         2.7          4.9         1.8   Iris-virginica
    34          7.2         3.2          6.0         1.8   Iris-virginica
    35          6.2         2.8          4.8         1.8   Iris-virginica
    36          6.1         3.0          4.9         1.8   Iris-virginica
    37          6.4         2.8          5.6         2.1   Iris-virginica
    38          7.2         3.0          5.8         1.6   Iris-virginica
    39          6.3         2.8          5.1         1.5   Iris-virginica
    40          6.4         3.1          5.5         1.8   Iris-virginica
    41          6.0         3.0          4.8         1.8   Iris-virginica
    42          6.8         3.2          5.9         2.3   Iris-virginica
    43          6.3         2.5          5.0         1.9   Iris-virginica
    44          6.2         3.4          5.4         2.3   Iris-virginica
    
    Executing WeightedSample...
    Command: PAI -name WeightedSample -project algo_public -DinputTableName="WB_BestPractice_dev.pyodps_iris" -DsampleSize="100" -DprobCol="sepallength" -Dreplace="false" -DoutputTableName="tmp_pyodps_1570690063_6a62857e_8f85_4ea7_99ef_08aa259546d4" -Dlifecycle="1";
    Instance ID: 20191010064743533gnpms38
    
    Sub Instance: create_output (20191010064748787gkdbn62m_8d47bfb7_e470_4cce_8b69_28811f190083)
    
    Sub Instance: without_replacement (20191010064751793gmdbn62m_230a1d26_5c2e_440e_a31d_fe9e63c6f906)
    
    Try to fetch data from tunnel
        sepallength  sepalwidth  petallength  petalwidth             name
    0           4.9         3.0          1.4         0.2      Iris-setosa
    1           4.7         3.2          1.3         0.2      Iris-setosa
    2           5.0         3.6          1.4         0.2      Iris-setosa
    3           5.4         3.9          1.7         0.4      Iris-setosa
    4           5.0         3.4          1.5         0.2      Iris-setosa
    5           4.4         2.9          1.4         0.2      Iris-setosa
    6           4.8         3.4          1.6         0.2      Iris-setosa
    7           4.8         3.0          1.4         0.1      Iris-setosa
    8           5.4         3.9          1.3         0.4      Iris-setosa
    9           5.1         3.5          1.4         0.3      Iris-setosa
    10          5.7         3.8          1.7         0.3      Iris-setosa
    11          4.6         3.6          1.0         0.2      Iris-setosa
    12          5.0         3.4          1.6         0.4      Iris-setosa
    13          5.2         3.5          1.5         0.2      Iris-setosa
    14          5.2         3.4          1.4         0.2      Iris-setosa
    15          4.7         3.2          1.6         0.2      Iris-setosa
    16          4.8         3.1          1.6         0.2      Iris-setosa
    17          5.5         4.2          1.4         0.2      Iris-setosa
    18          4.9         3.1          1.5         0.1      Iris-setosa
    19          5.0         3.2          1.2         0.2      Iris-setosa
    20          5.5         3.5          1.3         0.2      Iris-setosa
    21          4.9         3.1          1.5         0.1      Iris-setosa
    22          5.1         3.4          1.5         0.2      Iris-setosa
    23          4.5         2.3          1.3         0.3      Iris-setosa
    24          4.8         3.0          1.4         0.3      Iris-setosa
    25          5.1         3.8          1.6         0.2      Iris-setosa
    26          4.6         3.2          1.4         0.2      Iris-setosa
    27          5.3         3.7          1.5         0.2      Iris-setosa
    28          5.0         3.3          1.4         0.2      Iris-setosa
    29          7.0         3.2          4.7         1.4  Iris-versicolor
    ..          ...         ...          ...         ...              ...
    70          7.2         3.6          6.1         2.5   Iris-virginica
    71          6.4         2.7          5.3         1.9   Iris-virginica
    72          5.7         2.5          5.0         2.0   Iris-virginica
    73          5.8         2.8          5.1         2.4   Iris-virginica
    74          6.4         3.2          5.3         2.3   Iris-virginica
    75          7.7         3.8          6.7         2.2   Iris-virginica
    76          6.9         3.2          5.7         2.3   Iris-virginica
    77          5.6         2.8          4.9         2.0   Iris-virginica
    78          7.7         2.8          6.7         2.0   Iris-virginica
    79          6.3         2.7          4.9         1.8   Iris-virginica
    80          6.7         3.3          5.7         2.1   Iris-virginica
    81          7.2         3.2          6.0         1.8   Iris-virginica
    82          6.2         2.8          4.8         1.8   Iris-virginica
    83          6.1         3.0          4.9         1.8   Iris-virginica
    84          7.2         3.0          5.8         1.6   Iris-virginica
    85          7.9         3.8          6.4         2.0   Iris-virginica
    86          6.4         2.8          5.6         2.2   Iris-virginica
    87          6.3         2.8          5.1         1.5   Iris-virginica
    88          6.1         2.6          5.6         1.4   Iris-virginica
    89          6.3         3.4          5.6         2.4   Iris-virginica
    90          6.4         3.1          5.5         1.8   Iris-virginica
    91          6.0         3.0          4.8         1.8   Iris-virginica
    92          6.9         3.1          5.1         2.3   Iris-virginica
    93          5.8         2.7          5.1         1.9   Iris-virginica
    94          6.8         3.2          5.9         2.3   Iris-virginica
    95          6.7         3.3          5.7         2.5   Iris-virginica
    96          6.7         3.0          5.2         2.3   Iris-virginica
    97          6.5         3.0          5.2         2.0   Iris-virginica
    98          6.2         3.4          5.4         2.3   Iris-virginica
    99          5.9         3.0          5.1         1.8   Iris-virginica
    [100 rows x 5 columns]
    Executing WeightedSample...
    Command: PAI -name WeightedSample -project algo_public -DinputTableName="WB_BestPractice_dev.pyodps_iris" -DsampleSize="100" -DprobCol="sepalwidth" -Dreplace="true" -DoutputTableName="tmp_pyodps_1570690082_f55e899c_3cb4_4eeb_ade4_b8cb79e018dc" -Dlifecycle="1";
    Instance ID: 2019101006480392g9ers38
    
    Sub Instance: create_output (20191010064808827g9ebn62m_fb70c859_913a_4830_9248_8c8eaf134f1d)
    
    Sub Instance: with_replacement (20191010064811833gdebn62m_07544cc2_1d7d_4fa5_972d_196eb6b9f537)
    
    
        sepallength  sepalwidth  petallength  petalwidth             name
    0           5.1         3.5          1.4         0.2      Iris-setosa
    1           4.6         3.4          1.4         0.3      Iris-setosa
    2           5.0         3.4          1.5         0.2      Iris-setosa
    3           5.0         3.4          1.5         0.2      Iris-setosa
    4           4.4         2.9          1.4         0.2      Iris-setosa
    5           4.8         3.4          1.6         0.2      Iris-setosa
    6           5.8         4.0          1.2         0.2      Iris-setosa
    7           5.8         4.0          1.2         0.2      Iris-setosa
    8           5.1         3.5          1.4         0.3      Iris-setosa
    9           5.1         3.5          1.4         0.3      Iris-setosa
    10          5.1         3.5          1.4         0.3      Iris-setosa
    11          5.1         3.7          1.5         0.4      Iris-setosa
    12          4.6         3.6          1.0         0.2      Iris-setosa
    13          4.8         3.4          1.9         0.2      Iris-setosa
    14          5.0         3.0          1.6         0.2      Iris-setosa
    15          5.0         3.4          1.6         0.4      Iris-setosa
    16          5.2         3.4          1.4         0.2      Iris-setosa
    17          4.8         3.1          1.6         0.2      Iris-setosa
    18          4.8         3.1          1.6         0.2      Iris-setosa
    19          5.4         3.4          1.5         0.4      Iris-setosa
    20          5.4         3.4          1.5         0.4      Iris-setosa
    21          5.4         3.4          1.5         0.4      Iris-setosa
    22          5.2         4.1          1.5         0.1      Iris-setosa
    23          5.2         4.1          1.5         0.1      Iris-setosa
    24          5.5         4.2          1.4         0.2      Iris-setosa
    25          5.0         3.2          1.2         0.2      Iris-setosa
    26          4.9         3.1          1.5         0.1      Iris-setosa
    27          4.4         3.0          1.3         0.2      Iris-setosa
    28          5.1         3.4          1.5         0.2      Iris-setosa
    29          5.0         3.5          1.3         0.3      Iris-setosa
    ..          ...         ...          ...         ...              ...
    70          5.6         2.7          4.2         1.3  Iris-versicolor
    71          5.7         2.9          4.2         1.3  Iris-versicolor
    72          6.3         3.3          6.0         2.5   Iris-virginica
    73          7.1         3.0          5.9         2.1   Iris-virginica
    74          6.3         2.9          5.6         1.8   Iris-virginica
    75          7.3         2.9          6.3         1.8   Iris-virginica
    76          7.2         3.6          6.1         2.5   Iris-virginica
    77          6.4         2.7          5.3         1.9   Iris-virginica
    78          6.8         3.0          5.5         2.1   Iris-virginica
    79          6.8         3.0          5.5         2.1   Iris-virginica
    80          5.8         2.8          5.1         2.4   Iris-virginica
    81          6.2         2.8          4.8         1.8   Iris-virginica
    82          6.2         2.8          4.8         1.8   Iris-virginica
    83          6.1         3.0          4.9         1.8   Iris-virginica
    84          6.4         2.8          5.6         2.1   Iris-virginica
    85          7.2         3.0          5.8         1.6   Iris-virginica
    86          7.4         2.8          6.1         1.9   Iris-virginica
    87          7.4         2.8          6.1         1.9   Iris-virginica
    88          7.4         2.8          6.1         1.9   Iris-virginica
    89          6.4         3.1          5.5         1.8   Iris-virginica
    90          6.0         3.0          4.8         1.8   Iris-virginica
    91          6.9         3.1          5.4         2.1   Iris-virginica
    92          6.7         3.1          5.6         2.4   Iris-virginica
    93          6.7         3.1          5.6         2.4   Iris-virginica
    94          6.7         3.1          5.6         2.4   Iris-virginica
    95          6.9         3.1          5.1         2.3   Iris-virginica
    96          6.9         3.1          5.1         2.3   Iris-virginica
    97          6.7         3.0          5.2         2.3   Iris-virginica
    98          6.5         3.0          5.2         2.0   Iris-virginica
    99          5.9         3.0          5.1         1.8   Iris-virginica
    [100 rows x 5 columns]
    Executing StratifiedSample...
    Command: PAI -name StratifiedSample -project algo_public -Dlifecycle="1" -DoutputTableName="tmp_pyodps_1570690104_6cc52795_2a86_4634_a905_740b3a426d3f" -DsampleSize="Iris-setosa:10,Iris-versicolor:10" -DstrataColName="name" -DinputTableName="WB_BestPractice_dev.pyodps_iris";
    Instance ID: 20191010064824633gaco9592
    
    Sub Instance: create_output (20191010064829870g4fbn62m_dfa297c5_23b5_43f5_bb17_83ba8d263630)
    
    
    Sub Instance: stratified_sampling (20191010064831874g7fbn62m_fc9eddb7_42f1_49fe_8206_891ef451fb76)
    
    Try to fetch data from tunnel
        sepallength  sepalwidth  petallength  petalwidth             name
    0           5.4         3.9          1.7         0.4      Iris-setosa
    1           4.3         3.0          1.1         0.1      Iris-setosa
    2           5.4         3.9          1.3         0.4      Iris-setosa
    3           5.1         3.3          1.7         0.5      Iris-setosa
    4           4.7         3.2          1.6         0.2      Iris-setosa
    5           4.5         2.3          1.3         0.3      Iris-setosa
    6           5.0         3.5          1.6         0.6      Iris-setosa
    7           5.1         3.8          1.9         0.4      Iris-setosa
    8           4.8         3.0          1.4         0.3      Iris-setosa
    9           5.0         3.3          1.4         0.2      Iris-setosa
    10          7.0         3.2          4.7         1.4  Iris-versicolor
    11          5.5         2.3          4.0         1.3  Iris-versicolor
    12          6.5         2.8          4.6         1.5  Iris-versicolor
    13          5.6         3.0          4.5         1.5  Iris-versicolor
    14          5.7         2.6          3.5         1.0  Iris-versicolor
    15          5.5         2.4          3.7         1.0  Iris-versicolor
    16          5.0         2.3          3.3         1.0  Iris-versicolor
    17          5.6         2.7          4.2         1.3  Iris-versicolor
    18          5.7         3.0          4.2         1.2  Iris-versicolor
    19          5.1         2.5          3.0         1.1  Iris-versicolor
    Executing StratifiedSample...
    Command: PAI -name StratifiedSample -project algo_public -DsampleRatio="Iris-setosa:0.5,Iris-versicolor:0.4" -DoutputTableName="tmp_pyodps_1570690128_a68477cd_19e5_4fe0_bb39_4712f76dd967" -Dlifecycle="1" -DstrataColName="name" -DinputTableName="WB_BestPractice_dev.pyodps_iris";
    Instance ID: 20191010064848733gbers38
    
    Sub Instance: create_output (20191010064853918gwfbn62m_4eb22c22_7051_4372_8d13_05c5a417aa87)
    
    Sub Instance: stratified_sampling (20191010064855924g0gbn62m_b4242ac7_bd5a_47a8_a1f2_3367a6a101a7)
    
    Try to fetch data from tunnel
        sepallength  sepalwidth  petallength  petalwidth             name
    0           4.9         3.0          1.4         0.2      Iris-setosa
    1           4.7         3.2          1.3         0.2      Iris-setosa
    2           5.0         3.6          1.4         0.2      Iris-setosa
    3           5.4         3.9          1.7         0.4      Iris-setosa
    4           5.0         3.4          1.5         0.2      Iris-setosa
    5           5.4         3.7          1.5         0.2      Iris-setosa
    6           4.8         3.4          1.6         0.2      Iris-setosa
    7           4.8         3.0          1.4         0.1      Iris-setosa
    8           5.8         4.0          1.2         0.2      Iris-setosa
    9           5.4         3.4          1.7         0.2      Iris-setosa
    10          5.1         3.7          1.5         0.4      Iris-setosa
    11          4.8         3.4          1.9         0.2      Iris-setosa
    12          5.0         3.0          1.6         0.2      Iris-setosa
    13          5.0         3.4          1.6         0.4      Iris-setosa
    14          5.2         3.5          1.5         0.2      Iris-setosa
    15          5.2         3.4          1.4         0.2      Iris-setosa
    16          4.7         3.2          1.6         0.2      Iris-setosa
    17          5.2         4.1          1.5         0.1      Iris-setosa
    18          5.0         3.2          1.2         0.2      Iris-setosa
    19          5.1         3.4          1.5         0.2      Iris-setosa
    20          4.5         2.3          1.3         0.3      Iris-setosa
    21          5.0         3.5          1.6         0.6      Iris-setosa
    22          5.1         3.8          1.9         0.4      Iris-setosa
    23          5.1         3.8          1.6         0.2      Iris-setosa
    24          5.3         3.7          1.5         0.2      Iris-setosa
    25          7.0         3.2          4.7         1.4  Iris-versicolor
    26          6.4         3.2          4.5         1.5  Iris-versicolor
    27          6.9         3.1          4.9         1.5  Iris-versicolor
    28          6.5         2.8          4.6         1.5  Iris-versicolor
    29          5.7         2.8          4.5         1.3  Iris-versicolor
    30          6.6         2.9          4.6         1.3  Iris-versicolor
    31          5.6         2.9          3.6         1.3  Iris-versicolor
    32          5.6         3.0          4.5         1.5  Iris-versicolor
    33          5.6         2.5          3.9         1.1  Iris-versicolor
    34          6.1         2.8          4.7         1.2  Iris-versicolor
    35          6.8         2.8          4.8         1.4  Iris-versicolor
    36          5.5         2.4          3.8         1.1  Iris-versicolor
    37          5.5         2.4          3.7         1.0  Iris-versicolor
    38          6.0         2.7          5.1         1.6  Iris-versicolor
    39          5.6         3.0          4.1         1.3  Iris-versicolor
    40          5.5         2.6          4.4         1.2  Iris-versicolor
    41          6.1         3.0          4.6         1.4  Iris-versicolor
    42          5.7         3.0          4.2         1.2  Iris-versicolor
    43          5.7         2.9          4.2         1.3  Iris-versicolor
    44          6.2         2.9          4.3         1.3  Iris-versicolor
    ```


