---
keyword: switch billing methods
---

# Switch billing methods

This topic describes how to switch the billing methods between pay-as-you-go and subscription for MaxCompute after you purchase resources.

Pay-as-you-go and subscription resources are purchased.

-   The difference between subscription and pay-as-you-go lies only in the billing and running modes of the computing resources. The billing modes for storage and download resources are the same. In the subscription method, computing tasks can only use reserved and non-reserved computing resources that you purchase. Reserved computing resources are exclusive resources, whereas non-reserved computing resources and resources billed in the pay-as-you-go method are shared resources. The running speed of a task depends on the total number of running tasks.
-   If you switch the billing method for a project, the new method takes effect immediately. However, if a task is in the running state, the new method does not take effect until the next time you execute a task.
-   If you switch the billing method from **pay-as-you-go** to **subscription**, you must purchase MaxCompute compute units \(CUs\) in advance. You can only switch billing methods for projects in the same region.
-   If you switch the billing method from **subscription** to **pay-as-you-go**, the fees that you have paid are not refunded. However, you can create projects to use the purchased CUs. After you purchase MaxCompute CUs, you can create multiple projects to share these CUs.
-   If you frequently switch between billing methods, the efficiency of your computing tasks may be affected. Therefore, we recommend that you only switch billing methods as required.

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) by using your Alibaba Cloud account.

2.  Click **Workspaces** in the left-side navigation pane.

3.  In the top navigation bar, select the region to which the project belongs. Find the workspace for which you want to switch the billing method, and click **Modify service configuration** in the Actions column. In the Modify service configuration wizard, change the billing method for MaxCompute in the **Select Engines and Services** step.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1628521061/p68767.png)

4.  Click **Next**.

5.  In the **Engine Details** step, click **OK**.


