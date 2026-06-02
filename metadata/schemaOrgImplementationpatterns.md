# Schema.org Implementation of CDIF Metadata

JSON-LD has been chosen as the recommended serialization format for CDIF metadata following our principle to use existing mainstream technology. The JSON format is widely used for data serialization and popular with developers. JSON-LD adds additional syntax for the representation of linked data, compatible with existing JSON implementations so that integration with existing applications is relatively frictionless. Many metadata providers are using the [schema.org](https://schema.org/) vocabulary with JSON-LD serialization for metadata publication and interchange. Use of this format provides a low barrier to entry for data providers.

The JSON syntax is defined by the [ECMA JSON specification](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/), and JSON-LD is specified in the [JSON-LD 1.1 recommendation](https://www.w3.org/TR/json-ld11/) from the World Wide Web Consortium (W3C). This serialization is designed for linked data applications that will translate the JSON into a set of {subject, predicate, object} triples that can be loaded into an RDF database for processing. The JSON-LD context binds JSON keys to URIs for more precise semantics, and the use of URIs to identify entities and property values in the metadata will maximize the linkage with resources on the wider web to build an ever-expanding global knowledge graph.

The metadata about the resource has properties about the resource like title, description, responsible parties, spatial or temporal extent (as outlined in the [Metadata Content Requirements](./contentmodel.md) section).

In a harvesting/federated catalog system some metadata about the metadata is useful to keep track of where metadata came from, what format/profile it uses (harvesters need this to process), and update dates [see Metadata Content Requirements](./contentmodel.md). Unambiguous expression of this information requires making statements about a metadata record distinct from the thing in the world that the metadata describes. In an RDF framework, this requires a distinct identifier for the metadata record object that will serve as the subject for these triples.

Schema.org includes several properties that can be used to embed information about the metadata record in the resource metadata: [**sdDatePublished**](https://schema.org/sdDatePublished), [**sdLicense**](https://schema.org/sdLicense), [**sdPublisher**](https://schema.org/sdPublisher), but lacks a way to provide an identifier for the metadata record distinct from the resource it describes, to specify other agents responsible for the metadata except the publisher, or to assert specification or profile conformance for the metadata record itself.

In the RDF serialization, Schema.org metadata records are [JSON-LD node objects](https://www.w3.org/TR/json-ld/#node-objects), and include an "@id" keyword with a value that identifies the node, analogous to a primary key in a relational database.  This identifier can be interpreted to represent a thing in the world that the metadata record (the 'node') is about, or to represent the metadata record (a JSON object) itself.

To avoid this ambiguity, CDIF adopts the convention that the schema.org identifier property is used to identify a thing in the world that is the subject of the JSON-LD node.  The identified thing might be physical, imaginary, abstract, or a digital object.  The JSON-LD \@id property identifies a node in a graph, which is an abstract object. As a URI the \@id URI is expected to dereference to produce a JSON-LD object containing the properties that are attached to the graph node. Given this convention, when the metadata record is processed, the processor should use the schema:identifier as subject of triples about the subject of the metadata record to avoid ambiguity.  In addition, this convention would suggest that if a schema:identifier property is present, the \@id property should be interpreted to identify the JSON object that is the representation of the node in the knowledge graph.

Statements about the metadata record (the JSON object) as a distinct entity should be made using a separate identified node object. This node object is embedded in the resource metadata using the `schema:subjectOf` property (Example 1 below), or published as a separate node in the graph (Example 2 below). The embedded node uses `@type: ["schema:Dataset"]` with `schema:additionalType: ["dcat:CatalogRecord"]` to indicate that it functions as a catalog record, and links back to the resource via `schema:about`. Note that this approach parallels the [DCAT CatalogRecord](https://www.w3.org/TR/vocab-dcat-3/#Class:Catalog_Record).

## JSON-LD Context

The CDIF implementation requires that the `@context` be an object declaring namespace prefixes used in the metadata record. At minimum, the `schema`, `dcterms`, and `dcat` prefixes must be declared:

```json
  "@context": {
    "schema": "http://schema.org/",
    "cdi": "http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/",
    "dcterms": "http://purl.org/dc/terms/",
    "geosparql": "http://www.opengis.net/ont/geosparql#",
    "spdx": "http://spdx.org/rdf/terms#",
    "time": "http://www.w3.org/2006/time#",
    "skos": "http://www.w3.org/2004/02/skos/core#",
    "prov": "http://www.w3.org/ns/prov#"
  }
```

Additional prefixes may be needed depending on which optional properties are used (e.g. `dqv`). Because CDIF uses prefixed property names (e.g. `schema:name` rather than `name`), the context must map each prefix to its namespace IRI.

## Catalog Record (subjectOf)

The metadata record information is embedded using `schema:subjectOf`. The CDIF implementation types the catalog record node as `schema:Dataset` with `schema:additionalType` of `dcat:CatalogRecord`:

```json
{
  "@context": {
    "schema": "http://schema.org/",
    "dcterms": "http://purl.org/dc/terms/",
    "dcat": "http://www.w3.org/ns/dcat#",
    "spdx": "http://spdx.org/rdf/terms#",
    "ex": "https://example.com/99152/"
  },
  "@id": "ex:URIforNode1",
  "@type": ["schema:Dataset"],
  "schema:identifier": {
    "@type": "schema:PropertyValue",
    "schema:propertyID": "https://registry.identifiers.org/registry/doi",
    "schema:value": "10.1234/example",
    "schema:url": "https://doi.org/10.1234/example"
  },
  "schema:name": "unique title for the resource",
  "schema:description": "Description of the resource",
  "schema:dateModified": "2017-05-23",
  "schema:license": ["https://creativecommons.org/licenses/by/4.0/"],
  "schema:url": "https://example.com/resource-landing-page",
  "schema:subjectOf": {
    "@id": "ex:URIforNode2",
    "@type": ["schema:Dataset"],
    "schema:additionalType": ["dcat:CatalogRecord"],
    "schema:about": {"@id": "ex:URIforNode1"},
    "schema:sdDatePublished": "2017-05-23",
    "dcterms:conformsTo": [
      {"@id": "https://w3id.org/cdif/core/1.1/"},
      {"@id": "https://w3id.org/cdif/discovery/1.1/"}
    ]
  }
}
```
Example 1.  Metadata about the metadata embedded via subjectOf.

This can also be implemented in a more flattened form as a graph with a separate node for the "schema:Dataset" with schema:additionalType" "dcat:CatalogRecord".  This serialization will validate with the SHACL rules but not the JSON schema unless the instance document is framed using the CDIF framing documents in the release repositories.

```json
{
  "@context": {
    "schema": "http://schema.org/",
    "dcterms": "http://purl.org/dc/terms/",
    "dcat": "http://www.w3.org/ns/dcat#",
    "ex": "https://example.com/99152/"
  },
  "@graph": [
    {
      "@id": "ex:URIforNode1",
      "@type": ["schema:Dataset"],
      "schema:identifier": "ex:URIforDescribedResource",
      "schema:name": "unique title for the resource",
      "schema:description": "Description of the resource"
    },
    {
      "@id": "ex:URIforNode2",
      "@type": ["schema:Dataset"],
      "schema:additionalType": ["dcat:CatalogRecord"],
      "schema:about": {"@id": "ex:URIforNode1"},
      "schema:sdDatePublished": "2017-05-23",
      "dcterms:conformsTo": [
        {"@id": "https://w3id.org/cdif/core/1.1/"},
        {"@id": "https://w3id.org/cdif/discovery/1.1/"}
      ]
    }
  ]
}
```

Example 2. Metadata about metadata as a separate graph node.

The distinct identifier for the metadata record allows statements to be made about the metadata separately from statements about the resource it describes. The catalog record node requires `@type`, `schema:additionalType`, `@id`, `schema:about`, and `dcterms:conformsTo`.

## Conformance URIs

Each CDIF building block defines a conformance URI that must be listed in the catalog record's `dcterms:conformsTo` array. The URIs follow the pattern `https://w3id.org/cdif/{scope}/{version}/`. A CDIFDiscovery-conformant record must declare at minimum:

| Building block | Conformance URI |
|---|---|
| cdifCore | `https://w3id.org/cdif/core/1.1/` |
| cdifOptional (Discovery) | `https://w3id.org/cdif/discovery/1.1/` |

Extended profiles add additional conformance URIs (e.g. `https://w3id.org/cdif/datadescription/1.1/`, `https://w3id.org/cdif/provenance/1.1/`).

JSON keys prefixed with '@' are keywords defined in the [JSON-LD specification]( https://www.w3.org/TR/json-ld11/#keywords) (see table below)

 | Keyword  |   Description|
 |-----------|-------------|
 | \@context |  The value must be an object that maps namespace prefixes to their IRI expansions. CDIF requires at minimum `schema`, `dcterms`, and `dcat` prefix declarations. Additional prefixes (e.g. `geosparql`, `prov`, `dqv`, `time`) are needed when using properties from those namespaces. |
|  \@id    |    A string that identifies the subject of the assertions in the JSON object that contains the \@id key.|
|  \@type   |   An array of type identifiers for the JSON object. In CDIF, the array must include `schema:Dataset`. Additional schema.org types from the allowed set may also be included. Values use the `schema:` prefix (e.g. `schema:Dataset`, `schema:CreativeWork`). The `schema:additionalType` property should be used for types from other vocabularies (e.g. `dcat:CatalogRecord`). |


# Implementation Patterns

All property names use namespace prefixes as declared in the `@context` (e.g. `schema:`, `dcterms:`). The `schema:` prefix is required for all schema.org properties. The CDIF JSON-LD implementation uses a hierarchical JSON structure, and CURIE syntax to abbreviate URIs using prefixes defined in the JSON-LD context.  The implementation does not map un-prefixed JSON keys to URIs, rather prefixes a namespace abbreviation on the key label to represent the URI.  This enables using standard JSON schema to validate documents and avoids confusion about the vocabulary origin of keys used in the JSON.

-   cdifConceptOrTerm. {label, schemename, conceptURI, schemeURI}. This is a pattern used for property values that are concepts defined in a controlled vocabulary, ontology, or similar semantic artefact. The current implementation allows two approaches-- use schema.org DefinedTerm or skos:Concept.  In DefinedTerm values have a `schema:name` (label meaningful to humans), `schema:inDefinedTermSet` (identifies the source semantic resource), `schema:identifier` (a PropertyValue with the concept URI), and `schema:termCode` (a short code for the concept). In the cdif skos:Concept, values have a `skos:prefLabel` (label meaningful to humans), `skos:inScheme` (identifies the source semantic resource), `schema:identifier` (a URI), and `skos:notation` (a short code for the concept).

-   Identifier. Identifiers can be inserted as simple string literals. If the identifier can be provided as a string literal that is resolvable and for which the identifier scheme is evident, that is all that is required. If the identifier scheme is not well known, or a separate resolver must be used, use the schema:PropertyValue to provide additional information. The `schema:propertyID` specifies the identifier scheme. CDIF recommends using scheme identifiers from [https://registry.identifiers.org/registry/](https://registry.identifiers.org/registry/). The `schema:value` provides the identifier as a string value. If the identifier can be resolved on the web, the `schema:url` provides a resolvable URL.

-   Agent. This pattern is for specifying an Agent in the PROV sense: An agent is something that bears some form of responsibility for an activity taking place, for the existence of an entity, or for another agent\'s activity. Agents can be persons, organizations, or software-defined actors. Agents have a `schema:name` for human recognition, a type (schema:Person, schema:Organization), an `@id` identifier, `schema:contactPoint` and `schema:affiliation`. Machine agent contact points should be the accessible human who operates the environment running the machine agent. This pattern is used for hard-typed roles in the CDIF implementation-- schema:creator, schema:maintainer, schema:contributor, schema:provider. Other roles can be documented using the [schema.org role pattern](http://blog.schema.org/2014/06/introducing-role.html) in the schema:contributor property. Note that `schema:creator` uses the JSON-LD `@list` wrapper to preserve author ordering.

-   DistributionObject {contentUrl, encodingFormat, dcterms:conformsTo, distributionAgent}. This pattern specifies information for implementing machine access to a DigitalObject. Includes a URL (`schema:contentUrl`) for the web location at which the DigitalObject can be accessed, the specifications or profiles to which the serialization and content of the object conform using `dcterms:conformsTo` (an array of objects with \@id), the format of the digital object content (`schema:encodingFormat`), and the Agent responsible for the distribution platform (`schema:provider`). The `@type` must be an array containing `schema:DataDownload`.
