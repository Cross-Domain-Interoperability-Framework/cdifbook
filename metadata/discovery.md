# Discovery Profile

Resources: 
- [Structured JSON schema](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/blob/reviewRevision202606/cdifDiscoveryStructuredSchema.json)
- [Implementation guide](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/blob/reviewRevision202606/CDIFDiscoveryImplementationGuide.md)
- [SHACL rules](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/blob/reviewRevision202606/discoveryRules.shacl)
- [JSON-LD framing](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/blob/reviewRevision202606/cdifDiscovery-frame.jsonld)
- [Example instance files](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/tree/reviewRevision202606/examples)
- [Graphical view](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFDiscovery/index.html)

The Discovery profile defines properties to document the spatial or temporal extent of the resource content or subject, and to document variables that are specified in a structured dataset. These properties are not included in core based on the observation that the information is not necessarily applicable to any kind of resource. 

See also [graphical presentation of Discovery Profile](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFDiscovery/index.html)

Artefacts for the Discovery profile are in this [Github repository](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/tree/reviewRevision202606) (TBD--update link to release tag)

## Core elements
See [Core](core.md)

## Discovery metadata extension requirements
- **Geographic Extent** - (0..many)  Required if resource has a geographic extent for its subject, either a named location, bounding rectangle, linear trace, or point. To support cross-domain searches based on geospatial location, location coordinates must be given in decimal degrees using the WGS 84 datum. There are various other systems for describing location (see [Space](../universals/univgeography.md) ); these can be provided as alternate location descriptions, recognizing that they might be meaningful to some metadata harvesting agents. Some resources might not be usefully described by a WGS 84 extent, in which case indicate nil:notapplicable; this would include extraterrestrial resources, but named location can still be provided.
  - *Bounding Rectangle*: North Bounding Latitude, South Bounding Latitude, East Bounding Longitude, West Bounding Longitude. The minimum rectangle that completely contains the coverage extent for the resource content. Coordinate order and syntax are determined by the serialisation profile.
  - *Linear trace*: a linear trace e.g. of a ship's track, aircraft flight path, or surface traverse, represented as a series of points. Coordinate order and syntax are determined by the serialisation profile.
  - *Point*: Latitude, Longitude. A centroid point for the coverage extent of the resource, or the location of the resource content if a point location is appropriate. Coordinate order and syntax are determined by the serialisation profile.
  - *Named location*: Place name referenced to some gazetteer. Use scoped name pattern {label, authority, optional identifier}.
- **Temporal Coverage** (0..1 entry) Required if resource content is specific to some time interval. The time interval represented by or the subject of the described resource. This could be the time interval when data were collected, or an archaeological or geological time interval that is the subject of the resource. Need to account for clock time, calendar time (Gregorian, Julian, Hebrew, Islamic, Chinese, Mayan...), cyclical time (summer, first quarter, mating season, new moon, pay day) and for named time ordinal eras (Jurassic, Younger Dryas, Early Minoan I, Late Stone Age). See [OWL Time](https://www.w3.org/TR/owl-time/).
- **Variable** (0 to many entries): Required for datasets. The metadata about a dataset should include a list of variables that the dataset contains. Variable metadata should minimally specify the name of the variable as it appears in the dataset. That name should be, ideally, qualified by a controlled vocabulary or other semantic resource (e.g. represented by a resolvable URI), or minimally some descriptive text. Variable metadata should include as much content as needed for users to understand the type of the variable (e.g. measured, statistically derived, or simulated), its units, and any relevant reference systems for its values (see [Universals](../universals/univintro.md) ). Details of data structure and schema more closely related to interoperability, data integration, and usage than to data discovery are discussed in the [Data Description](../data_description/datadescriptionprofile.md) profile.
- **Measurement technique** (0..many) identifiers or names for measurement method used to acquire data.
- **Quality** (0..many) Provide statements about the quality of information in the described resource,  information about quality policies or certificates that apply to the resource, and results of quality measures with information about the measurement protocol/procedure used. In all cases the focus should be on information useful for initial assessment by potential users.


## Implementation of Discovery Extensions
Instance of the Discovery profile must conform to the requirements of the [core profile](core.md). The discovery profile adds these additional properties:

## Variables in the data
The metadata about a dataset should include a list of variables that the dataset contains. Variable metadata should minimally specify the name of the variable as it appears in the dataset. That name should be qualified by a controlled vocabulary or other semantic resource (e.g. represented by a resolvable URI), or minimally some descriptive text.

### Variable (PropertyValue)
- **Cardinality:** 0..*
- **JSON:**
  ```json
  "schema:variableMeasured": [{
    "@type": ["schema:PropertyValue"],
    "@id": "astm:var0011",
    "schema:propertyID": [
      "pato:PATO_0000025",
      "astm:prop/0405"
    ],
    "schema:name": "hostMineral",
    "schema:description": "..."
  }, ...]
  ```
- **Description:** Follow the [ESIP Science on Schema.org recommendation](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#variables); see also discussion for representing more complex data structures in [ESIP Experimental](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Experimental.md#AdvancedVariableValueType) and the [Data Integration module of CDIF](https://cross-domain-interoperability-framework.github.io/cdifbook/data_integration/ddidescriptiondatastructure.html). Variable must have a name and description, should have a `propertyID` with URI for the represented concept. The URI in the `propertyID` provides the semantic linkage for the meaning of the variable.

### Variable (StatisticalVariable)
- **Cardinality:** 0..*
- **JSON:**
  ```json
  "schema:variableMeasured": [{
    "@type": ["schema:StatisticalVariable"],
    "@id": "astm:var0011",
    "schema:measuredProperty": {
      "@type": "schema:Property",
      "schema:identifier": "astm:id/305978",
      "schema:name": "Average age"
    }
  }]
  ```
- **Description:** `StatisticalVariable` offers properties useful for describing social-science statistical variables like `populationType` and `statType`. Use of `StatisticalVariable` is preferred for variables with values calculated from some aggregation process.

### Temporal coverage
Temporal coverage is encoded as an array. It can be expressed in several ways: a calendar/clock dateTime or date-time interval using ISO 8601 serialization, a named time-ordinal era, an interval bounded by time-ordinal eras, or with a numeric coordinate in a temporal reference system.
- **Cardinality:** 0..*
### *Calendar date / clock time instant*
  - **JSON:**  `"schema:temporalCoverage": ["2018-01-22"]`
  - **Description:** Calendar date or clock time instant using ISO 8601 encoding.
### *Calendar date / clock time interval*
  - **JSON:**`"schema:temporalCoverage": ["2012-09-20/2016-01-22"]`
  - **Description:** Calendar date or clock time interval using ISO 8601 encoding.
### *Time ordinal era interval*
  - **JSON:**
    ```json
    "schema:temporalCoverage": [{
      "@type": "time:ProperInterval",
      "time:intervalStartedBy": "isc:LowerDevonian",
      "time:intervalFinishedBy": "isc:LowerPermian"
    }]
    ```
   - **Description:** Time-ordinal era interval, using the `owl:time` namespace (`time: http://www.w3.org/2006/time#`). This example uses the [International Chronostratigraphic Chart (isc)](http://resource.geosciml.org/classifier/ics/ischart/). See [PeriodO](https://perio.do/en/) for identifiers for many other named time intervals.
### *Geologic age interval (abbreviated form)*
  - **JSON:**`"schema:temporalCoverage": [{"time:ProperInterval-345/298 Ma"}]`
  - **Description:** For time intervals specified using geologic ages, in Ka, Ma, or Ga. The text string is an abbreviated `owl:time` interval (proposal, under discussion).
  
## Geographic extent
Required if the resource has a geographic extent for its subject — a bounding rectangle, line, or point. To support cross-domain searches based on geospatial location, location coordinates must be given in decimal degrees using the WGS 84 datum. Other systems for describing location can be provided as alternate descriptions, recognizing that they may not be meaningful to some metadata harvesting agents. Spatial coverage is encoded as an array.

### *Named place*
- **Cardinality:** 0..*
- **JSON:**
```json
  "schema:spatialCoverage": [{
    "@type": "schema:Place",
    "schema:name": {string} or {schema:DefinedTerm}
  }]
```
- **Description:** To specify location with place names. If the names are from a gazetteer, use the `schema:DefinedTerm` to provide a name, identifier, and `inDefinedTermSet` to fully document the concept.

### *Bounding box*
- **Cardinality:** 0..1
- **JSON:**
  ```json
  "schema:spatialCoverage": [{
    "@type": "schema:Place",
    "schema:geo": {
      "@type": "schema:GeoShape",
      "schema:box": "39.3280 120.1633 40.445 123.7878"
    }
  }]
  ```
- **Description:** For bounding-box specification of the spatial extent of resource content. See [ESIP SOSO for details](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#bounding-boxes). Recommend including only one bounding box; behavior of harvesting clients when multiple geometries are specified is unpredictable.

### *Curvilinear trace*
- **Cardinality:** 0..1
- **JSON:**
  ```json
  "schema:spatialCoverage": [{
    "@type": "schema:Place",
    "schema:geo": {
      "@type": "schema:GeoShape",
      "schema:line": "39.33 120.77 40.44 123.96 41.00 121.34"
    }
  }]
  ```
- **Description:** For resources related to a linear trace like a ship track or airplane flight line.

### *Point location*
- **Cardinality:** 0..1
- **JSON:**
  ```json
  "schema:spatialCoverage": [{
    "@type": "schema:Place",
    "schema:geo": {
      "@type": "schema:GeoCoordinates",
      "schema:latitude": 39.3280,
      "schema:longitude": 120.1633
    }
  }]
  ```
- **Description:** For a point-location specification of the spatial extent of resource content. Recommend including only one point; behavior of harvesting clients when multiple geometries are specified is unpredictable.

### *Other serialization*
- **Cardinality:** 0..*
- **JSON:**
  ```json
  "geosparql:hasGeometry": {
    "@type": "sf:Point",
    "geosparql:asWKT": {
      "@type": "geosparql:wktLiteral",
      "@value": "POINT(-76 -18)"
    },
    "geosparql:crs": {"@id": "http://www.opengis.net/def/crs/OGC/1.3/CRS84"}
  }
  ```
- **Description:** Optional geographic extent using other more interoperable geometries. GeoSPARQL is recommended; see [Ocean InfoHub](https://book.oceaninfohub.org/thematics/spatial/README.html#simple-geosparql-wkt). Other geometry schemes might be specified in a specific domain profile, e.g. for atmospheric, subsurface data, or local coordinate systems.

## Quality information for discovery
A text statement documenting quality of the resource should be included in `schema:description`. If there are quality policies or certificates that apply, these should be specified in `schema:publishingPrinciples`. Quality measurements or assessment protocols that have an output result specific to this resource can be specified using `dqv:hasQualityMeasurement`.

- **Cardinality:** 0..*
- **JSON:**
  ```json
  "dqv:hasQualityMeasurement": [{
    "@type": "dqv:QualityMeasurement",
    "dqv:isMeasurementOf": "nerc:collection/L27/current/ARGO_QC",
    "dqv:value": "good"
  }, {
    "@type": "dqv:QualityMeasurement",
    "dqv:isMeasurementOf": "imf:dsbb/2003/eng/dqaf.htm",
    "dqv:value": "http://linkToASpecificQualityReport"
  }]
  ```
- **Description:** Quality assessment or measurement conducted using the procedure or protocol specified by the `dqv:isMeasurementOf` property, with the result value specified in the `dqv:value` property. The result might be numeric, a categorical term, or a link to a document describing the quality assessment.

## Measuement technique
- **Cardinality:** 0..*
- **JSON:** string or 
  ```
  "schema:measurementTechnique": [
    {
      "@type": ["schema:DefinedTerm"],
      "schema:name": "{string}",
      "schema:identifier": "{URI}",
      "schema:inDefinedTermSet": "{URI}",
      "schema:termCode": "{string}"
    }
  ],
  ```
- **Description:** A string or schema:DefinedTerm that specifies how the data were acquired.