# CDIF Discovery Profile — Schema.org Implementation

This page documents the mapping between CDIF content items and their schema.org implementation. Each item lists its obligation, JSON encoding, and a scope note explaining usage. This enables using standard JSON Schema to validate documents and avoids confusion about the vocabulary origin of keys used in the JSON.

### Metadata identifier
- **Obligation:** mandatory
- **JSON:** `"schema:subjectOf" / "@id": {URI}`
- **Scope note:** The URI for the metadata record should be the `@id` value for the `schema:subjectOf` node. This node has `@type ["schema:Dataset"]` with `schema:additionalType ["dcat:CatalogRecord"]`, and a `schema:about` property referencing the `@id` of the root resource node.

### Resource identifier
- **Obligation:** mandatory
- **JSON:** `"schema:identifier": {PropertyValue or string}`
- **Scope note:** The primary identifier for the resource. Can be a simple string (ideally a resolvable URI), or a `schema:PropertyValue` with `propertyID` (identifier scheme, e.g. from [identifiers.org](https://registry.identifiers.org/registry/)), `value` (the identifier string), and `url` (resolvable link). The PropertyValue approach is strongly recommended following the [ESIP Science on Schema.org guidance](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#identifier).

### Title

- **Obligation:** mandatory
- **JSON:** `"schema:name": {string}`
- **Scope note:** A set of words that should identify the described resource for human use. Ideally, should be unique in the scope of the metadata catalog containing this metadata record.

### Distribution

*Obligation: mandatory.* Either a `schema:url` or a `contentUrl` inside `schema:distribution` is required to indicate how to get the resource.

- **Landing page URL**
  - **JSON:** `"schema:url": {URL}`
  - **Scope note:** This URL is generally expected to GET an HTML landing page about the resource.
- **Distribution array**
  - **JSON:**
    ```json
    "schema:distribution": [
      {"@type": ["schema:DataDownload"], "schema:contentUrl": {URL}, ... },
      {"@type": ["schema:WebAPI"], "schema:serviceType": ..., ... }
    ]
    ```
  - **Scope note:** An array of distribution objects. Items may be DataDownload (file-based access) or WebAPI (service-based access). A DataDownload must include `schema:contentUrl`, and should include `schema:encodingFormat` and `dcterms:conformsTo`. The `@type` is encoded as an array (e.g. `["schema:DataDownload"]`).

### Rights

- **Obligation:** 1..*
- **JSON:** `"schema:license": [{text or URI or CreativeWork}, ...]` or `"schema:conditionsOfAccess": [{text or URI}, ...]`
- **Scope note:** At least one of `schema:license` or `schema:conditionsOfAccess` must be provided (as arrays). URL to license document or text explanation of restrictions on use. There might be multiple links to documents specifying related security, privacy, usage, sharing, etc. concerns.

### Metadata profile identifier

- **Obligation:** 1..*
- **JSON:**
  ```json
  "schema:subjectOf" / "dcterms:conformsTo": [
    {"@id": "https://w3id.org/cdif/core/1.0/"},
    {"@id": "https://w3id.org/cdif/discovery/1.0/"}
  ]
  ```
- **Scope note:** An array of objects, each with an `@id` property whose value is a conformance URI. For CDIFDiscovery, both the core and discovery URIs are required. Extended profiles add their own conformance URIs to this array.

### Metadata date

- **Obligation:** 0..1
- **JSON:** `"schema:subjectOf" / "schema:sdDatePublished": {Date}`
- **Scope note:** Use ISO 8601 format. The most recent publication date for the metadata content. Harvesters use this to determine if they have already harvested and processed this record.

### Metadata contact

- **Obligation:** 0..1
- **JSON:** `"schema:subjectOf" / "schema:maintainer": {Person or Organization}`
- **Scope note:** Should include a name and contact point (institutional e-mail is best) for the agent responsible for metadata content. This is the contact point to report problems with metadata content. Person and Organization are Agent objects with various properties.

### Metadata catalog

- **Obligation:** 0..1
- **JSON:**
  ```json
  "schema:subjectOf" / "schema:includedInDataCatalog": {
    "@type": "schema:DataCatalog",
    "schema:name": ...,
    "schema:url": ...
  }
  ```
- **Scope note:** Identifies the data catalog or repository containing this metadata record. Value is a `schema:DataCatalog` with at least a name and URL.

### Resource type

- **Primary type — `@type`**
  - **Obligation:** mandatory
  - **JSON:** `"@type": ["schema:Dataset", ...]`
  - **Scope note:** An array of schema.org type values using the `schema:` prefix. Must include `"schema:Dataset"`. Additional allowed types: `schema:CreativeWork`, `schema:SoftwareApplication`, `schema:SoftwareSourceCode`, `schema:Product`, `schema:WebAPI`, `schema:DigitalDocument`, `schema:Collection`, `schema:ImageObject`, `schema:DataCatalog`, `schema:DefinedTermSet`, `schema:MediaObject`.
- **Additional type — `schema:additionalType`**
  - **Obligation:** 0..*
  - **JSON:** `"schema:additionalType": [{DefinedTerm or string}, ...]`
  - **Scope note:** If a more specific resource type needs to be specified using a vocabulary other than schema.org, add a text or URI value here. Must be consistent with the `@type`. Always encode as an array.

### Description

- **Obligation:** 0..1
- **JSON:** `"schema:description": {string}`
- **Scope note:** Free text, with as much detail as is feasible.

### Originators

- **Obligation:** 0..*
- **JSON:** `"schema:creator": {"@list": [{Person or Organization}, ...]}`
- **Scope note:** Author or originator of intellectual content. Uses the JSON-LD `@list` construct to preserve author order. Each item can be a Person, Organization, or an object reference (`{"@id": "..."}`) to an agent defined elsewhere. Use ORCID or other PID to identify persons where possible.

### Publication Date

- **Obligation:** 0..1
- **JSON:** `"schema:datePublished": {date time}`
- **Scope note:** Date on which the resource was made publicly accessible. Use ISO 8601 format.

### Modification Date

- **Obligation:** mandatory
- **JSON:** `"schema:dateModified": {date time}`
- **Scope note:** Date of most recent update to resource content. If Publication Date is not provided, defaults to the Modification Date. Use ISO 8601 format.

### Other identifiers

- **Obligation:** 0..*
- **JSON:** `"schema:sameAs": [{URI or PropertyValue}, ...]`
- **Scope note:** Other identifiers for the same resource, as IRI reference strings, object references (`{"@id": "..."}`), or structured identifiers using `schema:PropertyValue`.

### Version

- **Obligation:** 0..1
- **JSON:** `"schema:version": {string or number}`
- **Scope note:** The version number or identifier for this resource. Values should sort from oldest to newest using an alphanumeric sort on version strings.

### Language

- **Obligation:** 0..1
- **JSON:** `"schema:inLanguage": {string}`
- **Scope note:** The language of the dataset content (e.g. `"en"`, `"fr"`).

### Measurement technique

- **Obligation:** 0..*
- **JSON:** `"schema:measurementTechnique": {string or DefinedTerm or array}`
- **Scope note:** The technique, technology, or methodology used for measurement or determination of the dataset values. Can be a string, a `DefinedTerm` with vocabulary reference, or an array combining these.

### Keyword

- **Obligation:** 0..*
- **JSON:**
  ```json
  "schema:keywords": [
    {string},
    {
      "@type": "schema:DefinedTerm",
      "schema:name": "OCEANS",
      "schema:inDefinedTermSet": "gcmd:sciencekeywords",
      "schema:identifier": {...}
    },
    ...
  ]
  ```
- **Scope note:** Implement with text for tags, and `schema:DefinedTerm` for keywords from a controlled vocabulary. The DefinedTerm approach is used to represent concepts with links to their defining vocabulary. Recommend using DefinedTerm for all keywords if any are from a known vocabulary.

## Geographic extent

Required if the resource has a geographic extent for its subject — a bounding rectangle, line, or point. To support cross-domain searches based on geospatial location, location coordinates must be given in decimal degrees using the WGS 84 datum. Other systems for describing location can be provided as alternate descriptions, recognizing that they may not be meaningful to some metadata harvesting agents. Spatial coverage is encoded as an array.

### Named place

- **Obligation:** 0..*
- **JSON:**
  ```json
  "schema:spatialCoverage": [{
    "@type": "schema:Place",
    "schema:name": {string} or {schema:DefinedTerm}
  }]
  ```
- **Scope note:** To specify location with place names. If the names are from a gazetteer, use the `schema:DefinedTerm` to provide a name, identifier, and `inDefinedTermSet` to fully document the concept.

### Bounding box

- **Obligation:** 0..1
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
- **Scope note:** For bounding-box specification of the spatial extent of resource content. See [ESIP SOSO for details](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#bounding-boxes). Recommend including only one bounding box; behavior of harvesting clients when multiple geometries are specified is unpredictable.

### Curvilinear trace

- **Obligation:** 0..1
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
- **Scope note:** For resources related to a linear trace like a ship track or airplane flight line.

### Point location

- **Obligation:** 0..1
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
- **Scope note:** For a point-location specification of the spatial extent of resource content. Recommend including only one point; behavior of harvesting clients when multiple geometries are specified is unpredictable.

### Other serialization

- **Obligation:** 0..*
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
- **Scope note:** Optional geographic extent using other more interoperable geometries. GeoSPARQL is recommended; see [Ocean InfoHub](https://book.oceaninfohub.org/thematics/spatial/README.html#simple-geosparql-wkt). Other geometry schemes might be specified in a specific domain profile, e.g. for atmospheric, subsurface data, or local coordinate systems.

## Distribution

### Distribution Agent

- **Single provider**
  - **Obligation:** 0..*
  - **JSON:** `"schema:provider": [{Person or Organization}, ...]`
  - **Scope note:** Contact point for the provider of a distribution. For a simple digital object with a download URL, or a resource with multiple distributions all from the same provider.
- **Per-distribution provider**
  - **Obligation:** 0..*
  - **JSON:** `"schema:distribution": [{"@type": ["schema:DataDownload"], "schema:provider": [{Person or Organization}]}, ...]`
  - **Scope note:** If there are multiple distributions with different providers, each distribution can have a separate provider array.

## Variables in the data

The metadata about a dataset should include a list of variables that the dataset contains. Variable metadata should minimally specify the name of the variable as it appears in the dataset. That name should be qualified by a controlled vocabulary or other semantic resource (e.g. represented by a resolvable URI), or minimally some descriptive text.

### Variable (PropertyValue)

- **Obligation:** 0..*
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
- **Scope note:** Follow the [ESIP Science on Schema.org recommendation](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#variables); see also discussion for representing more complex data structures in [ESIP Experimental](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Experimental.md#AdvancedVariableValueType) and the [Data Integration module of CDIF](https://cross-domain-interoperability-framework.github.io/cdifbook/data_integration/ddidescriptiondatastructure.html). Variable must have a name and description, should have a `propertyID` with URI for the represented concept. The URI in the `propertyID` provides the semantic linkage for the meaning of the variable.

### Variable (StatisticalVariable)

- **Obligation:** 0..*
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
- **Scope note:** `StatisticalVariable` offers properties useful for describing social-science statistical variables like `populationType` and `statType`. Use of `StatisticalVariable` is preferred for variables with values calculated from some aggregation process.

### Temporal coverage

*Obligation: 0..*.* Temporal coverage is encoded as an array. It can be expressed in several ways: a calendar/clock dateTime or date-time interval using ISO 8601 serialization, a named time-ordinal era, an interval bounded by time-ordinal eras, or with a numeric coordinate in a temporal reference system.

- **Calendar date / clock time instant**
  - **JSON:** `"schema:temporalCoverage": ["2018-01-22"]`
  - **Scope note:** Calendar date or clock time instant using ISO 8601 encoding.
- **Calendar date / clock time interval**
  - **JSON:** `"schema:temporalCoverage": ["2012-09-20/2016-01-22"]`
  - **Scope note:** Calendar date or clock time interval using ISO 8601 encoding.
- **Time ordinal era interval**
  - **JSON:**
    ```json
    "schema:temporalCoverage": [{
      "@type": "time:ProperInterval",
      "time:intervalStartedBy": "isc:LowerDevonian",
      "time:intervalFinishedBy": "isc:LowerPermian"
    }]
    ```
  - **Scope note:** Time-ordinal era interval, using the `owl:time` namespace (`time: http://www.w3.org/2006/time#`). This example uses the [International Chronostratigraphic Chart (isc)](http://resource.geosciml.org/classifier/ics/ischart/). See [PeriodO](https://perio.do/en/) for identifiers for many other named time intervals.
- **Geologic age interval (abbreviated form)**
  - **JSON:** `"schema:temporalCoverage": [{"time:ProperInterval-345/298 Ma"}]`
  - **Scope note:** For time intervals specified using geologic ages, in Ka, Ma, or Ga. The text string is an abbreviated `owl:time` interval (proposal, under discussion).

### Related agents (contributor role)

- **Obligation:** 0..*
- **JSON:** `"schema:contributor": [{Person or Organization}, ...]`
- **Scope note:** Recognition for others who have contributed to the production of the resource but are not recognized as authors/creators.

### Related agent (other role)

- **Obligation:** 0..*
- **JSON:**
  ```json
  "schema:contributor": [{
    "@type": "schema:Role",
    "schema:roleName": "Principal Investigator",
    "schema:contributor": {
      "@type": "schema:Person",
      "@id": "https://orcid.org/...",
      "schema:name": "John Doe",
      "schema:affiliation": {
        "@type": "schema:Organization",
        "@id": "https://ror.org/...",
        "schema:name": "..."
      },
      "schema:contactPoint": {
        "@type": "schema:ContactPoint",
        "schema:email": "john.doe@example.org"
      }
    }
  }]
  ```
- **Scope note:** To assign roles to contributors like editor, maintainer, publisher, point of contact, copyright holder (e.g. DataCite contributor types), use the [role construction defined by schema.org](http://blog.schema.org/2014/06/introducing-role.html).

### Related resources

- **Obligation:** 0..*
- **JSON:**
  ```json
  "schema:relatedLink": [{
    "@type": "schema:LinkRole",
    "schema:linkRelationship": "...",
    "schema:target": {
      "@type": "schema:EntryPoint",
      "schema:encodingFormat": "text/html",
      "schema:name": "...",
      "schema:url": "https://example.org/data/stations"
    }
  }]
  ```
- **Scope note:** Use schema.org `relatedLink` with a `LinkRole` value, and the link URL in a `target` EntryPoint object. These properties expect WebPage and Action as their domain, so the [schema.org validator](https://validator.schema.org/) will throw a warning (not an error). Related-resource links are useful for evaluation and use of data, but because of the wide variety of relationship possibilities they are difficult to use in general search scenarios. Use a soft-type implementation, with a link-relationship type using a `schema:DefinedTerm`, and a resolvable identifier for the relationship target.

### Funding

- **Obligation:** 0..*
- **JSON:**
  ```json
  "schema:funding": [{
    "@type": "schema:MonetaryGrant",
    "schema:identifier": {
      "@type": "schema:PropertyValue",
      "schema:propertyID": "grant-id",
      "schema:value": "..."
    },
    "schema:name": "grant title",
    "schema:funder": {
      "@id": "https://ror.org/...",
      "@type": "schema:Organization",
      "schema:name": "org name"
    }
  }]
  ```
- **Scope note:** Use schema.org encoding and the [Science on Schema.org pattern](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#funding). Other organization properties can be included in the funder Organization.

### Policies

- **Obligation:** 0..*
- **JSON:**
  ```json
  "schema:publishingPrinciples": [{
    "@type": "schema:CreativeWork",
    "schema:name": "...",
    "schema:url": "..."
  }, ...]
  ```
- **Scope note:** FDOF `digitalObjectMutability`, RDA `digitalObjectPolicy`, FDOF `PersistencyPolicy`. Policies related to maintenance, update, and expected time to live.

### Checksum

- **Obligation:** 0..1
- **JSON:**
  ```json
  "schema:distribution": [{
    "@type": ["schema:DataDownload"],
    "spdx:checksum": {
      "@type": "spdx:Checksum",
      "spdx:algorithm": "SHA256",
      "spdx:checksumValue": "abc123..."
    },
    ...
  }, ...]
  ```
- **Scope note:** A string value calculated from the content of the resource representation, used to test if content has been modified. No schema.org property; follow DCAT v3 adoption of the [Software Package Data Exchange (SPDX)](https://spdx.org/rdf/terms/) property. The `spdx:Checksum` object has two properties: `algorithm` and `checksumValue`. The checksum is a property of each distribution / DataDownload.

## Provenance for discovery

Provenance for discovery is limited to documenting technology used in the creation of the dataset and documenting other datasets that were inputs to the content of the described resource. The cdifDiscovery profile specifies only that `prov:wasGeneratedBy` has a `prov:Activity` with `prov:used` items that are strings or `@id` references. Any additional structure under `prov:used` is optional and defined by extended profiles.

### Provenance (instruments, software, etc.)

- **Obligation:** 0..*
- **JSON:**
  ```json
  "prov:wasGeneratedBy": [{
    "@type": ["prov:Activity"],
    "prov:used": [
      "nerc:collection/L05/current/134",
      {"@id": "nerc:collection/B76/current/B7600031"}
    ]
  }]
  ```
- **Scope note:** Identify sensors, instruments, platforms, software, algorithms, etc. used in the creation of the described resource. The `prov:used` array accepts strings (URIs or labels) or object references with `@id`.

### Provenance (input datasets)

- **Obligation:** 0..*
- **JSON:**
  ```json
  "prov:wasDerivedFrom": [
    "http://doi.org/10.547/347848",
    {"@id": "http://doi.org/10.3578/h5ls"},
    {"@type": "schema:CreativeWork", "schema:name": "...", "schema:url": "..."}
  ]
  ```
- **Scope note:** Identify datasets that were inputs to the content of the described resource. Items can be strings (URIs), object references, or CreativeWork objects with name and URL.

## Quality information for discovery

A text statement documenting quality of the resource should be included in `schema:description`. If there are quality policies or certificates that apply, these should be specified in `schema:publishingPrinciples`. Quality measurements or assessment protocols that have an output result specific to this resource can be specified using `dqv:hasQualityMeasurement`.

### Quality measure

- **Obligation:** 0..*
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
- **Scope note:** Quality assessment or measurement conducted using the procedure or protocol specified by the `dqv:isMeasurementOf` property, with the result value specified in the `dqv:value` property. The result might be numeric, a categorical term, or a link to a document describing the quality assessment.