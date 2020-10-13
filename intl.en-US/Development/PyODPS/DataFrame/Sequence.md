---
keyword: Sequence
---

# Sequence

SequenceExpr represents a column in a two-dimensional dataset. You cannot manually create a SequenceExpr object. Instead, you can retrieve one from a Collection object.

## Column retrieval

You can use `collection.column_name` to retrieve a column.

```
iris.sepallength.head(5)
   sepallength
0          5.1
1          4.9
2          4.7
3          4.6
4          5.0
```

If the column name is stored in a string variable, you can use `df[column_name]` to retrieve a column.

```
iris['sepallength'].head(5)
   sepallength
0          5.1
1          4.9
2          4.7
3          4.6
4          5.0
```

## Column types

DataFrame has its own type system. When a table is initialized, the MaxCompute data types are cast. This design provides support for more types of execution backends. The execution backend of DataFrame can be MaxCompute SQL, Pandas, or a MySQL or PostgreSQL database.

The following table lists the mappings between MaxCompute and DataFrame data types.

|MaxCompute type|DataFrame type|
|:--------------|:-------------|
|BIGINT|INT64|
|DOUBLE|FLOAT64|
|STRING|STRING|
|DATETIME|DATETIME|
|BOOLEAN|BOOLEAN|
|DECIMAL|DECIMAL|
|ARRAY<VALUE\_TYPE\>|LIST<VALUE\_TYPE\>|
|MAP<KEY\_TYPE, VALUE\_TYPE\>|DICT<KEY\_TYPE, VALUE\_TYPE\>|
|When `options.sql.use_odps2_extension=True`, the following data types are also supported.|
|TINYINT|INT8|
|SMALLINT|INT16|
|INT|INT32|
|FLOAT|FLOAT32|

The following items describe DataFrame data types:

-   PyODPS DataFrame supports the following data types: INT8, INT16, INT32, INT64, FLOAT32, FLOAT64, BOOLEAN, STRING, DECIMAL, DATETIME, LIST, and DICT.
-   For LIST and DICT types, you must specify the types of the values they contain. Otherwise, an error occurs.
-   DataFrame does not support the TIMESTAMP and STRUCT types introduced in MaxCompute 2.0. The new types will be supported in future releases.
-   You can use `sequence.dtype` to retrieve the data type of a Sequence object.

    ```
    iris.sepallength.dtype
    float64
    ```

-   You can use the `astype` method to change the type of a column. This method requires a type as input and returns the converted Sequence object as output.

    ```
    iris.sepallength.astype('int')
       sepallength
    0            5
    1            4
    2            4
    3            4
    4            5
    ```


## Column name

In DataFrame computing, a Sequence object must have a column name. Typically, DataFrame automatically creates a name for each Sequence object.

```
iris.groupby('name').sepalwidth.max()
   sepalwidth_max
0             4.4
1             3.4
2             3.8
```

In the preceding example, `sepalwidth` is named `sepalwidth_max` after the max\(\) method is applied to take the maximum value. In some operations, such as adding a Scalar object to a Sequence object, the resulting column is automatically assigned the name of the Sequence object. In other cases, you need to manually name a Sequence object.

You can use the `rename` method to rename a Sequence object.

```
iris.sepalwidth.rename('sepal_width').head(5)
   sepal_width
0          3.5
1          3.0
2          3.2
3          3.1
4          3.6
```

## Simple column transformations

You can perform operations on a Sequence object to obtain a new Sequence object. This is similar to performing operations on a simple Python variable.

```
(iris.sepallength + 5).head(5)
   sepallength
0         10.1
1          9.9
2          9.7
3          9.6
4         10.0
```

Note that when two columns are involved in operations, you need to manually name the resulting column. For more information about column transformations, see [Column operations]().

```
(iris.sepallength + iris.sepalwidth).rename('sum_sepal').head(5)
   sum_sepal
0        8.6
1        7.9
2        7.9
3        7.7
4        8.6
```

