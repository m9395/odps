---
keyword: Join长尾优化
---

# JOIN长尾优化

本文为您介绍执行SQL时JOIN阶段常见的数据倾斜场景以及对应的解决办法。

## 背景信息

MaxComputeSQL在JOIN阶段会将JOIN Key相同的数据分发到同一个Instance上进行处理。如果某个Key上的数据量比较多，会导致该Instance执行时间比其他Instance执行时间长。执行日志中该JOIN Task的大部分Instance都已执行完成，但少数几个Instance一直处于执行中，这种现象称之为长尾。

数据量倾斜导致长尾的现象比较普遍，严重影响任务的执行时间，尤其是在双十一等大型活动期间，长尾程度比平时更为严重。例如，某些大型店铺的浏览PV远远超过一般店铺的PV，当用浏览日志数据和卖家维表关联时，会按照卖家ID进行分发，导致某个Instance处理的数据量远远超过其他Instance，而整个任务会因为这个长尾的Instance无法结束。

您可以从以下四方面进行长尾处理考虑：

-   如果两张表里有一张大表和一张小表，可以考虑使用MAP JOIN，对小表进行缓存，具体的语法和说明请参见[SELECT语法介绍](/cn.zh-CN/开发/SQL及函数/SELECT语句/SELECT语法介绍.md)。
-   如果两张表都比较大，就需要先尽量去重。
-   从业务上考虑，寻找两个大数据量的Key执行笛卡尔积的原因，从业务上进行优化。
-   小表LEFT JOIN大表，直接LEFT JOIN较慢。先将小表和大表进行MAP JOIN，得到小表和大表的交集中间表，且这个中间表一定是不大于大表的（Key倾斜程度与表的膨胀大小成正比）。然后小表再和这个中间表进行LEFT JOIN，这样操作的效果等于小表LEFT JOIN大表。

## 查看数据倾斜

执行如下步骤查看JOIN是否发生数据倾斜：

1.  打开SQL执行时产生的Log View日志，查看每个Fuxi Task的详细执行信息。**Long-Tails\(115\)**表示有115个长尾。

    ![查看数据倾斜.png](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2951652061/p68473.png)

2.  单击**FuxiInstance**后的![查看.png](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4004359951/p68474.png)图标，查看**StdOut**中Instance读入的数据量。

    例如，`Read from 0 num:52743413 size:1389941257`表示JOIN输入读取的数据量是1389941257行。如果Long-Tails中Instance读取的数据量远超过其它Instance读取的数据量，则表示是因为数据量导致长尾。


## 常见场景及解决方案

-   MAP JOIN方案：JOIN倾斜时，如果某路输入比较小，可以采用MAP JOIN避免分发引起的长尾。

    MAP JOIN的原理是将JOIN操作提前到Map端执行，这样可以避免因为分发Key不均匀导致数据倾斜。MAP JOIN使用限制如下：

    -   MAP JOIN使用时，JOIN中的从表比较小才可用。所谓从表，即LEFT OUTER JOIN中的右表，或者RIGHT OUTER JOIN中的左表。
    -   MAP JOIN使用时，对小表的大小有限制，默认小表读入内存后的大小不能超过512MB。用户可以通过如下语句加大内存，最大为2048MB。

        ```
        set odps.sql.mapjoin.memory.max=2048
        ```

    MAP JOIN的使用方法非常简单，在SQL语句中`SELECT`后加上`/*+ mapjoin(b) */` 即可，其中b代表小表（或者是子查询）的别名。举例如下。

    ```
    select   /*+mapjoin(b)*/       
               a.c2       
              ,b.c3
    from        
             (select   c1                 
                      ,c2         
               from     t1         ) a
    left outer join        
             (select   c1                 
                      ,c3         
              from     t2         ) b
    on       a.c1 = b.c1;
    ```

-   JOIN因为空值导致长尾

    如果是因为空值的聚集导致长尾，并且JOIN的输入比较大无法用MAP JOIN，可以将空值处理成随机值。因为空值是无法关联上，只是分发到了一处，因此给予随机值既不会影响关联也能避免聚集。

    ```
    select   ...
    from
            (select   *
             from     tbcdm.dim_tb_itm
             where    ds='${bizdate}'
             )son1
    left outer join
            (select  *
             from    tbods.s_standard_brand
             where   ds='${bizdate}'
             and     status=3
             )son2
    on       coalesce(son1.org_brand_id,rand()*9999)=son2.value_id;
    ```

    `ON`子句中使用`coalesce(son1.org_brand_id,rand()*9999)`表示当`org_brand_id`为空时用随机数替代，避免大量空值聚集引起长尾。

-   JOIN因为热点值导致长尾

    如果是因为热点值导致长尾，并且JOIN的输入比较大无法用MAP JOIN，可以先将热点Key取出，对于主表数据用热点Key切分成热点数据和非热点数据两部分分别处理，最后合并。以淘宝的PV日志表关联商品维表取商品属性为例：

    1.  取出热点Key：将PV大于50000的商品ID取出到临时表。

        ```
        insert   overwrite table topk_item
        select   item_id
        from
                (select   item_id
                         ,count(1) as cnt
                 from     dwd_tb_log_pv_di
                 where    ds = '${bizdate}'
                 and      url_type = 'ipv'
                 and      item_id is not null
                 group by item_id
                 ) a
        where    cnt >= 50000;
        ```

    2.  取出非热点数据。

        将主表（sdwd\_tb\_log\_pv\_di）和热点key表\(topk\_item\)外关联后通过条件`b1.item_id is null`，取出关联不到的数据即非热点商品的日志数据，此时需要用MAP JOIN。再用非热点数据关联商品维表，因为已经排除了热点数据，不会存在长尾。

        ```
        select   ...
        from
                (select   *
                 from     dim_tb_itm
                 where    ds = '${bizdate}'
                 ) a
        right outer join
                (select   /*+mapjoin(b1)*/
                          b2.*
                 from
                         (select   item_id
                          from     topk_item
                          where    ds = '${bizdate}'
                          ) b1
                 right outer join
                         (select   *
                          from     dwd_tb_log_pv_di
                          where    ds = '${bizdate}'
                          and      url_type = 'ipv'
                          ) b2
                 on       b1.item_id = coalesce(b2.item_id,concat("tbcdm",rand())
                 where    b1.item_id is null
                 ) l
        on       a.item_id = coalesce(l.item_id,concat("tbcdm",rand());
        ```

    3.  取出热点数据。

        将主表（sdwd\_tb\_log\_pv\_di）和热点Key表\(topk\_item\)内关联，此时需要用MAP JOIN，取到热点商品的日志数据。同时，需要将商品维表（dim\_tb\_itm）和热点Key表\(topk\_item\)内关联，取到热点商品的维表数据，然后将第一部分数据外关联第二部分数据，因为第二部分只有热点商品的维表，数据量比较小，可以用MAP JOIN避免长尾。

        ```
        select   /*+mapjoin(a)*/
                 ...
        from
                (select   /*+mapjoin(b1)*/
                          b2.*
                 from
                         (select   item_id
                          from     topk_item
                          where    ds = '${bizdate}'
                          )b1
                 join
                         (select   *
                          from     dwd_tb_log_pv_di
                          where    ds = '${bizdate}'
                          and      url_type = 'ipv'
                          and      item_id is not null
                          ) b2
                 on       (b1.item_id = b2.item_id)
                 ) l
        left outer join
                (select   /*+mapjoin(a1)*/
                          a2.*
                 from
                         (select   item_id
                          from     topk_item
                          where    ds = '${bizdate}'
                          ) a1
                 join
                         (select   *
                          from     dim_tb_itm
                          where    ds = '${bizdate}'
                          ) a2
                 on       (a1.item_id = a2.item_id)
                 ) a
        on       a.item_id = l.item_id;
        ```

    4.  将步骤2和步骤3的数据通过`union all`合并后即得到完整的日志数据，并且关联了商品的信息。
-   通过设置odps.sql.skewjoin参数解决长尾问题。

    此方法简单方便，但是如果倾斜的值发生变化需要修改代码重新执行命令，且变化无法提前预知。另外，如果倾斜值较多也不方便在参数中设置，需要根据实际情况选择拆分代码或者参数设置。参数设置的操作步骤如下：

    1.  开启功能。

        ```
        set odps.sql.skewjoin=true
        ```

    2.  设置倾斜的Key及对应的值。

        ```
        set odps.sql.skewinfo=skewed_src:(skewed_key) [("skewed_value")]
        ```

        其中，skewed\_key代表倾斜的列，skewed\_value代表倾斜列上的倾斜值。


