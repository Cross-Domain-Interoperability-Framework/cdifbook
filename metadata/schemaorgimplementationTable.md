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
    <td>"subjectOf"/"@id":{URI} or "@id":{uri} in node with "identifier":"@id" of the node containing the resource description</td>
    <td>The URI for the metadata record should be the \@id value for the 'subjectOf' element in the JSON instance document tree or "@id":{uri} in a separate graph node with "identifier":"@id" of the node containing the resource description</td>
  </tr>
  <tr>
    <td>Resource identifier</td>
    <td>1</td>
    <td>"identifier":{URI}</td>
    <td>The URI for the resource that is the subject of the metadata record should be the "identifier": value for the root of the JSON instance document tree</td>
  </tr>
  <tr>
    <td>Title</td>
    <td>1</td>
    <td>"name":{string}</td>
    <td>A set of words that should uniquely identify the described resource for human use, in the scope of the metadata catalog containing this metadata record.</td>
  </tr>
  <tr>
    <td rowspan="2">Distribution</td>
    <td>1</td>
    <td>"url":{URL}</td>
    <td>If metadata is about a single digital object</td>
  </tr>
  <tr>
    <td></td>
    <td>"distribution": <br> { "@type": "DataDownload", <br> "contentURL": {URL },\... }</td>
    <td>If the metadata is about an abstract, non-digital, or physical resource that has multiple distributions, with different URL, encodingFormat, conformsTo properties. Each distribution is considered a distinct digital object. The dataDownload MUST include the contentURL, and SHOULD include encodingFormat, dcterms:conformsTo to specify the media type and specification or profile documenting the specific serialization conventions for the download content.</td>
  </tr>
  <tr>
    <td>Rights</td>
    <td>1..*</td>
    <td>"license":{text or URI} <br> Or <br> "conditionsOfAccess":{text or URI}</td>
    <td>URL to license document or text explanation of restrictions on use. There might be multiple links to documents specifying related security, privacy, usage, sharing, etc... concerns.</td>
  </tr>
  <tr>
    <td>Metadata profile identifier</td>
    <td>1</td>
    <td>"subjectOf"/"dcterms:conformsTo": {identifier}</td>
    <td>Use Dublin Core terms property. The value for Base CDIF metadata is 'CDIF_basic_1.0' [tbd; this should be a PID]. Different profiles extending this must define unique identifier strings to use here. Note that the schema.org schemaVersion is used to indicate the version of the schema.org vocabulary, but in general this is not needed for CDIF.</td>
  </tr>
  <tr>
    <td>Metadata date</td>
    <td>0..1</td>
    <td>"subjectOf"/"dateModified":{Date or DateTime}</td>
    <td>Use ISO8601 format. The most recent update date for the metadata content. Harvesters use this to determine if they have already harvested and processed this record.</td>
  </tr>
  <tr>
    <td>Metadata contact</td>
    <td>0..1</td>
    <td>/ "subjectOf"/"maintainer":{Person or Organization}</td>
    <td>Should include a name and contact point (institutional e-mail is best) for the agent responsible for metadata content. This is the contact point to report problems with metadata content. Person and Organization are Agent objects with various properties.</td>
  </tr>
  <tr>
    <td rowspan="2">Resource type</td>
    <td>1</td>
    <td>"@type":{schema.org type}</td>
    <td>Use the most specific [Schema.org resource type](https://schema.org/docs/full.html) that is applicable. Multiple value can be provided but they must be logically consistent.</td>
  </tr>
  <tr>
    <td>0..*</td>
    <td>"additionalType": [{DefinedTerm or URI}, ...]</td>
    <td>If a more specific resource type needs to be specified, add a text or URI value here that identifies the type. MUST be consistent with the \@type. To simplify parsing, always encode as an array.</td>
  </tr>
  <tr>
    <td>Description</td>
    <td>0..1</td>
    <td>"description": {string}</td>
    <td>Free text, with as much detail as is feasible</td>
  </tr>
  <tr>
    <td>Originators</td>
    <td>0..*</td>
    <td>"creator" : [{Person or Organization}, ...]</td>
    <td>The value is a schema.org person or organization. To simplify parsing, always encode as an array. Use ORCID or other PID to identify person or organization where possible</td>
  </tr>
  <tr>
    <td>Publication Date</td>
    <td>0..1</td>
    <td>"datePublished" : {date time}</td>
    <td>Date on which the resource was made publicly accessible. Use ISO 8601 format.</td>
  </tr>
  <tr>
    <td>Modification Date</td>
    <td>1</td>
    <td>"dateModified" : {date time}</td>
    <td>Date of most recent update to resource content. If Publication date is not provided, defaults to the Modification Date. Use ISO 8601 format.</td>
  </tr>
  <tr>
    <td>GeographicExtent (named place)</td>
    <td>0..*</td>
    <td>"spatialCoverage": { "@type": "Place",<br>"name": {string} or {schema:DefinedTerm} }</td>
    <td>To specify location with place names; if the names are from a gazeteer, use the schema:DefinedTerm to provide a name, identifier, and inDefinedTermSet to fully document the concept.</td>
  </tr>
  <tr>
    <td>GeographicExtent (bounding box)</td>
    <td>0..1</td>
    <td>"spatialCoverage": { <br>"@type": "Place",<br> "geo": {  "@type": "GeoShape", <br> "box": "39.3280 120.1633 40.445  123.7878"   } }</td>
    <td>For bounding box specification of the spatial extent of resource content. See [ESIP SOSO for details](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#bounding-boxes). Recommend including only one bounding box; behavior of harvesting clients when multiple geometries are specified is unpredictable.</td>
  </tr>
  <tr>
    <td>GeographicExtent (curvilinear trace)</td>
    <td>0..1</td>
    <td>"spatialCoverage": { <br>"@type": "Place",<br> "geo": {  "@type": "GeoShape", <br> "line": "39.33 120.77 40.44 123.96 41.00 121.34"   } }</td>
    <td>For resource related to a linear trace like a ship track or airplane flight line</td>
  </tr>
  <tr>
    <td>GeographicExtent (point location)</td>
    <td>0..1</td>
    <td>"spatialCoverage": {<br> "@type": "Place", <br>"geo": {  "@type":  "GeoCoordinates",  <br> "latitude": 39.3280,   <br>  "longitude": 120.1633 } }</td>
    <td>For a point location specification of the spatial extent of resource content. Recommend including only one point; behavior of harvesting clients when multiple geometries are specified is unpredictable.</td>
    </tr>
  <tr>
    <td>GeographicExtent (other serialization)</th>
    <td>0..*</th>
    <td>"geosparql:hasGeometry": { <br> "@type": "sf#Point", <br> "geosparql:asWKT":  "@type":#wktLiteral", <br>"@value":"POINT(-76  -18)"},<br> "Geosparql:crs": {"@id":"CRS84"} }</th>
    <td>Optional geographic extent using other more interoperable geometries, GeoSPARQL us recommended, see <a href="https://book.oceaninfohub.org/thematics/spatial/README.html#simple-geosparql-wkt">Ocean InfoHub</a>. (Note URIs in example are truncated...) Other geometry schemes might be specified in a specific domain profile, e.g. for atmospheric, subsurface data, or local coordinate systems.</th>
  </tr>
  <tr>
    <td rowspan="2">Distribution Agent</td>
    <td>0..*</td>
    <td>"provider":{Person or Organization}</td>
    <td>Contact point for the provider of a distribution. For a simple digital object with a download URL, or a resource with multiple distributions all from the same provider.</td>
  </tr>
  <tr>
    <td>0..*</td>
    <td>"distribution": [ { "@type": "DataDownload","provider":{Person or Organization} }...]</td>
    <td>If there are multiple distributions with different providers, each distribution can have a separate provider</td>
  </tr>
  <tr>
    <td>Variable (PropertyValue)</td>
    <td>0..*</td>
    <td>"variableMeasured":<br> [ { "@type":"PropertyValue",<br>&emsp; "@id": "astm:var0011",<br>&emsp;  "propertyID": [ "pato:PATO_0000025",<br>&emsp;&emsp;&emsp;"astm:prop/0405" ],<br>&emsp;  "name": "hostMineral", <br>&emsp; "description": "...." }...]</td>
    <td>Follow <a href="https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#variables">ESIPfed Science on Schema.org recommendation</a>, see also discussion for representing more complex data structures in <a href="https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Experimental.md#AdvancedVariableValueType">ESIPfed Experimental</a> and the <a href="https://cross-domain-interoperability-framework.github.io/cdifbook/data_integration/ddidescriptiondatastructure.html">Data Integration module of CDIF</a>. Variable must have a name and description, should have a propertyID with URI for the represented concept. The URI in the propertyID provides the semantic linkage for meaning of the variable.</td>
  </tr>
  <tr>
    <td>Variable (StatisticalVariable)</td>
    <td>0..*</td>
    <td>"variableMeasured":<br> [ { "@type":"StatisticalVariable",<br> "@id": "astm:var0011",<br>"@type": "StatisticalVariable",<br>
&emsp;"measuredProperty":<br>
&emsp;&emsp;{"@type":"Property",      &emsp;&emsp;"identifier":"astm:id/305978",<br>
&emsp;&emsp;"name":"Average age"}]</td>
    <td>Statistical variable offers properties useful for describing social science statistical variables like populationType and statType. Use of StatisticalVariable is preferred for variables with values calculated from some aggregation process.</td>
  </tr>
  <tr>
    <td>Keyword</td>
    <td>0..*</td>
    <td>"keywords":<br>[ {string}, <br> {"@type":"DefinedTerm", <br> "name": "OCEANS", <br> "inDefinedTermSet": "gcmd:sciencekeywords", <br> "identifier": "gcmd:concept/916b....6167d" },...]</td>
    <td>Implement with text for tags, and schema:DefinedTerm for keywords from a controlled vocabulary. The DefinedTerm approach is used to represent concepts.</td>
  </tr>
  <tr>
    <td rowspan="5">Temporal coverage</td>
    <td rowspan="5">0..*
    </td>
    <td colspan="2">Temporal coverage can be expressed in several ways: a calendar/clock dateTime or date time interval using ISO8601 serialization, a named time ordinal era, an interval bounded by time ordinal era, or with a numeric coordinate in a temporal reference system.</td>
  </tr>
  <tr>
    <td>"temporalCoverage": "2018-01-22"</td>
    <td>Calendar data or clock time instant use ISO8601 encoding</td>
  </tr>
  <tr>
    <td>"temporalCoverage": "2012-09-20/2016-01-22"</td>
    <td>Calendar data or clock time interval use ISO8601 encoding</td>
  </tr>
  <tr> 
    <td>"temporalCoverage": <br> [{ "@type":"time:ProperInterval", <br> "time:intervalStartedBy": "isc:LowerDevonian, <br>  "time:intervalFinishedBy": "isc:LowerPermian" }]</td>
    <td>Time ordinal era interval, use owl:time namespace, time: http://www.w3.org/2006/time#. This example uses <a href="http://resource.geosciml.org/classifier/ics/ischart/">International chronostratigraphic chart, isc</a>. See <a href="https://perio.do/en/">PeriodO</a> for identifiers for many other named time intervals.</td>
  </tr>
  <tr>
    <td>"temporalCoverage": <br> [{ "time:ProperInterval- 345/298 Ma" }]</td>
    <td>For time interval specified using geologic ages, in Ka, Ma or Ga; The text string is an abbreviated owl time interval (proposal, under discussion)</td>
  </tr>
  <tr>
    <td>Related agents (contributor role)</td>
    <td>0..*</td>
    <td>"contributor": [ {Person or Organization}, ... ]</td>
    <td>Recognition for others who have contributed to the production of the resource but are not recognized as authors/creators.</td>
  </tr>
  <tr>
    <td>Related agent (other role)</td>
    <td></td>
    <td>"contributor": {"@type": "Role", <br>&emsp; "roleName": "Principal Investigator",<br>&emsp;"contributor": {"@type": "Person",&emsp;&emsp;"@id": "https://orcid.org/...",<br>&emsp;&emsp;"name": "John Doe",<br>&emsp;&emsp;"affiliation": {"@type": "Organization",<br>&emsp;&emsp;&emsp;"@id": "https://ror.org/...",<br>&emsp;&emsp;&emsp;"name": "..."},<br>&emsp;&emsp;"contactPoint": {"@type": "ContactPoint",<br>&emsp;&emsp;&emsp;"email": "john.chodacki@ucop.edu"}</td>
    <td>To assign roles to contributors like editor, maintainer, publisher, point of contact, copyright holder  (e.g.  DataCite contributor types), use the rather convoluted <a href="http://blog.schema.org/2014/06/introducing-role.html">role construction defined by schema.org</a></td>
  </tr>
  <tr>
    <td>Related resources</td>
    <td>0..*</td>
    <td>"relatedLink": [{"@type":"LinkRole", "linkRelationship": "...",<br>"target: {"@type": "EntryPoint", <br> "encodingType": "text/html",<br>"name": "...",<br>"url": "https://example.org/data/stations" } } ]</td>
    <td>Use schema.org relatedLink with a LinkRole value, and the link URL in a 'target' EntryPoint object. These properties expect WebPage and Action as their domain, so the <a href="https://validator.schema.org/">schema.org validator</a> will throw a warning (not an error). Related resource links are useful for evaluation and use of data, but because of the wide variety of relationship possibilities, difficult to use in general search scenarios. Use a soft-type implementation, with a link relationship type using a schema:DefinedTerm, and a resolvable identifier for the relationship target.</td>
    </tr>
  <tr>
    <td>Funding</th>
    <td>0..*</th>
    <td>"funding" :<br> { "@id": "URI for grant", <br> "@type": "MonetaryGrant",<br> "identifier": "grant id",  <br> "name": "grant title", <br> "funder":<br> { "@id": "ror for org", <br> "@type": "Organization", <br>  "name": "org name",  <br> "identifier": [ "other identifiers" ] } }</th>
    <td>Use schema.org encoding and <a href="https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#funding">science on schema.org pattern</a>. Other organization properties can be included in the funder/Organization.</th>
  </tr>
  <tr>
    <td>Policies</td>
    <td>0..*</td>
    <td>"publishingPrinciples": [ {"@type": "CreativeWork"}.... ]</td>
    <td>FDOF digitalObjectMutability, RDA digitalObjectPolicy, FDOF PersistencyPolicy. Policies related to maintenance, update, expected time to live.</td> </tr>
<tr> <td> Checksum  </td><td> 0..1  </td><td> "distribution\": \[ { \"@type\": \"DataDownload\",    \"spdx:checksum\": {<br>&nbsp;&nbsp;"spdx:algorithm":"string",<br>&nbsp;&nbsp; "spdx:checksumValue":"string" },..  }\...\]  </td>
<td>A string value calculated from the content of the resource representation, used to test if content has been modified. No schema.org property, follow DCAT v3 adoption of [Software Package Data Exchange (SPDX)](https://spdx.org/rdf/terms/) property; The [spdx Checksum object](https://spdx.org/rdf/spdx-terms-v2.1/classes/Checksum___-238837136.html) has two properties: algorithm and checksumValue. The checksum is a property of each distribution/DataDownload. </td></tr>
<tr >
<td colspan="4"><b>Provenance for discovery</b> is limited to documenting technology used in the creation of the dataset and documening other datasets (datasets) that were inputs to the content of the described resource.</td></tr>
<tr><td>Provenance (instruments, software etc.) </td><td>0..* </td><td>   "prov:wasGeneratedBy": {
        "@type": "prov:Activity",
        "prov:used": [
            "nerc:collection/L05/current/134",
            "nerc:collection/B76/current/B7600031" ]
 },</td><td>Identify sensors, instruments, platforms, software, algorithms etc. used in the creation of the described resource</td></tr>
    <td>Provenance (input datasets) </td><td>|0..* </td><td>   
    "prov:wasDerivedFrom": [<br>
        "http://doi.org/10.547/347848",
        "http://doi.org/10.3578/h5ls",
        "http://doi.org/10.547/93578" ],</td><td>"</td></tr>
<tr>
<td colspan="4"><b>Quality information for discovery</b>: A text statement documenting quality of the resource should be included in the   sdo:description. If there are quality policies or certificates that apply, these should be specified in the sdo:policies. Quality measurement or assessment protocols that have an output result specific to this resource can be specified using dqv:hasQualityMeaurement </td>
</tr><tr>
<td>Quality measure</td><td>0..*</td><td>"dqv:hasQualityMeasurement": [<br> {
"@type": "dqv:QualityMeasurement",<br>
&emsp;"dqv:isMeasurementOf": &emsp;&emsp;&nbsp;&nbsp;"nerc:collection/L27/current/ARGO_QC",
&emsp;&emsp;"dqv:value": "good" },<br>
        { "@type": "dqv:QualityMeasurement",
&emsp;&emsp;"dqv:isMeasurementOf":<br>&emsp;&emsp; "imf:dsbb/2003/eng/dqaf.htm",
&emsp;&emsp;"dqv:value":<br>
&emsp;&emsp;"http://linkToASpecificQualityReport" }]
</td><td>Quality assesment or measument conducted using procedure or protocol specified by the dqv:isMeasurementOf property, with result value specified in the dqv:value property. The result might be numeric, a categorical term, or a link to a document describing the quality assessment.</td>
</tr>
        </table>