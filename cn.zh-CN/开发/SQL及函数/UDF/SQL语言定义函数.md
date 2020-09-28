---
keyword: [SQL语言定义函数, SQL Function]
---

# SQL语言定义函数

本文为您介绍如何通过SQL语言定义函数（SQL Function）在SQL脚本中使用SQL定义的UDF。

## 背景信息

您可以通过MaxCompute的SQL语言定义函数解决如下问题：

-   代码中通常会存在很多相似部分，维护不方便，且容易出错。如果引入UDF并编写代码后，您还需要进行代码编译（Java）、创建资源和创建函数操作，过程比较繁琐，且性能不如内建函数。示例如下。

    ```
    SELECT
        NVL(STR_TO_MAP(GET_JSON_OBJECT(col, '$.key1')), 'default') AS key1,
        NVL(STR_TO_MAP(GET_JSON_OBJECT(col, '$.key2')), 'default') AS key2,
        ...
        NVL(STR_TO_MAP(GET_JSON_OBJECT(col, '$.keyN')), 'default') AS keyN
    FROM t;
    ```

-   使用类似Java中Lambda表达式的功能，把函数作为参数传给另一个函数。

## 功能介绍

SQL语言定义函数作为一种用户自定义函数，弥补了系统只能用Java或Python创建UDF的不足，还扩展了函数类型的参数和匿名函数特性，提升表达业务逻辑的灵活性。您可以通过该函数实现简单功能，提高代码复用率。具体功能如下：

-   支持在SQL脚本中使用SQL定义的UDF，并调用UDF。
-   支持函数类型的参数，调用时可以传入内置函数、UDF或SQL语言定义函数。
-   函数类型的参数可以为匿名函数，调用时可以传入匿名函数。

## 创建永久SQL语言定义函数

创建永久SQL语言定义函数并存入Meta系统后，所有的查询操作都可以引用该函数。引用SQL语言定义函数需要具备Function级别的权限，详情请参见[授权](/cn.zh-CN/管理/安全管理详解/用户及授权管理/授权.md)和[实现指定用户访问特定UDF最佳实践]()。命令格式如下。

```
CREATE SQL FUNCTION function_name(@parameter_in1 datatype[, @parameter_in2 datatype...]) 
[RETURNS (@parameter_out datatype)] 
AS [BEGIN] 
function_expression 
[END];
```

-   function\_name：新建SQL语言定义函数的名称。函数名称需要唯一，同名函数只能注册一次，不能与系统内建函数同名。
-   parameter\_in：函数的输入参数。
-   RETURNS：函数返回值变量。如果不指定，默认返回function\_name的同名变量。
-   parameter\_out：函数返回参数。
-   function\_expression：函数表达式。

命令示例如下。

```
CREATE SQL FUNCTION MY_ADD(@a BIGINT) AS @a + 1;
```

`@a + 1`为函数实现逻辑，可直接写为表达式，支持内置操作符、内建函数和UDF。

如果逻辑复杂，可以在定义中使用BEGIN和END，内部可以编写多条语句。RETURNS指定返回值变量，如果不指定，默认返回function\_name的同名变量。命令示例如下。

```
CREATE SQL FUNCTION MY_SUM(@a BIGINT, @b BIGINT, @c BIGINT) RETURNS @my_sum BIGINT
AS BEGIN 
    @temp := @a + @b;
    @my_sum := @temp + @c;
END;
```

**说明：** 写入多条语句时，需要使用脚本模式，详情请参见[脚本模式](/cn.zh-CN/开发/SQL及函数/脚本模式.md)。

## 查询SQL语言定义函数

查询SQL语言定义函数的方式与Java UDF或Python UDF保持一致。命令示例如下。

```
DESC FUNCTION my_add;
```

**说明：** 客户端版本需要升级至0.34.0以上。

## 删除SQL语言定义函数

删除SQL语言定义函数的方式与Java UDF或Python UDF保持一致。命令示例如下。

```
DROP FUNCTION my_add;
```

## 调用SQL语言定义函数

调用SQL语言定义函数的方式和现有内建函数的调用方式一致。命令示例如下。

```
SELECT my_sum(col1, col2 ,col3) from t;
```

## 临时SQL语言定义函数

如果您不需要把SQL语言定义函数存入MaxCompute的Meta系统，可以使用临时SQL语言定义函数。临时SQL语言定义函数只在当前脚本有效。命令示例如下。

```
FUNCTION MY_ADD(@a BIGINT) AS @a + 1;
SELECT MY_ADD(key), MY_ADD(value) FROM src;
```

**说明：** 写入多条语句时，需要使用脚本模式，详情请参见[脚本模式](/cn.zh-CN/开发/SQL及函数/脚本模式.md)。

## 支持函数类型参数

支持函数类型的参数，SQL语言定义函数调用时可以传入内置函数、UDF或SQL语言定义函数。命令示例如下。

```
FUNCTION ADD(@a BIGINT) AS @a + 1;
FUNCTION OP(@a BIGINT, @fun FUNCTION (BIGINT) RETURNS BIGINT) AS @fun(@a);
SELECT OP(key, ADD), OP(key, ABS) FROM VALUES (1),(2) AS t (key);

--返回结果如下。
+------------+------------+
| _c0        | _c1        |
+------------+------------+
| 2          | 1          |
| 3          | 2          |
+------------+------------+
```

示例中，函数OP定义了2个输入参数。`@a`定义一个BIGINT类型数值；`@fun`定义一个函数，输入和输出均为BIGINT类型。函数OP将`@a`传入`@fun`函数，并传入ADD和ABS函数，对`@a`进行操作。ABS函数详情请参见[数学函数](/cn.zh-CN/开发/SQL及函数/内建函数/数学函数.md)。

## 支持匿名函数

函数类型的参数可以为匿名函数，SQL语言定义函数调用时可以传入匿名函数。命令示例如下。

```
FUNCTION OP(@a BIGINT, @fun FUNCTION (BIGINT) RETURNS BIGINT) AS @fun(@a);
SELECT OP(key, FUNCTION (@a) AS @a + 1) FROM VALUES (1),(2) AS t (key);
```

示例中，`FUNCTION (@a) AS @a + 1`为匿名函数。匿名参数的输入参数`@a`不需要指定类型，编译器会根据OP函数的参数定义推导`@a`的类型。

