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

## Python Script for Conversion from UTM to WGS84

    import math
    # define function to convert from UTM to WGS84
    def utm_to_wgs84(easting, northing, utm_zone=55, is_north=False):
        # UTM cooridnate validation
        if (easting < 160000.0) or (easting > 840000.0):
            print('Outside permissible range of easting values.')
        elif (northing < 0.0):
            print('Negative values are not allowed for northing.')
        elif (northing > 10000000.0):
            print('Northing may not exceed 10,000,000.')

        # symbols as used in USGS PP 1395: Map Projections - A Working Manual
        DatumEqRad = [
        6378137.0, 6378137.0, 6378137.0, 6378135.0, 6378160.0, 6378245.0, 6378206.4,
        6378388.0, 6378388.0, 6378249.1, 6378206.4, 6377563.4, 6377397.2, 6377276.3
        ]
        DatumFlat = [
        298.2572236, 298.2572236, 298.2572215, 298.2597208, 298.2497323,
        298.2997381, 294.9786982, 296.9993621, 296.9993621, 293.4660167,
        294.9786982, 299.3247788, 299.1527052, 300.8021499
        ]

        Item = 0                              # default
        a = DatumEqRad[Item]                  # equatorial radius (meters)
        f = 1 / DatumFlat[Item]               # polar flattening
        drad = math.pi / 180                  # convert degrees to radians
        k0 = 0.9996                           # scale on central meridian
        b = a * (1 - f)                       # polar axis
        e = math.sqrt(1 - (b / a) * (b / a))  # eccentricity
        esq = (1 - (b / a) * (b / a))         # e² for use in expansions
        e0sq = e * e / (1 - e * e)            # e0² — always even powers

        # lat long calculation
        zcm = 3 + 6 * (utm_zone - 1) - 180  # central meridian of zone
        e1 = (1 - math.sqrt(1 - e * e)) / (1 + math.sqrt(1 - e * e))
        M0 = 0  # in case origin other than zero lat

        # arc length along standard meridian
        if is_north:
            M = M0 + northing / k0
        else:
            M = M0 + (northing - 10000000) / k0
        mu = M / (a * (1 - esq * (1 / 4 + esq * (3 / 64 + 5 * esq / 256))))
        phi1 = mu + e1 * (3 / 2 - 27 * e1 * e1 / 32) * math.sin(2 * mu) + e1 * e1 * (21 / 16 - 55 * e1 * e1 / 32) * math.sin(4 * mu)
        phi1 = phi1 + e1 * e1 * e1 * (math.sin(6 * mu) * 151 / 96 + e1 * math.sin(8 * mu) * 1097 / 512)
        C1 = e0sq * math.pow(math.cos(phi1), 2)
        T1 = math.pow(math.tan(phi1), 2)
        N1 = a / math.sqrt(1 - math.pow(e * math.sin(phi1), 2))
        R1 = N1 * (1 - e * e) / (1 - math.pow(e * math.sin(phi1), 2))
        D = (easting - 500000) / (N1 * k0)
        phi = (D * D) * (1 / 2 - D * D * (5 + 3 * T1 + 10 * C1 - 4 * C1 * C1 - 9 * e0sq) / 24)
        phi = phi + math.pow(D, 6) * (61 + 90 * T1 + 298 * C1 + 45 * T1 * T1 - 252 * e0sq - 3 * C1 * C1) / 720
        phi = phi1 - (N1 * math.tan(phi1) / R1) * phi
        lon = D * (1 + D * D * ((-1 - 2 * T1 - C1) / 6 + D * D * (5 - 2 * C1 + 28 * T1 - 3 * C1 * C1 + 8 * e0sq + 24 * T1 * T1) / 120)) / math.cos(phi1)

        lat = phi / drad
        long = zcm + lon / drad
        return(lat, long)
