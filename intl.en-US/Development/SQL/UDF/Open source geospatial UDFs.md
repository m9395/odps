---
keyword: open source geospatial UDF
---

# Open source geospatial UDFs

This topic describes how to use open source geospatial user-defined functions \(UDFs\) for spatial data analysis.

Apache Hive provides a set of open source geospatial UDFs. For more information, visit [GitHub](https://github.com/Esri/spatial-framework-for-hadoop). MaxCompute provides native support for Hive UDFs. Therefore, you can use Hive geospatial functions in MaxCompute.

For more information about how to use Hive UDFs in MaxCompute, see [Hive UDF compatibility example](/intl.en-US/Development/SQL/UDF/Java UDF.md).

**Note:** If you encounter any issues when you use UDFs, submit your issues on GitHub for help.

## Prepare local functions

1.  In the **Command Prompt**, run the following command to download the geospatial UDFs for Hive 2.1.0 \(corresponding to Hadoop 2.7.2\) to your local device:

    ```
    git clone -b "v2.1.0" --single-branch git@github.com:Esri/spatial-framework-for-hadoop.git
    ```

2.  Use Maven to create a project.

    ```
    cd spatial-framework-for-hadoop
    mvn clean package -DskipTests -P java-8,hadoop-2.7,hive-2.1
    ```

3.  Copy the created JAR package. This JAR package contains all methods of the open source geospatial UDFs.

    ```
    cd ..
    cp spatial-framework-for-hadoop/hive/target/spatial-sdk-hive-2.1.1-SNAPSHOT.jar ./spatial-sdk-hive.jar
    ```

4.  Run the following command to download the JAR packages that the project depends on:

    ```
    wget https://repo1.maven.org/maven2/com/esri/geometry/esri-geometry-api/2.2.0/esri-geometry-api-2.2.0.jar ./esri-geometry-api.jar
    -- The preceding command is equivalent to the following command:
    cp ~/.m2/repository/com/esri/geometry/esri-geometry-api/2.2.0/esri-geometry-api-2.2.0.jar ./esri-geometry-api.jar
    ```


## Register functions in MaxCompute

1.  On the MaxCompute [client](/intl.en-US/Tools and Downloads/Client.md), run the following commands to upload the two dependent JAR packages to the target project:

    ```
    ADD JAR esri-geometry-api.jar;
    ADD JAR spatial-sdk-hive.jar;
    ```

    For more information about how to use the ADD command, see [Add a resource](/intl.en-US/Development/Common commands/Resource operations.md).

2.  Run the following commands to register the functions:

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


## Test a UDF

Submit an SQL statement on the MaxCompute client \(odpscmd\) to test whether a UDF functions properly.

```
-- Enable Hive compatibility and submit a UDF for testing.
set odps.sql.hive.compatible=true;
SELECT ST_AsText(ST_Point(1, 2));
```

The following response indicates that the UDF package is installed:

```
+-----+
| _c0 |
+-----+
| POINT (1 2) |
+-----+
```

