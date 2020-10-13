---
keyword: [open-source feature, SDK, MaxCompute RODPS, MaxCompute JDBC, Mars, Data collector]
---

# Open-source features of MaxCompute

MaxCompute is a fast and fully managed one-stop data warehouse solution developed by Alibaba. MaxCompute can process terabytes or petabytes of data. This topic describes open-source features of MaxCompute.

## SDK

MaxCompute provides Java SDK and Python SDK interfaces to create, view, and delete MaxCompute tables. You can use the SDKs to manage MaxCompute by editing code. MaxCompute provides the following SDKs:

-   Java SDK

    For more information about how to use the Java SDK, see [SDK Reference \> Java SDK](/intl.en-US/SDK Reference/Java SDK/Java SDK.md).

    For more information about commonly used core interfaces of MaxCompute, see [Java SDK](https://github.com/aliyun/aliyun-odps-java-sdk). For more information, see [ODPS SDK for Java Developers](https://github.com/aliyun/aliyun-odps-java-sdk) on GitHub.

    **How to obtain service support**: Visit the official documentation or [submit a ticket](https://selfservice.console.aliyun.com/ticket/createIndex) online.

-   Python SDK

    PyODPS is the Python SDK of MaxCompute. PyODPS supports the DataFrame framework and basic operations on MaxCompute objects. You can use PyODPS to analyze data in MaxCompute. For more information, see [aliyun-odps-python-sdk](https://github.com/aliyun/aliyun-odps-python-sdk) on GitHub and [PyODPS documentation](), which describes all related interfaces and classes in detail.

    -   For more information about how to use PyODPS to develop code, see the [PyODPS developer guide](). Install PyODPS before you can use it. For more information, see the [PyODPS installation guide](/intl.en-US/Development/PyODPS/Installation guide and limits.md).
    -   For more information about the PyODPS community, see [PyODPS-related articles](https://yq.aliyun.com/album/19?spm=a2c4g.11186623.2.25.5ce96074vn9tRs).
    -   You are welcome to help build the PyODPS ecosystem. For more information, see [PyODPS DataFrame overview]().
    -   Feel free to share your feedback and suggestions to help PyODPS evolve. For more information, see [ODPS Python SDK and data analysis framework](https://github.com/aliyun/aliyun-odps-python-sdk) on GitHub.
    **How to obtain service support**: Visit the official documentation or [submit a ticket](https://selfservice.console.aliyun.com/ticket/createIndex) online.


## MaxCompute RODPS

RODPS is a plug-in that MaxCompute provides for R. For more information, see [ODPS Plugin for R](https://github.com/aliyun/aliyun-odps-r-plugin) on GitHub.

**How to obtain service support**: Leave a message or create an issue in [ODPS Plugin for R](https://github.com/aliyun/aliyun-odps-r-plugin) on GitHub.

## MaxCompute JDBC

MaxCompute JDBC is an official Java Database Connectivity \(JDBC\) driver provided by MaxCompute. MaxCompute JDBC provides a set of interfaces for Java programs to execute SQL tasks. The project is hosted in [ODPS JDBC](https://github.com/aliyun/aliyun-odps-jdbc) on GitHub.

**How to obtain service support**: Leave a message or create an issue in [ODPS JDBC](https://github.com/aliyun/aliyun-odps-jdbc) on GitHub.

## Data collector

MaxCompute provides a set of open-source data collectors.

MaxCompute provides data collectors for the following services:

-   Flume
-   Oracle GoldenGate \(OGG\)
-   Sqoop
-   Kettle
-   Hive Data Transfer UDTF

    The Flume and OGG data collectors are implemented based on the DataHub SDK, whereas the data collectors for Sqoop, Kettle, and Hive Data Transfer UDTF are implemented based on the Tunnel SDK. DataHub is a real-time data transfer channel, and Tunnel is a batch data transfer channel. The Flume and OGG data collectors are used to transfer data in real time. The data collectors for Sqoop, Kettle, and Hive Data Transfer UDTF are used to transfer data in batches in offline mode.


For more information about the source code, see [Aliyun MaxCompute Data Collectors](https://github.com/aliyun/aliyun-maxcompute-data-collectors) on GitHub. For more information about data collectors, see [wiki](https://github.com/aliyun/aliyun-maxcompute-data-collectors/wiki).

**How to obtain service support**: Leave a message or create an issue in [Aliyun MaxCompute Data Collectors](https://github.com/aliyun/aliyun-maxcompute-data-collectors) on GitHub.

