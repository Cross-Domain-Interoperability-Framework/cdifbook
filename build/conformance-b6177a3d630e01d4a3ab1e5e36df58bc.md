# Conformance with the CDIF Guidelines

CDIF is not a standard – rather, it is a recommended use of existing standards to support a greater degree of interoperability across domain and infrastructure boundaries. Thus, it is different from most technical standards: it does not establish new models of formats for metadata, but indicates how specific existing ones can be implemented to become more interoperable. There are two different levels of conformance, one requiring more strict adherence to technical implementation guidelines than the other. These are described as “forms of conformance.”

There different forms of conformance with the CDIF Guidelines are: (1) conformance at the content level, and (2) conformance at the implementation level. The difference is that while content conformance guarantees that the needed set of core information to support a FAIR function is present, it makes no guarantees regarding how that information is formatted or implemented. Thus, the use of the information may require processing or transformation to be interoperable at a machine level. While desirable, this is a lesser form of interoperability than implementation conformance. When implementations are conformant, direct machine-to-machine interoperability is possible without further processing or transformation being required. 
These two levels of conformance can be expressed as *contracts*:

## Content Conformance

The metadata instance in question satisfies all the requirements of the CDIF profile at the level of its conceptual model. All information that is required is provided, according to the requirements of the profile, even though it may be expressed in syntaxes which are not recommended by the guidelines. A mapping between the recommended implementation and the one provided must be possible, and should be provided.

## Implementation Conformance

The metadata instance in question satisfies all the requirements of the CDIF profile implementation, such that it validates against the models expressed in both the SHACL rules and the JSON Schema, and with any other specified constraints in the associated documentation.

Implementation conformance requires and builds on content conformance, because the implementations are based on the conceptual models for each profile. Ideally, every FAIR implementation exposing or consuming resources across domain or infrastructure boundaries would be implementation conformant, but that seems unlikely in the near term. CDIF hopes to promote both alignment of metadata content as well as implementation. 

The intention of the CDIF profiles is to have as small a set of required metadata as possible to support any given function, but to also have an agreed set of useful metadata which is optional but has a common expression when it is included. The existence of additional metadata is always allowed – CDIF describes a core set of information, not a closed, proscriptive one.

In order to inform metadata consumers about the profile(s) that a given metadata record conforms to at the Implementation comformance level, identifiers are assigned to each conformance class. These are resovable http URIs. By default, the identifier resolves to a web page describing the profile for humans. Using content negotiation or adding a file extension, the URI can be used get the JSON schema or SHACL rules to validate the instance.  These identifier a supplied in a CatalogRecord object for the metadata record, using the dcterms:conformsTo property. **[PDATE THIS PARAGRAPH]**

