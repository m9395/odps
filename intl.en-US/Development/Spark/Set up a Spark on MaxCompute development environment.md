---
keyword: set up a Spark on MaxCompute development environment
---

# Set up a Spark on MaxCompute development environment

This topic describes how to set up a Spark on MaxCompute development environment.

## Prerequisites

The following software has been installed:

-   JDK 1.8
-   Python2.7
-   Maven
-   Git

## Download the Spark on MaxCompute client

The Spark on MaxCompute package is released with the MaxCompute authorization function. This allows Spark on MaxCompute to serve as a client that submits jobs with the spark-submit script. The following packages have been released for Spark1.x and Spark2.x:

-   [Spark-1.6.3](http://odps-repo.oss-cn-hangzhou.aliyuncs.com/spark/1.6.3-public/spark-1.6.3-public.tar.gz): used to develop Spark1.x applications.
-   [Spark-2.3.0](http://odps-repo.oss-cn-hangzhou.aliyuncs.com/spark/2.3.0-odps0.32.2/spark-2.3.0-odps0.32.2.tar.gz): used to develop Spark2.x applications.

## Set environment variables

-   JAVA\_HOME settings

    ```
    # We recommend that you use JDK 1.8.
    export JAVA_HOME=/path/to/jdk
    export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    export PATH=$JAVA_HOME/bin:$PATH
    ```

-   SPARK\_HOME settings

    Download the Spark on the MaxCompute client and unzip it to a local path. Replace SPARK\_HOME with your unzip path.

    ```
    export SPARK_HOME=/path/to/spark_extracted_package
    export PATH=$SPARK_HOME/bin:$PATH
    ```

-   To use PySpark, you must install Python 2.7 and set PATH.

    ```
    export PATH=/path/to/python/bin/:$PATH
    ```


## Configure the spark-defaults.conf file

If you use the Spark on MaxCompute client for the first time, you must configure the spark-defaults.conf file.

The spark-defaults.conf.template file is stored in the $SPARK\_HOME/conf path. You can use it as a template for configurations of the spark-defaults.conf file.

**Note:** Rename spark-defaults.conf.template as spark-defaults.conf before you perform configurations. If the file is not renamed, the configuration cannot take effect.

```
# spark-defaults.conf
# Enter the MaxCompute project name and account information.
spark.hadoop.odps.project.name = XXX  
spark.hadoop.odps.access.id = XXX     
spark.hadoop.odps.access.key = XXX

# Retain the following default settings.
Spark.hadoop.odps.end.point = http://service.cn.maxcompute.aliyun.com/api # Configure the endpoint through which the Spark on MaxCompute client accesses MaxCompute projects. Specify the actual endpoint based on your requirements. For more information, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).
spark.hadoop.odps.runtime.end.point = http://service.cn.maxcompute.aliyun-inc.com/api # The endpoint of the Spark, which is the endpoint of the MaxCompute VPC in the selected region. You can modify this parameter if required.
spark.sql.catalogImplementation=odps
spark.hadoop.odps.task.major.version = cupid_v2
spark.hadoop.odps.cupid.container.image.enable = true
spark.hadoop.odps.cupid.container.vm.engine.type = hyper

spark.hadoop.odps.cupid.webproxy.endpoint = http://service.cn.maxcompute.aliyun-inc.com/api
spark.hadoop.odps.moye.trackurl.host = http://jobview.odps.aliyun.com
```

Other configurations are required for special scenarios and functions. For more information, see [Spark on MaxCompute configuration details](https://github.com/aliyun/MaxCompute-Spark/wiki/07.-Spark%E9%85%8D%E7%BD%AE%E8%AF%A6%E8%A7%A3).

## Make project preparations

Spark on MaxCompute provides a demo project template. We recommend that you download and copy the template to develop your application.

**Note:** In the demo project, the scope parameter for the Spark dependency is set to provided. Do not modify this parameter. Otherwise, the submitted job will not run normally.

-   Download and compile the Spark-1.x template

    ```
    git clone https://github.com/aliyun/MaxCompute-Spark.git
    cd spark-1.x
    mvn clean package
    ```

-   Download and compile the Spark-2.x template

    ```
    git clone https://github.com/aliyun/MaxCompute-Spark.git
    cd spark-2.x
    mvn clean package
    ```


## Configure dependencies

-   Configure the dependencies for Spark on MaxCompute jobs to access MaxCompute tables.

    Spark on MaxCompute jobs use the odps-spark-datasource module to access MaxCompute tables. Sample Maven configurations:

    ```
    <! -- Spark-2.x uses the following module: -->
    <dependency>
        <groupId>com.aliyun.odps</groupId>
        <artifactId>odps-spark-datasource_2.11</artifactId>
        <version>3.3.8-public</version>
    </dependency>
    
    <! -- Spark-1.x uses the following module: -->
    <dependency>
      <groupId>com.aliyun.odps</groupId>
      <artifactId>odps-spark-datasource_2.10</artifactId>
      <version>3.3.8-public</version>
    </dependency>
    ```

-   Configure the dependencies for Spark on MaxCompute jobs to access OSS.

    If Spark on MaxCompute jobs need to access OSS, add the following dependencies:

    ```
    <dependency>
        <groupId>com.aliyun.odps</groupId>
        <artifactId>hadoop-fs-oss</artifactId>
        <version>3.3.8-public</version>
    </dependency>
    ```

-   Spark-2.x dependency configuration

    For configurations in pom files, see [Spark-2.x pom files](https://github.com/aliyun/MaxCompute-Spark/blob/master/spark-2.x/pom.xml).

-   Spark-1.x dependency configuration

    For configurations in pom files, see [Spark-1.x pom files](https://github.com/aliyun/MaxCompute-Spark/blob/master/spark-1.x/pom.xml).


## Conduct a SparkPi test

After the preceding tasks are completed, perform a smoke test to check end-to-end connectivity. For example, you can run the following commands for Spark-2.x to conduct a SparkPi test:

```
# /path/to/MaxCompute-Spark Set the correct path of the compiled JAR package.
cd $SPARK_HOME
bin/spark-submit --master yarn-cluster --class com.aliyun.odps.spark.examples.SparkPi \
/path/to/MaxCompute-Spark/spark-2.x/target/spark-examples_2.11-1.0.0-SNAPSHOT-shaded.jar

# The following log means that the smoke test is successful.
19/06/11 11:57:30 INFO Client: 
         client token: N/A
         diagnostics: N/A
         ApplicationMaster host: 11.222.166.90
         ApplicationMaster RPC port: 38965
         queue: queue
         start time: 1560225401092
         final status: SUCCEEDED
```

## Notes on using IDEA locally

In most cases, run the code on the cluster after local debugging is successful. However, Spark on MaxCompute supports local execution in IDEA. Read the following notes before you run the code:

-   Set the spark.master parameter manually.

    ```
    val spark = SparkSession
          .builder()
          .appName("SparkPi")
          .config ("spark.master", "local [4]") // The code can run directly after you set spark.master to local[N]. N is the number of concurrencies.
          .getOrCreate()
    ```

-   Manually add the related dependency of the Spark on MaxCompute client in IDEA.

    ```
    <dependency>
        <groupId>org.apache.spark</groupId>
        <artifactId>spark-core_${scala.binary.version}</artifactId>
        <version>${spark.version}</version>
        <scope>provided</scope> 
    </dependency>
    ```

    In the pom.xml file, set the scope parameter to provided to avoid the "NoClassDefFoundError" error.

    ```
    Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/spark/sql/SparkSession$
        at com.aliyun.odps.spark.examples.SparkPi$.main(SparkPi.scala:27)
        at com.aliyun.odps.spark.examples.Spa. r. kPi.main(SparkPi.scala)
    Caused by: java.lang.ClassNotFoundException: org.apache.spark.sql.SparkSession$
        at java.net.URLClassLoader.findClass(URLClassLoader.java:381)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
        at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:335)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
        ... 2 more
    ```

    Use the following method to manually add the JARs directory in Spark on MaxCompute to the IDEA project template. The parameter `scope=provided` will remain unchanged. The code will not throw errors when it runs in IDEA directly.

    1.  Click **File** on the top menu bar in IDEA and select **Project Structure…**.

        ![44](../images/p94124.png)

    2.  On the **Project Structure** page, click **Modules** in the left-side navigation pane. Select Resource Packages, and click the **Dependencies** tab for the resource package.
    3.  On the **Dependencies** tab, click **+** in the lower-left corner and select **JARs or directories…**. Add the JARs directory in Spark on MaxCompute.
-   The spark-defaults.conf file cannot be used in local mode. Set your configurations manually.

    If you submit jobs with the spark-submit script, the system reads the configurations in the spark-defaults.conf file. In local mode, you must manually set the configurations. For example, if you allow Spark SQL to access MaxCompute tables, use the following configurations in local mode.

    ```
    val spark = SparkSession
          .builder()
          .appName("SparkPi")
          .config ("spark.master", "local [4]") // The code can run directly after you set spark.master to local[N]. N is the number of concurrencies.
          .config("spark.hadoop.odps.project.name", "****")
          .config("spark.hadoop.odps.access.id", "****")
          .config("spark.hadoop.odps.access.key", "****")
          .config("spark.hadoop.odps.end.point", "http://service.cn.maxcompute.aliyun.com/api")
          .config("spark.sql.catalogImplementation", "odps")
          .getOrCreate()
    ```


