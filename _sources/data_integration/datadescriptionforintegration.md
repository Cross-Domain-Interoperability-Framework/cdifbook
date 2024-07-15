# Data Description for Integration

A data provider is expected to describe the data as they manage and present it, along with information about its logical contents. The user can then re-structure the data as needed for their own use, and do so programmatically. Sufficient metadata must be available to support this programmatic restructuring, without losing any of the information about the data - especially its links to semantic definitions.

A separation of concept definitions that specify semantics must be separated from the structural description of data for a useful cross-domain data description scheme.

The process for providing such detailed descriptions of data can be broken down into a series of steps:
1. Identify the logical variables in the data, where each 'variable' measures a single characteristic of a single unit type, using a consistent set of values. The possible values must be enumerated or otherwise described in a detailed fashion. Representations must be able to identify domain-agnostic semantic descriptions for each possible value, and the variable definitions themselves must similarly be independent of any domain specificity.
2. Indicate how the logical variables fit into the structure of the file, by specifying also any 'presentational' variables used for structuring the data serialization in a file. Relationship between presentation and logical variables must be specified. The complete set of variables can then be described as a 'logical record'.
3. Describe which fields are used to identify a record (the 'primary key').
4. Describe the encoding of all variables physically present in the file, and how they are sequenced and stored for programmatic retrieval.

The concept definitions that specify semantics must be separated from the structural description of data for a useful cross-domain data description scheme, along with an indication of where the semantics for both the field and the values come from. 
 
