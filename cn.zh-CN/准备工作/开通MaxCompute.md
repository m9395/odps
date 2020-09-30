---
keyword: 开通MaxCompute
---

# 开通MaxCompute

完成准备账号操作后，您需要开通MaxCompute服务，才可以使用MaxCompute。本文为您介绍如何开通MaxCompute服务。

-   如果您是第一次使用大数据产品，建议使用阿里云账号开通MaxCompute服务。
-   如果您需要使用RAM用户账号登录阿里云官网和创建项目，请确认账号可用并已授权，详情请参见[创建RAM用户](/cn.zh-CN/准备工作/创建RAM用户.md)。如果验证无误，可继续开通MaxCompute服务。

1.  登录[阿里云官网](https://account.aliyun.com/login/login.htm)。

2.  进入[阿里云MaxCompute产品首页](https://www.aliyun.com/product/odps)，单击**立即开通**。

3.  在购买页面，选择**地域**，并选中服务协议，单击**确认订单并支付**。

    ![购买界面](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2986811061/p170560.png)

    **说明：**

    -   购买页面默认提供的规格类型为MaxCompute**按量计费标准版**+DataWorks基础版。
    -   MaxCompute的项目管理和查询编辑集成DataWorks的功能，因此需要同时开通DataWorks服务。DataWorks基础版为0元开通，如果您不使用数据集成、不执行调度任务，则不会产生费用。
    -   选择区域时，您需要考虑的最主要因素是MaxCompute与其他阿里云产品之间的关系，例如ECS所在区域，数据所在区域。
4.  在**支付**页面，单击**支付**，即可开通MaxCompute服务。

    ![支付](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2986811061/p170565.png)

    **说明：** 如果您选择的区域是首次开通MaxCompute和DataWorks服务，且没有存量项目，则开通成功后，系统会创建一个默认的项目，项目名称为主账号的UID。该项目默认仅阿里云主账号可见，RAM用户需要授权后才可见，授权详情请参见[授权](/cn.zh-CN/管理/安全管理详解/用户及授权管理/授权.md)。


-   开通MaxCompute服务后，您可以进入[MaxCompute控制台](https://workbench.data.aliyun.com/#/MCEngines)，单击**查询编辑**，运行SQL语句查询和分析数据。您也可以创建新的项目空间，创建项目空间操作详情请参见[创建项目空间](/cn.zh-CN/准备工作/创建项目空间.md)。
-   如果您需要开通MaxCompute的其他规格类型，例如包年包月，您可以登录MaxCompute控制台，单击**资源管理**，开通目标规格。规格类型详情请参见[规格类型](/cn.zh-CN/规格类型/包年包月标准版.md)。

