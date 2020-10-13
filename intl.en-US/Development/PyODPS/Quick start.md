---
keyword: [PyODPS, scenario]
---

# Quick start

PyODPS is MaxCompute SDK for Python. It provides easy-to-use Python programming interfaces. Similar to pandas, PyODPS provides fast, flexible, and expressive data structures. You can use the data processing feature of PyODPS, which is similar to that of pandas, by calling the DataFrame API provided by PyODPS. This topic describes how to use PyODPS in your projects.

-   MaxCompute is activated. For more information, see [Activate MaxCompute](/intl.en-US/Prepare/Activate MaxCompute.md).
-   DataWorks is activated and a workspace is created. For more information, see [Create a project](/intl.en-US/Prepare/Create a project.md).

You can develop most programs in MaxCompute by using SQL statements. However, you must use Python to develop complex business logic and user defined functions \(UDFs\). For example, you must use Python in the following scenarios:

-   Interface connection

    An external service must provide required information to complete authentication before the service can access any data record in MaxCompute tables by using an HTTP interface.

-   Asynchronous calls

    Generally, the system creates a node for each of thousands of tasks with similar data processing logic. These nodes are difficult to manage and occupy excessive resources at the same time. PyODPS allows you to use queues to asynchronously run SQL tasks at a high concurrency. Queues also help you manage all nodes in a unified manner.

-   UDF development

    If the built-in functions of MaxCompute cannot meet your requirements, you can develop UDFs. For example, you can develop a UDF to format data of the DECIMAL type with thousands separators.

-   SQL performance improvement

    To check whether database transactions follow the first in, first out \(FIFO\) rule, you must compare each new record with historical records in the only transaction table. Generally, this transaction table contains a large number of records. If you use SQL statements to complete the comparison, the time complexity is O\(N²\). In addition, the SQL statements may fail to return the expected result. To resolve this issue, you can call the cache method in Python code to traverse records in the transaction table only once. In this case, the time complexity is O\(N\) and the efficiency is improved significantly.

-   Other scenarios

    You can use Python to develop an amount allocation model. For example, you can develop a model to allocate USD 10 to three persons. In this model, you must define the logic for handling the cash over and short.


For more information about the application scenarios of PyODPS, see the following topics:

-   [Quick start](/intl.en-US/Development/PyODPS/DataFrame/Quick start.md)
-   [Use a PyODPS node to read data from a partitioned table](/intl.en-US/Development/PyODPS/Examples/Use a PyODPS node to read data from a partitioned table.md)
-   [Use a PyODPS node to pass parameters](/intl.en-US/Development/PyODPS/Examples/Use a PyODPS node to pass parameters.md)
-   [Reference a third-party package in a PyODPS node](/intl.en-US/Development/PyODPS/Examples/Reference a third-party package in a PyODPS node.md)
-   [Use a PyODPS node to read data from the level-1 partition of the specified table](/intl.en-US/Development/PyODPS/Examples/Use a PyODPS node to read data from the level-1 partition of the specified table.md)
-   [Use a PyODPS node to query data based on specific criteria](/intl.en-US/Development/PyODPS/Examples/Use a PyODPS node to query data based on specific criteria.md)
-   [Use a PyODPS node to perform sequence operations](/intl.en-US/Development/PyODPS/Examples/Use a PyODPS node to perform sequence operations.md)

1.  Create a PyODPS node.

    This section describes how to create a PyODPS 2 node in the DataWorks console. For more information, see [t1693701.md\#]().

    **Note:**

    -   The Python version of a PyODPS node is 2.7.
    -   Each PyODPS node can process a maximum of 50 MB data and can occupy a maximum of 1 GB memory. Otherwise, DataWorks terminates the PyODPS node. Do not write Python code that will process an extra large amount of data in PyODPS nodes.
    -   Writing and debugging code in DataWorks is inefficient. We recommend that you install IntelliJ IDEA locally to write code.
    1.  Create a workflow.

        Log on to the DataWorks console and go to the [Data Development](https://ide2-cn-shanghai.data.aliyun.com/) tab. Right-click **Business process** and select **New business process**.

    2.  Create a PyODPS node.

        Right-click the workflow that you created in the workflow list and choose **New** \> **MaxCompute** \> **PyODPS 2**. In the New node dialog box, set the Node name parameter and click **Submit**.

2.  Configure and run the PyODPS 2 node.

    1.  Write the code of the PyODPS 2 node.

        Write the test code in the code editor of the PyODPS 2 node. In this example, write the following code in the code editor, which covers a full range of table operations. For more information about table operations and SQL operations, see [Tables](/intl.en-US/Development/PyODPS/Basic operations/Tables.md) and [SQL](/intl.en-US/Development/PyODPS/Basic operations/SQL.md).

        ```
        from odps import ODPS
        import sys
        
        reload (sys)
        # Set UTF-8 as the default encoding format. You must execute this statement if the target data contains Chinese characters.
        sys.setdefaultencoding('utf8')
        
        # Create a non-partitioned table named my_new_table, which contains the fields with the specified names and of the specified data types.
        table = o.create_table('my_new_table', 'num bigint, id string', if_not_exists=True)
        
        # Write data to the my_new_table table.
        records = [[111, 'aaa'],
                  [222, 'bbb'],
                  [333, 'ccc'],
                  [444, 'Chinese']]
        o.write_table(table, records)
        
        # Read data from the my_new_table table.
        for record in o.read_table(table):
            print record[0],record[1]
        
        # Read data from the my_new_table table by executing an SQL statement.
        result = o.execute_sql('select * from my_new_table;',hints={'odps.sql.allow.fullscan': 'true'})
        
        # Obtain the execution result of the SQL statement.
        with result.open_reader() as reader:    
            for record in reader:            
                print record[0],record[1]
        
        # Delete the table.
        table.drop()
        ```

    2.  Run the code.

        After you write the code, click ![Run icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5392309951/p66566.png) in the toolbar. After the code is run, you can view the running result of the PyODPS 2 node on the **Run Log** tab. The result in the following figure indicates that the running is successful.

        ![Run Log tab](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9411330061/p94360.png)


