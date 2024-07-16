# Technical expression of metadata
The Internet and World Wide Web constitute an immense body of data, information, and knowledge accessible to anyone with a computer that can interact with the system. The Web can be thought of as a library containing a large fraction of the written, recorded, and graphic works of humanity, or as a database containing an almost unimaginable spectrum of data from scientific research, historical records, government records, sensor networks, etc.

The challenge with this mountain of resources is to find particular nuggets useful to answer questions and solve problems. Addressing this problem requires, _first_, understanding what kinds of things the World Wide Web offers, referred to as '**resources**' in this discussion. _Second_, how are these resources described and documented such that they are useful? _Third_, how can descriptions of resources be made available on the Web to guide users to find and use the resources? In the CDIF framework we think of things on the World Wide Web as Digital Objects (First issue). Metadata objects are a special kind of digital object that describes and documents things on the web (Second issue). The third issue is addressed using existing web technology and emerging conventions. 

## Digital Objects
A resource is an identifiable thing of interest to someone; it might be a Digital Object (DO) or a Non-digital Resource. A Digital Object is a packaged, identifiable sequence of digital bits that carries some information. A Digital Object has exactly one digital representation: a bitstream. The Digital Object bitstream might be the resource of interest, or it might be a representation of an abstract or physical resource that cannot be transmitted electronically (see [HTTP Range-14](https://en.wikipedia.org/wiki/HTTPRange-14) ). The identifier for a Digital Object can be dereferenced to access the object directly. A non-digital resource is a material entity (e.g. person, rock sample), an abstract entity (e.g. Donald Duck, The Land of Oz), or a 'Work' or 'Expression' in the [FRBR sense](https://www.loc.gov/cds/downloads/FRBR.PDF) (e.g. Beethoven's 9th Symphony, Dickens' 'Tale of Two Cities'). Identifiers for non-digital resources must dereference on the Web to a Digital Object that is a representation of the non-digital thing and can be transmitted electronically.

In the FAIR Digital Object Framework (FDOF), a Digital Object (DO) is a specific bit stream that carries some information and has a persistent, registered, resolvable identifier (PID) that can be resolved to obtain a PID kernel record. Note that 'Digital Object' is capitalised in this document to emphasise that it is being used in this specific sense. The PID kernel record provides documentation for the source of the PID, expected lifetime, type of resource it identifies, linkage to the resource it identifies, and other attributes specified in a schema identified in the PID kernel record [PID profile (Weigel et al., 2018)](https://doi.org/10.15497/RDA00031). The PID kernel record is a metadata record conforming to a particular PID profile.

Digital Objects are FAIR (FDOs) when they are part of an ecosystem comprising services and infrastructure to support realisation of the FAIR (Findable, Accessible, Interoperable, Reusable) principles. In the FAIR Digital Object Framework (FDOF) there must be a mechanism to access either the object or its metadata by dereferencing the object’s PID. Metadata content must enable the identified resource to be found, used and cited, enable interoperability and reuse, and include machine-actionable statements about dependencies and licensing. [Bonino et al. (2022)](https://fairdigitalobjectframework.org/) propose some approaches to access the FDOFIdentifierRecord (Kernel metadata) and other FDOF requirements; the level of adoption for this approach is uncertain.

## Metadata
In order for resources to be discoverable on the Web, the search applications that are used to find things must locate some representation of the resource and must be able to parse that representation and generate indexes for searching. In the realm of linked HTML Web pages, search engines parse the text content and links on Web pages to create text-based indexes and use links to find other pages to crawl the Web. This approach does not work for datasets, images, sound recordings, videos, and other non-narrative text resources, so separate representations of their content are constructed as metadata, in a format that can be parsed and indexed by search applications. This pattern is also applied to text resources to provide more explicit documentation. At the simplest level the content of the metadata can consist of text describing the resource and a link to access the resource, analogous to what is included on the cards in a legacy library card catalogue. Representing the metadata content using a structured, machine-readable format makes the information more precise and accessible to software agents.

### Metadata content
In the digital world, a wide variety of metadata schemes have evolved for describing resources. These schemes are structured to allow a richer understanding of the information, and typically at least include information about the set of fifteen generic elements identified as the Dublin Core: Creator, Contributor, Publisher, Title, Date, Language, Format, Subject, Description, Identifier, Relation, Source, Type, Coverage, and Rights, [first drafted at a 1995 meeting in Dublin, Ohio](https://www.dublincore.org/resources/metadata-basics/). These elements are defined at an abstract level and served well with free text content values for use by humans. Such semi-structured metadata is insufficient to support machine-actionable reuse of the described resources.  In order to be machine-actionable, the structure, syntax, and element-value representations in a metadata document must conform to conventions that client software can be programmed to parse and ‘understand’. ‘Understand’ in this context means recognise the incoming bitstream content and take appropriate, useful action. The metadata provider must communicate the conventions used to serialise the metadata they provide. Ideally this is done with an identifier for a specification document that details the conventions used. Some widely used metadata specifications include [DCAT](https://www.w3.org/TR/vocab-dcat-3/), [DataCite Schema](https://schema.datacite.org/), [ISO 19115-1](https://www.iso.org/standard/53798.html), [EML](https://eml.ecoinformatics.org/), [FGDC CSDGM](https://www.fgdc.gov/metadata/csdgm-standard), [CERIF](https://eurocris.org/eurocris_archive/cerifsupport.org/cerif-in-brief/index.html), [schema.org](https://schema.org/), and [DDI](https://ddialliance.org/Specification/). These specifications determine the structure and syntax of metadata documents, but leave latitude on how the values of some metadata elements are represented, and often offer multiple valid approaches to representing the same information.

### Metadata profiles
Achieving the level of metadata interoperability required for CDIF will require the adoption of one (or a small number of) metadata specification(s), along with more specific conventions on vocabularies used for metadata properties. We refer to such a set of specific conventions as a profile. CDIF provides recommendations for a metadata profile compatible with machine processing. This profile includes the base file MIME-type, an information model for the metadata content, and how that information is represented both syntactically and semantically. Most profiles are based on an existing metadata specification, e.g. schema.org, DCAT, ISO19115-1, EML, DDI-CDI, but provide additional detail to resolve ambiguities in the base specification, or rules for vocabularies and data types for element values that extend or restrict the base specification. The simplest presentation of a profile specification can be a text document that describes the information required, identifies the base specification, and states any conventions or rules for profile conformance. Such a document could be used by a software developer writing code to use information in metadata conforming to the profile. Profiles might also be specified in a machine-actionable way, e.g., the [Dublin Core Tabular Application Profiles (DCTAP)](https://www.dublincore.org/specifications/dctap/), [Profiles Vocabulary](https://www.w3.org/TR/dx-prof/), [SHACL rules](https://www.w3.org/TR/shacl/), [XML schematron rules](https://www.schematron.com/), or other schema or rule representations. Using a rule-based representation for metadata profiles provides an approach to defining and communicating metadata constraints that can be validated automatically to support metadata profile interoperability, reusability, and quality.