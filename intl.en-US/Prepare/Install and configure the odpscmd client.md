---
keyword: [odpscmd, odpscmd client, installation, configuration]
---

# Install and configure the odpscmd client

The odpscmd client allows you to access MaxCompute projects and features. This topic describes how to install, configure, and run the odpscmd client.

-   The odpscmd client is developed based on the Java language. Make sure that your device has Java 8 or a later version of Java.
-   A MaxCompute project is created. For more information, see [Create a project](/intl.en-US/Prepare/Create a project.md).

1.  Install the odpscmd client
2.  Download the [installation package of the odpscmd client](https://odps-repo.oss-cn-hangzhou.aliyuncs.com/odpscmd/latest/odpscmd_public.zip).

3.  Decompress the installation package to obtain the bin, conf, lib, and plugins folders.

4.  Configure the odpscmd client
5.  Edit the odps\_config.ini file in the conf folder to configure the odpscmd client. The following configurations show an example:

    ```
    # Specify the name of the project that you want to access.
    project_name=my_project
    # Specify the AccessKey ID and AccessKey secret of your Alibaba Cloud account. To obtain the AccessKey ID and AccessKey secret, log on to the Alibaba Cloud Management Console and view the AccessKey pair on the AccessKey Management page. Remove the angle brackets <> when you enter the AccessKey ID and AccessKey secret.
    access_id=*******************
    access_key=********************* 
    # Specify the endpoint of MaxCompute. The endpoint is determined based on the region and network environment of your MaxCompute project. For more information, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).
    end_point=xxxxxxxxxxx
    # Specify the Logview URL that the odpscmd client returns after a job is run. After you access the Logview URL, you can view detailed operational logs of the job.
    log_view_host=http://logview.odps.aliyun.com 
    # Specify whether to enable HTTPS access.
    https_check=true 
    
    # Specify the maximum size of input data. Unit: GB.
    data_size_confirm=100.0
    # Specify the URL for updating the odpscmd client.
    update_url=http://repo.aliyun.com/odpscmd
    # Specify whether to download SQL running results by using InstanceTunnel.
    use_instance_tunnel=true
    # Specify the maximum number of records in the SQL running results downloaded by using InstanceTunnel.
    instance_tunnel_max_record=10000
    # Specify the endpoint of MaxCompute Tunnel. The endpoint is determined based on the region and network environment of your MaxCompute project. For more information, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).
    tunnel_endpoint=xxxxxxxxxxx
    ```

    -   We recommend that you set end\_point and tunnel\_endpoint based on the region you selected when you created the project. Otherwise, errors such as access failure may be thrown. For more information, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).
    -   If your MaxCompute project is created when you create a DataWorks workspace in standard mode in the DataWorks console, pay attention to the name difference between the projects in the production environment and the development environment when you specify project\_name. The name of a project in the development environment ends with the \_dev suffix. For more information, see [Basic mode and standard mode]().
    -   A number sign \(\#\) is used to comment out a line in the odps\_config.ini file. Two consecutive minus signs \(**--**\) are used to comment out a command line on the odpscmd client.
    -   MaxCompute is accessible over the Internet, the classic network, and a virtual private cloud \(VPC\). Your download costs are subject to the connection method that you use. If you do not specify the endpoint of MaxCompute Tunnel, MaxCompute Tunnel may be automatically routed to the Internet and you may be charged for downloads over the Internet. For more information, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).
6.  Run the odpscmd client
7.  Run ./bin/odpscmd in Linux or ./bin/odpscmd.bat in Windows. If the following interface appears, the odpscmd client runs properly.

    ![Run the odpscmd client.png](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7412960061/p65816.png)


For more information about how to use the odpscmd client, see [Client](/intl.en-US/Tools and Downloads/Client.md).

**Note:**

-   We recommend that you use [MaxCompute Studio](/intl.en-US/Tools and Downloads/MaxCompute Studio/What is Studio.md) for big data development. MaxCompute Studio integrates with Java. You can use MaxCompute Studio to develop and run MaxCompute SQL scripts, manage data, and analyze logs in a visualized manner. MaxCompute Studio also allows you to develop Java programs, such as [user defined functions \(UDFs\)](/intl.en-US/Development/SQL/UDF/Overview.md) and [MapReduce programs](/intl.en-US/Development/MapReduce/Java SDK/Java SDK.md). In addition, the odpscmd client is integrated in MaxCompute Studio.
-   To use MaxCompute in the DataWorks console, create a workspace and click **Data Analytics** in the Actions column of the workspace on the **Workspaces** page. For more information, see [What is DataWorks?]() and [Create a workspace]().
-   You can add workspace members and grant permissions to them in the DataWorks console. For more information, see [Add workspace members]().

