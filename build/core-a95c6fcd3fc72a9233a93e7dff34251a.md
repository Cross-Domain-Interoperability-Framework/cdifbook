# Core

The core of the Cross Domain Interoperability Framerwork is a set of implementation-independent content that must be specified in any CDIF-conformant metadata. This core set is supplemented by a more extensive set of metadata properties that are expected to apply to any information resource of interest, but are optional in the model. These optional properties might not be applicable in some situations or, more commonly, are unknown, not available, or not provide for some reason. 

 This recommendation is a synthesis of various metadata schemes, including ISO 19115-1:2014, schema.org conventions from [ESIPFed Science on Schema.org]() and Ocean Data net, DCAT, DCAT-AP, and [FDO Kernel Attributes-2.0](https://docs.google.com/document/d/1OF49wTNVuv-6OXlNerhBTqVtHyc7jutTaUHjn6BZCs0). These core content requirements are scoped for a broad spectrum of resource types; other fields will be added in the CDIF extension profiles.

## Information Model

 ### Required
If the content of a required element does not provide useful information, the metadata is considered useless for even the most rudimentary discovery use cases. Conformant metadata MUST provide valid values: an identifier for the described resource, a meaningful title that identifies the resource, either a URL or Distribution object (details later) that enables access to the resource, a statement of any licensing, usage, or access constraints (i.e., Rights),  an identifier for the type of resource described in the metadata, and identifier(s) for the specification of the metadata serialisation.

- **Resource identifier** (1 entry): A globally unique, resolvable identifier for the resource described by the metadata record.
- **Title** (1 entry): Succinct (preferably &lt;250 characters) name of the resource; should be sufficient to uniquely identify the resource for a human user.
- **Distribution**: URL, Distribution object, or Access Instructions (1 entry): There are several options. If the resource is a single digital object accessible online, provide a URL that will retrieve the resource. If the resource has multiple representations, or to provide users more information about the resource representation, a Distribution Object should be used to document the various possible representations and component files with a URL  for each. Metadata for distributions through an API that allows query, filter, or processing as part of a data access request are described in the Queryable Distribution Interfaces (API) section, below. If the resource is not accessible online, provide a URL to a landing page that describes how to access the resource.
- **Rights** (1 to many entry): Information about required access permissions, licences, contractual requirements, use constraints, and security constraints. Might be described in text or through links to external documents. 
- **Resource type** (1 to many): A scoped name (label with classification scheme) that specifies the kind of resource described by the metadata. The resource type might be used to determine validation requirements specific to descriptions for that kind of resource.
- **Metadata profile identifier** (1 to many): Identifier for metadata specification (profile) used to create this metadata record. Generally this will be populated automatically if the metadata is created using CDIF aware tools.

### Recommended
Other properties that should be specified if possible and relevant. All are optional.
- **Description** (0..1 entry): Inform users about the resource's content, context, provenance, and any other information deemed useful for future cross-domain usage. 
- **Originators** (0 to many entries): One or more parties (person or organisation) that have a role related to the origin of the resource, e.g., author or editor. Each party has a name (label), identifier, and optional contact information. 
- **Modified Date** (0..1 entry): Date (not temporal extent) when the most recent changes to the resource were completed. Use [ISO 8601 date and time](https://en.wikipedia.org/wiki/ISO_8601) format. Alternative date formatting must be machine-readable and consistent across all datasets. 
- **Distribution Agent** (0..1 entry):The party (person or organisation) to contact about accessing the resource. Each party has a name (label), identifier, and optional contact information. If there are multiple distribution options with different contact points, the Distribution Agent should be specified as part of the Distribution Object.
- **Checksum**. (0 or 1): A string value calculated from the content of a digital object that allows verification that the content of the object has not been modified. Even insignificant changes to the content of the file will change its checksum. The algorithm used to calculate the checksum must be documented. See also [RFC-6920 'Naming things with hashes'](https://www.rfc-editor.org/rfc/rfc6920.html) that establishes ways to identify checksum algorithms and to represent checksum values as a URI. Note that checksums apply to specific digital objects, typically a unique resource representation. Non-digital resources do not have checksums; their representations can have checksums. See implementation notes in Appendix 1.
- **Funding**. (0 to many entries): Cite funding sources (Grants, contracts...). Each source has a grant or contract identifier, source organisation, and label.
- **Keyword** (0 to many entries): Distinguish 'tags' and 'controlled terms'. Tags are simply words that a metadata creator thinks will be useful for users to identify resources of interest. Controlled terms are words defined in a vocabulary that minimally include the word (a fixed string to identify the term for humans) and a definition. Each term represents some concept. More semantically rich vocabularies would include resolvable identifiers, source information, and links to related terms (see [Cox et al., 2021](https://doi.org/10.1371/journal.pcbi.1009041) ). One common set of relationships in a vocabulary is a kind-of hierarchy linking broader to narrower concepts. Controlled terms should minimally be represented with a label and scheme name that identifies the source vocabulary; ideally a term URI and scheme URI could be included for more accurate identification and data integration. 
- **Policies** (0 to many entries): Policies used in management of the described resource, including whether the content may be changed (mutable or immutable), any scheduled updates, what is the expected lifetime for resource availability, what (if any) is the maintenance schedule, versioning, documentation for changes and change requests. Explicit support for specific policy frameworks can be included (e.g., CARE).
- **Publication Date** (0 or 1): Date (not temporal extent) when the resource was made accessible. Use a ‘year’ or ISO 8601 date and time format. Alternative date formatting must be machine-readable and consistent across all datasets. If no publication date is known, estimate the publication date range, enter the oldest year as the publication date, and include the estimated date range in the Description field.
- **Other related agents** (0 to many entries): Recognition for others who have contributed to the production of the resource but are not recognized as authors/creators. Includes a variety of roles like maintainer, publisher, point of contact, copyright holder, contributor (see e.g. [DataCite contributor types](https://datacite-metadata-schema.readthedocs.io/en/4.5_draft/properties/recommended_optional/property_contributor.html#a-contributortype), [ISO19115-1 role code](https://wiki.esipfed.org/ISO_19115_and_19115-2_CodeList_Dictionaries#CI_RoleCode) )
- **Related resources** (0 to many entries): Links to related data, publications, annotation, data sources, software used, etc. Links have at least a label, relationship type, and resolvable target resource identifier.
- **Version** (0 or 1): If the resource is versioned, specify the label for this version. Version labels should follow a scheme that allows alphanumeric sorting reflecting the order of version release.
- **Provenance** (0..many): For discovery, provide information about datasets that were used in the creation of the described resource and specify sensors, platforms, software, algorithms etc. used to aquire information contained in the resource.  Details about workflows, activity sequences, association of sensors etc. with specific variables, individuals associate with particular activities in workflow etc. require used of cdif prov extension [tbd](./tbd).
- **Quality** (0..many) Provide statements about the quality of information in the described resource,  information about quality policies or certificates that apply to the resource, and results of quality measures with information about the measurement protocol/procedure used. In all cases the focus should be on information useful for initial assessment by potential users.

### Properties for metadata management
These elements provide information for the operation of a distributed catalogue system with harvesting of metadata between catalogue servers. Values should be populated automatically by metadata creation tools, requiring no user input.  Some providers might not include this information in metadata interchange files. 
- **Metadata Date** (0..1 entry): Last metadata update/creation date-time stamp in ISO 8601 date and time format. This may be automatically updated on metadata import if a metadata format conversion is necessary.
- **Metadata Contact Agent** (0..1 entry): The party responsible for metadata content and accuracy; Agent object includes a name (label), identifier, and optional contact information
- **Metadata Identifier** (0..1 entry): The identifier for the Digital object that contains the metadata.

# Implementation

The current recommended implementation uses the schema.org vocabulary, with a few entities and properties from other vocabularies to fill gaps. For background on JSON, JSON-LD and general implementation patters CDIF is using, see [Schema.org implementation notes](schemaOrgImplementationv2.md)


## Implementation of metadata content items

The following table maps the metadata content items described in the Information Model section (above). Some example metadata documents are accessible in the [Core Github repository](https://github.com/Cross-Domain-Interoperability-Framework/core/tree/main/examples). The \'Obl.\' column specifies the cardinality obligation for the property; \'1\' means one value required; 1..\* means at least one value is required; 0..\* means the property is optional and more that one value can be provided. Properties with path from "subjectOf" describe the metadata.

All property names use namespace prefixes as declared in the `@context` (e.g. `schema:`, `dcterms:`). The `schema:` prefix is required for all schema.org properties. The CDIF JSON-LD implementation uses a hierarchical JSON structure, and CURIE syntax to abbreviate URIs using prefixes defined in the JSON-LD context.  The implementation does not map un-prefixed JSON keys to URIs, rather prefixes a namespace abbreviation on the key label to represent the URI.  This enables using standard JSON schema to validate documents and avoids confusion about the vocabulary origin of keys used in the JSON.

<table class="table" border="1" style="width: 100%; table-layout: fixed; border-collapse: collapse;">
  <tr>
    <th style="width: 10%;"><b>CDIF content item</b></th>
    <th style="width: 5%;"><b>Obl.</b></th>
    <th style="width: 25%;"><b>Schema.org implementation</b></th>
    <th style="width: 60%;"><b>Scope note</b></th>
  </tr>
  <tr >
    <td >Metadata identifier</td>
    <td >1</td>
    <td > "schema:subjectOf"/"@id":{URI}</td>
    <td >The URI for the metadata record should be the @id value for the 'schema:subjectOf' node. This node has \@type ["schema:Dataset"] with schema:additionalType ["dcat:CatalogRecord"], and a schema:about property referencing the \@id of the root resource node.</td>
  </tr>
  <tr>
    <td>Resource identifier</td>
    <td>1</td>
    <td>"schema:identifier":{PropertyValue or string}</td>
    <td>The primary identifier for the resource. Can be a simple string (ideally a resolvable URI), or a schema:PropertyValue with propertyID (identifier scheme, e.g. from <a href="https://registry.identifiers.org/registry/">identifiers.org</a>), value (the identifier string), and url (resolvable link). The PropertyValue approach is strongly recommended following the <a href="https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#identifier">ESIP Science on Schema.org guidance</a>.</td>
  </tr>
  <tr>
    <td>Title</td>
    <td>1</td>
    <td>"schema:name":{string}</td>
    <td>A set of words that should identify the described resource for human use. Ideally, should be unique in the scope of the metadata catalog containing this metadata record.</td>
  </tr>
<tr>
    <td rowspan="3">Distribution</td>
    <td rowspan="3">1</td>
    <td colspan="2">either a url or a contentURL in a distribution is required to indicate how to get the resource.</td>
</tr>
<tr>
    <td>"schema:url":{URL}</td>
    <td>This url is generally expected to GET an html landing page about the resource...</td>
</tr>
  <tr>
    <td>"schema:distribution": <br>[ {"@type": ["schema:DataDownload"], <br> "schema:contentUrl": {URL}, ... },<br> {"@type": ["schema:WebAPI"], <br> "schema:serviceType": ..., ... } ]</td>
    <td>An array of distribution objects. Items may be DataDownload (file-based access) or WebAPI (service-based access). A DataDownload must include schema:contentUrl, and should include schema:encodingFormat and dcterms:conformsTo. The \@type is encoded as an array (e.g. ["schema:DataDownload"]).</td>
  </tr>
  <tr>
    <td>Rights</td>
    <td>1..*</td>
    <td>"schema:license":[{text or URI or CreativeWork}, ...] <br> Or <br> "schema:conditionsOfAccess":[{text or URI}, ...]</td>
    <td>At least one of schema:license or schema:conditionsOfAccess must be provided (as arrays). URL to license document or text explanation of restrictions on use. There might be multiple links to documents specifying related security, privacy, usage, sharing, etc. concerns.</td>
  </tr>
  <tr>
    <td>Metadata profile identifier</td>
    <td>1..*</td>
    <td>"schema:subjectOf"/"dcterms:conformsTo": <br>[{"@id": "https://w3id.org/cdif/core/1.0/"}, <br>{"@id": "https://w3id.org/cdif/discovery/1.0/"}]</td>
    <td>An array of objects, each with an @id property whose value is a conformance URI. For CDIFDiscovery, both the core and discovery URIs are required. Extended profiles add their own conformance URIs to this array.</td>
  </tr>
  <tr>
    <td>Metadata date</td>
    <td>0..1</td>
    <td>"schema:subjectOf"/"schema:sdDatePublished":{Date}</td>
    <td>Use ISO8601 format. The most recent publication date for the metadata content. Harvesters use this to determine if they have already harvested and processed this record.</td>
  </tr>
  <tr>
    <td>Metadata contact</td>
    <td>0..1</td>
    <td>"schema:subjectOf"/"schema:maintainer":{Person or Organization}</td>
    <td>Should include a name and contact point (institutional e-mail is best) for the agent responsible for metadata content. This is the contact point to report problems with metadata content. Person and Organization are Agent objects with various properties.</td>
  </tr>
  <tr>
    <td>Metadata catalog</td>
    <td>0..1</td>
    <td>"schema:subjectOf"/"schema:includedInDataCatalog": <br>{"@type": "schema:DataCatalog", <br>"schema:name": ..., "schema:url": ...}</td>
    <td>Identifies the data catalog or repository containing this metadata record. Value is a schema:DataCatalog with at least a name and URL.</td>
  </tr>
  <tr>
    <td rowspan="2">Resource type</td>
    <td>1</td>
    <td>"@type":["schema:Dataset", ...]</td>
    <td>An array of schema.org type values using the schema: prefix. Must include "schema:Dataset". Additional allowed types: schema:CreativeWork, schema:SoftwareApplication, schema:SoftwareSourceCode, schema:Product, schema:WebAPI, schema:DigitalDocument, schema:Collection, schema:ImageObject, schema:DataCatalog, schema:DefinedTermSet, schema:MediaObject.</td>
  </tr>
  <tr>
    <td>0..*</td>
    <td>"schema:additionalType": [{DefinedTerm or string}, ...]</td>
    <td>If a more specific resource type needs to be specified using a vocabulary other than schema.org, add a text or URI value here. Must be consistent with the \@type. Always encode as an array.</td>
  </tr>
  <tr>
    <td>Description</td>
    <td>0..1</td>
    <td>"schema:description": {string}</td>
    <td>Free text, with as much detail as is feasible</td>
  </tr>
  <tr>
    <td>Originators</td>
    <td>0..*</td>
    <td>"schema:creator": {"@list": [{Person or Organization}, ...]}</td>
    <td>Author or originator of intellectual content. Uses the JSON-LD \@list construct to preserve author order. Each item can be a Person, Organization, or an object reference ({"@id": "..."}) to an agent defined elsewhere. Use ORCID or other PID to identify persons where possible.</td>
  </tr>
  <tr>
    <td>Publication Date</td>
    <td>0..1</td>
    <td>"schema:datePublished" : {date time}</td>
    <td>Date on which the resource was made publicly accessible. Use ISO 8601 format.</td>
  </tr>
  <tr>
    <td>Modification Date</td>
    <td>1</td>
    <td>"schema:dateModified" : {date time}</td>
    <td>Date of most recent update to resource content. If Publication date is not provided, defaults to the Modification Date. Use ISO 8601 format.</td>
  </tr>
    <tr>
    <td>Other identifiers</td>
    <td>0..*</td>
    <td>"schema:sameAs": [{URI or PropertyValue}, ...]</td>
    <td>Other identifiers for the same resource, as IRI reference strings, object references ({"@id": "..."}), or structured identifiers using schema:PropertyValue.</td>
  </tr>
  <tr>
    <td>Version</td>
    <td>0..1</td>
    <td>"schema:version": {string or number}</td>
    <td>The version number or identifier for this resource. Values should sort from oldest to newest using an alphanumeric sort on version strings.</td>
  </tr>
  <tr>
    <td>Language</td>
    <td>0..1</td>
    <td>"schema:inLanguage": {string}</td>
    <td>The language of the dataset content (e.g. "en", "fr").</td>
  </tr>
  <tr>
    <td>Measurement technique</td>
    <td>0..*</td>
    <td>"schema:measurementTechnique": {string or DefinedTerm or array}</td>
    <td>The technique, technology, or methodology used for measurement or determination of the dataset values. Can be a string, a DefinedTerm with vocabulary reference, or an array combining these.</td>
  </tr>
  <tr>
    <td>Keyword</td>
    <td>0..*</td>
    <td>"schema:keywords":<br>[ {string}, <br> {"@type":"schema:DefinedTerm", <br> "schema:name": "OCEANS", <br> "schema:inDefinedTermSet": "gcmd:sciencekeywords", <br> "schema:identifier": {...} },...]</td>
    <td>Implement with text for tags, and schema:DefinedTerm for keywords from a controlled vocabulary. The DefinedTerm approach is used to represent concepts with links to their defining vocabulary. Recommend using DefinedTerm for all keywords if any are from a known vocabulary.</td>
  </tr>

  <tr><td colspan="4"><b>GeographicExtent</b>  Required if resource has a geographic extent for its subject, a bounding rectangle, line, or point.  To support cross-domain searches based on geospatial location, location coordinates must be given in decimal degrees using the WGS 84 datum. There are various other systems for describing location; these can be provided as alternate location descriptions, recognizing that they might not be meaningful to some metadata harvesting agents. Spatial coverage is encoded as an array.</td>
  </tr>
  <tr>
    <td> Named place</td>
    <td>0..*</td>
    <td>"schema:spatialCoverage": [{ "@type": "schema:Place",<br>"schema:name": {string} or {schema:DefinedTerm} }]</td>
    <td>To specify location with place names; if the names are from a gazeteer, use the schema:DefinedTerm to provide a name, identifier, and inDefinedTermSet to fully document the concept.</td>
  </tr>
  <tr>
    <td>Bounding box</td>
    <td>0..1</td>
    <td>"schema:spatialCoverage": [{ <br>"@type": "schema:Place",<br> "schema:geo": {  "@type": "schema:GeoShape", <br> "schema:box": "39.3280 120.1633 40.445  123.7878"   } }]</td>
    <td>For bounding box specification of the spatial extent of resource content. See <a href="https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#bounding-boxes">ESIP SOSO for details</a>. Recommend including only one bounding box; behavior of harvesting clients when multiple geometries are specified is unpredictable.</td>
  </tr>
  <tr>
    <td>Curvilinear trace</td>
    <td>0..1</td>
    <td>"schema:spatialCoverage": [{ <br>"@type": "schema:Place",<br> "schema:geo": {  "@type": "schema:GeoShape", <br> "schema:line": "39.33 120.77 40.44 123.96 41.00 121.34"   } }]</td>
    <td>For resource related to a linear trace like a ship track or airplane flight line</td>
  </tr>
  <tr>
    <td>Point location</td>
    <td>0..1</td>
    <td>"schema:spatialCoverage": [{<br> "@type": "schema:Place", <br>"schema:geo": {  "@type":  "schema:GeoCoordinates",  <br> "schema:latitude": 39.3280,   <br>  "schema:longitude": 120.1633 } }]</td>
    <td>For a point location specification of the spatial extent of resource content. Recommend including only one point; behavior of harvesting clients when multiple geometries are specified is unpredictable.</td>
    </tr>
  <tr>
    <td>Other serialization</th>
    <td>0..*</th>
    <td>"geosparql:hasGeometry": { <br> "@type": "sf:Point", <br> "geosparql:asWKT": {"@type":"geosparql:wktLiteral", <br>"@value":"POINT(-76  -18)"},<br> "geosparql:crs": {"@id":"http://www.opengis.net/def/crs/OGC/1.3/CRS84"} }</th>
    <td>Optional geographic extent using other more interoperable geometries, GeoSPARQL is recommended, see <a href="https://book.oceaninfohub.org/thematics/spatial/README.html#simple-geosparql-wkt">Ocean InfoHub</a>. Other geometry schemes might be specified in a specific domain profile, e.g. for atmospheric, subsurface data, or local coordinate systems.</th>
  </tr>
  <tr><td colspan="4"><b>Distribution</b></td></tr>
  <tr>
    <td rowspan="2">Distribution Agent</td>
    <td>0..*</td>
    <td>"schema:provider":[{Person or Organization}, ...]</td>
    <td>Contact point for the provider of a distribution. For a simple digital object with a download URL, or a resource with multiple distributions all from the same provider.</td>
  </tr>
  <tr>
    <td>0..*</td>
    <td>"schema:distribution": [ { "@type": ["schema:DataDownload"],"schema:provider":[{Person or Organization}] }...]</td>
    <td>If there are multiple distributions with different providers, each distribution can have a separate provider array.</td>
  </tr>
  <tr><td colspan="4"><b>Variables in the data</b>  The metadata about a dataset should include a list of variables that the dataset contains. Variable metadata should minimally specify the name of the variable as it appears in the dataset. That name should be, ideally, qualified by a controlled vocabulary or other semantic resource (e.g. represented by a resolvable URI), or minimally some descriptive text. </td></tr>
  <tr>
    <td>Variable (PropertyValue)</td>
    <td>0..*</td>
    <td>"schema:variableMeasured":<br> [ { "@type":["schema:PropertyValue"],<br>&emsp; "@id": "astm:var0011",<br>&emsp;  "schema:propertyID": [ "pato:PATO_0000025",<br>&emsp;&emsp;&emsp;"astm:prop/0405" ],<br>&emsp;  "schema:name": "hostMineral", <br>&emsp; "schema:description": "...." }...]</td>
    <td>Follow <a href="https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#variables">ESIPfed Science on Schema.org recommendation</a>, see also discussion for representing more complex data structures in <a href="https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Experimental.md#AdvancedVariableValueType">ESIPfed Experimental</a> and the <a href="https://cross-domain-interoperability-framework.github.io/cdifbook/data_integration/ddidescriptiondatastructure.html">Data Integration module of CDIF</a>. Variable must have a name and description, should have a propertyID with URI for the represented concept. The URI in the propertyID provides the semantic linkage for meaning of the variable.</td>
  </tr>
  <tr>
    <td>Variable (StatisticalVariable)</td>
    <td>0..*</td>
    <td>"schema:variableMeasured":<br> [ { "@type":["schema:StatisticalVariable"],<br> "@id": "astm:var0011",<br>
&emsp;"schema:measuredProperty":<br>
&emsp;&emsp;{"@type":"schema:Property", &emsp;&emsp;"schema:identifier":"astm:id/305978",<br>
&emsp;&emsp;"schema:name":"Average age"}]</td>
    <td>Statistical variable offers properties useful for describing social science statistical variables like populationType and statType. Use of StatisticalVariable is preferred for variables with values calculated from some aggregation process.</td>
  </tr>
  <tr>
    <td rowspan="5">Temporal coverage</td>
    <td rowspan="5">0..*
    </td>
    <td colspan="2">Temporal coverage is encoded as an array. Can be expressed in several ways: a calendar/clock dateTime or date time interval using ISO8601 serialization, a named time ordinal era, an interval bounded by time ordinal era, or with a numeric coordinate in a temporal reference system.</td>
  </tr>
  <tr>
    <td>"schema:temporalCoverage": ["2018-01-22"]</td>
    <td>Calendar data or clock time instant use ISO8601 encoding</td>
  </tr>
  <tr>
    <td>"schema:temporalCoverage": ["2012-09-20/2016-01-22"]</td>
    <td>Calendar data or clock time interval use ISO8601 encoding</td>
  </tr>
  <tr>
    <td>"schema:temporalCoverage": <br> [{ "@type":"time:ProperInterval", <br> "time:intervalStartedBy": "isc:LowerDevonian", <br>  "time:intervalFinishedBy": "isc:LowerPermian" }]</td>
    <td>Time ordinal era interval, use owl:time namespace, time: http://www.w3.org/2006/time#. This example uses <a href="http://resource.geosciml.org/classifier/ics/ischart/">International chronostratigraphic chart, isc</a>. See <a href="https://perio.do/en/">PeriodO</a> for identifiers for many other named time intervals.</td>
  </tr>
  <tr>
    <td>"schema:temporalCoverage": <br> [{ "time:ProperInterval- 345/298 Ma" }]</td>
    <td>For time interval specified using geologic ages, in Ka, Ma or Ga; The text string is an abbreviated owl time interval (proposal, under discussion)</td>
  </tr>
  <tr>
    <td>Related agents (contributor role)</td>
    <td>0..*</td>
    <td>"schema:contributor": [ {Person or Organization}, ... ]</td>
    <td>Recognition for others who have contributed to the production of the resource but are not recognized as authors/creators.</td>
  </tr>
  <tr>
    <td>Related agent (other role)</td>
    <td></td>
    <td>"schema:contributor": [{"@type": "schema:Role", <br>&emsp; "schema:roleName": "Principal Investigator",<br>&emsp;"schema:contributor": {"@type": "schema:Person",&emsp;&emsp;"@id": "https://orcid.org/...",<br>&emsp;&emsp;"schema:name": "John Doe",<br>&emsp;&emsp;"schema:affiliation": {"@type": "schema:Organization",<br>&emsp;&emsp;&emsp;"@id": "https://ror.org/...",<br>&emsp;&emsp;&emsp;"schema:name": "..."},<br>&emsp;&emsp;"schema:contactPoint": {"@type": "schema:ContactPoint",<br>&emsp;&emsp;&emsp;"schema:email": "john.chodacki@ucop.edu"}}}]</td>
    <td>To assign roles to contributors like editor, maintainer, publisher, point of contact, copyright holder  (e.g.  DataCite contributor types), use the <a href="http://blog.schema.org/2014/06/introducing-role.html">role construction defined by schema.org</a></td>
  </tr>
  <tr>
    <td>Related resources</td>
    <td>0..*</td>
    <td>"schema:relatedLink": [{"@type":"schema:LinkRole", "schema:linkRelationship": "...",<br>"schema:target": {"@type": "schema:EntryPoint", <br> "schema:encodingFormat": "text/html",<br>"schema:name": "...",<br>"schema:url": "https://example.org/data/stations" } } ]</td>
    <td>Use schema.org relatedLink with a LinkRole value, and the link URL in a 'target' EntryPoint object. These properties expect WebPage and Action as their domain, so the <a href="https://validator.schema.org/">schema.org validator</a> will throw a warning (not an error). Related resource links are useful for evaluation and use of data, but because of the wide variety of relationship possibilities, difficult to use in general search scenarios. Use a soft-type implementation, with a link relationship type using a schema:DefinedTerm, and a resolvable identifier for the relationship target.</td>
    </tr>
  <tr>
    <td>Funding</th>
    <td>0..*</th>
    <td>"schema:funding" :<br> [{ "@type": "schema:MonetaryGrant",<br> "schema:identifier": {"@type": "schema:PropertyValue", <br>&emsp;"schema:propertyID": "grant-id", "schema:value": "..."}, <br> "schema:name": "grant title", <br> "schema:funder":<br> { "@id": "https://ror.org/...", <br> "@type": "schema:Organization", <br>  "schema:name": "org name" } }]</th>
    <td>Use schema.org encoding and <a href="https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#funding">science on schema.org pattern</a>. Other organization properties can be included in the funder/Organization.</th>
  </tr>
  <tr>
    <td>Policies</td>
    <td>0..*</td>
    <td>"schema:publishingPrinciples": [ {"@type": "schema:CreativeWork", "schema:name": "...", "schema:url": "..."}... ]</td>
    <td>FDOF digitalObjectMutability, RDA digitalObjectPolicy, FDOF PersistencyPolicy. Policies related to maintenance, update, expected time to live.</td> </tr>
<tr> <td> Checksum  </td><td> 0..1  </td><td> "schema:distribution": [ { "@type": ["schema:DataDownload"], "spdx:checksum": {<br>&nbsp;&nbsp;"@type": "spdx:Checksum",<br>&nbsp;&nbsp;"spdx:algorithm":"SHA256",<br>&nbsp;&nbsp; "spdx:checksumValue":"abc123..." },..  }...]  </td>
<td>A string value calculated from the content of the resource representation, used to test if content has been modified. No schema.org property, follow DCAT v3 adoption of <a href="https://spdx.org/rdf/terms/">Software Package Data Exchange (SPDX)</a> property; The spdx:Checksum object has two properties: algorithm and checksumValue. The checksum is a property of each distribution/DataDownload. </td></tr>
<tr >
<td colspan="4"><b>Provenance for discovery</b> is limited to documenting technology used in the creation of the dataset and documenting other datasets that were inputs to the content of the described resource. The cdifDiscovery profile specifies only that wasGeneratedBy has a prov:Activity with prov:used items that are strings or @id references. Any additional structure under prov:used is optional and defined by extended profiles.</td></tr>
<tr><td>Provenance (instruments, software etc.) </td><td>0..* </td><td>   "prov:wasGeneratedBy": [{
        "@type": ["prov:Activity"],
        "prov:used": [
            "nerc:collection/L05/current/134",
            {"@id": "nerc:collection/B76/current/B7600031"} ]
 }]</td><td>Identify sensors, instruments, platforms, software, algorithms etc. used in the creation of the described resource. The prov:used array accepts strings (URIs or labels) or object references with @id.</td></tr>
<tr>
    <td>Provenance (input datasets) </td><td>0..* </td><td>
    "prov:wasDerivedFrom": [<br>
        "http://doi.org/10.547/347848",<br>
        {"@id": "http://doi.org/10.3578/h5ls"},<br>
        {"@type": "schema:CreativeWork", "schema:name": "...", "schema:url": "..."} ]</td><td>Identify datasets that were inputs to the content of the described resource. Items can be strings (URIs), object references, or CreativeWork objects with name and URL.</td></tr>
<tr>
<td colspan="4"><b>Quality information for discovery</b>: A text statement documenting quality of the resource should be included in the   schema:description. If there are quality policies or certificates that apply, these should be specified in the schema:publishingPrinciples. Quality measurement or assessment protocols that have an output result specific to this resource can be specified using dqv:hasQualityMeasurement </td>
</tr><tr>
<td>Quality measure</td><td>0..*</td><td>"dqv:hasQualityMeasurement": [<br> {
"@type": "dqv:QualityMeasurement",<br>
&emsp;"dqv:isMeasurementOf": &emsp;&emsp;&nbsp;&nbsp;"nerc:collection/L27/current/ARGO_QC",
&emsp;&emsp;"dqv:value": "good" },<br>
        { "@type": "dqv:QualityMeasurement",
&emsp;&emsp;"dqv:isMeasurementOf":<br>&emsp;&emsp; "imf:dsbb/2003/eng/dqaf.htm",
&emsp;&emsp;"dqv:value":<br>
&emsp;&emsp;"http://linkToASpecificQualityReport" }]
</td><td>Quality assessment or measurement conducted using procedure or protocol specified by the dqv:isMeasurementOf property, with result value specified in the dqv:value property. The result might be numeric, a categorical term, or a link to a document describing the quality assessment.</td>
</tr>
        </table>


# Service-based distribution

An API builds on a basic communication protocol (e.g. HTTP) by defining functionality and formatting to enable providing the specific data a user requires. This might involve filtering, subsetting, or various transformations for e.g. schema mapping, aggregating or anonymizing data. The focus here is on Web APIs that provide data using a URL for the endpoint location (the server that implements the data access protocol), with parameters to specify the particular data requested. The query parameters might be appended to this base URL as part of the URL, or provided as a message with the request. The implementation is based on the schema.org Action patterns. A WebAPI distribution is included as an item in the `schema:distribution` array alongside DataDownload items.

Implementation of metadata to describe a service-based (API) distribution:

| **CDIF content item**       | **Obl.** | **Schema.org implementation**   | **Scope note**                              |
|----------- |-------------|-------------|-------------|
| Service type | 1 | "schema:distribution"/{"@type":["schema:WebAPI"]}/<br>"schema:serviceType": {string or DefinedTerm}| Specify the kind of service. Ideally this should be a resolvable identifier. Currently there is no widely adopted registry for serviceType identifiers. For interoperability, there must be an external arrangement between data providers and consumers on the strings that will be used to specify service types.  |
| Service description document | 0..1 | "schema:documentation": {string or CreativeWork} | Document that provides a machine-actionable description of a service instance. Examples include OpenAPI documents, OGC Capabilities documents. |
| Endpoint URL | 1 | "schema:potentialAction":[{"@type":["schema:Action"],<br>"schema:target":{"@type":"schema:EntryPoint",<br>"schema:urlTemplate": ...}}] | Web location to invoke service; if there are parameters on the URL, the URL template construct enables description of the parameters. |
| Access constraints | 1 | "schema:termsOfService": {string or CreativeWork} | Description of access privileges required to use the API, e.g. registration, licensing, payments. |