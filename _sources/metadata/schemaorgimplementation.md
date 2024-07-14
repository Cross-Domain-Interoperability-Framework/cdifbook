# Serialization of CDIF metadata 

JSON-LD has been chosen as the recommended serialization format for CDIF metadata following our principle to use existing mainstream technology. The JSON format is widely used for data serialization and popular with developers. JSON-LD adds additional syntax for the representation of linked data, compatible with existing JSON implementations so that integration with existing applications is relatively frictionless. Many metadata providers are using the schema.org[^32] vocabularies with JSON-LD serialization for metadata publication and interchange. Use of this format provides a low barrier to entry for data providers.

The JSON syntax is defined by the [ECMA JSON specification](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/), and JSON-LD is specified in the [JSON-LD 1.1 recommendation](https://www.w3.org/TR/json-ld11/) from the World Wide Web Consortium (W3C). This serialization is designed for linked data applications that will translate the JSON into a set of {subject, predicate, object} triples that can be loaded into an RDF database for processing. The JSON-LD context binds JSON keys to URIs for more precise semantics, and the use of URIs to identify entities and property values in the metadata will maximize the linkage with resources on the wider web to build an ever-expanding global knowledge graph.

A metadata record has two parts; one part is about the metadata record itself, the other part is the content about the resource that the metadata documents. The part about the record specifies the identifier for the metadata record, agents with responsibility for the record, when it was last updated, what specification or profiles the metadata serialization conforms to, and other optional properties of the metadata that are deemed useful. The metadata about the resource has properties about the resource like title, description, responsible parties, spatial or temporal extent (as outlined in the [Metadata Content Requirements](#mdcontent) section).

Schema.org includes several properties that can be used to embed information about the metadata record in the resource metadata: [**sdDatePublished**](https://schema.org/sdDatePublished), [**sdLicense**](https://schema.org/sdLicense), [**sdPublisher**](https://schema.org/sdPublisher), but lacks a way to provide an identifier for the metadata record distinct from the resource it describes, to specify other agents responsible for the metadata except the publisher, or to assert specification or profile conformance for the metadata record itself.

To include this information in CDIF metadata, the recommendation is the following pattern, using schema:subjectOf property to provide an @id for the metadata record, and assert properties useful for metadata management in a distributed, federated catalog system.   The pattern is like this:

```
{   "@context": [
        "https://schema.org",
        {"dcterms": "http://purl.org/dc/terms/",
         "ex":"https://example.com/99152/"
        }
    ],
    "@id": "ex:URIforDescribedResource",
    "@type": "appropriate schema.org type",
    "name": "unique title for the resource",
    "description": "Description of the resource",
    "subjectOf": {
        "@id": "ex:URIforTheMetadata",
        "@type": "DigitalDocument",
        "dateModified": "2017-05-23",
        "description":"this metadata document",
        "encoding": {
            "@type": "MediaObject",
    	    "dcterms:conformsTo": {"@id":"ex:cdif-metadataSpec"}
          },
        "about":{"@id":"ex:URIforDescribedResource"}
		}
```
 ..... other metadata content omitted
```          
   }
```

Including the schema:description with the string "this metadata document" will allow disambiguating different usages of the subjectOf property.  Including the 'about' property with the back link to ex:URIforDescribedResource is useful but could be calculated with inverse of the subjectOf property.  The ex namespace in the example above is only included so the example is valid; actual metadata would likely have its own namespace for resource and metadata URIs.

JSON keys prefixed with '@' are keywords defined in the [JSON-LD specification]( https://www.w3.org/TR/json-ld11/#keywords) (see table below)

 | Keyword  |   Description|
 |----------- |-------------|
 | \@context |  The value of the context is an object that specifies set of rules for interpreting the JSON-LD document. The rules can be specified inline in, or via a URI that identifies a context object containing a set of rules. |
|  \@id    |    A string that identifies the subject of the assertions in the JSON object that contains the \@id key.|
|  \@type   |   An identifier for the definition of the structure of the JSON object that contains the \@type key. The type determines what keys or values should be expected in the JSON object that contains the key. Values are types defined in the schema.org vocabulary. In the CDIF framework (and for compatibility with FDOF FDOF digitalObjectType), the schema:additionalType property should be used (see implementation table below) |
 
