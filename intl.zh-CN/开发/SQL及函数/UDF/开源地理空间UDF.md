---
keyword: 开源地理空间UDF
---

# 开源地理空间UDF

本文为您介绍如何使用开源地理空间UDF分析空间数据。

Apache Hive有一套开源的地理空间UDF，详情请参见[Github](https://github.com/Esri/spatial-framework-for-hadoop)。MaxCompute原生支持直接使用Hive UDF，因此也支持在MaxCompute中使用Hive地理空间函数。

MaxCompute使用Hive UDF的示例请参见[Hive UDF兼容示例](/intl.zh-CN/开发/SQL及函数/UDF/Java UDF.md)。

**说明：** 在使用过程中，如果您有任何问题，请直接在GitHub上提交issues获取帮助。

## 准备本地函数

1.  在本地**命令提示符**中执行如下命令下载2.1.0版本Hive（对应Hadoop版本为2.7.2）下的地理空间UDF代码至本地。

    ```
    git clone -b "v2.1.0" --single-branch git@github.com:Esri/spatial-framework-for-hadoop.git
    ```

2.  使用Maven构建项目。

    ```
    cd spatial-framework-for-hadoop
    mvn clean package -DskipTests -P java-8,hadoop-2.7,hive-2.1
    ```

3.  复制构建好的JAR包。此JAR包包含开源地理空间UDF的所有方法。

    ```
    cd ..
    cp spatial-framework-for-hadoop/hive/target/spatial-sdk-hive-2.1.1-SNAPSHOT.jar ./spatial-sdk-hive.jar
    ```

4.  执行如下命令下载项目所依赖的JAR包。

    ```
    wget https://repo1.maven.org/maven2/com/esri/geometry/esri-geometry-api/2.2.0/esri-geometry-api-2.2.0.jar ./esri-geometry-api.jar
    --等同于下面命令。
    cp ~/.m2/repository/com/esri/geometry/esri-geometry-api/2.2.0/esri-geometry-api-2.2.0.jar ./esri-geometry-api.jar
    ```


## 在MaxCompute上注册函数

1.  在MaxCompute[客户端](/intl.zh-CN/工具及下载/客户端.md)上执行如下命令将本地准备好的两个依赖JAR包上传至目标Project。

    ```
    ADD JAR esri-geometry-api.jar;
    ADD JAR spatial-sdk-hive.jar;
    ```

    关于ADD命令的使用，更多信息请参见[添加资源](/intl.zh-CN/开发/常用命令/资源操作.md)。

2.  执行如下命令注册函数。

    ```
    CREATE FUNCTION ST_Aggr_ConvexHull  AS  'com.esri.hadoop.hive.ST_Aggr_ConvexHull' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Aggr_Intersection    AS  'com.esri.hadoop.hive.ST_Aggr_Intersection'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Aggr_Union   AS  'com.esri.hadoop.hive.ST_Aggr_Union'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Area AS  'com.esri.hadoop.hive.ST_Area'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_AsBinary AS  'com.esri.hadoop.hive.ST_AsBinary'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_AsGeoJson    AS  'com.esri.hadoop.hive.ST_AsGeoJson'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_AsJson   AS  'com.esri.hadoop.hive.ST_AsJson'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_AsShape  AS  'com.esri.hadoop.hive.ST_AsShape' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_AsText   AS  'com.esri.hadoop.hive.ST_AsText'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Bin  AS  'com.esri.hadoop.hive.ST_Bin' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_BinEnvelope  AS  'com.esri.hadoop.hive.ST_BinEnvelope' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Boundary AS  'com.esri.hadoop.hive.ST_Boundary'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Buffer   AS  'com.esri.hadoop.hive.ST_Buffer'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Centroid AS  'com.esri.hadoop.hive.ST_Centroid'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Contains AS  'com.esri.hadoop.hive.ST_Contains'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_ConvexHull   AS  'com.esri.hadoop.hive.ST_ConvexHull'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_CoordDim AS  'com.esri.hadoop.hive.ST_CoordDim'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Crosses  AS  'com.esri.hadoop.hive.ST_Crosses' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Difference   AS  'com.esri.hadoop.hive.ST_Difference'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Dimension    AS  'com.esri.hadoop.hive.ST_Dimension'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Disjoint AS  'com.esri.hadoop.hive.ST_Disjoint'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Distance AS  'com.esri.hadoop.hive.ST_Distance'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_EndPoint AS  'com.esri.hadoop.hive.ST_EndPoint'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Envelope AS  'com.esri.hadoop.hive.ST_Envelope'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_EnvIntersects    AS  'com.esri.hadoop.hive.ST_EnvIntersects'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Equals   AS  'com.esri.hadoop.hive.ST_Equals'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_ExteriorRing AS  'com.esri.hadoop.hive.ST_ExteriorRing'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_GeodesicLengthWGS84  AS  'com.esri.hadoop.hive.ST_GeodesicLengthWGS84' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_GeomCollection   AS  'com.esri.hadoop.hive.ST_GeomCollection'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Geometry AS  'com.esri.hadoop.hive.ST_Geometry'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_GeometryN    AS  'com.esri.hadoop.hive.ST_GeometryN'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_GeometryType AS  'com.esri.hadoop.hive.ST_GeometryType'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_GeomFromGeoJson  AS  'com.esri.hadoop.hive.ST_GeomFromGeoJson' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_GeomFromJson AS  'com.esri.hadoop.hive.ST_GeomFromJson'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_GeomFromShape    AS  'com.esri.hadoop.hive.ST_GeomFromShape'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_GeomFromText AS  'com.esri.hadoop.hive.ST_GeomFromText'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_GeomFromWKB  AS  'com.esri.hadoop.hive.ST_GeomFromWKB' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_InteriorRingN    AS  'com.esri.hadoop.hive.ST_InteriorRingN'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Intersection AS  'com.esri.hadoop.hive.ST_Intersection'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Intersects   AS  'com.esri.hadoop.hive.ST_Intersects'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Is3D AS  'com.esri.hadoop.hive.ST_Is3D'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_IsClosed AS  'com.esri.hadoop.hive.ST_IsClosed'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_IsEmpty  AS  'com.esri.hadoop.hive.ST_IsEmpty' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_IsMeasured   AS  'com.esri.hadoop.hive.ST_IsMeasured'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_IsRing   AS  'com.esri.hadoop.hive.ST_IsRing'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_IsSimple AS  'com.esri.hadoop.hive.ST_IsSimple'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Length   AS  'com.esri.hadoop.hive.ST_Length'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_LineFromWKB  AS  'com.esri.hadoop.hive.ST_LineFromWKB' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_LineString   AS  'com.esri.hadoop.hive.ST_LineString'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_M    AS  'com.esri.hadoop.hive.ST_M'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MaxM AS  'com.esri.hadoop.hive.ST_MaxM'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MaxX AS  'com.esri.hadoop.hive.ST_MaxX'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MaxY AS  'com.esri.hadoop.hive.ST_MaxY'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MaxZ AS  'com.esri.hadoop.hive.ST_MaxZ'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MinM AS  'com.esri.hadoop.hive.ST_MinM'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MinX AS  'com.esri.hadoop.hive.ST_MinX'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MinY AS  'com.esri.hadoop.hive.ST_MinY'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MinZ AS  'com.esri.hadoop.hive.ST_MinZ'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MLineFromWKB AS  'com.esri.hadoop.hive.ST_MLineFromWKB'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MPointFromWKB    AS  'com.esri.hadoop.hive.ST_MPointFromWKB'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MPolyFromWKB AS  'com.esri.hadoop.hive.ST_MPolyFromWKB'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MultiLineString  AS  'com.esri.hadoop.hive.ST_MultiLineString' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MultiPoint   AS  'com.esri.hadoop.hive.ST_MultiPoint'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_MultiPolygon AS  'com.esri.hadoop.hive.ST_MultiPolygon'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_NumGeometries    AS  'com.esri.hadoop.hive.ST_NumGeometries'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_NumInteriorRing  AS  'com.esri.hadoop.hive.ST_NumInteriorRing' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_NumPoints    AS  'com.esri.hadoop.hive.ST_NumPoints'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Overlaps AS  'com.esri.hadoop.hive.ST_Overlaps'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Point    AS  'com.esri.hadoop.hive.ST_Point'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_PointFromWKB AS  'com.esri.hadoop.hive.ST_PointFromWKB'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_PointN   AS  'com.esri.hadoop.hive.ST_PointN'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_PointZ   AS  'com.esri.hadoop.hive.ST_PointZ'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_PolyFromWKB  AS  'com.esri.hadoop.hive.ST_PolyFromWKB' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Polygon  AS  'com.esri.hadoop.hive.ST_Polygon' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Relate   AS  'com.esri.hadoop.hive.ST_Relate'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_SetSRID  AS  'com.esri.hadoop.hive.ST_SetSRID' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_SRID AS  'com.esri.hadoop.hive.ST_SRID'    USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_StartPoint   AS  'com.esri.hadoop.hive.ST_StartPoint'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_SymmetricDiff    AS  'com.esri.hadoop.hive.ST_SymmetricDiff'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Touches  AS  'com.esri.hadoop.hive.ST_Touches' USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Union    AS  'com.esri.hadoop.hive.ST_Union'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Within   AS  'com.esri.hadoop.hive.ST_Within'  USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_X    AS  'com.esri.hadoop.hive.ST_X'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Y    AS  'com.esri.hadoop.hive.ST_Y'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    CREATE FUNCTION ST_Z    AS  'com.esri.hadoop.hive.ST_Z'   USING 'spatial-sdk-hive.jar,esri-geometry-api.jar';
    ```


## 测试UDF

在MaxCompute客户端（odpscmd）中提交一个SQL语句来测试UDF是否可以正常使用。

```
--打开Hive兼容性并提交测试函数。
set odps.sql.hive.compatible=true;
SELECT ST_AsText(ST_Point(1, 2));
```

返回结果如下，说明安装成功。

```
+-----+
| _c0 |
+-----+
| POINT (1 2) |
+-----+
```

