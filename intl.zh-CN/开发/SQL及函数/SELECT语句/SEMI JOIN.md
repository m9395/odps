---
keyword: [半连接, LEFT SEMI JOIN, ]
---

# SEMI JOIN

MaxCompute支持SEMI JOIN（半连接），支持LEFT SEMI JOIN和LEFT ANTI JOIN两种语法。SEMI JOIN中，右表只用于过滤左表的数据而不出现在结果集中。

## LEFT SEMI JOIN

当`JOIN`条件成立时，返回左表中的数据。如果`mytable1`中某行的`id`在`mytable2`的所有`id`中出现过，则此行保留在结果集中。

示例如下。

```
SELECT * from mytable1 a LEFT SEMI JOIN mytable2 b on a.id=b.id;
```

只会返回`mytable1`中的数据，只要`mytable1`的`id`在`mytable2`的`id`中出现。

## LEFT ANTI JOIN

当`JOIN`条件不成立时，返回左表中的数据。如果`mytable1`中某行的`id`在`mytable2`的所有`id`中没有出现过，则此行保留在结果集中。

示例如下。

```
SELECT * from mytable1 a LEFT ANTI JOIN mytable2 b on a.id=b.id;
```

只会返回`mytable1`中的数据，只要`mytable1`的`id`在`mytable2`的`id`没有出现。

