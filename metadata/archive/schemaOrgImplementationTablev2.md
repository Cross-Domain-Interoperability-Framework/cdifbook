# Implementation of metadata content items

The following table maps the metadata content items described in the [Metadata Content Requirements](./contentmodel.md) section to the schema.org JSON-LD keys to use in metadata serialization. Some example metadata documents follow. The \'Obl.\' column specifies the cardinality obligation for the property; \'1\' means one value required; 1..\* means at least one value is required; 0..\* means the property is optional and more that one value can be provided. Properties with path from "subjectOf" describe the metadata.


<table>
  <tr>
    <th><b>CDIF content item</b></th>
    <th><b>Obl.</b></th>
    <th><b>Schema.org implementation</b></th>
    <th><b>Scope note</b></th>
  </tr>
  <tr>
    <td>Metadata identifier</td>
    <td>1</td>
    <td>"schema:subjectOf"/"@id":{URI}</td>
    <td>The URI for the metadata record should be the \@id value for the 'schema:subjectOf' node. This node has \@type ["schema:Dataset"] with schema:additionalType ["dcat:CatalogRecord"], and a schema:about property referencing the \@id of the root resource node.</td>
  </tr>
  <tr>
    <td>Resource identifier</td>
    <td>1</td>
    <td>"schema:identifier":{PropertyValue or string}</td>
    <td>The primary identifier for the resource. Can be a simple string (ideally a resolvable URI), or a schema:PropertyValue with schema:propertyID (identifier scheme), schema:value (the identifier string), and schema:url (resolvable link). The PropertyValue approach is strongly recommended.</td>
  </tr>
  <tr>
    <td>Title</td>
    <td>1</td>
    <td>"schema:name":{string}</td>
    <td>A set of words that should uniquely identify the described resource for human use, in the scope of the metadata catalog containing this metadata record.</td>
  </tr>
  <tr>
    <td rowspan="2">Distribution</td>
    <td>1</td>
    <td>"schema:url":{URL}</td>
    <td>If metadata is about a single digital object. Either schema:url or schema:distribution (or both) must be present.</td>
  </tr>
  <tr>
    <td></td>
    <td>"schema:distribution": <br> [{ "@type": ["schema:DataDownload"], <br> "schema:contentUrl": {URL}, ... }, <br>{ "@type": ["schema:WebAPI"], ... }]</td>
    <td>An array of distribution objects. Items may be DataDownload (file-based, requires schema:contentUrl) or WebAPI (service-based). The \@type is encoded as an array. DataDownload should include schema:encodingFormat and dcterms:conformsTo.</td>
  </tr>
  <tr>
    <td>Rights</td>
    <td>1..*</td>
    <td>"schema:license":[{text or URI or CreativeWork}, ...] <br> Or <br> "schema:conditionsOfAccess":[{text or URI}, ...]</td>
    <td>At least one of schema:license or schema:conditionsOfAccess must be provided (as arrays). URL to license document or text explanation of restrictions on use.</td>
  </tr>
  <tr>
    <td>Metadata profile identifier</td>
    <td>1..*</td>
    <td>"schema:subjectOf"/"dcterms:conformsTo": <br>[{"@id": "https://w3id.org/cdif/core/1.0/"}, <br>{"@id": "https://w3id.org/cdif/discovery/1.0/"}]</td>
    <td>An array of objects with @id values that are conformance URIs. For CDIFDiscovery, both the core and discovery URIs are required. Extended profiles add their own conformance URIs to this array.</td>
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
    <td>Should include a name and contact point (institutional e-mail is best) for the agent responsible for metadata content. This is the contact point to report problems with metadata content.</td>
  </tr>
  <tr>
    <td>Metadata catalog</td>
    <td>0..1</td>
    <td>"schema:subjectOf"/"schema:includedInDataCatalog": <br>{"@type": "schema:DataCatalog", "schema:name": ..., "schema:url": ...}</td>
    <td>Identifies the data catalog or repository containing this metadata record.</td>
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
    <td>If a more specific resource type needs to be specified from a vocabulary other than schema.org, add a text or URI value here. Must be consistent with the \@type. Always encode as an array.</td>
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
    <td>Author or originator of intellectual content. Uses the JSON-LD \@list construct to preserve author order. Each item can be a Person, Organization, or an object reference ({"@id": "..."}) to an agent defined elsewhere.</td>
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
    <td>The version number or identifier for this resource. Values should sort from oldest to newest using an alphanumeric sort.</td>
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
    <td>The technique, technology, or methodology used for measurement or determination of the dataset values.</td>
  </tr>
  <tr>
    <td>Keyword</td>
    <td>0..*</td>
    <td>"schema:keywords":<br>[ {string}, <br> {"@type":"schema:DefinedTerm", <br> "schema:name": "OCEANS", <br> "schema:inDefinedTermSet": "gcmd:sciencekeywords", <br> "schema:identifier": {...} },...]</td>
    <td>Implement with text for tags, and schema:DefinedTerm for keywords from a controlled vocabulary. Recommend using DefinedTerm for all keywords if any are from a known vocabulary.</td>
  </tr>
  <tr>
    <td>GeographicExtent (named place)</td>
    <td>0..*</td>
    <td>"schema:spatialCoverage": [{ "@type": "schema:Place",<br>"schema:name": {string} or {schema:DefinedTerm} }]</td>
    <td>To specify location with place names; if the names are from a gazeteer, use the schema:DefinedTerm to provide a name, identifier, and inDefinedTermSet to fully document the concept.</td>
  </tr>
  <tr>
    <td>GeographicExtent (bounding box)</td>
    <td>0..1</td>
    <td>"schema:spatialCoverage": [{ <br>"@type": "schema:Place",<br> "schema:geo": {  "@type": "schema:GeoShape", <br> "schema:box": "39.3280 120.1633 40.445  123.7878"   } }]</td>
    <td>For bounding box specification of the spatial extent of resource content. See <a href="https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#bounding-boxes">ESIP SOSO for details</a>. Recommend including only one bounding box.</td>
  </tr>
  <tr>
    <td>GeographicExtent (curvilinear trace)</td>
    <td>0..1</td>
    <td>"schema:spatialCoverage": [{ <br>"@type": "schema:Place",<br> "schema:geo": {  "@type": "schema:GeoShape", <br> "schema:line": "39.33 120.77 40.44 123.96 41.00 121.34"   } }]</td>
    <td>For resource related to a linear trace like a ship track or airplane flight line</td>
  </tr>
  <tr>
    <td>GeographicExtent (point location)</td>
    <td>0..1</td>
    <td>"schema:spatialCoverage": [{<br> "@type": "schema:Place", <br>"schema:geo": {  "@type":  "schema:GeoCoordinates",  <br> "schema:latitude": 39.3280,   <br>  "schema:longitude": 120.1633 } }]</td>
    <td>For a point location specification of the spatial extent of resource content.</td>
    </tr>
  <tr>
    <td>GeographicExtent (other serialization)</th>
    <td>0..*</th>
    <td>"geosparql:hasGeometry": { <br> "@type": "sf:Point", <br> "geosparql:asWKT": {"@type":"geosparql:wktLiteral", <br>"@value":"POINT(-76  -18)"},<br> "geosparql:crs": {"@id":"http://www.opengis.net/def/crs/OGC/1.3/CRS84"} }</th>
    <td>Optional geographic extent using other more interoperable geometries, GeoSPARQL is recommended, see <a href="https://book.oceaninfohub.org/thematics/spatial/README.html#simple-geosparql-wkt">Ocean InfoHub</a>.</th>
  </tr>
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
  <tr>
    <td>Variable (PropertyValue)</td>
    <td>0..*</td>
    <td>"schema:variableMeasured":<br> [ { "@type":["schema:PropertyValue"],<br>&emsp; "@id": "astm:var0011",<br>&emsp;  "schema:propertyID": [ "pato:PATO_0000025",<br>&emsp;&emsp;&emsp;"astm:prop/0405" ],<br>&emsp;  "schema:name": "hostMineral", <br>&emsp; "schema:description": "...." }...]</td>
    <td>Follow <a href="https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#variables">ESIPfed Science on Schema.org recommendation</a>. Variable must have a name and description, should have a propertyID with URI for the represented concept.</td>
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
    <td>Use schema.org relatedLink with a LinkRole value, and the link URL in a 'target' EntryPoint object. Use a soft-type implementation, with a link relationship type using a schema:DefinedTerm.</td>
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
        {"@type": "schema:CreativeWork", "schema:name": "...", "schema:url": "..."} ]</td><td>Identify datasets that were inputs to the content of the described resource. Items can be strings (URIs), object references, or CreativeWork objects.</td></tr>
<tr>
<td colspan="4"><b>Quality information for discovery</b>: A text statement documenting quality of the resource should be included in the schema:description. If there are quality policies or certificates that apply, these should be specified in the schema:publishingPrinciples. Quality measurement or assessment protocols that have an output result specific to this resource can be specified using dqv:hasQualityMeasurement </td>
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
