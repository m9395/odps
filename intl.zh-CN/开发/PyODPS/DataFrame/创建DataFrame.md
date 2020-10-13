---
keyword: 创建DataFrame
---

# 创建DataFrame

本文为您介绍如何创建DataFrame引用数据源。

## 背景信息

在使用DataFrame时，您需要了解`Collection`（`DataFrame`）、`Sequence`和`Scalar`三类对象的操作。三类对象分别表示表结构（或者二维结构）、列（一维结构）和标量。

您使用Pandas数据创建的对象包含实际数据，但通过MaxCompute表创建的对象并不包含实际数据，仅包含对数据的操作。MaxCompute完成数据存储和计算。

## 创建DataFrame

您唯一需要直接创建的Collection对象是DataFrame。DataFrame用于引用MaxCompute表、MaxCompute分区、Pandas DataFrame或Sqlalchemy Table（数据库表）数据源。这几种数据源的操作相同，您可以不更改数据处理代码，仅修改输入和输出指向，便可以将本地运行的小数据量测试代码迁移到MaxCompute，迁移正确性由PyODPS保证。

创建DataFrame，只需要将Table对象、Pandas DataFrame对象或者Sqlalchemy Table对象传入即可。

```
from odps.df import DataFrame

# 从ODPS表创建DataFrame。
 iris = DataFrame(o.get_table('pyodps_iris'))
 iris2 = o.get_table('pyodps_iris').to_df()  # 使用表的to_df方法。

# 从MaxCompute分区创建DataFrame。
 pt_df = DataFrame(o.get_table('partitioned_table').get_partition('pt=20171111'))
 pt_df2 = o.get_table('partitioned_table').get_partition('pt=20171111').to_df()  # 使用分区的to_df方法。

# 从Pandas DataFrame创建DataFrame。
 import pandas as pd
 import numpy as np
 df = DataFrame(pd.DataFrame(np.arange(9).reshape(3, 3), columns=list('abc')))

# 从Sqlalchemy Table创建DataFrame。
 engine = sqlalchemy.create_engine('mysql://root:123456@localhost/movielens')
 metadata = sqlalchemy.MetaData(bind=engine) # 需要绑定到Engine。
 table = sqlalchemy.Table('top_users', metadata, extend_existing=True, autoload=True)
 users = DataFrame(table)
```

用Pandas DataFrame初始化时：

-   PyODPS DataFrame会尝试对NUMPY OBJECT或STRING类型进行推断。如果一整列都为空，则会报错。为避免报错，您可以设置`unknown_as_string`值为True，将这些列指定为STRING类型。
-   您可以通过`as_type`参数，强制转换类型。如果类型为基本类型，会在创建PyODPS DataFrame时强制转换类型。如果Pandas DataFrame中包含LIST或DICT列，系统不会推断该列的类型，必须手动使用`as_type`指定类型。`as_type`参数类型必须是DICT。

```
df2 = DataFrame(df, unknown_as_string=True, as_type={'null_col2': 'float'})
df2.dtypes
odps.Schema {
  sepallength           float64
  sepalwidth            float64
  petallength           float64
  petalwidth            float64
  name                  string
  null_col1             string   # 无法识别，通过unknown_as_string设置成STRING类型。
  null_col2             float64  # 强制转换成FLOAT类型。
}
 df4 = DataFrame(df3, as_type={'list_col': 'list<int64>'})
 df4.dtypes
odps.Schema {
  id        int64
  list_col  list<int64>  # 无法识别且无法自动转换，通过as_type设置。
}
```

**说明：** PyODPS不支持上传外部表OSS或OTS。

