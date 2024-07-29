# Serialization of CDIF metadata 

JSON-LD has been chosen as the recommended serialization format for CDIF metadata following our principle to use existing mainstream technology. The JSON format is widely used for data serialization and popular with developers. JSON-LD adds additional syntax for the representation of linked data, compatible with existing JSON implementations so that integration with existing applications is relatively frictionless. Many metadata providers are using the [schema.org](https://schema.org/) vocabulary with JSON-LD serialization for metadata publication and interchange. Use of this format provides a low barrier to entry for data providers.

The JSON syntax is defined by the [ECMA JSON specification](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/), and JSON-LD is specified in the [JSON-LD 1.1 recommendation](https://www.w3.org/TR/json-ld11/) from the World Wide Web Consortium (W3C). This serialization is designed for linked data applications that will translate the JSON into a set of {subject, predicate, object} triples that can be loaded into an RDF database for processing. The JSON-LD context binds JSON keys to URIs for more precise semantics, and the use of URIs to identify entities and property values in the metadata will maximize the linkage with resources on the wider web to build an ever-expanding global knowledge graph.

The metadata about the resource has properties about the resource like title, description, responsible parties, spatial or temporal extent (as outlined in the [Metadata Content Requirements](./contentmodel.md) section).

In a harvesting/federated catalog system some metadata about the metadata is important to keep track of where metadata came from, what format/profile it uses (harvesters need this to process), and update dates [see Metadata Content Requirements](./contentmodel.md). Unambiguous expression of this information requires makeing statements about a metadata record distinct from the thing in the world that the metadata describes (See Github issues [1](https://github.com/Cross-Domain-Interoperability-Framework/Discovery/issues/13),[2](https://github.com/schemaorg/schemaorg/issues/3569) ). In an RDF framework, this requires a distinct identifier for the metadata record object that will serve as the subject for these triples.

Schema.org includes several properties that can be used to embed information about the metadata record in the resource metadata: [**sdDatePublished**](https://schema.org/sdDatePublished), [**sdLicense**](https://schema.org/sdLicense), [**sdPublisher**](https://schema.org/sdPublisher), but lacks a way to provide an identifier for the metadata record distinct from the resource it describes, to specify other agents responsible for the metadata except the publisher, or to assert specification or profile conformance for the metadata record itself.

In the RDF serialization, Schema.org metadata records are [JSON-LD node objects](https://www.w3.org/TR/json-ld/#node-objects), and include an "@id" keyword with a value that identifies the node.  This identifier can be interpreted to represent a thing in the world that the metadata record (the 'node') is about, or to represent the metadata record (a JSON object) itself. Here is a short example record (other '@' properties are explained below):

```
{   "@context": "https://schema.org",
    "@id": "ex:URIforResource",
    "name": "unique title for the resource",
    "description": "Description of the resource",
	"dateModified": "2017-05-23"
}
```

When this JSON-LD is converted to RDF triples (e.g. using the [JSON-LD playground](https://json-ld.org/playground/) ), this results:

```
<ex:URIforResource> <http://schema.org/description> "Description of the resource" .
<ex:URIforResource> <http://schema.org/name> "unique title for the resource" .
<ex:URIforResource> <http://schema.org/dateModified> "2017-05-23"^^<http://schema.org/Date> .
```
The interpretation of the first two sets of triples would be that they are statements about the thing in the world that the metadata record is about.  The third triple is ambiguous-- was the metadata content modified, or the described resource in the world?  There does not seem to be any recognized best practice or consensus for dealing with this issue, so CDIF defines these conventions. 

Use the schema.org identifier property to identify a thing in the world that is the subject of the JSON-LD node.  The identified thing might be physical, imaginary, abstract, or a digital object.  The JSON-LD @id property identifies a node in a graph, and can be interpreted in different ways; as a URI it is expected to dereference to produce the same JSON-LD object in which it is defined. Given this convention, when the metadata record is processed, the processor should use the schema:identifier as subject of triples about the subject of the metadata record to avoid ambiguity.  In addition, this convention would suggest that if a schema:identifier property is present, the @id property should be interpreted to identify the JSON object that is the representation of the node in the knowledge graph. 

**NOTE-- these recommendations are under discussion** 
see [Github issue](https://github.com/Cross-Domain-Interoperability-Framework/Discovery/issues/13)


Statements about the metadata record as a distinct entity should be made using a separate identified node object. This node object can be embedded in the metadata record about the resource in the world (Example 1 below), or published as a separate node (Example 2 below). Note that this second approach is like the [DCAT CatalogRecord](https://www.w3.org/TR/vocab-dcat-3/#Class:Catalog_Record). 

```
{   "@context": [
        "https://schema.org",
        {"dcterms": "http://purl.org/dc/terms/",
         "ex":"https://example.com/99152/"
        }
    ],
    "@id": "ex:URIforNode1",
    "@type": "appropriate schema.org type",
	"identifier":"ex:URIforDescribedResource",
    "name": "unique title for the resource",
    "description": "Description of the resource",
    "subjectOf": {
        "@id": "ex:URIforNode2",
        "@type": "DigitalDocument",
        "dateModified": "2017-05-23",
		"identifier":"ex:URIforNode1",
        "description":"metadata about documentation for ex:URIforDescribedResource",
    	"dcterms:conformsTo": {"@id":"ex:cdif-metadataSpec"}
	}        
   }
```
Example 1.  Metadata about the metadata embedded.

```
{
    "@context": [
        "https://schema.org",
        {"ex": "https://example.com/99152/"}
    ],
    "@graph": [
        {
            "@id": "ex:URIforNode1",
            "@type": "Dataset",
            "identifier": "ex:URIforDescribedResource",
            "name": "unique title for the resource",
            "description": "Description of the resource"
        },
        {
            "@id": "ex:URIforNode2",
            "@type": "DigitalDocument",
            "dateModified": "2017-05-23",
            "identifier": "ex:URIforNode1",
            "description": "metadata about documentation for ex:URIforDescribedResource",
            "dcterms:conformsTo": {"@id": "ex:cdif-metadataSpec"}
        }
    ]
}
```

Example 2. Metadata about metadata as a separate graph node.

Including the schema:description with the string "metadata about documentation for ex:URIforDescribedResource" will allow disambiguating different usages of the subjectOf property.  The ex namespace in the example above is only included so the example is valid; actual metadata would likely have its own namespace for resource and metadata URIs. The distinct identifier for the metadata record (ex:URIforNode1) allows statements to be made about the metadata separately from statements about the resource it describes. 

Note that the @type for the metadata node (root node) is 'DigitalDocument'. This is a schema.org type that corresponds broadly to the concept of DigitalObject as used by the Fair Digital Object (FDO) community ([Bonino et al., 2022](https://fairdigitalobjectframework.org/) ), recognizing that the metadata record is a digital object. [tbd-- alternative, use schema:Dataset, and add schema:additionalProperty : cdif:metadata or something like that]



JSON keys prefixed with '@' are keywords defined in the [JSON-LD specification]( https://www.w3.org/TR/json-ld11/#keywords) (see table below)

 | Keyword  |   Description|
 |----------- |-------------|
 | \@context |  The value of the context is an object that specifies set of rules for interpreting the JSON-LD document. The rules can be specified inline in, or via a URI that identifies a context object containing a set of rules. |
|  \@id    |    A string that identifies the subject of the assertions in the JSON object that contains the \@id key.|
|  \@type   |   An identifier for the definition of the structure of the JSON object that contains the \@type key. The type determines what keys or values should be expected in the JSON object that contains the key. Values are types defined in the schema.org vocabulary. In the CDIF framework (and for compatibility with FDOF FDOF digitalObjectType), the schema:additionalType property should be used (see implementation table below) |
 

# Implementation of metadata content items

The following table maps the metadata content items described in the [Metadata Content Requirements](./contentmodel.md) section to the schema.org JSON-LD keys to use in metadata serialization. Some example metadata documents follow. The \'Obl.\' column specifies the cardinality obligation for the property; \'1\' means one value required; 1..\* means at least one value is required; 0..\* means the property is optional and more that one value can be provided. Properties with path from "subjectOf" describe the metadata.


| **CDIF content item**       | **Obl.** | **Schema.org implementation**   | **Scope note**                              |
|----------- |-------------|-------------|-------------|
| Metadata identifier         | 1        | "subjectOf"/"@id":{URI}    | The URI for the metadata record should be the \@id value for the 'subjectOf' element in the JSON instance document tree   |
| Resource identifier         | 1        | "@id":{URI}    | The URI for the resource should be the @id value for the root of the JSON instance document tree   | 
| Title      | 1        | "name":{string}     | A set of words that should uniquely identify the described resource for human use, in the scope of the metadata catalog containing this metadata record.     |
| Distribution        | 1        | "url":{URL}       | If metadata is about a single digital object      |
|             |          | "distribution": <br>   { \"@type\": \"DataDownload\", <br>    \"contentURL\": {URL },\...   }   | If the metadata is about an abstract, non-digital, or physical resource that has multiple distributions, with different URL, encodingFormat, conformsTo properties. Each distribution is considered a distinct digital object. |
| Rights                      | 1..\*    | "license":{text or URI} <br> Or <br> "conditionsOfAccess":{text or URI}    | URL to license document or text explanation of restrictions on use. There might be multiple links to documents specifying related security, privacy, usage, sharing, etc\... concerns.    |
| Metadata profile identifier | 1        | "subjectOf"/"encoding"/"dcterms:conformsTo": {identifier} <br> or <br>  "schemaVersion":{identifier}          | Use Dublin Core terms property. The value for Base CDIF metadata is 'CDIF_basic_1.0'. Different profiles extending this must define unique identifier strings to use here. *QUESTION--Which to use dct:conformsTo or schema:schema:Version?*  |
| Metadata date               | 0..1     | "subjectOf"/"dateModified":{Date or DateTime}     | Use ISO8601 format. The most recent update date for the metadata content. Harvesters use this to determine if they have already harvested and processed this record.   |
| Metadata contact            | 0..1     | / "subjectOf"/"maintainer":{Person or Organization}   | Should include a name and contact point (institutional e-mail is best) for the agent responsible for metadata content. This is the contact point to report problems with metadata content. Person and Organization are Agent objects with various properties.   |
| Resource type               | 1        | "@type":{schema.org type}      | If the [Schema.org resource types](https://schema.org/docs/full.html) are specific enough to scope the metadata record, use those.   |
|                             | 0..\*    | "additionalType": \[{DefinedTerm or URI}, \...\]      | If a more specific resource type needs to be specified, add a text or URI value here that identifies the type. MUST be consistent with the \@type. To simplify parsing, always encode as an array.     |
| Description                 | 0..1     | "description": {string}   | Free text, with as much detail as is feasible   |
| Originators                 | 0..\*    | "creator" : \[{Person or Organization}, \...\]    | The value is a schema.org person or organization. To simplify parsing, always encode as an array. Use ORCID or other PID to describe person or organization where possible    |
| Publication Date            | 0..1     | "datePublished" : {date time}  | Date on which the resource was made publicly accessible. Use ISO 8601 format.   |
| Modification Date           | 1        | "dateModified" : {date time}                                 | Date of most recent update to resource content. If Publication date is not provided, defaults to the Modification Date. Use ISO 8601 format.  |
| GeographicExtent (named place)           | 0..\*     |  \"spatialCoverage\": { \"@type\": \"Place\",<br>\"name\": {string} or {schema:DefinedTerm} }  | To specify location with place names; if the names are from a gazeteer, use the schema:DefinedTerm to provide a name, identifier, and inDefinedTermSet to fully document the concept. |
| GeographicExtent (bounding box)  | 0..1 | \"spatialCoverage\": { <br>\"@type\": \"Place\",<br> \"geo\": {  \"@type\": \"GeoShape\", <br> \"box\": \"39.3280    120.1633  40.445    123.7878\"   } } }  | For bounding box specification of the spatial extent of resource content. See [ESIP SOSO for details](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#bounding-boxes). Recommend including only one bounding box; behavior of harvesting clients when multiple geometries are specified is unpredictable.   |  
|  GeographicExtent (point location)     |   0..1     | "spatialCoverage\": {<br> \"@type\": \"Place\", <br>\"geo\": {  \"@type\":  \"GeoCoordinates\",  <br> \"latitude\": 39.3280,   <br>  \"longitude\": 120.1633 } } }    | For a point location specification of the spatial extent of resource content. Recommend including only one point; behavior of harvesting clients when multiple geometries are specified is unpredictable.   |
|  GeographicExtent (other serialization)    | 0..\*     | "geosparql:hasGeometry\": { <br> \"@type\": \"sf#Point\", <br> \"geosparql:asWKT\":  \"@type\":#wktLiteral\", <br>\"@value\":\"POINT(-76  -18)\"},<br> \"Geosparql:crs\": {\"@id\":"CRS84\"} }    | Optional geographic extent using other more interoperable geometries, e.g., GeoSPARQL, see [Ocean InfoHub](https://book.oceaninfohub.org/thematics/spatial/README.html#simple-geosparql-wkt). (Note URIs in example are truncated\...)  Other geometry schemes might be specified in a specific domain profile, e.g. for atmospheric, subsurface data, or local coordinate systems.  |
| Distribution Agent          | 0..\*    | "provider":{Person or Organization}                          | Contact point for the provider of a distribution. For a simple digital object with a download URL, or a resource with multiple distributions all from the same provider.     |
|                             |          | "distribution": \[  {    \"@type\": \"DataDownload\",\"provider\":{Person or Organization}   }\...\]     | If there are multiple distributions with different providers, each distribution can have a separate provider                                     
| Variable measured           | 0..\*    | "variableMeasured\":<br> \[ { \"@type\":"PropertyValue\",<br>&emsp; \"@id\": \"astm:var0011\",<br>&emsp;  \"propertyID\": \[ \"pato:PATO_0000025\",<br>&emsp;&emsp;&emsp;\"astm:prop/0405\" \],<br>&emsp;  \"name\": \"hostMineral\", <br>&emsp; \"description\": \"\...."   }\....\]  | Follow [ESIPfed Science on Schema.org recommendation](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#variables), see also discussion for representing more complex data structures in [ESIPfed Experimental](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Experimental.md#AdvancedVariableValueType) and the [Data Integration module of CDIF](https://cross-domain-interoperability-framework.github.io/cdifbook/data_integration/ddidescriptiondatastructure.html). Variable must have a name and description, should have a propertyID with URI for the represented concept. The URI in the propertyID provides the semantic linkage for meaning of the variable.     |
| Keyword                     | 0..\*    | "keywords":<br>\[    {string}, <br>  {\"@type\":\"DefinedTerm\", <br> \"name\": \"OCEANS\", <br> \"inDefinedTermSet\": \"gcmd:sciencekeywords\", <br>  \"identifier\": \"gcmd:concept/916b....6167d\"   },\....     \]   | Implement with text for tags, and schema:DefinedTerm for keywords from a controlled vocabulary. The DefinedTerm approach is used to represent concepts.   |
| Temporal coverage           | 0..1     | "temporalCoverage\": \"2018-01-22\"  | Calendar data or clock time instant use ISO8601 encoding   |
|     |  | "temporalCoverage\": \"2012-09-20/2016-01-22\"  | Calendar data or clock time interval use ISO8601 encoding   |
|    |  | "temporalCoverage\": <br> \[{ \"@type\":\"time:ProperInterval\", <br> \"time:intervalStartedBy\": \"isc:LowerDevonian, <br>  \"time:intervalFinishedBy\": \"isc:LowerPermian\"  }\]             | Time ordinal era interval, use owl:time namespace, time: http://www.w3.org/2006/time#. This example uses [International chronostratigraphic chart, isc](http://resource.geosciml.org/classifier/ics/ischart/). See https://perio.do/en/ for identifiers for many other named time intervals.   |
| Other related agents- simple contributor        | 0..\*    | "contributor": \[ {Person or Organization}, \... \]        | Recognition for others who have contributed to the production of the resource but are not recognized as authors/creators.    |
| related agent with role | | "contributor": {"@type": "Role", <br>&emsp; "roleName": "Principal Investigator",<br>&emsp;"contributor": {"@type": "Person",<br>&emsp;&emsp;	"@id": "https://orcid.org/...",<br>&emsp;&emsp;	"name": "John Doe",<br>&emsp;&emsp;	"affiliation": {"@type": "Organization",<br>&emsp;&emsp;&emsp;"@id": "https://ror.org/...",<br>&emsp;&emsp;&emsp;"name": "..."	},<br>&emsp;&emsp;"contactPoint": {"@type": "ContactPoint",<br>	&emsp;&emsp;&emsp;	"email": "john.chodacki@ucop.edu"<br>	}  }   } |  To assign roles to contributors like editor, maintainer, publisher, point of contact, copyright holder  (e.g.  DataCite contributor types), use the rather convoluted [role construction defined by schema.org](http://blog.schema.org/2014/06/introducing-role.html) |
| Related resources           | 0..\*    | "relatedLink": \[{"@type":"LinkRole", "linkRelationship": "...",<br>"target: {"@type": "EntryPoint", <br> "encodingType": "text/html",<br>"name": "...",<br>"url": "https://example.org/data/stations" } } \]  | Use schema.org relatedLink with a LinkRole value, and the link URL in a 'target' EntryPoint object. These properties expect WebPage and Action as their domain, so the [schema.org validator](https://validator.schema.org/) will throw a warning (not an error). Related resource links are useful for evaluation and use of data, but because of the wide variety of relationship possibilities, difficult to use in general search scenarios. Use a soft-type implementation, with a link relationship type using a schema:DefinedTerm, and a resolvable identifier for the relationship target.   |
| Funding                     | 0..\*    | "funding" :<br> { \"@id\": \"URI for grant\", <br> \"@type\": \"MonetaryGrant\",<br> \"identifier\": \"grant id\",  <br> \"name\": \"grant title\", <br> \"funder\":<br> { \"@id\": \"ror for org\", <br> \"@type\": \"Organization\", <br>  \"name\": \"org name\",  <br> \"identifier\": \[    \"other identifiers\" \] } }     | Use schema.org encoding and [science on schema.org pattern](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#funding). Other organization properties can be included in the funder/Organization .    |
| Policies                    | 0..\*    | "publishingPrinciples\": \[  {\"@type\": \"CreativeWork\"}\....  \]     | FDOF digitalObjectMutability, RDA digitalObjectPolicy, FDOF PersistencyPolicy. Policies related to maintenance, update, expected time to live.    |
| Checksum                    | 0..1     | "spdx:checksum\" or "distribution\": \[ { \"@type\": \"DataDownload\",    \"spdx:checksum\": {URL },..    }\...\]    | A string value calculated from the content of the resource representation, used to test if content has been modified. No schema.org property, follow DCAT v3 adoption of [Software Package Data Exchange (SPDX)](https://spdx.org/rdf/terms/) property; The [spdx Checksum object](https://spdx.org/rdf/spdx-terms-v2.1/classes/Checksum___-238837136.html) has two properties: algorithm and checksumValue. If the resource is a single DigitalObject, use the first partter, if there are multiple distributions, the checksum is a property of each distribution/DataDownload. |

# Implementation patterns

-   DefinedTerm. {label, schemename, conceptURI, schemeURI}. This is a pattern used for property values that are concepts defined in a controlled vocabulary, ontology, or similar semantic artefact. Values have a label, which is a string that will be meaningful to a human user, a 'schemename', which is a label that similarly identifies the source semantic resource in which the concept is defined, the conceptURI is a globally unique,resolvable identifier forthe concept value; schemeURI is a globally unique identifier for teh semantic resource in which the concept is defined.

-   Identifier. {identifier scheme, identifier string, resolvable identifier string}. This pattern is for identifiers that are useful to scope in the context of a scheme. The identifier scheme is associated with some authority (e.g. IBSN) that manages unique identifiers within their scope. If the identifier can be associated with a resolver to create a resolvable identifier string, typically an HTTP URL with a resolver host name (e.g https://n2t.net/) to which the identifier is suffixed to obtain a representation of the thing identified.

-   AgentObject. {name, agenttype, identifier, contactpoint, if the agen is a Person, include affiliation}. This pattern is for specifying an Agent in the prove sense: An agent is something that bears some form of responsibility for an activity taking place, for the existence of an entity, or for another agent\'s activity. Agents can be persons, organizations, or software-defined actors. Agents have a name for human recognition, a type, an identifier, contactPoint and affiliation. Machine agent contact points should be the accessible human who operations the environment running the machine agent.

-   DistributionObject {contentURL, encodingFormat, conformsTo, distributionAgent }. A pattern for specifying information necessary or useful for implementing machine access to a DigitalObject that is or represents a resource of interest. Includes a URL for the web location at which the DigitalObject can be accesses, the specifications or profiles to which the serialization and content of the object conform, and the Agent responsible for the distribution platform. This agent is the contact point if there are problems accessing the distributed digitalObject.