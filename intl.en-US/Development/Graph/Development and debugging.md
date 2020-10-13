---
keyword: [Graph development, Graph debugging]
---

# Development and debugging

This topic describes how to use Eclipse to develop MaxCompute Graph programs because Graph development plug-ins are unavailable in MaxCompute.

Development process:

1.  Compile Graph code and perform local debugging to test basic functions.
2.  Perform cluster debugging to verify the result.

## Development example

This topic uses the [SSSP](/intl.en-US/Development/Graph/Examples/SSSP.md) algorithm as an example to describe how to use Eclipse to develop and debug a MaxCompute Graph program.

Procedure:

1.  Create a Java project named graph\_examples.
2.  Add the JAR package in the lib directory on the MaxCompute client to **Java Build Path** of the Eclipse project.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2192659951/p2233.png)

3.  Develop a MaxCompute Graph program.

    In the actual development process, an example program, such as [SSSP](/intl.en-US/Development/Graph/Examples/SSSP.md), is first copied and then modified. In this example, only the package path is changed to package com.aliyun.odps.graph.example.

4.  Compile and package the code.

    In an Eclipse environment, right-click the source code directory, namely, the src directory, and choose **Export** \> **Java** \> **JAR file** to generate a JAR package. Select the path to store the JAR package, such as D:\\\\odps\\\\clt\\\\odps-graph-example-sssp.jar.

5.  Run SSSP on the MaxCompute client. For more information, see [\(Optional\) Submit Graph jobs](/intl.en-US/Quick Start/(Optional) Submit Graph jobs.md).

## Local debugging

MaxCompute Graph supports the local debugging mode. You can use Eclipse for breakpoint debugging.

Procedure:

1.  Download a Maven package named odps-graph-local.
2.  Select the Eclipse project, right-click the main program file that contains the `main` function of a Graph job, and choose **Run As** \> **Run Configurations** to configure parameters.
3.  On the **Arguments** tab, set Program arguments to 1 sssp\_in sssp\_out as the input parameter of the main program.
4.  On the **Arguments** tab, set VM arguments to the following content:

    ```
    -Dodps.runner.mode=local
    -Dodps.project.name=<project.name>
    -Dodps.end.point=<end.point>
    -Dodps.access.id=<access.id> 
    -Dodps.access.key=<access.key>
    ```

    ![Parameter configuration](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2192659951/p2234.png)

5.  In local mode in which odps.end.point is not specified, create the sssp\_in and sssp\_out tables in the warehouse directory and add the following data to the sssp\_in table:

    ```
    1,"2:2,3:1,4:4"
    2,"1:2,3:2,4:1"
    3,"1:1,2:2,5:1"
    4,"1:4,2:1,5:1"
    5,"3:1,4:1"
    ```

    For more information about the warehouse directory, see [Local run](/intl.en-US/Development/MapReduce/Function Introduction/Local run.md).

6.  Click **Run** to run SSSP on the local client.

    **Note:** Configure the parameters based on the settings in conf/odps\_config.ini on the MaxCompute client. The preceding parameters are commonly used. Descriptions of other parameters:

    -   odps.runner.mode: The value is local. This parameter is required for the local debugging feature.
    -   odps.project.name: specifies the current project. This parameter is required.
    -   odps.end.point: specifies the endpoint of MaxCompute. This parameter is optional. If this parameter is not specified, metadata and data of tables or resources are only read from the warehouse directory. An exception is reported if such data does not exist in the directory. If this parameter is specified, metadata and data are read from the warehouse directory first and then from the remote MaxCompute server if such data does not exist in the directory.
    -   odps.access.id: specifies the AccessKey ID used to access MaxCompute. This parameter is valid only if odps.end.point is specified.
    -   odps.access.key: specifies the AccessKey secret used to access MaxCompute. This parameter is valid only if odps.end.point is specified.
    -   odps.cache.resources: specifies the resources you want to use. This parameter functions the same as `-resources` of the jar command.
    -   odps.local.warehouse: specifies the local path to warehouse. The default value is ./warehouse.
    Output of the local SSSP debugging in Eclipse:

    ```
    Counters: 3
             com.aliyun.odps.graph.local.COUNTER
                     TASK_INPUT_BYTE=211
                     TASK_INPUT_RECORD=5
                     TASK_OUTPUT_BYTE=161
                     TASK_OUTPUT_RECORD=5
     graph task finish
    ```

    **Note:** In this example, the sssp\_in and sssp\_out tables must exist in the local warehouse directory. For more information about the sssp\_in and sssp\_out tables, see [\(Optional\) Submit Graph jobs](/intl.en-US/Quick Start/(Optional) Submit Graph jobs.md#).


## Temporary directory of a local job

A temporary directory is created in the Eclipse project directory each time local debugging is performed.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3192659951/p2235.png)

The temporary directory of a local Graph job contains the following directories and files:

-   counters: stores the counter information that is generated during the job running.
-   inputs: stores input data of the job. Data is read from the local warehouse directory first. If no data is available, the MaxCompute SDK reads data from the server if odps.end.point is specified. An input directory reads only 10 data records by default. This threshold can be changed by using the `-Dodps.mapred.local.record.limit` parameter, of which the maximum value is 10000.
-   outputs: stores output data of the job. If an output table exists in the local warehouse directory, the results in outputs will overwrite data in that table after the job is completed.
-   resources: stores resources used by the job. Similar to input data, resource data is read from the local warehouse directory first. If no data is available, the MaxCompute SDK reads data from the server if odps.end.point is specified.
-   job.xml: stores job configurations.
-   superstep: stores persistent messages from each iteration.

**Note:** If detailed logs must be recorded during local debugging, you must create a log4j configuration file named log4j.properties\_odps\_graph\_cluster\_debug in the src directory.

## Cluster debugging

After local debugging, you can submit the job to a cluster for testing.

Procedure:

1.  Configure the MaxCompute client.
2.  Run the `add jar /path/work.jar -f;` command to update the JAR package.
3.  Run a jar command to execute the job and check the operational log and command output.

**Note:** For more information about how to run a Graph job in a cluster, see [\(Optional\) Submit Graph jobs](/intl.en-US/Quick Start/(Optional) Submit Graph jobs.md#).

## Performance optimization

Configurations that affect the Graph performance:

-   `setSplitSize(long)`: specifies the split size of an input table. The unit is MB. The value must be greater than 0. The default value is 64.
-   `setNumWorkers(int)`: specifies the number of workers for a job. The value ranges from 1 to 1000. The default value is 1. The number of workers is determined by the input bytes of the job and `splitSize`.
-   `setWorkerCPU(int)`: specifies the CPU resources of the Map. The value ranges from 50 to 800. The default value is 200. A one-core CPU contains 100 resources.
-   `setWorkerMemory(int)`: specifies the memory resources of the Map. The unit is MB. The memory ranges from 256 to 12288. The default value is 4096.
-   `setMaxIteration(int)`: specifies the maximum number of iterations. The default value is -1. If the value is less than or equal to 0, the job does not stop when the maximum number of iterations is reached.
-   `setJobPriority(int)`: specifies the job priority. The value ranges from 0 to 9. The default value is 9. A greater value indicates a lower priority.

We recommend that you optimize the performance by using one or more of the following methods:

-   Use `setNumWorkers` to increase the number of workers.
-   Use `setSplitSize` to reduce the split size and increase the data loading speed.
-   Increase the CPU or memory resources for workers.
-   Set the maximum number of iterations. For applications that do not require precise results, you can reduce the number of iterations to accelerate the execution process.

`setNumWorkers` and `setSplitSize` can be used together to accelerate data loading. Assume that the value of `setNumWorkers` equals that of `workerNum`, the value of `setSplitSize` equals that of `splitSize`, and the total number of input bytes is the value of `inputSize`. The number of split data records is calculated by using the following formula: `splitNum = inputSize/splitSize`. Relationship between `workerNum` and `splitNum`:

-   If the value of `splitNum` is equal to that of `workerNum`, each worker loads one split data record.
-   If the value of `splitNum` is greater than that of `workerNum`, each worker loads one or more split data records.
-   If the value of `splitNum` is less than that of `workerNum`, each worker uploads one or no split data record.

Therefore, you can adjust `workerNum` and `splitSize` to obtain a suitable data loading speed. In the first two cases, data is loaded faster. In the iteration phase, you only need to adjust `workerNum`. If you set `runtime partitioning` to False, we recommend that you either use `setSplitSize` to adjust the number of workers or make sure that the conditions in the first two cases are met. In the third case, some workers do not load split data records. Therefore, you can insert `set odps.graph.split.size=<m>; set odps.graph.worker.num=<n>;` before the jar command to achieve the same effect as `setNumWorkers` and `setSplitSize`.

Another common performance issue is data skew. As indicated by the counters, some workers process excessive split data records or edges than others. Data skew occurs when the number of split data records, edges, or messages that correspond to some keys is much greater than other keys. These keys are processed by a small number of workers, which results in longer runtimes. To address this issue, try one or more of the following methods:

-   Use a combiner to aggregate the messages of the split data records that correspond to the keys to reduce the number of messages generated.

    Developers can define a combiner to reduce the memory and network traffic consumed by message storage, which reduces the job execution duration.

-   Improve the business logic.

    If the data volume is large, reading data in a disk may take up the processing time. Therefore, you can reduce the data bytes to be read to increase the overall throughput. This improves job performance. To reduce data bytes, use one of the following methods:

    -   Reduce data input: For some decision-making applications, processing sampled data only affects the precision of the results, not the overall accuracy. In this case, you can perform special data sampling and import the data to the input table for processing.
    -   Avoid reading fields that are not used: The TableInfo class of the MaxCompute Graph framework supports reading specific columns that are transferred by using column name arrays, rather than reading the entire table or partition. This reduces the input data volume and improves job performance.

## Built-in JAR packages

By default, the following JAR packages are loaded on a JVM that runs Graph programs. You do not need to manually upload these resources or use `-libjars` to specify them in a command:

-   commons-codec-1.3.jar
-   commons-io-2.0.1.jar
-   commons-lang-2.5.jar
-   commons-logging-1.0.4.jar
-   commons-logging-api-1.0.4.jar
-   guava-14.0.jar
-   json.jar
-   log4j-1.2.15.jar
-   slf4j-api-1.4.3.jar
-   slf4j-log4j12-1.4.3.jar
-   xmlenc-0.52.jar

**Note:** In the classpath of the JVM, the preceding built-in JAR packages are placed before your JAR packages, which may result in a version conflict. For example, your program calls a specific class function in commons-codec-1.5.jar, but the function is not included in commons-codec-1.3.jar. In this case, you can choose to call a similar function in commons-codec-1.3.jar or wait until MaxCompute is updated to the required version.

