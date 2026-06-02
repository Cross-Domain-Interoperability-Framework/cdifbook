# CDIF Profiles

## Profile Content

A CDIF profile is a set of recommended common metadata to be provided in support of a function which implements the FAIR principles, sufficient for use across domain and infrastructure boundaries. The profiles do not describe comprehensive sets of metadata – every FAIR resource and domain is different, and may require specialized metadata description.

Each CDIF profile has a stated purpose and set of requirements. Based on this purpose and the requirements, the profile is represents is an implementation-independent conceptual model. To make the profile useable, CDIF recommends a specific implementation. In some cases there might be more than one such implementation, based on the existing culture of practice. Regardless, the conceptual model is consistent across all implementations. This conceptual model is documented here for each profile in text, listing the required information items, in some cases supplemented by a formal UML model.

For technical use, a set of artefacts is made available for each profile implementation, published in a GitHub repository. These artefacts include:

1. A specification of all implemented classes, properties, and datatypes in a text document labeled “Implementation Guide.” This serves as the core documentation of what each implemented profile contains.

2. A JSON Schema for validating JSON instance documents, and for helping developers understand what is included in the profile and requires or is available for support in applications. Because the implementation uses JSON-LD, these JSON schema require instance documents to be in compacted form (see [CDIF profiles metadata validation](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/docs/CDIF-profiles-metadata-validation.md))

3. A set of SHACL rules for RDF validation of JSON-LD instances, and to help Linked Data developers understand what is available within a CDIF graph. The SHACL rules can be used to validate metadata instances in any JSON-LD serialization (compacted, flattened, expanded)

4. A set of example instances, showing how the conforming metadata should appear in JSON-LD.

5. A JSON Framing document. This is a special JSON-LD document that maps JSON-LD keys to a particular compacted JSON structure, in this case the structure expected by the CDIF JSON schema. This framed format is a typical JSON hierarchical tree structure with nested inline properties, typically much easier for humans to understand. Framing allows any instance document to be validated with the JSON schema. Each profile repository also includes a python program (FrameAndValidate.py) that takes a JSON-LD document as input, applies the framing document and validates with the JSON schema in that repository. This is documented in the repository readme.md file.

6. For those profiles which have been implemented as a UML model, hyper-linked field-level documentation will be made available as an html document, connecting specific classes to their expression in implementation artefacts such as SHACL and JSON Schema, as well as in a version of the model expressed in the XMI interchange format.


## Overview of Profiles in Version 1.1

This section provides a brief overview of the currently targeted profiles. More details are presented in subsequent sections.

### Core
The CDIF Core profile defines the mandatory and optional base properties for any CDIF metadata record, implemented as JSON-LD using the schema.org vocabulary.  The Core profile release repository is here: [https://github.com/Cross-Domain-Interoperability-Framework/profile-core](https://github.com/Cross-Domain-Interoperability-Framework/profile-core/blob/reviewRevision202606/README.md)

### Data Discovery
 The Discovery profile defines optional properties for documenting spatial or temporal extent, and simple documentation of variables specified in a resource. This recognizes that there are a variety of resources of interest that might not have relevant spatial or temporal extent, and might not explicitly define variables with values. The Discovery release repository is here: [https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery](https://github.com/Cross-Domain-Interoperability-Framework/profile-discovery/blob/reviewRevision202606/README.md}

### Data description
The CDIF Data Description profile defines metadata elements for documentation of variable value domains, statistics aggregating variable values, physical data file layout, and roles of variables in a dataset (e.g. identifier, measure, attribute).  The Data Description release repository is here: [https://github.com/Cross-Domain-Interoperability-Framework/profile-datadescription](https://github.com/Cross-Domain-Interoperability-Framework/profile-datadescription/blob/reviewRevision202606/README.md}

### Codelist

The CDIF Codelist profile defines how controlled vocabularies and classification schemes are represented as SKOS ConceptSchemes in JSON-LD. The profile composes skos:ConceptScheme and skos:Concept with CDIF-specific requirements inherited from cdifCore.  Concept properties include a preferred label, bidirectional hierarchy, notation.  CDIF core metadata properties are included on the ConceptScheme. The key feature of the codelist is specification of the 'notation' for a concept -- the strings that actually appear in data, along with a human-readable lable conveying the meaning of the code.

The implementation uses the SKOS (Simple Knowledge Organization System) vocabulary with JSON-LD serialization. This profile aligns with the approach described in ['Modelling of Eurostat's Statistical Classifications in ShowVoc'](https://cros.ec.europa.eu/book-page/modeling-eurostats-statistical-classifications-showvoc), but in alignment with cdifCore, the required properties from cdifCore are implemented using schema.org elements. The Codelist release repository is here: [https://github.com/Cross-Domain-Interoperability-Framework/profile-codelist](https://github.com/Cross-Domain-Interoperability-Framework/profile-codelist)