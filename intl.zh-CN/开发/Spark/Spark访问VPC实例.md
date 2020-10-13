---
keyword: [Spark, VPC]
---

# Spark访问VPC实例

本文为您介绍如何使用Spark on MaxCompute访问阿里云VPC内的用户实例。

## 访问VPC说明

Spark on MaxCompute可以访问阿里云VPC内的云服务器ECS（Elastic Compute Service）、云数据库HBase（Hadoop Database）和云关系型数据库RDS（Relational Database Service）等实例，同时还可以访问自定义私有域名。

访问VPC实例时，在spark-defaults.conf或者DataWorks的配置文件中添加`spark.hadoop.odps.cupid.vpc.domain.list`参数，表明需要访问的一个或多个实例的网络情况。该参数值为JSON格式，配置时需要删除参数中多行文本之间的空格和换行符，合并JSON文本为一行。

访问不同实例时，`spark.hadoop.odps.cupid.vpc.domain.list`参数取值请参见下文示例。您需要将示例中的RegionID、VPCID、实例域名和端口等替换为实际使用场景下的值。区域的RegionID请参见[项目空间操作](/intl.zh-CN/开发/常用命令/项目空间操作.md)。

**说明：**

-   需要添加100.104.0.0/16网段至所访问的服务的白名单。
-   如果RegionID为cn-shanghai或者cn-beijing，需要设置`spark.hadoop.odps.cupid.smartnat.enable=true`。
-   Spark on MaxCompute只支持访问本Region下某一个VPC的服务，不支持同时访问多个VPC的服务。

## 访问MongoDB示例

访问MongoDB时，`spark.hadoop.odps.cupid.vpc.domain.list`参数取值如下。该MongoDB有主备2个实例。

```
{
  "regionId":"cn-beijing",
  "vpcs":[
    {
      "vpcId":"vpc-2zeaeq21mb1dmkqh0****",
      "zones":[
        {
          "urls":[
            {
              "domain":"dds-2ze3230cfea08****.mongodb.rds.aliyuncs.com",
              "port": 3717
            },
            {
              "domain":"dds-2ze3230cfea08****.mongodb.rds.aliyuncs.com",
              "port":3717
            }
          ]
        }
      ]
    }
  ]
}
```

合并示例中JSON文本为一行的结果如下。

```
{"regionId":"cn-beijing","vpcs":[{"vpcId":"vpc-2zeaeq21mb1dmkqh0****","zones":[{"urls":[{"domain":"dds-2ze3230cfea08****.mongodb.rds.aliyuncs.com","port": 3717},{"domain":"dds-2ze3230cfea08****.mongodb.rds.aliyuncs.com","port":3717}]}]}]}
```

## 访问RDS示例

访问RDS时，`spark.hadoop.odps.cupid.vpc.domain.list`参数取值如下。

```
{
  "regionId":"cn-beijing",
  "vpcs":[
    {
      "vpcId":"vpc-2zeaeq21mb1dmkqh0****",
      "zones":[
        {
          "urls":[
            {
              "domain":"rm-2zem49k73c54z****.mysql.rds.aliyuncs.com",
              "port": 3306
            }
          ]
        }
      ]
    }
  ]
}
```

合并示例中JSON文本为一行的结果如下。

```

{"regionId":"cn-beijing","vpcs":[{"vpcId":"vpc-2zeaeq21mb1dmkqh0****","zones":[{"urls":[{"domain":"rm-2zem49k73c54z****.mysql.rds.aliyuncs.com","port": 3306}]}]}]}
```

## 访问HBase示例

访问HBase时，`spark.hadoop.odps.cupid.vpc.domain.list`参数取值如下。

```
{
  "regionId":"cn-beijing",
  "vpcs":[
    {
      "vpcId":"vpc-2zeaeq21mb1dmkqh0exox",
      "zones":[
        {
          "urls":[
            {
              "domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com",
              "port":2181
            },
            {
              "domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com",
              "port":16000
            },
            {
              "domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com",
              "port":16020
            },
            {
              "domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com",
              "port":2181
            },
            {
              "domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com",
              "port":16000
            },
            {
              "domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com",
              "port":16020
            },
            {
              "domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com",
              "port":2181
            },
            {
              "domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com",
              "port":16000
            },
            {
              "domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com",
              "port":16020
            },
            {
              "domain":"hb-2zecxg2ltnpeg8me4-cor*-***.hbase.rds.aliyuncs.com",
              "port":16020
            },
            {
              "domain":"hb-2zecxg2ltnpeg8me4-cor*-***.hbase.rds.aliyuncs.com",
              "port":16020
            }，
            {
              "domain":"hb-2zecxg2ltnpeg8me4-cor*-***.hbase.rds.aliyuncs.com",
              "port":16020
            }
          ]
        }
      ]
    }
  ]
}
```

合并示例中JSON文本为一行的结果如下。

```

{"regionId":"cn-beijing","vpcs":[{"vpcId":"vpc-2zeaeq21mb1dmkqh0exox","zones":[{"urls":[{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":2181},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":16000},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":16020},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":2181},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":16000},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":16020},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":2181},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":16000},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":16020},{"domain":"hb-2zecxg2ltnpeg8me4-cor*-***.hbase.rds.aliyuncs.com","port":16020},{"domain":"hb-2zecxg2ltnpeg8me4-cor*-***.hbase.rds.aliyuncs.com","port":16020},{"domain":"hb-2zecxg2ltnpeg8me4-cor*-***.hbase.rds.aliyuncs.com","port":16020}]}]}]}
```

## 访问Redis示例

当访问Redis时，`spark.hadoop.odps.cupid.vpc.domain.list`参数取值如下。

```
{
  "regionId":"cn-beijing",
  "vpcs":[
    {
      "vpcId":"vpc-2zeaeq21mb1dmkqh0****",
      "zones":[
        {
          "urls":[
            {
              "domain":"r-2zebda0d3c05****.redis.rds.aliyuncs.com",
              "port":3717
            }
          ]
        }
      ]
    }
  ]
}
```

合并示例中JSON文本为一行的结果如下。

```

{"regionId":"cn-beijing","vpcs":[{"vpcId":"vpc-2zeaeq21mb1dmkqh0****","zones":[{"urls":[{"domain":"r-2zebda0d3c05****.redis.rds.aliyuncs.com","port":3717}]}]}]}
```

## 访问LogHub示例

访问LogHub时，`spark.hadoop.odps.cupid.vpc.domain.list`参数取值如下。

```
{
  "regionId":"cn-beijing",
  "vpcs":[
    {
      "zones":[
        {
          "urls":[
            {
              "domain":"cn-beijing-intranet.log.aliyuncs.com",
              "port":80
            }
          ]
        }
      ]
    }
  ]
}
```

合并示例中JSON文本为一行的结果如下。

```

{"regionId":"cn-beijing","vpcs":[{"zones":[{"urls":[{"domain":"cn-beijing-intranet.log.aliyuncs.com","port":80}]}]}]}
```

domain请使用LogHub Endpoint的经典网络或VPC网络服务入口，各Region对应的Endpoint请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。

## 访问DataHub示例

访问DataHub时，`spark.hadoop.odps.cupid.vpc.domain.list`参数取值如下。

```
{
  "regionId":"cn-beijing",
  "vpcs":[
    {
      "zones":[
        {
          "urls":[
            {
              "domain":"dh-cn-beijing.aliyun-inc.com",
              "port":80
            }
          ]
        }
      ]
    }
  ]
}
```

合并示例中JSON文本为一行的结果如下。

```

{"regionId":"cn-beijing","vpcs":[{"zones":[{"urls":[{"domain":"dh-cn-beijing.aliyun-inc.com","port":80}]}]}]}
```

domain请使用Datahub Endpoint的经典网络下的ECS Endpoint。

## 访问自定义域名示例

假设您在VPC内自定义了域名`a.b.com`，Spark通过域名和端口`a.b.com:80`发起访问。您在使用前需要完成以下配置：

1.  在PrivateZone中将Zone关联至VPC。
2.  单击[一键授权](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22:%20%7B%22request1%22:%20%7B%22RoleName%22:%20%22AliyunODPSDefaultRole%22,%20%22TemplateId%22:%20%22DefaultRole%22%7D%7D,%20%22ReturnUrl%22:%20%22https:%2F%2Fodps.console.aliyun.com%2F%22,%20%22Service%22:%20%22ODPS%22%7D)，为MaxCompute赋予PrivateZone的只读权限。
3.  在Spark节点配置里新增如下2个参数。

    ```
    spark.hadoop.odps.cupid.pvtz.rolearn=acs:ram::xxxxxxxxxxx:role/aliyunodpsdefaultrole 
    spark.hadoop.odps.cupid.vpc.usepvtz=true
    ```

    `spark.hadoop.odps.cupid.pvtz.rolearn`为用户的ARN信息，从[RAM控制台](https://ram.console.aliyun.com/roles/AliyunODPSDefaultRole)可以获取。

4.  在Spark的配置文件中新增`spark.hadoop.odps.cupid.vpc.domain.list`参数，取值如下。

    ```
    {
      "regionId":"cn-beijing",
      "vpcs":[
        {
          "vpcId":"vpc-2zeaeq21mb1dmkqh0****",
          "zones":[
            {
              "urls":[
                {
                  "domain":"a.b.com",
                  "port":80
                }
              ],
              "zoneId":"9b7ce89c6a6090e114e0f7c415ed****"
            }
          ]
        }
      ]
    }
    ```

    合并示例中JSON文本为一行的结果如下。

    ```
    
    {"regionId":"cn-beijing","vpcs":[{"vpcId":"vpc-2zeaeq21mb1dmkqh0****","zones":[{"urls":[{"domain":"a.b.com","port":80}],"zoneId":"9b7ce89c6a6090e114e0f7c415ed****"}]}]}
    ```


## 访问文件存储HDFS示例

-   在Spark中使用文件存储HDFS需要新增hdfs-site.xml，内容如下所示。

    ```
    <?xml version="1.0"?>
    <configuration>
        <property>
            <name>fs.defaultFS</name>
            <value>dfs://DfsMountpointDomainName:10290</value>
        </property>
        <property>
            <name>fs.dfs.impl</name>
            <value>com.alibaba.dfs.DistributedFileSystem</value>
        </property>
        <property>
            <name>fs.AbstractFileSystem.dfs.impl</name>
            <value>com.alibaba.dfs.DFS</value>
        </property>
    </configuration>
    ```

-   在Spark配置文件中新增`spark.hadoop.odps.cupid.vpc.domain.list`参数，取值如下。

    ```
    {
        "regionId": "cn-shanghai",
        "vpcs": [{
            "vpcId": "vpc-xxxxxx",
            "zones": [{
                "urls": [{
                    "domain": "DfsMountpointDomainName",
                    "port": 10290
                }]
            }]
        }]
    }
    ```

    合并示例中JSON文本为一行的结果如下。

    ```
    
    {"regionId": "cn-shanghai","vpcs": [{"vpcId": "vpc-xxxxxx","zones": [{"urls": [{"domain": "DfsMountpointDomainName","port": 10290}]}]}]}
    ```


