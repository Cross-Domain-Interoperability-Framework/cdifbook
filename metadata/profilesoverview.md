# Overview of CDIF Profiles

# Profile Content

A CDIF profile is a set of recommended common metadata to be provided in support of a function which implements the FAIR principles, sufficient for use across domain and infrastructure boundaries. The profiles do not describe comprehensive sets of metadata – every FAIR resource and domain is different, and may require specialized metadata description.

Each CDIF profile has a stated purpose and set of requirements. Based on this purpose and the requirements, the profile is represents is an implementation-independent conceptual model. To make the profile useable, CDIF recommends a specific implementation. In some cases there might be more than one such implementation, based on the existing culture of practice. Regardless, the conceptual model is consistent across all implementations. This conceptual model is documented here for each profile in text, listing the required information items, in some cases supplemented by a formal UML model.

For technical use, a set of artefacts is made available for each profile implementation, published in a GitHub repository. These artefacts include:
1. A specification of all implemented classes, properties, and datatypes in a text document labeled “Implementation Guide.” This serves as the core documentation of what each implemented profile contains.
2. A JSON Schema for validating JSON instance documents, and for helping developers understand what is included in the profile and requires or is available for support in applications. Because the implementation uses JSON-LD, these JSON schema require instance documents to be in compacted form (see [CDIF profiles metadata validation](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/docs/CDIF-profiles-metadata-validation.md))
3. A set of SHACL rules for RDF validation of JSON-LD instances, and to help Linked Data developers understand what is available within a CDIF graph. The SHACL rules can be used to validate metadata instances in any JSON-LD serialization (compacted, flattened, expanded)
4. A set of example instances, showing how the conforming metadata should appear in JSON-LD.
5. A JSON Framing document. This is a special JSON-LD document that maps JSON-LD keys to a particular compacted JSON structure, in this case the structure expected by the CDIF JSON schema. This framed format is a typical JSON hierarchical tree structure with nested inline properties, typically much easier for humans to understand. Framing allows any instance document to be validated with the JSON schema. Each profile repository also includes a python program (FrameAndValidate.py) that takes a JSON-LD document as input, applies the framing document and validates with the JSON schema in that repository. This is documented in the repository readme.md file.
6. For those profiles which have been implemented as a UML model, hyper-linked field-level documentation will be made available as an html document, connecting specific classes to their expression in implementation artefacts such as SHACL and JSON Schema, as well as in a version of the model expressed in the XMI interchange format.


# Overview of Profiles in Version 1.1

This section provides a brief overview of the currently targeted profiles. More details are presented in subsequent sections.

## Core
The CDIF Core profile defines the mandatory and optional base properties for any CDIF metadata record, implemented as JSON-LD using the schema.org vocabulary.  The Core profile release repository is here: [https://github.com/Cross-Domain-Interoperability-Framework/profile-core](https://github.com/Cross-Domain-Interoperability-Framework/profile-core/blob/reviewRevision202606/README.md)

Resources: 
- [Structured JSON schema](https://github.com/Cross-Domain-Interoperability-Framework/profile-core/blob/reviewRevision202606/cdifCoreStructuredSchema.json)
- [Implementation guide](https://github.com/Cross-Domain-Interoperability-Framework/profile-core/blob/reviewRevision202606/CDIFCoreImplementationGuide.md)
- [SHACL rules](https://github.com/Cross-Domain-Interoperability-Framework/profile-core/blob/reviewRevision202606/coreRules.shacl)
- [JSON-LD framing](https://github.com/Cross-Domain-Interoperability-Framework/profile-core/blob/reviewRevision202606/cdifCore-frame.jsonld)
- [Example instance files](https://github.com/Cross-Domain-Interoperability-Framework/profile-core/tree/reviewRevision202606/examples)
- [Graphical view](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFCore/index.html)

## Data Discovery
 The Discovery profile defines optional properties for documenting spatial or temporal extent, and simple documentation of variables specified in a resource. This recognizes that there are a variety of resources of interest that might not have relevant spatial or temporal extent, and might not explicitly define variables with values. The Discovery release repository is here: [https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/blob/reviewRevision202606/README.md)
 
 Resources: 
- [Structured JSON schema](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/blob/reviewRevision202606/cdifDiscoveryStructuredSchema.json)
- [Implementation guide](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/blob/reviewRevision202606/CDIFDiscoveryImplementationGuide.md)
- [SHACL rules](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/blob/reviewRevision202606/discoveryRules.shacl)
- [JSON-LD framing](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/blob/reviewRevision202606/cdifDiscovery-frame.jsonld)
- [Example instance files](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/tree/reviewRevision202606/examples)
- [Graphical view](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFDiscovery/index.html)

## Data Description
The CDIF Data Description profile defines metadata elements for documentation of variable value domains, statistics aggregating variable values, physical data file layout, and roles of variables in a dataset (e.g. identifier, measure, attribute).  The Data Description release repository is here: [https://github.com/Cross-Domain-Interoperability-Framework/profile-datadescription](https://github.com/Cross-Domain-Interoperability-Framework/profile-datadescription/blob/reviewRevision202606/README.md)

Resources:
- [Structured JSON schema](https://github.com/Cross-Domain-Interoperability-Framework/profile-datadescription/blob/reviewRevision202606/cdifDataDescriptionStructuredSchema.json)
- [Implementation guide](https://github.com/Cross-Domain-Interoperability-Framework/profile-datadescription/blob/reviewRevision202606/CDIFDataDescriptionImplementationGuide.md)
- [SHACL rules](https://github.com/Cross-Domain-Interoperability-Framework/profile-datadescription/blob/reviewRevision202606/dataDescriptionRules.shacl)
- [JSON-LD framing](https://github.com/Cross-Domain-Interoperability-Framework/profile-datadescription/blob/reviewRevision202606/cdifDataDescription-frame.jsonld)
- [Example instance files](https://github.com/Cross-Domain-Interoperability-Framework/profile-datadescription/tree/reviewRevision202606/examples)
- [Graphical view](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFDataDescription/index.html)

## Codelist

The CDIF Codelist profile defines how controlled vocabularies and classification schemes are represented as SKOS ConceptSchemes in JSON-LD. The profile composes skos:ConceptScheme and skos:Concept with CDIF-specific requirements inherited from cdifCore.  Concept properties include a preferred label, bidirectional hierarchy, notation.  CDIF core metadata properties are included on the ConceptScheme. The key feature of the codelist is specification of the 'notation' for a concept -- the strings that actually appear in data, along with a human-readable lable conveying the meaning of the code.

The implementation uses the SKOS (Simple Knowledge Organization System) vocabulary with JSON-LD serialization. This profile aligns with the approach described in ['Modelling of Eurostat's Statistical Classifications in ShowVoc'](https://cros.ec.europa.eu/book-page/modeling-eurostats-statistical-classifications-showvoc), but in alignment with cdifCore, the required properties from cdifCore are implemented using schema.org elements. The Codelist release repository is here: [https://github.com/Cross-Domain-Interoperability-Framework/profile-codelist](https://github.com/Cross-Domain-Interoperability-Framework/profile-codelist/blob/reviewRevision202606/README.md)

Resources:
- [Structured JSON schema](https://github.com/Cross-Domain-Interoperability-Framework/profile-codelist/blob/reviewRevision202606/CDIFCodelistProfileStructuredSchema.json)
- [Implementation guide](https://github.com/Cross-Domain-Interoperability-Framework/profile-codelist/blob/reviewRevision202606/CDIFCodelistImplementationGuide.md)
- [SHACL rules](https://github.com/Cross-Domain-Interoperability-Framework/profile-codelist/blob/reviewRevision202606/rules.shacl)
- [JSON-LD framing](https://github.com/Cross-Domain-Interoperability-Framework/profile-codelist/blob/reviewRevision202606/CDIFCodelist-frame.jsonld)
- [Example instance files](https://github.com/Cross-Domain-Interoperability-Framework/profile-codelist/tree/reviewRevision202606/Examples)
- [Graphical view](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFCodelist/index.html)

## Data Structure

This profile supports the description of reusable data structures and/or their component variables. Enumerated values for variables are described using the Codelist profile. This profile primarily uses the DDI-CDI standard. 

Resources:
- [Structured JSON schema](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/blob/reviewRevision202606/cdifDataStructureStructuredSchema.json)
- [Implementation guide](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/blob/reviewRevision202606/CDIFDataStructureImplementationGuide.md)
- [SHACL rules](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/blob/reviewRevision202606/dataStructureRules.shacl)
- [JSON-LD framing](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/blob/reviewRevision202606/CDIFDataStructure-frame.jsonld)
- [Example instance files](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/tree/reviewRevision202606/examples)
- [Graphical view](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFDataStructure/index.html)

## Concept Scheme

This profile uses SKOS to describe concept systems which are meaningful for purposes other than the representation of variable values. Domain ontologies may need to be expressed for FAIR use: this profile is intended as a supplement to ontologies described in OWL as it can be rendered using tools such as SKOSify from them, so that they are more widely accessible.

Resources:
- [Structured JSON schema](https://github.com/Cross-Domain-Interoperability-Framework/profile-conceptscheme/blob/reviewRevision202606/cdifConceptSchemeStructuredSchema.json)
- [Implementation guide](https://github.com/Cross-Domain-Interoperability-Framework/profile-conceptscheme/blob/reviewRevision202606/CDIFConceptSchemeImplementationGuide.md)
- [SHACL rules](https://github.com/Cross-Domain-Interoperability-Framework/profile-conceptscheme/blob/reviewRevision202606/conceptSchemeRules.shacl)
- [JSON-LD framing](https://github.com/Cross-Domain-Interoperability-Framework/profile-conceptscheme/blob/reviewRevision202606/cdifConceptScheme-frame.jsonld)
- [Example instance files](https://github.com/Cross-Domain-Interoperability-Framework/profile-conceptscheme/tree/reviewRevision202606/examples)
- [Graphical view](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/cdifConceptScheme/index.html)

## Manifest

This profile is used to package metadata and FAIR resources, to form bundles for dissemination, archiving, etc. It can be used to render "webby" FDOs. The recommended implementation uses [RO Crate](https://www.researchobject.org/ro-crate/).

Resources: 
- [Structured JSON schema](https://github.com/Cross-Domain-Interoperability-Framework/profile-manifest/blob/reviewRevision202606/cdifManifestStructuredSchema.json)
- [Implementation guide](https://github.com/Cross-Domain-Interoperability-Framework/profile-manifest/blob/reviewRevision202606/CDIFManifestImplementationGuide.md)
- [SHACL rules](https://github.com/Cross-Domain-Interoperability-Framework/profile-manifest/blob/reviewRevision202606/manifestRules.shacl)
- [JSON-LD framing](https://github.com/Cross-Domain-Interoperability-Framework/profile-manifest/blob/reviewRevision202606/cdifManifest-frame.jsonld)
- [Transform RO-CRATE to/from CDIF](https://github.com/Cross-Domain-Interoperability-Framework/profile-manifest/tree/reviewRevision202606/tools)
- [Example instance files](https://github.com/Cross-Domain-Interoperability-Framework/profile-manifest/tree/reviewRevision202606/examples)
- [Graphical view](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFManifest/index.html)

## Access 

This is a general guideline for the use of the [Open Digital Rights Language (ODRL)](https://www.w3.org/TR/odrl-model/) to describe the policies for the access conditions and use of FAIR resources.

## Universals

Time, geography, and units of measure are used ubiquitously in data and metadata, and are important for the integration and use of data. This profile gives some general guidance on these important topics.