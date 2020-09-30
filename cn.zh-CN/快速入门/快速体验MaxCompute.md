# 快速体验MaxCompute

本文指导您基于MaxCompute提供的公开数据集，通过MaxCompute查询编辑器，快速体验MaxCompute产品，完成开通、执行SQL语句查询数据及下载查询结果到本地的操作。

已创建阿里云账号或RAM用户，详情请参见[创建阿里云账号](/cn.zh-CN/准备工作/创建阿里云账号.md)或[创建RAM用户](/cn.zh-CN/准备工作/创建RAM用户.md)。

MaxCompute提供公开的数据集供您试用产品，详情请参见[公开数据集](/cn.zh-CN/公开数据集/公开数据集概述.md)。

MaxCompute提供的公开数据集数据只能用于产品测试，数据将不做周期更新，且不保障数据准确性，因此请您勿用于正式生产。

您可以在MaxCompute查询编辑器中执行各种SQL命令和授权命令，与在MaxCompute客户端（odpscmd）执行结果等效。您还可以切换到分析模式使用Web Excel强大而丰富的分析功能分析查询结果。

本文以通过MaxCompute查询编辑器查询并下载MAXCOMPUTE\_PUBLIC\_DATA.dwd\_product\_phoneno\_basic\_info\_2020（2020年手机号归属地基本信息表）的数据为例，筛选出运营商为中国移动、省区为浙江省、城市为杭州市的数据，并下载到本地。

## 步骤一：开通MaxCompute服务

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


## 步骤二：查询并下载数据

1.  登录MaxCompute控制台后，在左上角选择MaxCompute服务开通的区域，单击**查询编辑**。

    ![查询编辑](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4759811061/p170391.png)

2.  在**选择数据源**对话框，选择**数据源类型**为**MaxCompute**，**工作空间**为已创建好的项目空间，单击**确认**。

    ![选择项目空间](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4759811061/p170358.png)

    **说明：** **工作空间**默认为您最新创建的项目空间，如果您未自行创建过项目空间，默认选中开通MaxCompute服务时，系统为您创建的项目空间。

3.  在代码编辑区域，输入如下SQL语句，获取运营商、省区和城市字段名称，单击![运行](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5972911061/p170698.png)图标。

    命令示例如下。

    ```
    DESC MAXCOMPUTE_PUBLIC_DATA.dwd_product_phoneno_basic_info_2020;
    ```

    返回结果如下。

    ![查看表结构](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4759811061/p170366.png)

    从执行结果可以得到运营商、省区和城市的字段名称分别为isp、province和city。

4.  在代码编辑区域，输入如下SQL语句，查看运营商、省区和城市字段的值。

    命令示例如下。

    ```
    SELECT DISTINCT isp , province,city FROM MAXCOMPUTE_PUBLIC_DATA.dwd_product_phoneno_basic_info_2020;
    ```

    返回结果如下。

    ![查询结果](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4759811061/p170377.png)

5.  在代码编辑区域，输入如下SQL语句，筛选出运营商为中国移动、省区为浙江省、城市为杭州市的数据。

    命令示例如下。

    ```
    SELECT * FROM MAXCOMPUTE_PUBLIC_DATA.dwd_product_phoneno_basic_info_2020 where isp='中国移动' and province='浙江' and city='杭州';
    ```

    返回结果如下。

    ![查询结果](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4759811061/p170381.png)

6.  单击左上角的**查询模式**，切换为**分析模式**。

    ![切换模式](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7293911061/p170699.png)

7.  在**分析模式**页面，单击右上角**下载**，即可下载查询结果至本地。下载的文件为Excel格式。


