# Jupyter Notebook增强

PyODPS针对Jupyter Notebook下的探索性数据分析功能进行了增强， 包括结果探索功能以及进度展示功能。

## 结果探索

PyODPS在Jupyter Notebook中为SQL Cell和DataFrame提供了数据探索功能。对于已拉到本地的数据，可使用交互式的数据探索工具浏览数据，交互式地绘制图形。

1.  当执行结果为DataFrame时，PyODPS会读取执行结果，并以分页表格的形式展示出来。您可以单击页号或前进、后退按钮，在数据中导航。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1879190061/p11733.png)

2.  结果区的顶端为模式选择区。除数据表外，也可以选择柱状图、饼图、折线图和散点图。使用默认的字段选择（即前三个字段）时， 绘制的散点图如下。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1879190061/p11734.png)

3.  在绘图模式下，单击右上角的配置按钮可以修改图表设置。将name设置为分组列，X轴选择为petallength，Y轴选择为petalwidth。由下图可见，在petallength-petalwidth维度下，数据对name有较好的区分度。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1879190061/p11735.png)

    对于柱状图和饼图，值字段支持选择聚合函数。PyODPS对柱状图的默认聚合函数为`sum`，对饼图则为`count`。如需修改聚合函数， 可以在值字段名称后的聚合函数名上单击，选择所需的聚合函数。

    对于折线图，需要避免X轴包含空值，否则可能导致图像不符合预期。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1879190061/p11736.png)

4.  完成绘图后，可单击**下载**保存绘制的图表。

**说明：** 使用此功能前，您需要安装Pandas和ipywidgets。

## 进度展示

大型作业通常需要执行较长的时间，因此PyODPS提供了进度展示功能。当在Jupyter Notebook中执行DataFrame、机器学习作业或通过执行`%sql`编写的SQL语句时，Jupyter Notebook会显示当前正在执行的作业列表及总体进度。

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1879190061/p11737.png)

当点击某个作业名称上的链接时，会弹出一个对话框，显示该作业中每个Task的具体执行进度。

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1879190061/p11738.png)

当作业运行成功后，将弹出作业是否成功的提示信息。

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1879190061/p11739.png)

