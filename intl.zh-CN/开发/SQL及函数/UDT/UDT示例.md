---
keyword: UDT示例
---

# UDT示例

本文为您提供常用的UDT示例。例如Java数组、JSON、复杂类型、聚合操作、表值函数、函数重载和引用嵌入式代码。

**说明：** 请您在脚本模式下运行如下示例代码，脚本模式详情请参见[脚本模式](/intl.zh-CN/开发/SQL及函数/脚本模式.md)。

## Java数组

```
set odps.sql.type.system.odps2=true;
set odps.sql.udt.display.tostring=true;
SELECT
    new Integer[10],    --创建一个包含10个元素的数组。
    new Integer[] {c1, c2, c3},  --通过初始化列表创建一个长度为3的数组。
    new Integer[][] { new Integer[] {c1, c2}, new Integer[] {c3, c4} },  --创建多维数组。
    new Integer[] {c1, c2, c3} [2], --通过下标操作访问数组元素。
    java.util.Arrays.asList(c1, c2, c3);    --创建一个List<Integer>，也能当做array<int>来用，是另一种创建内置ARRAY数据的方法。
FROM VALUES (1,2,3,4) AS t(c1, c2, c3, c4);
```

## JSON

UDT运行时自带一个JSON的依赖（2.2.4），可以直接使用JSON。

**说明：** 除JSON外，MaxCompute运行时自带的依赖还包括`commons-logging（1.1.1）`、`commons-lang（2.5）`、`commons-io（2.4）`和`protobuf-java（2.4.1）`。

```
set odps.sql.type.system.odps2=true;
set odps.sql.session.java.imports=java.util.*,java,com.google.gson.*; --同时导入多个Package时用逗号隔开。
@a := select new Gson() gson;   --构建Gson对象。
select 
gson.toJson(new ArrayList<Integer>(Arrays.asList(1, 2, 3))), --将任意对象转成JSON字符串。
cast(gson.fromJson('["a","b","c"]', List.class) as List<String>) --反序列化JSON字符串，注意Gson的接口，直接反序列化后是List<Object>类型，这里强制转换成List<String>，方便后续使用。
from @a;
```

相比于内建函数[GET\_JSON\_OBJECT](/intl.zh-CN/开发/SQL及函数/内建函数/字符串函数.md)，该方法不仅使用方便，还会在提取JSON字符串内容时，将JSON字符串反序列化为格式化数据，提升工作效率。

## 复杂类型

内置类型ARRAY与`java.util.List`、MAP和`java.util.Map`存在映射关系。

-   Java中实现`java.util.List`或`java.util.Map`接口类的对象，都可参与MaxCompute SQL的复杂类型操作。
-   MaxCompute中ARRAY或MAP的数据，能够直接调用List或者MAP的接口。

```
set odps.sql.type.system.odps2=true;
set odps.sql.session.java.imports=java.util.*;
select
    size(new ArrayList<Integer>()),        --对ArrayList数据调用内置函数Size。
    array(1,2,3).size(),                   --对内置类型ARRAY调用List的Size方法。
    sort_array(new ArrayList<Integer>()),  --对ArrayList的数据进行排序。
    al[1],                                 --虽然Java的List不支持下标操作，但ARRAY支持。
    Objects.toString(a),        --之前不支持将ARRAY类型Cast成STRING，现在有绕过方法了。
    array(1,2,3).subList(1, 2)             --计算subList。
from (select new ArrayList<Integer>(array(1,2,3)) as al, array(1,2,3) as a) t;
```

## 聚合操作

UDT实现聚合的原理是，先用内建函数[COLLECT\_SET](/intl.zh-CN/开发/SQL及函数/内建函数/聚合函数.md)或[COLLECT\_LIST](/intl.zh-CN/开发/SQL及函数/内建函数/聚合函数.md)将数据转变成List，之后对该List应用UDT的标量方法计算数据的聚合值。

示例如下，计算BigInteger的中位数（由于数据是`java.math.BigInteger`类型的，所以不能直接用内建函数[MEDIAN](/intl.zh-CN/开发/SQL及函数/内建函数/聚合函数.md)）。

```
set odps.sql.session.java.imports=java.math.*;
@test_data := select * from values (1),(2),(3),(5) as t(value);
@a := select collect_list(new BigInteger(value)) values from @test_data;  --先把数据聚合成List。
@b := select sort_array(values) as values, values.size() cnt from @a;  --计算中位数的逻辑，先将数据排序。
@c := select if(cnt % 2 == 1, new BigDecimal(values[cnt div 2]), new BigDecimal(values[cnt div 2 - 1].add(values[cnt div 2])).divide(new BigDecimal(2))) med from @b;
--最终结果。
select med.toString() from @c;
```

`collect_list`会先将所有数据都收集到一起，无法实现Partial Aggregate，处理效率比内置聚合函数或者UDAF低，请尽量使用内置聚合函数。同时，把一个Group的所有数据都收集到一起，会增加数据倾斜的风险。

如果UDAF的逻辑是要将所有数据收集到一起（例如类似内置聚合函数`WM_CONCAT`的功能），使用上述方法，处理效率比UDAF高。

## 表值函数

表值函数允许输入多行多列数据，输出多行多列数据。可以按照如下操作实现：

-   输入多行多列数据，详情请参见[聚合操作](#section_jei_63w_tbz)。
-   输出多行多列数据，可以用UDT方法输出一个Collection类型的数据（List或者MAP），然后调用Explode函数，将Collections展开成多行。
-   UDT可以包含多个数据域，通过调用不同的Getter方法获取各个域的内容即可展开成多列。

展开一个JSON字符串的内容，示例如下。

```
@a := select '[{"a":"1","b":"2"},{"a":"1","b":"2"}]' str; --示例数据。
@b := select new com.google.gson.Gson().fromJson(str, java.util.List.class) l from @a; --反序列化JSON。
@c := select cast(e as java.util.Map<Object,Object>) m from @b lateral view explode(l) t as e;  --用explode展开成多行。
@d := select m.get('a') as a, m.get('b') as b from @c; --展开成多列。
select a.toString() a, b.toString() b from @d; --最终结果输出（注意变量d的输出中a、b两列是Object类型）。
```

## 函数重载

MaxCompute UDF使用`evaluate`方法重载函数。该方式不支持泛型，当您需要定义一个支持任何数据类型的函数时，必须为每种类型都写一个`evaluate`函数。该方法无法实现个别输入类型（例如ARRAY）的重载函数。在没有提供Resolve注解的情况下，Python UDF或UDTF会根据参数个数决定输入参数，同时支持变长参数，但会导致编译器无法静态找到某些错误。

通过UDT实现函数重载，可以解决上述问题。UDT支持泛型、类继承和变长参数，为您提供灵活的函数定义方式，示例如下。

```
public class UDTClass {
    // 这个函数支持一个数值类型（可以是TINYINT、SMALLINT、INT、BIGINT、FLOAT、DOUBLE以及任何以Number为基类的UDT），返回DOUBLE。
    public static Double doubleValue(Number input) {
        return input.doubleValue();
    }
    // 这个方法支持一个数值类型参数和一个任意类型的参数，返回值类型与第二个参数的类型相同。
    public static <T extends Number, R> R nullOrValue(T a, R b) {
        return a.doubleValue() > 0 ? b : null;
    }
    // 这个方法支持一个任意元素类型的ARRAY或List，返回BIGINT。
    public static Long length(java.util.List<? extends Object> input) {
        return input.size();
    }
    // 注意在不做强制转换的情况下参数只支持UDT的java.util.Map<Object, Object>对象。如果需要传入任何MAP对象，例如map<bigint,bigint>可以考虑:
    // 1. 定义函数时使用java.util.Map<? extends Object, ? extends Object>。
    // 2. 调用时做强制转换，例如UDTClass.mapSize(cast(mapObj as java.util.Map<Object, Object>))。
    public static Long mapSize(java.util.Map<Object, Object> input) {
        return input.size();
    }
}
```

特定场景下，UDF可以通过`com.aliyun.odps.udf.ExecutionContext`（在`setup`方法中传入）获取上下文；UDT可以通过`com.aliyun.odps.udt.UDTExecutionContext.get()`方法获取`ExecutionContext`对象。

## UDT引用嵌入式代码

代码嵌入式UDF允许您将SQL脚本和第三方代码放入同一个源码文件，减少使用UDT的操作步骤，方便日常开发。详情请参见[代码嵌入式UDF](/intl.zh-CN/开发/SQL及函数/UDF/代码嵌入式UDF.md)。

