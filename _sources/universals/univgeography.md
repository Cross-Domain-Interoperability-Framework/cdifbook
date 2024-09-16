# Geography
In order to support discovery and selection of datasets, we need to:
1. describe the spatial extent or footprint of a dataset (e.g. name, bounding box)
2. Say something about the spatial distribution and representation of values within a dataset (e.g. grid definition, point-cloud, precision, spacing)

Where not specifically indicated in CDIF, location should be expressed according to the pattern ( value , location-system or authority [ , time ] ), for example:
- (coordinate geometry, Coordinate Reference System (, date) )
- (placename, Gazetteer (, date) )
- (cell id, grid definition)

## Location systems
There are many different location systems, including:
- Coordinate-based location, used for
	- Point location
	- Bounding box
	- Polygon
- Addresses, delivery points, lot numbers
- Named places, points-of-interest
- Named areas:
	- Administrative areas (many ranks and functions)
	- Post-codes
	- Electoral districts
	- Statistical areas
- Grids, DGGS

Different applications use different systems, reflecting information requirements which often cannot be controlled by those managing or disseminating data (e.g., in the natural sciences you may find coordinates or grid-cells, where dissemination and implementation science might use coordinates or point-of-interest, social scientists and official statisticians would use named areas, statistical areas, or administrative areas, and utilities might use addresses or post-codes).

It is often best to describe locations using the systems employed by the creator of the data, leaving it up to the data integrator to perform needed translations, because the approach to harmonising locations can be driven by the methods employed for analysis and depend on the research question in a particular case. What we can do, however, is to make sure that needed information is present to unambiguously understand the locations as described. The following sections look at some of the common location systems in use.

## Coordinate-based location

### Coordinate values

For points, lines, and polygons (including bounding-boxes) coordinate-based locations are used, usually longitude and latitude, in decimal-degrees.

- It is important to pay attention to the coordinate sign: longitudes in the western hemisphere (west of the Greenwich meridian) are negative, and latitudes south of the equator are negative.
-  Coordinate order is also important: in traditional cartographic and navigation systems the order was (latitude,longitude), i.e. (y,x); however most modern digital systems follow the more common (x,y) order from maths and graphics i.e. (longitude, latitude).

With unfamiliar data, it is always worth checking both sign and coordinate order before doing any other data manipulations.

#### Coordinate reference system
It is essential to supply a coordinate reference system (usually by reference) with any set of coordinates; without this, they are easily misinterpretated and the errors can be in kiometres on the ground.

The most commonly used coordinate reference system (CRS) for geographic coordinates is [WGS 84](https://earth-info.nga.mil/index.php?dir=wgs84&action=wgs84). This is the default system used by GPS receivers, and most Web-mapping applications, so if the CRS is not explicit it can generally be assumed to be WGS 84. This is also the default for the common geometry representations, such as [GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) and the [Well Known Text Representation of Geometry (WKT)](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry). However, it should be noted that WGS 84 as a satellite based system is ignorant of change in the surface of the earth (e.g. tectonic drift) so if the data relates to phenomena on or near the surface of the earth, either provide a time stamp with the coordinates or use a terrestrial reference frame, e.g. ETRS-89 (EPSG::4258) for the European tectonic plate.

The definition of a CRS involves several pieces of information, including the axis directions and measurement units, the origin location where the value is (0,0), as well as the ellipsoid that is used to approximate the shape of the earth. Some CRS are three-dimensional, including elevation. The de facto authority for definitions of coordinate reference systems is [EPSG](https://epsg.org). Properly speaking, EPSG now refers to a set of products maintained by the International Association of Oil and Gas Producers (IOGP)'s Geoinformatics Committee. These products originated with the European Petroleum Survey Group (EPSG), which merged with the IOGP in 2005. They maintain a registry with a Web API at https://apps.epsg.org/api/swagger/ui/index. The EPSG is used by many mapping software systems, and other Web copies are available (e.g. https://epsg.io). WGS 84 is denoted EPSG:4326 and the full definition can be found at https://apps.epsg.org/api/v1/CoordRefSystem/4326/export/?format=wkt.

Some jurisdictions have legal requirements to use other coordinate reference systems for some applications for official purposes. For example, the British National Grid is standard in the UK, in which the coordinates are given in metres, and the origin is fixed so that all locations within the UK have positive coordinate values. The British National Grid is denoted EPSG:27700 and the full definition is found at https://epsg.org/crs_27700/OSGB36-British-National-Grid.html or https://epsg.io/27700.

## Shapes
Point-location is defined by a single set of coordinates. Areas are usually represented by their perimeter polygon. This is defined by an ordered sequence of points, where the last point should coincide with the first in order to close the polygon. The minimum and maximum values for an extensive location define a bounding-box. The orientation of the boundaries of a bounding box align to the CRS used to describe the corners of the box. For example, for coordinates specified using latitude and longitude, the bounding box is delimited by meridians and parallels.

GeoJSON and WKT provide specific encodings. DCAT provides the following predicates that relate spatial information to a dataset:
1. dcterms:spatial for the spatial extent of the data;
2. dcat:spatialResolutionInMeters for the spatial precision or spacing of items within a
dataset.

The Global Biodiversity Information Facility (GBIF) has a geo-referencing [best practice guide](https://doi.org/10.15468/doc-gg7h-s853) that covers latitude, longitude, altitude, and depth. This is a good resource for determining good practice.

## Identifier-based location
Nominal systems associate a name or code to some spatial location or region. Commonly used cases include:
- Countries;
- Administrative units (e.g. [NUTS - Nomenclature of Territorial Units for Statistics](https://en.wikipedia.org/wiki/Nomenclature_of_Territorial_Units_for_Statistics)): states, provinces, cities, local government areas);
- Statistical areas (defined by statistical agencies or census bureaus, e.g., the [Australian Statistical Geography](https://www.abs.gov.au/statistics/standards/australian-statistical-geography-standard-asgs-edition-3/jul2021-jun2026#asgs-diagram));
- Electoral districts (whose geographic footprint may change frequently so date is required);
- Postcodes.

### Reference system
In most - but not all - cases, there is a well-defined mapping of the name or code to a geospatial area. For names the list of mappings is called a gazetteer. The authority for this mapping may be well known, or may be more informal.
- Statutory naming authorities typically have a formal process for gazetting (publishing) geographic names.
- Postcodes are usually well-known and often convenient, however the exact mapping to space may be proprietary to a local postal delivery service.
- [GeoNames](https://www.geonames.org/v3/) is a crowd-sourced service that associates a point-location with many geographic names. The location used for arealy-extensive places can be inconsistent.

In many or most cases the mapping of a name to a geospatial area is time dependent. In some cases the area attached to a name is contested, for political and cultural reasons, or because of historical uncertainty. Contemporary name-based systems are generally managed nationally or locally. Historical systems are generally more difficult and there may not be an authoritative source. Geonames is a pretty good general-purpose service, but it only gives a point-location falling somewhere within the place.

## Grids
Where a dataset is composed of variables or properties whose value varies across a spatial domain, it is often represented as sampled discretely at locations on a regular grid. Discrete Global Grids (DGG) are an emerging alternative to cartesian grids.

### Reference system
The most common grid arrangement is specified in terms of an origin and axis directions (with respect to a CRS), and cell spacing. A location within the grid is then indicated by an integer index or count e.g. (234, 8916). Spatial grids may be one-, two-, three-, or four-, (spatio-temporal) dimensional.

### Usage
Spatial variation of a property across a spatial domain is commonly represented and exchanged using grids. Visualisations almost always use a cartesian (rectangular) gridded representation. Grids are the most common representation for spatial analysis and numerical modelling. Grids are convenient for data integration. However, they may need to be re-sampled or interpolated to a common orientation and spacing. Different DGGs are efficient for topology and proximity analysis, and area-based calculations.

## Addresses
Many different sources exist for the description of addresses. Among these are EPSG, ISO [19101-1:2014](https://www.iso.org/standard/59164.html) and [ISO 19107:2003](https://www.iso.org/obp/ui/#iso:std:iso:19107:ed-1:v1:en).
