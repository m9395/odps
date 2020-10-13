---
keyword: optimization of JOIN long tails
---

# Optimize JOIN long tails

This topic describes the data skew when the JOIN statement in MaxCompute SQL is executed and related solutions.

## Background information

When the JOIN statement in MaxCompute SQL is executed, the data with the same JOIN key is sent to and processed on the same instance. If a key contains a large amount of data, the related instance takes a longer time to process the data than other instances. In the execution log, a few instances in this JOIN task remain in the executing state, while other instances are in the completed state. This is called a long tail.

Long tails caused by data skew are common and significantly prolong task execution. During promotion events such as Double 11, severe long tails may occur. For example, the page views of large sellers are much more than the page views of small sellers. When page view log data is associated with the seller dimension table, data is allocated by seller ID. This causes some instances to process far more data than others. In this case, the task cannot be completed due to a few long tails.

You can solve long tails from four perspectives:

-   If one large table and one small table exist, you can execute the MAP JOIN statement to cache the small table. For more information about the MAP JOIN statement, see [SELECT syntax](/intl.en-US/Development/SQL/Select Operation/SELECT syntax.md).
-   If two large tables exist, perform data deduplication first.
-   Try to find out the cause for the Cartesian product of two large keys and optimize these keys from the business perspective.
-   It takes a long time to directly execute the LEFT JOIN statement for a small table and a large table. In this case, we recommend that you execute the MAP JOIN statement for the small and large tables to generate an intermediate table that contains the intersection of the two tables. This intermediate table is not larger than the large table because the MAP JOIN statement filters out unnecessary data from the large table. Then, execute the LEFT JOIN statement for the small and intermediate tables. The effect of this operation process is equivalent to that of executing the LEFT JOIN statement for the small and large tables.

## Check data skew

Perform the following steps to check data skew:

1.  Open the Logview log file generated when SQL statements are executed and check the execution details of each Fuxi task. **Long-Tails\(115\)** indicates that 115 long tails exist.

    ![View data skew](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7951652061/p68473.png)

2.  Click the ![View Long-Tails](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7951652061/p68474.png) icon next to a **Fuxi instance** to check the data read by the instance in stdout.

    For example, `Read from 0 num:52743413 size:1389941257` indicates that 1,389,941,257 rows of data are being read when the JOIN statement is executed. If an instance listed in Long-Tails reads far more data than other instances, the long tail is caused by a large amount of data.


## Common causes and solutions

-   MAP JOIN statement: If data skew occurs when the JOIN statement is executed and a small table is involved, you can execute the MAP JOIN statement to prevent a long tail.

    The MAP JOIN statement works in this way: The JOIN statement is executed at the Map side. This prevents data skew caused by uneven key distribution. The MAP JOIN statement is subject to the following limits:

    -   The MAP JOIN statement is applicable only when the secondary table is small. A secondary table refers to the right table when the LEFT OUTER JOIN statement is executed or the left table when the RIGHT OUTER JOIN statement is executed.
    -   The size of the small table is also limited when the MAP JOIN statement is executed. By default, the maximum size is 512 MB after the small table is read to the memory. You can execute the following statement to expand the maximum size to 2,048 MB:

        ```
        set odps.sql.mapjoin.memory.max=2048
        ```

    The MAP JOIN statement is easy to use. You can add `/*+ mapjoin(b) */` after the `SELECT` statement, where b indicates the alias of the small table or the subquery. Example:

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

-   JOIN long tails caused by null values

    If accumulated null values cause a long tail and the MAP JOIN statement cannot be used because no small table is involved, these null values are processed as random values. Null values cannot be associated and therefore are sent to one instance. Random values can be associated, which helps prevent value accumulation.

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

    If the `ON` clause contains `coalesce(son1.org_brand_id,rand()*9999)`, `org_brand_id` is replaced with a random value if it is null. This prevents long tails caused by accumulated null values.

-   JOIN long tails caused by hot key values

    If hot key values cause a long tail and the MAP JOIN statement cannot be used because no small table is involved, extract hot keys. Hot key data in the primary table is separated from other data, processed independently, and then combined with other data. In the following example, the page view log table of the Taobao website is associated with the commodity dimension table.

    1.  Extract data of hot keys: Extract the IDs of the commodities whose page views are greater than 50,000 to a temporary table.

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

    2.  Extract data of non-hot keys

        Execute the OUTER JOIN statement to associate primary table sdwd\_tb\_log\_pv\_di with hot key table topk\_item. Then, apply condition `b1.item_id is null` to extract the log data of non-hot commodities that cannot be associated. Execute the MAP JOIN statement to extract data of non-hot keys. Then, associate the non-hot key table with the commodity dimension table. No long tails exist because hot key data has been removed.

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

    3.  Extract data of hot keys

        Execute the INNER JOIN statement to associate primary table sdwd\_tb\_log\_pv\_di with hot key table topk\_item. Execute the MAP JOIN statement to obtain the log data of hot commodities. Execute the INNER JOIN statement to associate commodity dimension table dim\_tb\_itm with hot key table topk\_item to obtain data of the hot commodity dimension table. Execute the OUTER JOIN statement to associate the log data and data of the dimension table.

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

    4.  Execute the `UNION ALL` statement to combine the data obtained in Steps ii and iii and generate the entire log data, with commodity information associated.
-   Set the odps.sql.skewjoin parameter to solve long tails

    This is a simple solution. However, you must modify code and execute the statements again if skewed key values change. In addition, value changes cannot be predicted. If many skewed key values are involved, it is inconvenient to set them. In this case, you can split code or parameter settings as required. Perform the following steps to set the odps.sql.skewjoin parameter:

    1.  Set the odps.sql.skewjoin parameter to true.

        ```
        set odps.sql.skewjoin=true
        ```

    2.  Set a skewed key and its value.

        ```
        set odps.sql.skewinfo=skewed_src:(skewed_key) [("skewed_value")]
        ```

        skewed\_key indicates the skewed column and skewed\_value indicates its value.


