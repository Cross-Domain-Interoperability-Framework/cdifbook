# Basic discovery metadata content model

The core of the CDIF profile for resource discovery is a set of implementation-independent content requirements that specify the required information to support a basic level of discovery interoperability for resources of any type.  The following list includes the minimum required content for basic resource description, discovery, and access. This recommendation is a synthesis of various metadata schemes, including ISO 19115-1:2014, schema.org conventions from ESIPFed Science on Schema.org and Ocean Data net, DCAT, DCAT-AP, and [FDO Kernel Attributes-2.0](https://docs.google.com/document/d/1OF49wTNVuv-6OXlNerhBTqVtHyc7jutTaUHjn6BZCs0). A mapping between these various schemas and CDIF content elements is available in [TBD](tbd). Note that these content requirements are scoped for a broad spectrum of resource types. It is expected that other fields will need to be added in extensions for specific kinds of resources.

## Required
If the content of a required element does not provide useful information, the metadata is considered useless for even the most rudimentary discovery use cases. Conformant metadata MUST provide valid values, i.e., a meaningful title that identifies the resource, either a URL or text statement of how to obtain the resource, a statement of any licensing, usage, or access constraints (i.e., Rights), and identifiers for the specification of the metadata serialisation and the type of the resource described.

- **Resource identifier** (1 entry): A globally unique, resolvable identifier for the resource described by the metadata record.
- **Title** (1 entry): Succinct (preferably &lt;250 characters) name of the resource; should be sufficient to uniquely identify the resource for a human user.
- **Distribution**: URL, Distribution object, or Access Instructions (1 entry): If the resource is a digital object accessible online, provide a URL that will retrieve the resource. If the resource has multiple representations, provide a Distribution Object documenting the various options with a URL and representation profile for each. Metadata for distributions through an API that allows query, filter, or processing as part of a data access request are described in the Queryable Distribution Interfaces (API) section, below. If the resource is not accessible online, provide a URL to a landing page used to access the resource, or minimally, provide a text description explaining how to access the resource in the metadata (Access Instructions).
- **Rights** (1 to many entry): Information about required access permissions, licences, contractual requirements, use constraints, and security constraints. Might be described in text or through links to external documents. (See 6.4. Data Access for providing machine-actionable rights descriptions.)
- **Metadata profile identifier** (1 to many): Identifier for metadata specification (profile) used to create this metadata record. Generally this will be populated automatically if the metadata is created using CDIF aware tools.
- **Resource type** (1 to many): A scoped name (label with classification scheme) that specifies the kind of resource described by the metadata. The resource type might be used to determine validation requirements specific to descriptions for that kind of resource.

## Conditional elements
These are content elements for which every resource should have useful information, but for which the information may not be applicable for some kinds of resources. 

- **Variable** (0 to many entries): Required for datasets. The metadata about a dataset should include a list of variables that the dataset contains. Variable metadata should minimally specify the name of the variable as it appears in the dataset. That name should be, ideally, qualified by a controlled vocabulary or other semantic resource (e.g. represented by a resolvable URI), or minimally some descriptive text. Variable metadata should include as much content as needed for users to understand the type of the variable (e.g. measured, statistically derived, or simulated), its units, and any relevant reference systems for its values (see [Universals](../universals/univintro.md) ). Details of data structure and schema more closely related to interoperability, data integration, and usage than to data discovery are discussed in [tbd](tbd). Describing Data to Make it "Integration-Ready".
- **Temporal Coverage** (0..1 entry) Required if resource content is specific to some time interval. The time interval represented by or the subject of the described resource. This could be the time interval when data were collected, or an archaeological or geological time interval that is the subject of the resource. Need to account for clock time, calendar time (Gregorian, Julian, Hebrew, Islamic, Chinese, Mayan...), cyclical time (summer, first quarter, mating season, new moon, pay day) and for named time ordinal eras (Jurassic, Younger Dryas, Early Minoan I, Late Stone Age). See [OWL Time](https://www.w3.org/TR/owl-time/).
- **Geographic Extent** - (0..many)  Required if resource has a geographic extent for its subject. (1 entry, minimum bounding rectangle or point): Horizontal location, in order to support cross-domain searches based on geospatial location, location coordinates must be given in decimal degrees using the WGS 8486 datum. There are various other systems for describing location (see [Space](../universals/univgeography.md) ); these can be provided as alternate location descriptions, recognizing that they might be meaningful to some metadata harvesting agents. Some resources may not be usefully described by a WGS 84 extent, in which case indicate nil:notapplicable; this would include extraterrestrial resources.
  - *Bounding Rectangle*: North Bounding Latitude, South Bounding Latitude, East Bounding Longitude, West Bounding Longitude. The minimum rectangle that completely contains the coverage extent for the resource content. Coordinate order and syntax are determined by the serialisation profile.
  - *Point*: Latitude, Longitude. A centroid point for the coverage extent of the resource, or the location of the resource content if a point location is appropriate. Coordinate order and syntax are determined by the serialisation profile.
  - *Named location*: Place name referenced to some gazetteer. Use scoped name pattern {label, authority, optional identifier} (see [placename](tbd) ).
  
## Recommended
Other properties that should be specified if possible and relevant. All are optional.
- **Description** (0..1 entry): Inform the reader about the resource's content, context, provenance, and any other information deemed useful for future cross-domain usage. SHACL validation will throw warning if not present.
- **Originators** (0 to many entries): One or more parties (person or organisation) that have a role related to the origin of the resource, e.g., author or editor. Each party has a name (label), identifier, and optional contact information. SHACL validation will throw warning if not present.
- **Modified Date** (0..1 entry): Date (not temporal extent) when the most recent changes to the resource were completed. Use a "year" or [ISO 8601 date and time](https://en.wikipedia.org/wiki/ISO_8601) format. Alternative date formatting must be machine-readable and consistent across all datasets. SHACL validation will throw warning if not present.
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

## Properties for metadata management
These elements provide information for the operation of a distributed catalogue system with harvesting of metadata between catalogue servers. Values should be populated automatically by metadata creation tools, requiring no user input.  Some providers might not include this information in metadata interchange files. 
- **Metadata Date** (0..1 entry): Last metadata update/creation date-time stamp in ISO 8601 date and time format. This may be automatically updated on metadata import if a metadata format conversion is necessary.
- **Metadata Contact Agent** (0..1 entry): The party responsible for metadata content and accuracy; Agent object includes a name (label), identifier, and optional contact information
- **Metadata Identifier** (0..1 entry): The identifier for the Digital object that contains the metadata.
