---
keyword: [Spark, VPC]
---

# Access instances in a VPC from Spark on MaxCompute

This topic describes how to access instances in a VPC from Spark on MaxCompute.

## VPC access

You can access instances \(for example ECS, ApsaraDB for HBase, and ApsaraDB for RDS\) in a VPC, or access user-defined private domain names from Spark on MaxCompute.

When you access instances in a VPC from Spark on MaxCompute, add the `spark.hadoop.odps.cupid.vpc.domain.list` parameter to the spark-defaults.conf file or the DataWorks file to specify one or more instances. The value of this parameter is in the JSON format. When you configure this parameter, you must delete the spaces and line breaks between multiple lines in the parameter and merge JSON text into one line.

When you access different instances from Spark on MaxCompute, set `spark.hadoop.odps.cupid.vpc.domain.list` to a required value, as described in the following examples. You must replace the RegionID, VPCID, instance domain name, and port number with actual values. For information about RegionID of a region, see [Regions and zones](/intl.en-US/Product Introduction/Regions and zones.md).

**Note:**

-   You must add the CIDR block 100.104.0.0/16 to the whitelist of the service that you want to access.
-   If RegionID is cn-shanghai or cn-beijing, you must set `spark.hadoop.odps.cupid.smartnat.enable` to true.
-   You can access only the services in one VPC in the current region from Spark on MaxCompute.

## Example 1: access ApsaraDB for MongoDB

When you access ApsaraDB for MongoDB from Spark on MaxCompute, set `spark.hadoop.odps.cupid.vpc.domain.list`, as shown in the following example. This ApsaraDB for MongoDB instance has a primary instance and a secondary instance.

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

Results of merging JSON text into one line:

```
{"regionId":"cn-beijing","vpcs":[{"vpcId":"vpc-2zeaeq21mb1dmkqh0****","zones":[{"urls":[{"domain":"dds-2ze3230cfea08****.mongodb.rds.aliyuncs.com","port": 3717},{"domain":"dds-2ze3230cfea08****.mongodb.rds.aliyuncs.com","port":3717}]}]}]}
```

## Example 2: access ApsaraDB for RDS

When you access ApsaraDB for RDS from Spark on MaxCompute, set `spark.hadoop.odps.cupid.vpc.domain.list`, as shown in the following example:

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

Results of merging JSON text into one line:

```

{"regionId":"cn-beijing","vpcs":[{"vpcId":"vpc-2zeaeq21mb1dmkqh0****","zones":[{"urls":[{"domain":"rm-2zem49k73c54z****.mysql.rds.aliyuncs.com","port": 3306}]}]}]}
```

## Example 3: access ApsaraDB for HBase

When you access ApsaraDB for HBase from Spark on MaxCompute, set `spark.hadoop.odps.cupid.vpc.domain.list`, as shown in the following example:

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
            },
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

Results of merging JSON text into one line:

```

{"regionId":"cn-beijing","vpcs":[{"vpcId":"vpc-2zeaeq21mb1dmkqh0exox","zones":[{"urls":[{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":2181},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":16000},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":16020},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":2181},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":16000},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":16020},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":2181},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":16000},{"domain":"hb-2zecxg2ltnpeg8me4-master*-***.hbase.rds.aliyuncs.com","port":16020},{"domain":"hb-2zecxg2ltnpeg8me4-cor*-***.hbase.rds.aliyuncs.com","port":16020},{"domain":"hb-2zecxg2ltnpeg8me4-cor*-***.hbase.rds.aliyuncs.com","port":16020},{"domain":"hb-2zecxg2ltnpeg8me4-cor*-***.hbase.rds.aliyuncs.com","port":16020}]}]}]}
```

## Example 4: access ApsaraDB for Redis

When you access ApsaraDB for Redis from Spark on MaxCompute, set `spark.hadoop.odps.cupid.vpc.domain.list`, as shown in the following example:

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

Results of merging JSON text into one line:

```

{"regionId":"cn-beijing","vpcs":[{"vpcId":"vpc-2zeaeq21mb1dmkqh0****","zones":[{"urls":[{"domain":"r-2zebda0d3c05****.redis.rds.aliyuncs.com","port":3717}]}]}]}
```

## Example 5: access LogHub

When you access LogHub from Spark on MaxCompute, set `spark.hadoop. odps.cupid.vpc.domain.list`, as shown in the following example:

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

Results of merging JSON text into one line:

```

{"regionId":"cn-beijing","vpcs":[{"zones":[{"urls":[{"domain":"cn-beijing-intranet.log.aliyuncs.com","port":80}]}]}]}
```

For the domain parameter, use the LogHub endpoint of the classic network or the VPC endpoint. For the endpoint of each region, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).

## Example 6: access DataHub

When you access DataHub from Spark on MaxCompute, set `spark.hadoop.odps.cupid.vpc.domain.list`, as shown in the following example:

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

Results of merging JSON text into one line:

```

{"regionId":"cn-beijing","vpcs":[{"zones":[{"urls":[{"domain":"dh-cn-beijing.aliyun-inc.com","port":80}]}]}]}
```

For the domain parameter, use the ECS endpoint of the classic network as the DataHub endpoint.

## Example 7: access a user-defined domain name

Assume that you customize a domain name `a.b.com` in a VPC. An access is initiated from Spark on MaxCompute by using this domain name and port `a.b.com:80`. You must complete the following configurations before the access:

1.  Associate a zone with a VPC in PrivateZone.
2.  Click [Authorize](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22:%20%7B%22request1%22:%20%7B%22RoleName%22:%20%22AliyunODPSDefaultRole%22,%20%22TemplateId%22:%20%22DefaultRole%22%7D%7D,%20%22ReturnUrl%22:%20%22https:%2F%2Fodps.console.aliyun.com%2F%22,%20%22Service%22:%20%22ODPS%22%7D) to grant MaxCompute the read-only access permissions on PrivateZone.
3.  In Spark node configuration, add the following two parameters:

    ```
    spark.hadoop.odps.cupid.pvtz.rolearn=acs:ram::xxxxxxxxxxx:role/aliyunodpsdefaultrole 
    spark.hadoop.odps.cupid.vpc.usepvtz=true
    ```

    The `spark.hadoop.odps.cupid.pvtz.rolearn` parameter indicates your ARN information. You can obtain the information from the [RAM console](https://ram.console.aliyun.com/roles/AliyunODPSDefaultRole).

4.  In the Spark configuration file, set `spark.hadoop.odps.cupid.vpc.domain.list`, as shown in the following example:

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

    Results of merging JSON text into one line:

    ```
    
    {"regionId":"cn-beijing","vpcs":[{"vpcId":"vpc-2zeaeq21mb1dmkqh0****","zones":[{"urls":[{"domain":"a.b.com","port":80}],"zoneId":"9b7ce89c6a6090e114e0f7c415ed****"}]}]}
    ```


## Example 8: access HDFS

-   To use HDFS, add hdfs-site.xml. The file contains the following content:

    ```
    <? xml version="1.0"? >
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

-   In the Spark configuration file, set `spark.hadoop.odps.cupid.vpc.domain.list`, as shown in the following example:

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

    Results of merging JSON text into one line:

    ```
    
    {"regionId": "cn-shanghai","vpcs": [{"vpcId": "vpc-xxxxxx","zones": [{"urls": [{"domain": "DfsMountpointDomainName","port": 10290}]}]}]}
    ```


