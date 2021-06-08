# Geographic Coordinate System
Geographic coordinate system associates with positions on the earth. The positions can be represented in different cooridnate systems. Commonly, they are:
- Geographic coordinate system; using 3-D shperical surface in latitude, longitude and elevation
- Projected coordinate system; using flat, 2-D surface in x, y and elevation

## Geographic Cooridnate System
Geographic cooridnate system uses geodetic datum as a reference ellipsoid with a given origin and orientation to map the coordinate system. The datum may be global or local. 
One of the most widely used global datum is:
- World Geodetic System 1984 (WGS84) using the WGS1984 spheroid

Note that datum may be used interchangeably with geographic coordinate system. 

A point in geographic cooridnate system is referenced by its latitude and longitude which are angles measured from the centre of the earth. Both latitude and longitude
are measured from the equator (midway between the poles) and the prime merdian (longitude passing Greenwich, England). The network of these lines called graticule and
the origin of the graticule is where the equator and the prime merdian intersect. 

## Projected Coordinate System
Projected coordinate system is mapped based on the geographic coordinate system. And it has x-coordinate and y-coordinate that represents a position relative to the 
centre location (0, 0). 

One common way of projecting a datum into a flat, 2-D coordinate system is by using transverse mercator. Transverse mercator projection can refer to an unrolled cyclinder,
which intersects the earth along the prime merdian, to make a flat map. As the spherical earth is mapped into a flat projection, distortion is inevitable. The most widely
used projected coordinate system is:
- Universal Transverse Mercator (UTM); divides the earth into 60 zones, each 6 degrees of longitude in width. For its datum, WGS84 is generally used. In using UTM, it is 
important to refer to its UTM zone. 

## Geometry vs Geography
- Geography assumes that data is on the earth’s surface, as specified by latitudes and longitude (Geographic coordinate system)
- Geometry assumes data is on a Cartesian plane, as specified by x-coordinate and y-coordinate (Projected coordinate system)

In SQL DB, spatial data can be stored as either geometry and geography data types. Geography results in higher accuracy, yet, geometry brings better performance. 

For processing spatial data in SQL Server, refer to the [documentation](https://docs.microsoft.com/en-us/sql/relational-databases/spatial/spatial-data-sql-server?view=sql-server-ver15).

## Important Parameters
As geographic position can be represented in many differeny ways, it is important to know the followings when using GIS data:
- Geographic coordinate system (datum)
- Unit of measure
- Zone 
- Projection
- Projection parameters

## Coordinate Conversion between WGS84 and UTM
To be updated

## Well-Known Text (WKT)
Well-known text (WKT) is a text markup language for representing vector geometry/geography objects. It can represent the following geomteric/geographic objects:
- Point, MultiPoint
- LineString, MultiLineString
- Polygon, MultiPolygon
- etc.

Coordinates are wraped in the following manner to represent an object:
- Point: POINT (30 10)
- LineString: LINESTRING (30 10, 10 30, 40 40)
- Polygon: POLYGON ((30 10, 40 40, 20 40, 10 20, 30 10))
- MultiPoint:	MULTIPOINT ((10 40), (40 30), (20 20), (30 10))
- MultiLineString: MULTILINESTRING ((10 10, 20 20, 10 40), (40 40, 30 30, 40 20, 30 10))
- MultiPolygon: MULTIPOLYGON (((30 20, 45 40, 10 40, 30 20)), ((15 5, 40 10, 10 20, 5 10, 15 5)))

Note that both geometry and geography formats can be used for WKT. 
