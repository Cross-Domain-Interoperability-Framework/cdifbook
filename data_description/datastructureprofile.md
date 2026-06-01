# Data Structure Profile

This profile is focused on the definition of a physical or logical dataset structure in a way that can be packages and reused for documenting different datasets that use the same structure, for instance periodically released statistics reported in the same format. 

Conformance to this profile entails populating all mandatory content from cdifCore, using recommended discovery properties, and conforming to  additional data description profile constraints. The implementation target is an rdf serialization, which is an open world logical model; users are thus free to add additional properties that they find useful for dataset documentation in their community, but these can be ignored by other users without penalty.

Requirements:

- define data structure components
- define represented variables used by each data structure components
- define or identify value domains for each represented variables
- when a reusable DataStructure is used in a dataset description, the represented variables must be mapped to instance variables.

TBD a DataStructure class that defines the file format mappings for the data structure components in a physical implemenation; The only things that the InstanceVariables can modify in datasets using the resusable DataStructure are the labels for the variables and the physicalDataType. 

See [graphical presentation of Data Structure Profile](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFDataStructure/index.html)