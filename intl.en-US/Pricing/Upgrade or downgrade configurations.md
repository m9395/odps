---
keyword: [subscription, configuration upgrade, configuration downgrade]
---

# Upgrade or downgrade configurations

This topic describes how to upgrade or downgrade the configurations of a MaxCompute subscription instance. You can scale out the resources allocated to a subscription instance to handle extra workload or scale in the resources to save costs.

## Configuration upgrade

If the resources allocated to your MaxCompute subscription instance cannot handle the required workload, you can upgrade your resource configurations.

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) by using your Alibaba Cloud account.
2.  In the left-side navigation pane, choose **Compute Engines** \> **MaxCompute**. In the top navigation bar, select a region.
3.  In the Subscription section, click **Upgrade**.
4.  Set Computing Unit to the number of compute units \(CUs\) that you need and select **MaxCompute \(Subscription\) Agreement of Service**. Then, click **Buy Now** to complete the upgrade.

    After you submit the order, go to the **Orders** page to view the details of the upgrade order.


## Configuration downgrade

Idle resources are still charged. If your actual workload is not able to use all of the resources allocated to your MaxCompute subscription instance, you can release these resources to save costs.

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) by using your Alibaba Cloud account.
2.  In the left-side navigation pane, choose **Compute Engines** \> **MaxCompute**. In the top navigation bar, select a region.
3.  In the Subscription section, click **Downgrade**.
4.  Set Computing Unit to the number of CUs that you need and select **MaxCompute \(Subscription\) Agreement of Service**. Then, click **Buy Now** to complete the downgrade.

    After you submit the order, go to the **Orders** page to view the details of the downgrade order.


**Note:**

-   You cannot get a full refund for the remaining period on your subscription if you downgrade the configurations of an instance in the following situations:
    -   A discount is applied to the previous order of your instance. For example, you placed a one-year subscription order. A discount of 15% was applied to the order. If the amount is USD 1,000, you actually paid USD 850. In this case, the refund will be calculated based on the amount after the discount, which is USD 850.
    -   A voucher is applied to the previous order of your instance. In this case, the refund does not include the vouchers that you have used.
-   You can downgrade the configurations of a subscription instance but you cannot cancel the subscription.

    If you want to cancel a subscription, go to the [My ticket records](https://workorder-intl.console.aliyun.com/) page to submit an application. After the application is approved, the projects for which the billing method is subscription are frozen and then deleted after 14 days. If you need to switch the billing method to pay-as-you-go, perform the switch before you submit an application.


