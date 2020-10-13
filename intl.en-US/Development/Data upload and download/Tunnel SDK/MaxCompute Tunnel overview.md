---
keyword: [Tunnel SDK, download data, upload data]
---

# MaxCompute Tunnel overview

This topic provides an overview of MaxCompute Tunnel, which is a data tunnel that is used to upload and download data.

[Data upload and download tools](/intl.en-US/Tools and Downloads/Client.md) provided by MaxCompute are based on Tunnel SDK. The following table describes the interfaces of Tunnel SDK, which may differ based on the SDK versions. For more information, see [SDK Java Doc](https://www.javadoc.io/doc/com.aliyun.odps/odps-sdk-core/0.31.3-public).

|Interface|Description|
|:--------|:----------|
|TableTunnel|The entry-class interface that is used to access MaxCompute Tunnel. You can access MaxCompute and MaxCompute Tunnel on the Internet or an internal network on Alibaba Cloud. Tunnel-based data downloads on an internal network are free of charge.|
|TableTunnel.UploadSession|The session that uploads data to a MaxCompute table.|
|TableTunnel.DownloadSession|The session that downloads data from a MaxCompute table.|
|InstanceTunnel|The entry-class interface that is used to access MaxCompute Tunnel. You can access MaxCompute and MaxCompute Tunnel on the Internet or an internal network on Alibaba Cloud. Tunnel-based data downloads on an internal network are free of charge.|
|InstanceTunnel.DownloadSession|The session that downloads data to a MaxCompute SQL instance. The SQL instance must start with the SELECT keyword and is used to query data.|

**Note:**

-   If you use Maven, you can search for odps-sdk-core in the [Maven repository](http://search.maven.org/) to find the latest version of the SDK for Java. You can configure the Maven dependency in the following way:

    ```
    <dependency>
        <groupId>com.aliyun.odps</groupId>
        <artifactId>odps-sdk-core</artifactId>
        <version>0.24.0-public</version>
    </dependency>
    ```

-   For more information about the SDK, see [SDK Java Doc](https://www.javadoc.io/doc/com.aliyun.odps/odps-sdk-core/0.31.3-public).
-   You can also use the Java Database Connectivity \(JDBC\) driver to write data. For more information, see [Overview](/intl.en-US/JDBC Reference/Overview.md).
-   For more information about service connections, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).

