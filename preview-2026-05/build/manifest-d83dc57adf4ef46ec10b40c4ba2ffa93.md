# CDIF Manifest Profile

Resources: 
- [Structured JSON schema](https://github.com/Cross-Domain-Interoperability-Framework/profile-manifest/blob/reviewRevision202606/cdifManifestStructuredSchema.json)
- [Implementation guide](https://github.com/Cross-Domain-Interoperability-Framework/profile-manifest/blob/reviewRevision202606/CDIFManifestImplementationGuide.md)
- [SHACL rules](https://github.com/Cross-Domain-Interoperability-Framework/profile-manifest/blob/reviewRevision202606/manifestRules.shacl)
- [JSON-LD framing](https://github.com/Cross-Domain-Interoperability-Framework/profile-manifest/blob/reviewRevision202606/cdifManifest-frame.jsonld)
- [Transform RO-CRATE to/from CDIF](https://github.com/Cross-Domain-Interoperability-Framework/profile-manifest/tree/reviewRevision202606/tools)
- [Example instance files](https://github.com/Cross-Domain-Interoperability-Framework/profile-manifest/tree/reviewRevision202606/examples)
- [Graphical view](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFManifest/index.html)

## Overview
Within the set of FAIR functions supported by the CDIF guidelines is a practical need to construct packages of related files that are treated as a single resource. This requirement appears in different forms: Researchers must be able to collect and group the various resources involved in their research, so that sense can be made of it for the purposes of replication, comprehension, and reuse. Archives and repositories have a requirement for packages of related resources to be submitted and stored, and these form the basis for dissemination. There is the popular concept of a FAIR Digital Object (FDO) which can be anything FAIR – even an atomic metadata item – but in practical terms requires that coherent packages be assembled to support practical use.

In a networked scenario, it may not always be the case that every required resource is stored at the same location or is found within the same repository (even a distributed one). In such a case, the idea of a "package" is not so much a physical assembly as it is a list of needed resources and the addresses – local or otherwise – which can be used to retrieve them. Different scenarios of use will impose different restrictions on how such packages need to be stored, but in their most basic form, they are a list of resources and locations: a manifest.

This CDIF recommendation is designed to support documentation of resource packages, the core items which make up a manifest. This document outlines the basic requirement, the conceptual model, and discusses specific implementation using schema.org and RO-Crate.  

## Requirements
1. Support the retrieval of complete packages of related resources sufficient for the FAIR use of a data or metadata object
2. Contain a listing of each component part, along with needed identifiers, descriptors, typing, and charaterization so that both humans and machines can understand its content and relationship to other parts of the package
3. Provide information sufficient to retrieve each of its component parts within the context of the system or network on which it is located. This does not require that the individual parts of the package can be retrieved on the web.
4. Be self-describing so that a receiving system can determine how to process it. Ideally, it will be useable by any application which supports the type of packaging which was used to implement the CDIF model (that is, it will not require a CDIF-aware application to unpack).
5. Identify conformance to the CDIF Manifest profile, so that receiving systems can understand how to operate on it if they support the CDIF profile.

Note that functioning as an FDO is not necessarily a requirement out of the starting gate - that should be added later, once we can figure out what it means. It is not a business requirement.

## Information Model
-	Protocol Conformance Statement (Required) – a statement of the protocol used to constitute the package being described, and to which supplied information conforms (RO Crate, Frictionless Data, etc.)
-	Package identification  (R) – a unique identifier for the package, according to a known scheme
-	Package name (O) – a human-readable name for the package to help distinguish it from others.
-	Package description (O) – a human-readable description of the package and its contents and purpose.
-	Package date (O) – The data of the creation of the package (may include time).
-	Package creator (O) – Information about the creator of the package for the purposes of attribution. May contain contact information.
-	Distribution Information (conditional) – information needed to locate and retrieve the package (unless the metadata is an item inside the package).
-	Item List (R) – A list of the items which are the parts of the package. Each has an ID and a location. These may be local (within the package container) or somewhere on a network, with the provided location information being sufficient for them to be retrieved. 
-   Item type (R) - categorization of kinds of item, typically MIME type, other categories or  semantic classification also allowed (e.g., "data entities," "context entities").
-	Licensing information (R) – according to IP law, assemblages can have different licensing than their constituent parts. The license for the package is required, but more restrictive licenses may be associated with individual resources in the package.

## Implementation
This model can be implemented in different ways – it could be an RO Crate, a Frictionless Data Data Package, a schema.org DataDownload with parts, etc.  There are two main use scenarios:1) a standalone metadata record describes a packaged resource for use by metadata harvesters indexing metadata records for discovery interfaces; 2) The metadata record is included inside a data package for use by applications using the contents of the package. RO-CRATE is focused on the second case, whereas most of the CDIF effort has been in support of the first case. The CDIF data description and data structure profiles provide metadata elements to document the structure of data in individual media object in a package. 

### RO Crate 
RO Crate implementation will conform to the [RO-CRATE 1.2 specification](https://www.researchobject.org/ro-crate/specification/1.2/ro-crate-preview.html). RO-CRATE extensively uses schema.org, and is thus broadly compatible with CDIF. A cdif conformant RO-CRATE must include a CDIF metadata file in the package, and reference that file in the manifest. 

Thus a node like this must be included in the RO-CRATE metadata file:

    {
      "@id": "cdifmetadata.json",
      "@type": "File",
      "name": "CDIF metadata",
      "contentSize": "3866",
      "description": "CDIF formatted file conforming to manifest URI",
      "encodingFormat": "application/JSON-LD",
	  "dcterms:conformsTo":"https://w3id.org/cdif/manifest/1.1"
    },


Some important distinctions include: 
-	 Metadata File Descriptor -- a CreativeWork entity with \@id: "ro-crate-metadata.json" that points to the Root Data Entity via about.
- Serialize as flattened, condensed JSON-LD file. all entities (dataset, files, people, organizations) appear as top-level objects in a flat \@graph array, cross-referenced by \@id. 
-	Document root \@type "schema:CreativeWork" (same as RO Crate 1.2). 
-	Will have "ro-crate-metadata.json" as the graph \@id, and the file will be named "ro-crate-metadata.json" and appear in the root of the package.
-	Data Entities -- File (alias for MediaObject) and Dataset entities representing files and folders.
- Contextual Entities -- Person, Organization, Place, etc. entities referenced from the data entities. These match CDIF core requirements.


schema.org implementation 
- CDIF core covers the basic RO-CRATE metadata	
- Packages delivered as zipped (or similar single-file) archive are considered schema:DataDownload objects.
- indidividual parts of the package are typed as schema:MediaObject (like RO-CRATE); they are not required to have a schema:contentURL property because they're not expected to be individually downloadable.  Other \@Type can be assigned to the package parts, as well as schema:additionalTypes that don't impact the content model for the node, but assist in semantic interpretation. 

# Dataset Properties added by the CDIF Manifest Profile

## schema:Dataset {#sec-schema-dataset}

Profile module for archive distributions. Marks the catalog record as conformant to the CDIF manifest spec (https://w3id.org/cdif/manifest/1.1) and lets schema:distribution items carry schema:hasPart describing the component files inside an archive (ZIP, etc.). The base schema:distribution anyOf [DataDownload, WebAPI] contributed by cdifCore is preserved — this BB only adds property constraints, no new anyOf branch. (Merged from the previous cdifProfile/cdifArchive BB, which held only the $defs for ArchivePart; everything now lives here.)

### schema:subjectOf
- (required) conformance statement in the subjectOf/dcat:catalogRecord must include "dcterms:conformsTo" includes    "https://w3id.org/cdif/manifest/1.1"

### schema:distribution
If the DataDownload type is application/zip (might need more general way to identify bundled packages of files), then the DataDownload must have hasPart properties that are schema:MediaObject instances describing the contained files. 
- **Cardinality:** Optional

# Class Definitions

## MediaObject

### \@type
-  (Required) May include additional types for categorization.  type: array of string, must contain "schema:MediaObject", may not contain "schema:DataDownload" since the media objects in the package are not independently downloadable.
### schema:name":
- (Required) locator for the mediaObject within the package. If Some package components are remote (external to the package) this must be a resolvable locator (e.g. http URI). Type: string
### schema:description
- Description of the file content. Type: string
### schema:encodingFormat
- Type(s) of the media object. type: array of string. MIME type is expected, other classifiers  may be included
### schema:size
- File size as a schema:QuantitativeValue value, with a numeric value and unit of measure: type: schema:QuantitativeValue.
### schema:about
- For metadata sidecar files, references the data file this metadata describes. type: array of object reference to the \@id of the data file described by this sidecar.
### spdx:checksum
- checksum object contains string value calculated algorithmically from the mediaObject content to allow determination if the object has been corrupted. type: spdx:Checksum object.


## schema:QuantitativeValue"
- object that specifies a numeric value and units of measure
### schema:value":
- (required) numeric value. type: number
### schema:unitText":
- Unit of measure for size (e.g. 'byte'). type: string

				
## spdx:Checksum
### spdx:algorithm
- (Required) Name or identifier for the algorithm used to calculate the checksum. type: string
### spdx:checksumValue
- (required) the checksum string. type: string


The formats can be interchanged losslessly, Code for the tranformation is located here:

<!-- cdif-footer-include -->
:::{include} ../_static/footer.md
:::
