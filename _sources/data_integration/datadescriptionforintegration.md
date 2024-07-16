# Data Description for Integration

A data provider is expected to describe the data as they manage and present it, along with information about its logical contents. The user can then re-structure the data as needed for their own use, and do so programmatically. Sufficient metadata must be available to support this programmatic restructuring, without losing any of the information about the data - especially its links to semantic definitions.

The concept definitions that specify semantics must be separated from the structural description of data for a useful cross-domain data description scheme, along with an indication of where the semantics for both the field and the values come from. 

CDIF recommends a subset of the classes in the DDI-CDI specification for data description.  For a static set of data there are four steps. For a service, where the structure and physical format of the data will depend on the service, the last two steps are not required.

The process for providing such detailed descriptions of data can be broken down into a series of steps:
1. **Describe the Data Set or Service**: Identify the logical variables in the data, where each 'variable' measures a single characteristic of a single unit type, using a consistent set of values. The possible values must be enumerated or otherwise described in a detailed fashion. Representations must be able to identify domain-agnostic semantic descriptions for each possible value, and the variable definitions themselves must similarly be independent of any domain specificity.
2. **Describe the Variables**: Indicate how the logical variables fit into the structure of the file, by specifying also any 'presentational' variables used for structuring the data serialization in a file. Relationship between presentation and logical variables must be specified. The complete set of variables can then be described as a 'logical record'.
3. **Describe the Data Structure**: Including the fields used to identify a record (the 'primary key').
4. **Describe the Physical Format of the Data**: Describe the encoding of all variables physically present in the file, and how they are sequenced and stored for programmatic retrieval.


 
