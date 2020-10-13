---
keyword: Sequence
---

# Sequence

SequenceExpr代表二维数据集中的一列。SequenceExpr只可以从一个Collection中获取，不支持手动创建SequenceExpr。

## 获取列

您可以使用`collection.column_name`取出一列。

```
iris.sepallength.head(5)
   sepallength
0          5.1
1          4.9
2          4.7
3          4.6
4          5.0
```

当列名存储在一个字符串变量中，可以使用`df[column_name]`实现取出一列。

```
iris['sepallength'].head(5)
   sepallength
0          5.1
1          4.9
2          4.7
3          4.6
4          5.0
```

## 列类型

DataFrame拥有自己的类型系统，在使用表初始化时，MaxCompute的类型会被转换。这样可以支持更多类型的计算后端。目前，DataFrame的执行后端支持MaxCompute SQL、Pandas和数据库（MySQL和Postgres）。

MaxCompute和DataFrame的数据类型映射关系如下。

|MaxCompute类型|DataFrame类型|
|:-----------|:----------|
|BIGINT|INT64|
|DOUBLE|FLOAT64|
|STRING|STRING|
|DATETIME|DATETIME|
|BOOLEAN|BOOLEAN|
|DECIMAL|DECIMAL|
|ARRAY<VALUE\_TYPE\>|LIST<VALUE\_TYPE\>|
|MAP<KEY\_TYPE, VALUE\_TYPE\>|DICT<KEY\_TYPE, VALUE\_TYPE\>|
|当`options.sql.use_odps2_extension=True`时，还支持以下数据类型。|
|TINYINT|INT8|
|SMALLINT|INT16|
|INT|INT32|
|FLOAT|FLOAT32|

数据类型说明如下：

-   PyODPS DataFrame支持的数据类型有INT8、INT16、INT32、INT64、FLOAT32、FLOAT64、BOOLEAN、STRING、DECIMAL、DATETIME、LIST、DICT。
-   LIST和DICT必须填写其包含值的类型，否则会报错。
-   目前DataFrame暂不支持MaxCompute 2.0中新增的TIMESTAMP及STRUCT类型，后续版本将会陆续支持新增的类型。
-   在Sequence中可以通过`sequence.dtype`获取数据类型。

    ```
    iris.sepallength.dtype
    float64
    ```

-   如果要修改一列的类型，可以使用`astype`方法。该方法输入一个类型，并返回类型转换后的Sequence。

    ```
    iris.sepallength.astype('int')
       sepallength
    0            5
    1            4
    2            4
    3            4
    4            5
    ```


## 列名

在DataFrame的计算过程中，每个Sequence必须要有列名。通常，DataFrame会为每个Sequence起一个名字。

```
iris.groupby('name').sepalwidth.max()
   sepalwidth_max
0             4.4
1             3.4
2             3.8
```

上述示例中，`sepalwidth`取最大值后被命名为`sepalwidth_max`。除部分操作外（例如，给指定Sequence加上一个Scalar时，结果会自动被命名为这个Sequence的名字），为Sequence命名的操作需要您自己手动完成。

Sequence提供`rename`方法对一列进行重命名。

```
iris.sepalwidth.rename('sepal_width').head(5)
   sepal_width
0          3.5
1          3.0
2          3.2
3          3.1
4          3.6
```

## 简单的列变换

您可以对一个Sequence进行运算，返回一个新的Sequence，这种操作类似于对简单的Python变量进行运算。Sequence支持对数值列进行四则运算，而对字符串则仅支持字符串的相加操作。

```
(iris.sepallength + 5).head(5)
   sepallength
0         10.1
1          9.9
2          9.7
3          9.6
4         10.0
```

两列共同参与运算时，PyODPS无法确定最终显示的列名，需要您手动指定。更多列变换说明，请参见[列运算]()。

```
(iris.sepallength + iris.sepalwidth).rename('sum_sepal').head(5)
   sum_sepal
0        8.6
1        7.9
2        7.9
3        7.7
4        8.6
```

