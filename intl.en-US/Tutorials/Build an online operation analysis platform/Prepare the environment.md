---
keyword: [prepare the environment, MaxCompute]
---

# Prepare the environment

This topic describes how activate Tablestore, MaxCompute, DataWorks, and Quick BI before you build an online operational analytics platform.

-   An Alibaba Cloud account is created. If you do not have an Alibaba Cloud account, go to the [international site \(alibabacloud.com\)](https://www.alibabacloud.com/) and click **Free Account** to create an Alibaba Cloud account.
-   Your account completes real-name verification. If your account has not completed real-name verification, follow the instructions on the [Security Settings](https://account-intl.console.aliyun.com/#/secure) page to verify your identity.

The following Alibaba Cloud services are used in this topic:

-   [Tablestore](https://www.alibabacloud.com/zh/product/table-store)
-   [MaxCompute](https://www.alibabacloud.com/zh/product/maxcompute)
-   [DataWorks](https://www.alibabacloud.com/zh/product/ide)
-   [Quick BI](https://www.alibabacloud.com/zh/product/quickbi)

**Note:** In this topic, Tablestore is activated in the China \(Beijing\) region.

1.  Activate Tablestore and create a Tablestore instance.

    1.  Go to the [Tablestore product landing page](https://www.alibabacloud.com/zh/product/table-store), and click **Get it Free** to activate Tablestore.

    2.  On the Confirm Order page, read and agree to the **Tablestore \(Pay-As-You-Go\) Agreement of Service** by selecting the check box and click **Enable Now**.

        ![Confirm Order page](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0913711061/p81630.png)

    3.  On the page that appears, click **Console**.

        ![Management Console](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0913711061/p49612.png)

    4.  On the Overview page, select China \(Beijing\) from the drop-down list at the top and click **Create Instance**. In the **Create Instance** dialog box, Region is automatically set to **China \(Beijing\)**. Enter a name in the **Instance Name** field, set **Instance Type** to **Capacity**, and then click **OK**.

        ![Create an instance](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0913711061/p81634.png)

        **Note:** The name of the Tablestore instance must be globally unique in each region. We recommend that you enter a name that is recognizable and complies with the naming rule. The instance name is required when you synchronize data from MaxCompute to the Tablestore instance. In this topic, the instance is named workshop-bj-001. For more information about Tablestore instances, see [Instance](/intl.en-US/Developer Guide/Terms/Instance.md).

    5.  Click **All Instances** in the left-side navigation pane. On the All Instances page, view the created instance, which is in the **Running** status.

2.  Activate MaxCompute.

    1.  Go to the [MaxCompute product landing page](https://www.alibabacloud.com/zh/product/maxcompute), and click **Buy Now**.

    2.  On the page that appears, set **Product Type** to **Pay-As-You-Go** and **Region** to **China \(Shanghai\)**. Then, click **Buy Now**.

        **Note:** The data transfer cost is reduced if you activate MaxCompute in the same region as Tablestore. Therefore, you can set Region to **China \(Beijing\)**. In this topic, the region of MaxCompute is set to **China \(Shanghai\)** to demonstrate the capability of using foreign tables across different regions.

3.  Activate DataWorks.

    1.  Go to the [DataWorks product landing page](https://www.alibabacloud.com/zh/product/ide), and click **Buy Now**.

    2.  Set Region to **China \(Shanghai\)** and click **Buy Now**.

        **Note:** The data transfer cost can be reduced if you activate DataWorks in the same region as Tablestore. Therefore, you can set Region to **China \(Beijing\)**. In this topic, the region of DataWorks is set to **China \(Shanghai\)** to demonstrate the capability of accessing data across different regions.

4.  Create a DataWorks workspace.

    1.  Log on to the DataWorks console. Click [Workspaces](https://workbench.data.aliyun.com/consolenew#/projectlist) in the left-side navigation pane. On the Workspaces page, select **China \(Hangzhou\)** from the drop-down list at the top and click **Create Workspace**.

        ![Create a DataWorks workspace](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1913711061/p81627.png)

    2.  In the Create Workspace wizard, set parameters in the **Basic Settings** step and click **Next**.

        For ease of use, the mode of the DataWorks workspace is set to **Basic Mode \(Production Environment Only\)** in this topic. In basic mode, a DataWorks workspace is associated with only one MaxCompute project. For more information, see [Basic mode and standard mode]().

        ![Basic mode](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1913711061/p58447.png)

        **Note:** The workspace name must be globally unique. We recommend that you use a recognizable name.

    3.  In the **Select Engines and Services** step, select the required compute engines and services, and click **Next**.

        In this topic, you must select **MaxCompute** as a compute engine and select **Pay-As-You-Go** as the billing method of MaxCompute.

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1913711061/p66904.png)

    4.  In the **Engine Details** step, set parameters for the selected engines and services.

        ![44](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1913711061/p93486.png)

        |Engine or Service|Parameter|Description|
        |:----------------|:--------|:----------|
        |**MaxCompute**|**Instance display name**|The display name of the compute engine instance. The display name can be up to 27 characters in length. It must start with a letter and can contain letters, underscores \(\_\), and digits.|
        |**Resource Group**|The quotas of computing resources and disk spaces for the compute engine instance.|
        |**MaxCompute Data Type Edition**|The data type version of the MaxCompute project.|
        |**MaxCompute Project Name**|The name of the MaxCompute project. By default, the name is the same as that of the DataWorks workspace.|
        |**Account for Accessing MaxCompute**|The account for accessing the MaxCompute project. Valid values: **Alibaba Cloud Account** and **Node Owner**. If you create a DataWorks workspace in standard mode, the DataWorks workspace is associated with two MaxCompute projects, which are in the development environment and production environment, respectively. For the MaxCompute project in the development environment, the value is set to **Node Owner** by default. For the MaxCompute project in the production environment, we recommend that you set the value to **Alibaba Cloud Account**.|

    5.  Click **Create Workspace**.

        After the workspace is created, you can view information about the workspace on the Workspaces page.

5.  Activate Quick BI.

    1.  Go to the [Quick BI product landing page](https://www.alibabacloud.com/zh/product/quickbi), and click **Buy Now**.

    2.  On the page that appears, set Edition, Region, Number of Users, and Subscription Period as needed. Then, click **Buy Now**.

        **Note:** In the Quick BI console, you can use **Personal Workspace** or **Default Workspace**. We recommend that you use **Default Workspace**.


