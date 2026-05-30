# Data Description for Integration

This version of the CDIF recommendations does not contain a full profile for the description of data integration, but this topic has received a lot of attention from the working group. An exploration into different data integration scenarios has been conducted, notably an effort to integrate data from ILO, the World Health Organization (WHO), and the SDG Indicators, with a goal of publishing this data into Google Data Commons and the knowledge graph model used there. This work is on-going, but it has demonstrated the set of metadata needed to fully describe a data integration.

This set of metadata requirements can be summarised as follows, based on the assumption that there is access to the data being integrated:
1. Detailed data description (variable-level)
2. Structural metadata of the data sets
3. Enumerated values (codelists and classifications)
4. Mappings between sets of enumerated values
5. Processing description, indicating how mappings were implemented in transformations, and what other operations were performed for data integration

While this seems like a daunting set of information, the exploratory work has shown that if the metadata are available in a sufficiently detailed form, then the actual integration itself is straightforward.

A data provider is expected to describe the data as they manage and present it, along with information about its logical contents. The user can then re-structure the data as needed for their own use, and do so programmatically. Sufficient metadata must be available to support this programmatic restructuring, without losing any of the information about the data - especially its links to semantic definitions.

The concept definitions that specify semantics must be separated from the structural description of data for a useful cross-domain data description scheme, along with an indication of where the semantics for both the field and the values come from. 

# Data structure basics

A dataset provides values for a set of variables that characterize some unit of interest. Each record in the dataset is about a particular unit or individual in the world. In the CDIF framework, a data descovery description provides basic information about the units that are the subject of a dataset, and can provide a list of variables associated with those units. The data description profile provides information about the physical representation of the values for variables, and how they are arranged to serialize in a file that can be shared between computer systems.  

The DDI-CDI model provides a framework for describing data structures. A foundation concept is the variable cascade.  A variable can be defined at the conceptual level-- independent of any particular approach to representing values the variable might have. Temperature could be considered a conceptual variable.  Conceptual variables can be represented in inforamtion systems in various ways.  Temperature can be represented with categories like 'really hot', 'hot', 'cold', or numerically with one of several quantitative scales like kelvin or farenheit. A represented variable specifies how a conceptual variable's values are quantified in an implementation independent way. A set of temperature categories can be represented using a different vocabularies; quantitative temperatures might be represented as integers or decimal numbers. An instance variable specifies a the implementation of a represented variable in a particular data set-- the exact set of strings used to represent categories, data types that are defined in programming languages, constraints on string lengths, constraints on string syntax using regular expressions, etc. 

Another foundation concept useful for describing data structures is the data structure component.  Variables in a data structure have different roles in their relationship to description of the unit that is the subject of a record. Key roles include:
- identifier: variables that serve to uniquely identify the individual that is the subject of a record
- measure: variables that quantify properties of the subject of the record
- attribute: variables that qualify the values of other variables in the dataset. 
- reference: variables the provide identifiers for linking between datasets. 
- Other more complex roles will be described later.

The data description profile is focused on describing the physical implementation of variables in a particular dataset, based on a set of instance variable descriptions and a physical mapping that documents how the values of variables and their binding to individual records are located in a file containing the dataset.   The data structure profile provides a way to describe a dataset that can be applied to more than one dataset instance, using represented variables and data structure components.  We will refer to this 'portable' data structure description as a logical data structure.  

# Data description workflow

CDIF recommends a subset of the classes in the DDI-CDI specification for data description.  For a static set of data there are four steps. For a service, where the structure and physical format of the data will depend on the service, the last two steps are not required.

The process for providing such detailed descriptions of data can be broken down into a series of steps:
1. **Describe the Data Set or Service**: Identify the logical variables in the data, where each 'variable' measures a single characteristic of a single unit type, using a consistent set of values. The possible values must be enumerated or otherwise described in a detailed fashion. Representations must be able to identify domain-agnostic semantic descriptions for each possible value, and the variable definitions themselves must similarly be independent of any domain specificity.
2. **Describe the Variables**: Indicate how the logical variables fit into the structure of the file, by specifying also any 'presentational' variables used for structuring the data serialization in a file. Relationship between presentation and logical variables must be specified. The complete set of variables can then be described as a 'logical record'.
3. **Describe the Data Structure**: Including the fields used to identify a record (the 'primary key').
4. **Describe the Physical Format of the Data**: Describe the encoding of all variables physically present in the file, and how they are sequenced and stored for programmatic retrieval.


 
