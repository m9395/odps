---
keyword: [DataFrame, execution method]
---

# Execution

This topic describes the execution methods that are supported in DataFrame API operations.

## Deferred execution

DataFrame API operations are not automatically executed. These operations are executed only when you explicitly call the `execute` method or when you call the methods that internally call `execute`. The following table lists the methods that internally call the execute method.

|Method|Description|Return value|
|:-----|:----------|:-----------|
|persist|Saves the execution results to MaxCompute tables.|PyODPS DataFrame|
|execute|Executes the operations and returns all the results.|ResultFrame|
|head|Executes the operations and returns the first N rows of result data.|ResultFrame|
|tail|Executes the operations and returns the last N rows of result data.|ResultFrame|
|to\_pandas|Converts a Collection object to a pandas DataFrame object or a Sequence object to a Series object. If you set the wrap parameter to True, an Alibaba Cloud MaxCompute SDK for python \(PyODPS\) DataFrame object is returned.|-   If the wrap parameter is set to True, a PyODPS DataFrame object is returned.
-   If the wrap parameter is set to False, a pandas DataFrame object is returned. The default value of the wrap parameter is False. |
|plot, hist, and boxplot|Plotting methods.|N/A|

**Note:** In an interactive environment, PyODPS DataFrame automatically calls the `execute` method when it displays result data or when it calls the `repr` method. You do not need to manually call the `execute` method.

**Example**

```
>>> iris[iris.sepallength < 5][:5]
   sepallength  sepalwidth  petallength  petalwidth         name
0          4.9         3.0          1.4         0.2  Iris-setosa
1          4.7         3.2          1.3         0.2  Iris-setosa
2          4.6         3.1          1.5         0.2  Iris-setosa
3          4.6         3.4          1.4         0.3  Iris-setosa
4          4.4         2.9          1.4         0.2  Iris-setosa
```

To disable automatic execution, run the following code:

```
>>> from odps import options
>>> options.interactive = False
>>>
>>> iris[iris.sepallength < 5][:5]
Collection: ref_0
  odps.Table
    name: odps_test_sqltask_finance.`pyodps_iris`
    schema:
      sepallength           : double
      sepalwidth            : double
      petallength           : double
      petalwidth            : double
      name                  : string

Collection: ref_1
  Filter[collection]
    collection: ref_0
    predicate:
      Less[sequence(boolean)]
        sepallength = Column[sequence(float64)] 'sepallength' from collection ref_0
        Scalar[int8]
          5

Slice[collection]
  collection: ref_1
  stop:
    Scalar[int8]
      5
```

If you call `repr` to return a printable representation of the object, the entire abstract syntax tree is displayed.

**Note:** The ResultFrame is a result set and cannot be used in subsequent calculations.

To retrieve all records from the ResultFrame, use an iterative method.

```
>>> result = iris.head(3)
>>> for r in result:
>>>     print(list(r))
[5.0999999999999996, 3.5, 1.3999999999999999, 0.20000000000000001, u'Iris-setosa']
[4.9000000000000004, 3.0, 1.3999999999999999, 0.20000000000000001, u'Iris-setosa']
[4.7000000000000002, 3.2000000000000002, 1.3, 0.20000000000000001, u'Iris-setosa']
```

If pandas is installed, a ResultFrame can be converted into a pandas DataFrame or a PyODPS DataFrame that uses the pandas backend.

```
pd_df = iris.head(3).to_pandas()  # Return a pandas DataFrame.
 wrapped_df = iris.head(3).to_pandas(wrap=True)  # Return a PyODPS DataFrame that uses the pandas backend.
```

## Save results to MaxCompute tables

You can call the `persist` method to return a new DataFrame object for a Collection object. The persist method uses the table name as the parameter.

```
>>> iris2 = iris[iris.sepalwidth < 2.5].persist('pyodps_iris2')
>>> iris2.head(5)
   sepallength  sepalwidth  petallength  petalwidth             name
0          4.5         2.3          1.3         0.3      Iris-setosa
1          5.5         2.3          4.0         1.3  Iris-versicolor
2          4.9         2.4          3.3         1.0  Iris-versicolor
3          5.0         2.0          3.5         1.0  Iris-versicolor
4          6.0         2.2          4.0         1.0  Iris-versicolor
```

You can pass the `partitions` parameter to the `persist` method to create a partitioned table. The table is partitioned based on the columns that are specified by `partitions`.

```
iris3 = iris[iris.sepalwidth < 2.5].persist('pyodps_iris3', partitions=['name'])
 iris3.data
odps.Table
  name: odps_test_sqltask_finance.`pyodps_iris3`
  schema:
    sepallength           : double
    sepalwidth            : double
    petallength           : double
    petalwidth            : double
  partitions:
    name                  : string
```

To write data to a partition of an existing table, you can pass the `partition` parameter to the `persist` method. The partition parameter specifies the destination partition. For example, set the partition parameter to `ds=******`. The table must contain all columns of the DataFrame object and the columns must be of the same type. The `drop_partition` and `create_partition` parameters are valid only if the partition parameter is specified. The drop\_partition parameter specifies whether to delete the specified partition if the partition exists. The create\_partition parameter specifies whether to create the specified partition if the partition does not exist.

```
iris[iris.sepalwidth < 2.5].persist('pyodps_iris4', partition='ds=test', drop_partition=True, create_partition=True)
```

When you write data to a table, you can specify the time-to-live \(TTL\) of the table. For example, the following statement sets the TTL of the table to 10 days.

```
iris[iris.sepalwidth < 2.5].persist('pyodps_iris5', lifecycle=10)
```

If the data source does not contain MaxCompute objects, you must manually specify the MaxCompute object or mark the entrance object as global when you call the `persist` method.

```
# Assume that the entrance object of PyODPS is o.
# Specify the entrance object.
df.persist('table_name', odps=o)
# Alternatively, mark the entrance object as global.
o.to_global()
df.persist('table_name')
```

## Save results to pandas DataFrame

You can call the `to_pandas` method to save results to pandas DataFrame objects. If the `wrap` parameter is set to True, a PyODPS DataFrame object is returned.

```
type(iris[iris.sepalwidth < 2.5].to_pandas())
pandas.core.frame.DataFrame
type(iris[iris.sepalwidth < 2.5].to_pandas(wrap=True))
odps.df.core.DataFrame
```

After data is read from [Tables](), PyODPS can execute the `open_reader` method and use `reader.to_pandas()` to convert Collection objects to pandas DataFrame objects.

## Set runtime parameters

You can set the runtime parameters for methods such as `execute`, `persist`, and `to_pandas`. This setting is valid only for the MaxCompute SQL backend.

-   Set global parameters. For more information, see [t21167.md\#section\_jr3\_w4m\_cfb](/intl.en-US/Development/PyODPS/Basic operations/SQL.md).
-   You can specify the `hints` parameter in these methods. This ensures that the specified runtime parameters are valid only for the current calculation.

    ```
    iris[iris.sepallength < 5].to_pandas(hints={'odps.sql.mapper.split.size': 16})
    ```


## Display details at runtime

To view the logview of an instance at runtime, you must modify the global configurations.

```
from odps import options
 options.verbose = True

 iris[iris.sepallength < 5].exclude('sepallength')[:5].execute()
Sql compiled:
SELECT t1.`sepalwidth`, t1.`petallength`, t1.`petalwidth`, t1.`name`
FROM odps_test_sqltask_finance.`pyodps_iris` t1
WHERE t1.`sepallength` < 5
LIMIT 5
logview:
http://logview

   sepalwidth  petallength  petalwidth         name
0         3.0          1.4         0.2  Iris-setosa
1         3.2          1.3         0.2  Iris-setosa
2         3.1          1.5         0.2  Iris-setosa
3         3.4          1.4         0.3  Iris-setosa
4         2.9          1.4         0.2  Iris-setosa
```

You can specify a logging function.

```
my_logs = []
 def my_logger(x):
     my_logs.append(x)

 options.verbose_log = my_logger

 iris[iris.sepallength < 5].exclude('sepallength')[:5].execute()
   sepalwidth  petallength  petalwidth         name
0         3.0          1.4         0.2  Iris-setosa
1         3.2          1.3         0.2  Iris-setosa
2         3.1          1.5         0.2  Iris-setosa
3         3.4          1.4         0.3  Iris-setosa
4         2.9          1.4         0.2  Iris-setosa

 print(my_logs)
['Sql compiled:', 'SELECT t1.`sepalwidth`, t1.`petallength`, t1.`petalwidth`, t1.`name` \nFROM odps_test_sqltask_finance.`pyodps_iris` t1 \nWHERE t1.`sepallength` < 5 \nLIMIT 5', 'logview:', u'http://logview']
```

## Cache intermediate results

When PyODPS DataFrame calculates data, some Collection objects are used multiple times. To view the execution results of an intermediate process, you can call the `cache` method to mark a Collection object that you want to calculate first.

**Note:** The execution of the `cache` method is deferred. Automatic calculation is not immediately triggered after the `cache` method is called.

```
cached = iris[iris.sepalwidth < 3.5].cache()
 df = cached['sepallength', 'name'].head(3)
 df
   sepallength         name
0          4.9  Iris-setosa
1          4.7  Iris-setosa
2          4.6  Iris-setosa
 cached.head(3)  # The result can be immediately retrieved because the cached object is calculated.
   sepallength         name
0          4.9  Iris-setosa
1          4.7  Iris-setosa
2          4.6  Iris-setosa
```

## Asynchronous and parallel execution

PyODPS DataFrame supports asynchronous operations. For the `execute`, `persist`, `head`, `tail`, and `to_pandas` methods, you can pass the `async` parameter to enable asynchronous execution. The `timeout` parameter specifies the time-out period. Asynchronous operations return [Future](https://docs.python.org/3/library/concurrent.futures.html#future-objects) objects.

```
from odps.df import Delay
 delay = Delay()  # Create the Delay object.

 df = iris[iris.sepal_width < 5].cache()  # Common dependency of subsequent expressions.
 future1 = df.sepal_width.sum().execute(delay=delay)  # Return the Future object. The execution is not started.
 future2 = df.sepal_width.mean().execute(delay=delay)
 future3 = df.sepal_length.max().execute(delay=delay)
 delay.execute(n_parallel=3)  # The execution starts with three concurrent threads.
|==========================================|   1 /  1  (100.00%)        21s
 future1.result()
 458.10000000000014
 future2.result()
3.0540000000000007
```

In the preceding example, PyODPS DataFrame first executes the object of the shared dependency. Then, PyODPS DataFrame sets the degree of parallelism to 3 and executes objects from `future1` to `future3`. If `n_parallel` is set to 1, the execution time reaches 37 seconds.

You can pass the `async` parameter to `delay.execute` to specify whether to enable asynchronous execution. If asynchronous execution is enabled, you can also use the `timeout` parameter to specify the time-out period.

