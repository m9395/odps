---
keyword: develop a demo project on Spark on MaxCompute by using Java or Scala
---

# Develop a demo project on Spark on MaxCompute by using Java or Scala

This topic describes how to develop a demo project on Spark on MaxCompute by using Java or Scala.

## Download a demo project

Spark on MaxCompute provides a demo project template. We recommend that you download and copy the template to develop your application.

Run the following commands to download the demo project template:

```
Download and compile the Spark-1.x template  
git clone https://github.com/aliyun/MaxCompute-Spark.git  
cd spark-1.x  
mvn clean package  
Download and compile the Spark-2.x template  
git clone https://github.com/aliyun/MaxCompute-Spark.git  
cd spark-2.x  
mvn clean package
```

**Note:** In the demo project, the scope parameter for the Spark dependency is set to provided. Do not modify this parameter. Otherwise, the submitted job will not run normally.

## Spark-1.x demo project

Examples of a Spark-1.x demo project:

-   [WordCount example](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-1.x examples.md)
-   [Example of Spark-SQL on MaxCompute Table](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-1.x examples.md)
-   [GraphX PageRank example](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-1.x examples.md)
-   [Mllib Kmeans-ON-OSS example](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-1.x examples.md)
-   [OSS UnstructuredData example](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-1.x examples.md)

## Spark-2.x demo project

Examples of a Spark-2.x demo project:

-   [WordCount example](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-2.x examples.md)
-   [GraphX PageRank example](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-2.x examples.md)
-   [Mllib Kmeans-ON-OSS examples](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-2.x examples.md)
-   [OSS UnstructuredData example](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-2.x examples.md)
-   [MaxCompute table I/O example](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-2.x examples.md)
-   [Example of PySpark I/O by MaxCompute table](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-2.x examples.md)
-   [PySpark writing to OSS example](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-2.x examples.md)
-   [Example of supporting Spark Streaming Loghub](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-2.x examples.md)
-   [Example of supporting Spark Streaming Datahub](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-2.x examples.md)
-   [Example of supporting Spark Streaming Kafka](/intl.en-US/Development/Spark/Java and Scala development examples/Spark-2.x examples.md)

