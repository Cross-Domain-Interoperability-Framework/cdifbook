# Data Structure Profile

Resources:
- [Structured JSON schema](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/blob/reviewRevision202606/cdifDataStructureStructuredSchema.json)
- [Implementation guide](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/blob/reviewRevision202606/CDIFDataStructureImplementationGuide.md)
- [SHACL rules](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/blob/reviewRevision202606/dataStructureRules.shacl)
- [JSON-LD framing](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/blob/reviewRevision202606/CDIFDataStructure-frame.jsonld)
- [Example instance files](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/tree/reviewRevision202606/examples)
- [Graphical view](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFDataStructure/index.html)

This profile is focused on the definition of a physical or logical dataset structure in a way that can be packaged and reused for documenting different datasets that have the same structure, for instance periodically released statistics reported in the same format. 

This profile adds DDI-CDI properties for describing a data structure in terms of DataStructureComponents and RepresentedVariables, primary and foreign keys, and mapping of components to their positions in a physical dataset. Value domains for variables are specified in the same way as in the Data Description profile. It has schema for Wide, Long and Dimensional datastructures. The implementation target is an rdf serialization, which is an open world logical model; users are thus free to add additional properties that they find useful for dataset documentation in their community, but these can be ignored by other users without penalty.

Requirements:

- define data structure components
- define represented variables used by each data structure components
- define or identify value domains for each represented variables
- when a reusable DataStructure is used in a dataset description, the represented variables must be mapped to instance variables.

TBD a DataStructure class that defines the file format mappings for the data structure components in a physical implemenation; The only things that the InstanceVariables can modify in datasets using the resusable DataStructure are the labels for the variables and the physicalDataType. 

See [graphical presentation of Data Structure Profile](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFDataStructure/index.html)