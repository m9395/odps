---
keyword: [Pandas, DataFrame]
---

# DataFrame概述

PyODPS提供了DataFrame API，它提供了类似Pandas的接口，但是能充分利用MaxCompute的计算能力。同时能在本地使用同样的接口，用Pandas进行计算。

-   [快速入门](/intl.zh-CN/开发/PyODPS/DataFrame/快速入门.md)：为您介绍如何创建和操作DataFrame对象，以及使用Dataframe完成基本的数据处理。
-   [创建DataFrame](/intl.zh-CN/开发/PyODPS/DataFrame/创建DataFrame.md)：为您介绍如何创建DataFrame，用于引用数据源。
-   [Sequence](/intl.zh-CN/开发/PyODPS/DataFrame/Sequence.md)：为您介绍Sequence。Sequence Expr代表二维数据集中的一列。SequenceExpr只可以从一个Collection中获取，不支持手动创建SequenceExpr。
-   [Collection](/intl.zh-CN/开发/PyODPS/DataFrame/Collection.md)：为您介绍Collection。CollectionExpr中包含针对二维数据集的列操作、筛选、变换等大量操作。
-   [执行](/intl.zh-CN/开发/PyODPS/DataFrame/执行.md)：为您介绍DataFrame操作支持的执行方法。
-   [MapReduce API](/intl.zh-CN/开发/PyODPS/DataFrame/MapReduce API.md)：本文为您介绍DataFrame下MapReduce API的使用。
-   [列运算](/intl.zh-CN/开发/PyODPS/DataFrame/列运算.md)：本文为您介绍DataFrame API中的列运算。
-   [聚合操作](/intl.zh-CN/开发/PyODPS/DataFrame/聚合操作.md)：本文为您介绍DataFrame支持的聚合操作以及如何实现分组聚合、编写自定义聚合。
-   [排序、去重、采样、数据变换](/intl.zh-CN/开发/PyODPS/DataFrame/排序、去重、采样、数据变换.md)：本文为您介绍DataFrame对象执行排序、去重、采样、数据变换操作。
-   [数据合并](/intl.zh-CN/开发/PyODPS/DataFrame/数据合并.md)：本文向您介绍DataFrame支持的数据表的JOIN操作、UNION操作等数据合并操作。
-   [窗口函数](/intl.zh-CN/开发/PyODPS/DataFrame/窗口函数.md)：本文为您介绍DataFrame API支持使用窗口函数。
-   [绘图](/intl.zh-CN/开发/PyODPS/DataFrame/绘图.md)：本文为您介绍PyODPS DataFrame提供的绘图方法。
-   [调试指南](/intl.zh-CN/开发/PyODPS/DataFrame/调试指南.md)：由于PyODPS DataFrame本身会对整个操作执行优化，为了更直观地反应整个过程， 您可以使用可视化的方式显示整个表达式的计算过程。本文为您介绍DataFrame的调试过程。

您可以参见[Python数据处理库pandas入门教程](https://yq.aliyun.com/articles/596296)了解Python数据处理库Pandas的更多信息。

