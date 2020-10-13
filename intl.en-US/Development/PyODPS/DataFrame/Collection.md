---
keyword: [Collection, Two-dimensional dataset, Filter, Column operations]
---

# Collection

CollectionExpr supports all operations on DataFrame two-dimensional datasets. It can be seen as a MaxCompute table or a spreadsheet. DataFrame objects are also CollectionExpr objects. CollectionExpr supports various operations on two-dimensional datasets such as column operations, filter operations, and transformation operations.

## Retrieve types

You can use the `dtypes` method to retrieve the types of all columns in a CollectionExpr object. The `dtypes` method returns a [Schema type]().

```
iris.dtypes
odps.Schema {
  sepallength           float64
  sepalwidth            float64
  petallength           float64
  petalwidth            float64
  name                  string
}
```

## Select, add, and remove columns

-   Select columns

    You can use the `expr[columns]` syntax to select certain columns from a CollectionExpr object to form a new dataset.

    ```
    iris['name', 'sepallength'].head(5)
              name  sepallength
    0  Iris-setosa          5.1
    1  Iris-setosa          4.9
    2  Iris-setosa          4.7
    3  Iris-setosa          4.6
    4  Iris-setosa          5.0
    ```

    **Note:**

    If only one column is needed, you need to add a comma \(,\) after the column name or explicitly mark the column as a list, such as`iris[iris.sepal_length,]` or `iris[[iris.sepal_length]]`. Otherwise, a Sequence object, instead of a Collection object, is returned.

-   Delete columns

    You can use the `exclude` method to exclude certain columns of the original dataset from the new dataset.

    ```
    iris.exclude('sepallength', 'petallength')[:5]
       sepalwidth  petalwidth         name
    0         3.5         0.2  Iris-setosa
    1         3.0         0.2  Iris-setosa
    2         3.2         0.2  Iris-setosa
    3         3.1         0.2  Iris-setosa
    4         3.6         0.2  Iris-setosa
    ```

    In PyODPS version 0.7.2 and later, you can use a new method to directly exclude certain columns from the dataset.

    ```
    del iris['sepallength']
    del iris['petallength']
    iris[:5]
       sepalwidth  petalwidth         name
    0         3.5         0.2  Iris-setosa
    1         3.0         0.2  Iris-setosa
    2         3.2         0.2  Iris-setosa
    3         3.1         0.2  Iris-setosa
    4         3.6         0.2  Iris-setosa
    ```

-   Add columns

    You can use the `expr[expr, new_sequence]` syntax to add a transformed column to an existing Collection object. The new column is part of the new Collection.

    In the following example, a new column is created by adding one to each element in the `sepalwidth` column of `iris`, renamed to `sepalwidthplus1`, and appended to the dataset to form a new dataset.

    ```
    iris[iris, (iris.sepalwidth + 1).rename('sepalwidthplus1')].head(5)
       sepallength  sepalwidth  petallength  petalwidth         name  \
    0          5.1         3.5          1.4         0.2  Iris-setosa
    1          4.9         3.0          1.4         0.2  Iris-setosa
    2          4.7         3.2          1.3         0.2  Iris-setosa
    3          4.6         3.1          1.5         0.2  Iris-setosa
    4          5.0         3.6          1.4         0.2  Iris-setosa
    
       sepalwidthplus1
    0              4.5
    1              4.0
    2              4.2
    3              4.1
    4              4.6
    ```

    When you use the `expr[expr, new_sequence]` syntax, note that the transformed column may have the same name as the original column. Rename the new column if you want to merge it with the original Collection.

    In PyODPS version 0.7.2 and later, you can directly append a column to the current dataset.

    ```
    iris['sepalwidthplus1'] = iris.sepalwidth + 1
    iris.head(5)
       sepallength  sepalwidth  petallength  petalwidth         name  \
    0          5.1         3.5          1.4         0.2  Iris-setosa
    1          4.9         3.0          1.4         0.2  Iris-setosa
    2          4.7         3.2          1.3         0.2  Iris-setosa
    3          4.6         3.1          1.5         0.2  Iris-setosa
    4          5.0         3.6          1.4         0.2  Iris-setosa
    
       sepalwidthplus1
    0              4.5
    1              4.0
    2              4.2
    3              4.1
    4              4.6
    ```

-   Add and delete columns simultaneously

    You can also use the `exclude` method to exclude the original column and append the transformed column to the dataset, without the need to rename the new column.

    ```
    iris[iris.exclude('sepalwidth'), iris.sepalwidth * 2].head(5)
       sepallength  petallength  petalwidth         name  sepalwidth
    0          5.1          1.4         0.2  Iris-setosa         7.0
    1          4.9          1.4         0.2  Iris-setosa         6.0
    2          4.7          1.3         0.2  Iris-setosa         6.4
    3          4.6          1.5         0.2  Iris-setosa         6.2
    4          5.0          1.4         0.2  Iris-setosa         7.2
    ```

    In PyODPS version 0.7.2 and later, you can directly overwrite an existing column on the current dataset. The following example illustrates the syntax.

    ```
    iris['sepalwidth'] = iris.sepalwidth * 2
    iris.head(5)
       sepallength  sepalwidth  petallength  petalwidth         name
    0          5.1         7.0          1.4         0.2  Iris-setosa
    1          4.9         6.0          1.4         0.2  Iris-setosa
    2          4.7         6.4          1.3         0.2  Iris-setosa
    3          4.6         6.2          1.5         0.2  Iris-setosa
    4          5.0         7.2          1.4         0.2  Iris-setosa
    ```

    The `select` method provides another way to create a new Collection. When you use this method, you need to pass in the selected columns as input parameters. You can use the `keyword` argument to rename a column to the given keyword.

    ```
    iris.select('name', sepalwidthminus1=iris.sepalwidth - 1).head(5)
              name  sepalwidthminus1
    0  Iris-setosa               2.5
    1  Iris-setosa               2.0
    2  Iris-setosa               2.2
    3  Iris-setosa               2.1
    4  Iris-setosa               2.6
    ```

    You can also pass in a lambda expression, which takes the result from the previous operation as a parameter. During the execution, PyODPS checks the lambda expression and passes in the Collection object generated from the previous operation and replaces it with the correct columns.

    ```
    iris['name', 'petallength'][[lambda x: x.name]].head(5)
              name
    0  Iris-setosa
    1  Iris-setosa
    2  Iris-setosa
    3  Iris-setosa
    4  Iris-setosa
    ```

    In PyODPS version 0.7.2 and later, conditional assignments are supported.

    ```
    iris[iris.sepallength > 5.0, 'sepalwidth'] = iris.sepalwidth * 2
    iris.head(5)
       sepallength  sepalwidth  petallength  petalwidth         name
    0          5.1        14.0          1.4         0.2  Iris-setosa
    1          4.9         6.0          1.4         0.2  Iris-setosa
    2          4.7         6.4          1.3         0.2  Iris-setosa
    3          4.6         6.2          1.5         0.2  Iris-setosa
    4          5.0         7.2          1.4         0.2  Iris-setosa
    ```


## Introduce constants and random numbers

-   DataFrame allows you to append a column of constants to a Collection object. [Scalar](https://pyodps.readthedocs.io/zh_CN/latest/api-df.html) is required for this operation, and you need to manually specify the column name.

    ```
    from odps.df import Scalar
    iris[iris, Scalar(1).rename('id')][:5]
       sepallength  sepalwidth  petallength  petalwidth         name  id
    0          5.1         3.5          1.4         0.2  Iris-setosa   1
    1          4.9         3.0          1.4         0.2  Iris-setosa   1
    2          4.7         3.2          1.3         0.2  Iris-setosa   1
    3          4.6         3.1          1.5         0.2  Iris-setosa   1
    4          5.0         3.6          1.4         0.2  Iris-setosa   1
    ```

    You can use [NullScalar](https://pyodps.readthedocs.io/zh_CN/latest/api-df.html) to specify a null column. In this case, you need to specify the field type.

    ```
    from odps.df import NullScalar
    iris[iris, NullScalar('float').rename('fid')][:5]
       sepal_length  sepal_width  petal_length  petal_width     category   fid
    0           5.1          3.5           1.4          0.2  Iris-setosa  None
    1           4.9          3.0           1.4          0.2  Iris-setosa  None
    2           4.7          3.2           1.3          0.2  Iris-setosa  None
    3           4.6          3.1           1.5          0.2  Iris-setosa  None
    4           5.0          3.6           1.4          0.2  Iris-setosa  None
    ```

    In PyODPS version 0.7.12 and later, a simpler method is provided.

    ```
    iris['id'] = 1
    iris
       sepallength  sepalwidth  petallength  petalwidth         name  id
    0          5.1         3.5          1.4         0.2  Iris-setosa   1
    1          4.9         3.0          1.4         0.2  Iris-setosa   1
    2          4.7         3.2          1.3         0.2  Iris-setosa   1
    3          4.6         3.1          1.5         0.2  Iris-setosa   1
    4          5.0         3.6          1.4         0.2  Iris-setosa   1
    ```

    Note that this method cannot automatically recognize the type of null values. Therefore, we recommend that you use the following syntax to add null columns.

    ```
    iris['null_col'] = NullScalar('float')
    iris
       sepallength  sepalwidth  petallength  petalwidth         name  null_col
    0          5.1         3.5          1.4         0.2  Iris-setosa      None
    1          4.9         3.0          1.4         0.2  Iris-setosa      None
    2          4.7         3.2          1.3         0.2  Iris-setosa      None
    3          4.6         3.1          1.5         0.2  Iris-setosa      None
    4          5.0         3.6          1.4         0.2  Iris-setosa      None
    ```

-   DataFrame also allows you to append a column of random numbers to a Collection object. The column type is FLOAT and the value range is 0-1. Each number has a different value. [RandomScalar](https://pyodps.readthedocs.io/zh_CN/latest/api-df.html) is required for this operation, and its parameter is an optional random seed.

    ```
    from odps.df import RandomScalar
    iris[iris, RandomScalar().rename('rand_val')][:5]
       sepallength  sepalwidth  petallength  petalwidth         name  rand_val
    0          5.1         3.5          1.4         0.2  Iris-setosa  0.000471
    1          4.9         3.0          1.4         0.2  Iris-setosa  0.799520
    2          4.7         3.2          1.3         0.2  Iris-setosa  0.834609
    3          4.6         3.1          1.5         0.2  Iris-setosa  0.106921
    4          5.0         3.6          1.4         0.2  Iris-setosa  0.763442
    ```


## Filter data

A Collection object allows you to filter data. In the following example, the records where `sepallength` is greater than 5 are retrieved.

```
iris[iris.sepallength > 5].head(5)
   sepallength  sepalwidth  petallength  petalwidth         name
0          5.1         3.5          1.4         0.2  Iris-setosa
1          5.4         3.9          1.7         0.4  Iris-setosa
2          5.4         3.7          1.5         0.2  Iris-setosa
3          5.8         4.0          1.2         0.2  Iris-setosa
4          5.7         4.4          1.5         0.4  Iris-setosa
```

The following example shows how to query records by using multiple filter conditions:

-   AND \(&\) operator

    ```
    iris[(iris.sepallength < 5) & (iris['petallength'] > 1.5)].head(5)
       sepallength  sepalwidth  petallength  petalwidth             name
    0          4.8         3.4          1.6         0.2      Iris-setosa
    1          4.8         3.4          1.9         0.2      Iris-setosa
    2          4.7         3.2          1.6         0.2      Iris-setosa
    3          4.8         3.1          1.6         0.2      Iris-setosa
    4          4.9         2.4          3.3         1.0  Iris-versicolor
    ```

-   OR \(\|\) operator

    ```
    iris[(iris.sepalwidth < 2.5) | (iris.sepalwidth > 4)].head(5)
       sepallength  sepalwidth  petallength  petalwidth             name
    0          5.7         4.4          1.5         0.4      Iris-setosa
    1          5.2         4.1          1.5         0.1      Iris-setosa
    2          5.5         4.2          1.4         0.2      Iris-setosa
    3          4.5         2.3          1.3         0.3      Iris-setosa
    4          5.5         2.3          4.0         1.3  Iris-versicolor
    ```

    **Note:** You must use an ampersand `&` to reprent the AND operator and use a vertical bar `|` to represent the OR operator. `and` and `or` are not allowed.

-   NOT \(~\) operator

    ```
    iris[~(iris.sepalwidth > 3)].head(5)
       sepallength  sepalwidth  petallength  petalwidth         name
    0          4.9         3.0          1.4         0.2  Iris-setosa
    1          4.4         2.9          1.4         0.2  Iris-setosa
    2          4.8         3.0          1.4         0.1  Iris-setosa
    3          4.3         3.0          1.1         0.1  Iris-setosa
    4          5.0         3.0          1.6         0.2  Iris-setosa
    ```

-   You can explicitly call the `filter` method to specify multiple conditions by using AND \(&\) operators.

    ```
    iris.filter(iris.sepalwidth > 3.5, iris.sepalwidth < 4).head(5)
       sepallength  sepalwidth  petallength  petalwidth         name
    0          5.0         3.6          1.4         0.2  Iris-setosa
    1          5.4         3.9          1.7         0.4  Iris-setosa
    2          5.4         3.7          1.5         0.2  Iris-setosa
    3          5.4         3.9          1.3         0.4  Iris-setosa
    4          5.7         3.8          1.7         0.3  Iris-setosa
    ```

-   You can use a lambda expression to perform sequential operations.

    ```
    iris[iris.sepalwidth > 3.8]['name', lambda x: x.sepallength + 1]
              name  sepallength
    0  Iris-setosa          6.4
    1  Iris-setosa          6.8
    2  Iris-setosa          6.7
    3  Iris-setosa          6.4
    4  Iris-setosa          6.2
    5  Iris-setosa          6.5
    ```

-   If a Collection object contains a BOOLEAN column, you can use the column as a filter condition.

    ```
    df.dtypes
    odps.Schema {
      a boolean
      b int64
    }
    df[df.a]
          a  b
    0  True  1
    1  True  3
    ```

    Therefore, when you retrieve a single Sequence from a Collection object, only the BOOLEAN column can be used as a valid filter condition.

    ```
    df[df.a, ] # retrieve a one-column collection.
    df[[df.a]] # retrieve a one-column collection.
    df.select(df.a) # retrieve a one-column collection explicitly.
    df[df.a] # column 'a' is a boolean column. Filter data on the boolean column.
    df.a # retrieve a column from a collection.
    df['a'] # retrieve a column from a collection.
    ```

-   You can also use the`query` method in Pandas to filter data, and use column names such as `sepallength` in an expression. In query statements, both the ampersand `&` and `and` represent the AND operator, and both the vertical bar `|` and `or` represent the OR operator.

    ```
    iris.query("(sepallength < 5) and (petallength > 1.5)").head(5)
       sepallength  sepalwidth  petallength  petalwidth             name
    0          4.8         3.4          1.6         0.2      Iris-setosa
    1          4.8         3.4          1.9         0.2      Iris-setosa
    2          4.7         3.2          1.6         0.2      Iris-setosa
    3          4.8         3.1          1.6         0.2      Iris-setosa
    4          4.9         2.4          3.3         1.0  Iris-versicolor
    ```

    When a local variable is required in an expression, add an at sign `@` before the variable name.

    ```
    var = 4
    iris.query("(iris.sepalwidth < 2.5) | (sepalwidth > @var)").head(5)
       sepallength  sepalwidth  petallength  petalwidth             name
    0          5.7         4.4          1.5         0.4      Iris-setosa
    1          5.2         4.1          1.5         0.1      Iris-setosa
    2          5.5         4.2          1.4         0.2      Iris-setosa
    3          4.5         2.3          1.3         0.3      Iris-setosa
    4          5.5         2.3          4.0         1.3  Iris-versicolor
    ```

    The `query` method supports the following syntax.

    |Syntax|Description|
    |:-----|:----------|
    |name|Names without the at sign `@` are processed as column names. Names with the at sign \(@\) are processed as local variables.|
    |operator|The following operators are supported: `+`, `-`, `*`, `/`, `//`, `%`, `**`, `==`,`! =`, `<`, `<=`, `>`, `>=`, `in`, `not in`.|
    |bool|An ampersand `&` and `and` represent the AND operator. A vertical bar `|` and `or` represent the OR operator.|
    |attribute|This syntax is used to retrieve the attribute of the object.|
    |index, slice, subscript|Slice operations.|


## Lateral view

You can use the `explode` method to convert the LIST and MAP columns into multiple rows for output. You can also use the `apply` method to generate a lateral view. You often need to merge the output with the columns in the original table for operations such as aggregation. In this case, you can use the lateral view function of DataFrame. That is, map the collection generated by the lateral view function with the column names in the original collection.

The following example shows how to generate a lateral view.

```
df
   id         a             b
0   1  [a1, b1]  [a2, b2, c2]
1   2      [c1]      [d2, e2]
df[df.id, df.a.explode(), df.b]
   id   a             b
0   1  a1  [a2, b2, c2]
1   1  b1  [a2, b2, c2]
2   2  c1      [d2, e2]
df[df.id, df.a.explode(), df.b.explode()]
   id   a   b
0   1  a1  a2
1   1  a1  b2
2   1  a1  c2
3   1  b1  a2
4   1  b1  b2
5   1  b1  c2
6   2  c1  d2
7   2  c1  e2
```

If the lateral view method does not produce output for a row, the row does not appear in the output by default. To display the row in the output, you can set keep\_nulls to True. In this case, the column value for the row is None in the output.

```
df
   id         a
0   1  [a1, b1]
1   2        []
df[df.id, df.a.explode()]
   id   a
0   1  a1
1   1  b1
df[df.id, df.a.explode(keep_nulls=True)]
   id     a
0   1    a1
1   1    b1
2   2  None
```

For more information about how to generate a lateral view by using the `explode` method, see [Collection-related operations]().

## Limit output records

Output the first three records only.

```
iris[:3]
   sepallength  sepalwidth  petallength  petalwidth         name
0          5.1         3.5          1.4         0.2  Iris-setosa
1          4.9         3.0          1.4         0.2  Iris-setosa
2          4.7         3.2          1.3         0.2  Iris-setosa
```

For MaxCompute SQL backend, `start` and `step` methods are not supported in slice operations. But you can use the `limit` method.

```
iris.limit(3)
   sepallength  sepalwidth  petallength  petalwidth         name
0          5.1         3.5          1.4         0.2  Iris-setosa
1          4.9         3.0          1.4         0.2  Iris-setosa
2          4.7         3.2          1.3         0.2  Iris-setosa
```

**Note:** You can perform slice operations on Collection objects only, not Sequence objects.

