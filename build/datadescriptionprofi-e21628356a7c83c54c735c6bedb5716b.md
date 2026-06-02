# Data Description Profile

This profile specifies metadata for describing quantitative data sets at a detailed level, sufficient to support the machine-to-machine exchange of data for processing, including links to all needed semantic artefacts (i.e., codelists, controlled vocabularies) for scientists to understand the data. The emphasis is on structural metadata describing a physical dataset instance,  to enable parsing and re-organizing data for use. The profile covers the description of wide ("unit record") data sets, long (event stream) data sets, and multi-dimensional data sets ("data cubes"). The profile uses [Schema.org](https://schema.org/) and [DDI-CDI](https://ddialliance.org/ddi-cdi), with a reliance on the Codelist profile for describing enumerated value domains. Documentation of physical dataset structure that is reusable for description of many dataset instance is specified in the Data Structure profile.

Conformance to this profile entails populating all mandatory content from cdifCore, using recommended discovery properties, and providing the additional data description constraints. The implementation target is an rdf serialization, which is an open world logical model; users are thus free to add additional properties that they find useful for dataset documentation in their community, but these can be ignored by other users without penalty.

# Requirements

This profile imports all requirements from CDIF Core and CDIF Data Discovery profile. This profile adds additional requirements:

- Define the structure of the serialization used to deliver a specific dataset representation. Focus is on columnar data represented in tables (e.g. csv—any delimited text format.) and multidimensional data represented in structured binary formats (e.g. HDF5, NetCDF). 
- Required properties
- Vocabularies used for enumerated domains
- Locators for variable values within the physical data structure (column number, hdf path…). 
- Datatypes used to represent values
- Domain for values, including substantive and sentinel values, or other restrictions on values (string length, regular expressions)
- Roles of instance variable in the data structure, e.g. measure, unit identifer, attribute, dimension. 
- Primary key-- the variable(s) that uniquely identify each data instance
- Linkage of attribute variable to variable(s) it qualifies. 
- Statistics on InstanceVariables

# Implementation

## class--Dataset properties added in Data Description Profile

(cdifhasprimarykey)=
### cdif:hasPrimaryKey
- **Cardinality:** Optional, Repeatable
- **Content:** [cdif:Key](#sec-cdifkey)
- **Description:** Primary key of the dataset: a `cdif:Key` whose `cdif:isComposedOf` is an ordered list of `cdi:ComponentPosition` wrappers. Each wrapper carries `cdi:indexes` (the `cdi:InstanceVariable` at that position, drawn from `schema:variableMeasured`, inline or `@id`-reference) and `cdi:value` (the integer position in the key, 0- or 1-based). Together the wrappers identify each data instance. Matches the canonical DDI-CDI PrimaryKey structure defined in `ddi-cdif-data-structure`.


### cdif:statistics
- **Cardinality:** Optional, Repeatable
- **Content:** [cdi:Statistics](#sec-cdistatistics), [cdi:CategoryStatistics](#sec-cdicategorystatistics), or [cdif:StatisticsCollection](#sec-cdifstatisticscollection); inline or `@id`-reference
- **Description:** Summary statistics describing the dataset's values. Each entry is a `cdi:Statistics` bundle (one or more Statistic value objects, optionally weighted by an InstanceVariable, optionally broken down by Category), a `cdi:CategoryStatistics` (per-category statistics), or a `cdif:StatisticsCollection` (groups multiple Statistics nodes and records which InstanceVariables they index). Either inline a node here, or use an `@id`-reference to one declared elsewhere in the document.


(sec-cdifinstancevariable)=
## class--InstanceVariable
A `schema:variableMeasured` item at the Data Description level is a CDIF profile of the DDI-CDI InstanceVariable. It composes the basic Discovery `variableMeasured` shape ([PropertyValue-(variableMeasured)](#sec-propertyvalue-vm)) and extends it with properties describing the variable's data type, role, source, value domain, weighting, and summary statistics. The schema.org base properties on PropertyValue (`@id`, `schema:name`, `schema:description`, `schema:alternateName`, `schema:propertyID`, `schema:measurementTechnique`, `schema:unitText`, `schema:unitCode`, `schema:minValue`, `schema:maxValue`, `schema:url`) remain available unchanged; the additions below are CDIF-specific.

### \@type
- **Cardinality:** Required, Repeatable
- **Content:** string.uri
- **Description:** MUST include both `schema:PropertyValue` and `cdi:InstanceVariable`. Additional types may be included.

### cdif:physicalDataType
- **Cardinality:** Optional, Repeatable
- **Content:** [DefinedTerm](#sec-definedterm), [skos:Concept](#sec-skosconcept), or string
- **Description:** Identifier or name for the data type concept describing the physical representation of values for this variable.


### cdif:role
- **Cardinality:** Optional
- **Content:** string (controlled-vocabulary entry)
- **Description:** Specifies the role this variable plays in a data structure. Common values: `UnitIdentifier` (names the unit a row describes), `Measure` (holds observed/derived values), `Attribute` (qualifies an observation), `Dimension` (addresses a position in a multi-dimensional value space).


### cdif:simpleUnitOfMeasure
- **Cardinality:** Optional
- **Content:** string, [DefinedTerm](#sec-definedterm), or [skos:Concept](#sec-skosconcept)
- **Description:** Simple text-based unit of measure for the values of this variable. For a controlled-vocabulary unit entry, use `cdi:describedUnitOfMeasure` instead.


### cdif:uses
- **Cardinality:** Optional, Repeatable
- **Content:** [DefinedTerm](#sec-definedterm), [skos:Concept](#sec-skosconcept), or string
- **Description:** Essentially the same as `schema:propertyID`. References to concepts that this variable measures or represents. When the dataset's distribution carries `cdi:isStructuredBy` (CDIF Data Structure profile), `cdif:uses` connects the InstanceVariable to a reusable RepresentedVariable concept.


### cdif:isDescribedBy_StatisticsCollection
- **Cardinality:** Optional
- **Content:** [cdif:StatisticsCollection](#sec-cdifstatisticscollection) or [object reference](#object-reference)
- **Description:** The StatisticsCollection holding summary / category statistics for this InstanceVariable (InstanceVariable.isDescribedBy). `cdif:` namespaced and target-suffixed because the DDI-CDI `isDescribedBy` association is polymorphic.


### cdi:function
- **Cardinality:** Optional, Repeatable
- **Content:** [DefinedTerm](#sec-definedterm), [skos:Concept](#sec-skosconcept), or string
- **Description:** Immutable characteristic of the variable such as geographic designator, weight, temporal designation, etc. (InstanceVariable.function).


### cdi:platformType
- **Cardinality:** Optional
- **Content:** [DefinedTerm](#sec-definedterm), [skos:Concept](#sec-skosconcept), or string
- **Description:** The application or technical system context in which the variable has been realized -- typically a statistical processing package or processing environment (InstanceVariable.platformType).


### cdi:source
- **Cardinality:** Optional
- **Content:** [object reference](#object-reference) or string
- **Description:** Reference capturing provenance information for this InstanceVariable (InstanceVariable.source).


### cdi:hasIntendedDataType

**Cardinality:** Optional
- **Content:** [xsdDataType](#sec-xsddatatype), [DefinedTerm](#sec-definedterm), or [skos:Concept](#sec-skosconcept)
- **Description:** The data type intended to be used by this variable, independent of its physical representation (RepresentedVariable.hasIntendedDataType). Recommended values are XML Schema datatypes; see [xsdDataType](#sec-xsddatatype).


### cdi:describedUnitOfMeasure
- **Cardinality:** Optional
- **Content:** [DefinedTerm](#sec-definedterm), [skos:Concept](#sec-skosconcept), or string
- **Description:** The unit in which the data values are measured, expressed as a controlled-vocabulary entry (RepresentedVariable.describedUnitOfMeasure). For a plain-string unit, use `cdif:simpleUnitOfMeasure` instead.


### cdi:takesSentinelValuesFrom
- **Cardinality:** Optional, Repeatable
- **Content:** [cdif:SentinelValueDomain](#sec-cdifsentinelvaluedomain) inline, or [object reference](#object-reference) (`@id` only)
- **Description:** Sentinel (missing / not-applicable) value domain(s) for this variable (RepresentedVariable.takesSentinelValuesFrom). The value MUST be a `cdif:SentinelValueDomain` node — referencing a `cdif:SubstantiveValueDomain` here is a schema violation. Added at the Data Description profile level; not present at the Discovery level; disallowed at the Data Structure level (where the property lives on the RepresentedVariable instead).


### cdi:takesSubstantiveValuesFrom
- **Cardinality:** Optional
- **Content:** [cdif:SubstantiveValueDomain](#sec-cdifsubstantivevaluedomain) inline, or [object reference](#object-reference) (`@id` only)
- **Description:** The substantive value domain for this variable -- the set of valid, meaningful values (RepresentedVariable.takesSubstantiveValuesFrom). The value MUST be a `cdif:SubstantiveValueDomain` node — referencing a `cdif:SentinelValueDomain` here is a schema violation. Added at the Data Description profile level; same profile rules as `cdi:takesSentinelValuesFrom` above.


### cdi:qualifies
- **Cardinality:** Optional
- **Content:** [object reference](#object-reference)
- **Description:** Reference to another InstanceVariable in this dataset that this variable qualifies (provides additional context for; e.g. a measurement-channel attribute qualifying a measure variable).


(sec-cdifphysicalmapping)=
## class--cdif:PhysicalMapping
Defines the physical realization of one field in a tabular or structured dataset distribution — the column index (for tabular), the locator (for structured/hierarchical formats like NetCDF/HDF5), the physical type, format pattern, length, null sequence, defaults, etc., and a `cdif:formats_InstanceVariable` reference linking the column or path back to the `cdi:InstanceVariable` it realises in the parent dataset's `schema:variableMeasured`. Each item in a distribution's `cdif:hasPhysicalMapping` array is one CdifPhysicalMapping node. When a WebAPI distribution's `schema:potentialAction/schema:result` carries `cdif:hasPhysicalMapping`, the same shape applies to the response columns and the same `@id`s are referenced (a WebAPI response is another physical realization of the same conceptual variables; do not redeclare the InstanceVariables themselves on the result).

### cdif:index
- **Cardinality:** Optional (required for tabular text)
- **Content:** integer (≥ 0)
- **Description:** Non-negative integer that orders the fields in the data structure (column number, 0-based). Required for `cdi:TabularTextDataSet`; for `cdi:StructuredDataSet` use `cdif:locator` instead.


### cdif:locator
- **Cardinality:** Optional
- **Content:** string
- **Description:** Path to the field inside a structured (hierarchical) physical container — for example a NetCDF/HDF5 group path like `/measurements/intensity`, a JSON Pointer, or a Zarr array path. Used in place of `cdif:index` for `cdi:StructuredDataSet` distributions where column-order positioning does not apply.


### cdif:format
- **Cardinality:** Optional
- **Content:** string
- **Description:** Format pattern for the field — for numbers a token like `decimal`, `scientific`, `integer`; for dates a pattern such as `YYYY/MM` or `YYYY-MM-DDTHH:mm:ssZ`; for booleans the literal token(s) used; etc.


### cdif:physicalDataType
- **Cardinality:** Optional
- **Content:** string
- **Description:** Name of the physical data type for the field as it appears in the file (e.g., `float64`, `int32`, `string`, `dateTime`). Distinct from `cdi:hasIntendedDataType` on the InstanceVariable, which is the conceptual data type.


### cdif:formats_InstanceVariable
- **Cardinality:** Required (Warning if absent)
- **Content:** [object reference](#object-reference) (`@id` to a `schema:variableMeasured` item on the parent Dataset)
- **Description:** Links this column / path back to the `cdi:InstanceVariable` it physically realises. The `@id` MUST match the `@id` of an item in the parent dataset's `schema:variableMeasured`. SHACL warns if missing (the link is what makes the mapping useful).


### cdi:length
- **Cardinality:** Optional
- **Content:** integer
- **Description:** Column width for fixed-width tabular text.


### cdi:nullSequence
- **Cardinality:** Optional
- **Content:** string
- **Description:** Literal token that represents a null/missing value for this field (e.g., `NA`, `-9999`, empty string). Becomes the null annotation for the described column.


### cdi:defaultValue
- **Cardinality:** Optional
- **Content:** string
- **Description:** Default value substituted when the field is empty.


### cdi:scale
- **Cardinality:** Optional
- **Content:** integer
- **Description:** Scale factor to apply to stored values to recover the conceptual value.


### cdi:decimalPositions
- **Cardinality:** Optional
- **Content:** integer
- **Description:** Number of decimal positions (digits after the decimal separator) used to encode the value.


### cdi:minimumLength, cdi:maximumLength
- **Cardinality:** Optional
- **Content:** integer
- **Description:** Bounds on the textual length of values for this field.


### cdi:isRequired
- **Cardinality:** Optional, default `false`
- **Content:** boolean
- **Description:** Whether a non-null value MUST be present in each row for this field.


(sec-cdifsubstantivevaluedomain)=
## class--cdif:SubstantiveValueDomain
The set of valid, meaningful values an InstanceVariable can take — distinct from sentinel (missing/not-applicable) codes, which live on a sibling `cdif:SentinelValueDomain`. Used as the value of `cdi:takesSubstantiveValuesFrom`. A single SubstantiveValueDomain node provides EITHER `cdif:takesValuesFrom` (an enumerated list of allowed values) OR `cdif:recommendedDataType` (one or more XSD data type tokens), or both.

### \@type
- **Cardinality:** Required
- **Content:** string.uri array, MUST contain `cdif:SubstantiveValueDomain`


### \@id
- **Cardinality:** Optional
- **Content:** string.uri
- **Description:** Identifier for this SubstantiveValueDomain node, used when the same domain is referenced from multiple InstanceVariables.


### cdif:takesValuesFrom
- **Cardinality:** Optional
- **Content:** [cdif:EnumerationDomain](#sec-cdifenumerationdomain) inline, or [object reference](#object-reference)
- **Description:** Enumerated list of allowed substantive values. Use when the value set is a closed vocabulary; combine with `cdif:recommendedDataType` to additionally constrain the data type.


### cdif:displayLabel
- **Cardinality:** Optional
- **Content:** string
- **Description:** Human-readable label for the domain (e.g., shown in UI).


### cdif:recommendedDataType
- **Cardinality:** Optional, Repeatable
- **Content:** [xsdDataType](#sec-xsddatatype)
- **Description:** One or more XSD data type tokens recommended for values from this domain. Required if `cdif:takesValuesFrom` is not provided; the SubstantiveValueDomain node MUST carry at least one of `cdif:takesValuesFrom` or `cdif:recommendedDataType`.


(sec-cdifsentinelvaluedomain)=
## class--cdif:SentinelValueDomain
The set of sentinel (missing / not-applicable / refusal / etc.) codes for an InstanceVariable, distinct from the substantive values the variable takes. Used as the value of `cdi:takesSentinelValuesFrom`. Same shape as `cdif:SubstantiveValueDomain` but typed `cdif:SentinelValueDomain` and intended for the non-substantive value codes (so survey "Don't know" / "Refused" codes, sensor `-9999`-style fill values, etc. are represented separately from valid measurements).

### @type
- **Cardinality:** Required
- **Content:** string.uri array, MUST contain `cdif:SentinelValueDomain`


### \@id
- **Cardinality:** Optional
- **Content:** string.uri


### cdif:takesValuesFrom
- **Cardinality:** Optional
- **Content:** [cdif:EnumerationDomain](#sec-cdifenumerationdomain) inline, or [object reference](#object-reference)
- **Description:** Enumerated list of sentinel codes (e.g., a SKOS concept scheme of missing-value codes).


### cdif:displayLabel
- **Cardinality:** Optional
- **Content:** string


### cdif:recommendedDataType
- **Cardinality:** Optional, Repeatable
- **Content:** [xsdDataType](#sec-xsddatatype)
- **Description:** Same semantics as on `cdif:SubstantiveValueDomain`. At least one of `cdif:takesValuesFrom` or `cdif:recommendedDataType` MUST be present.


(sec-cdifenumerationdomain)=
## class--cdif:EnumerationDomain

A codification vocabulary documented as an enumerated value domain — typically a SKOS ConceptScheme listing the allowed values for a `cdif:SubstantiveValueDomain` or `cdif:SentinelValueDomain`. Provides a named extension point so that an EnumerationDomain can either declare an external concept scheme via `cdif:references` or be defined inline.

### \@type
- **Cardinality:** Required
- **Content:** string.uri array, MUST contain `cdif:EnumerationDomain`


### \@id
- **Cardinality:** Optional
- **Content:** string.uri


### cdif:identifier
- **Cardinality:** Optional
- **Content:** [Identifier](#sec-propertyvalue-id)
- **Description:** Identifier for this enumerated (categorical) domain.


### schema:name
- **Cardinality:** Optional
- **Content:** string
- **Description:** Human-understandable name (linguistic signifier, word, phrase, or mnemonic) for the domain.


### cdif:references
- **Cardinality:** Optional
- **Content:** SKOS ConceptScheme inline, or [object reference](#object-reference)
- **Description:** SKOS concept scheme that contains the concepts defining the allowed values of this enumeration domain. Reference an external published vocabulary, or inline one. See [skos:Concept](#sec-skosconcept) for individual concept entries.


(sec-cdifkey)=
## class--cdif:Key
The CDIF profile of DDI-CDI PrimaryKey: an ordered set of `cdi:InstanceVariable` references that uniquely identify a data instance. Used as the value of [cdif:hasPrimaryKey](#cdifhasprimarykey) on the root Dataset. Each variable's position in the key is recorded with an explicit `cdi:ComponentPosition` wrapper carrying `cdi:indexes` (the variable) and `cdi:value` (the integer position), matching the canonical DDI-CDI PrimaryKey structure defined in `ddi-cdif-data-structure`.

### \@type
- **Cardinality:** Required -- `cdif:Key`, Repeatable
- **Content:** string.uri
- **Description:** MUST include `cdif:Key`.


### \@id
- **Cardinality:** Optional
- **Content:** string.uri
- **Description:** Identifier for this Key node.


### cdif:isComposedOf
- **Cardinality:** Required, Repeatable
- **Content:** Array of [cdi:ComponentPosition](#sec-cdicomponentposition) wrappers
- **Description:** Ordered list of `cdi:ComponentPosition` wrappers, one per key component. Each wrapper holds `cdi:indexes` (the `cdi:InstanceVariable` at that position -- inline `cdifInstanceVariable` or `@id`-reference) and `cdi:value` (the integer position, 0- or 1-based).


(sec-cdicomponentposition)=
## class--cdi:ComponentPosition
Indexes a single component within a `cdif:Key` (or other ordered DDI-CDI component structure). Used as the items of `cdif:isComposedOf` on a [cdif:Key](#sec-cdifkey): each wrapper pairs an InstanceVariable with its position number in the key.

### \@type
- **Cardinality:** Required -- 'cdi:ComponentPosition', Repeatable
- **Content:** string.uri
- **Description:** MUST include `cdi:ComponentPosition`.


### \@id
- **Cardinality:** Optional
- **Content:** string.uri
- **Description:** Identifier for this ComponentPosition node.


### cdi:indexes
- **Cardinality:** Required
- **Content:** [CdifInstanceVariable](#sec-cdifinstancevariable) or [object reference](#object-reference)
- **Description:** Reference to the `cdi:InstanceVariable` at this position. Either an inline `cdifInstanceVariable` node or an `@id`-reference to one declared elsewhere (typically in `schema:variableMeasured`).


### cdi:value
- **Cardinality:** Required
- **Content:** integer
- **Description:** Integer position of this component in the key, incrementing from 0 or 1.


(sec-cdifstatisticscollection)=
## class--cdif:StatisticsCollection
Groups one or more `cdi:Statistics` nodes. A typical use is a dataset-level collection holding row-count / mean / stddev Statistics for each measured variable. Referenced from a CdifInstanceVariable via `cdif:isDescribedBy_StatisticsCollection`, or from the root Dataset via `cdif:statistics`.

### \@id
- **Cardinality:** Optional
- **Content:** string.uri
- **Description:** Identifier for this StatisticsCollection node.


### \@type
- **Cardinality:** Required -- 'cdif:StatisticsCollection', Repeatable
- **Content:** string.uri
- **Description:** MUST include `cdif:StatisticsCollection`.


### cdif:has_Statistics
- **Cardinality:** Required, Repeatable
- **Content:** [cdi:Statistics](#sec-cdistatistics) or [object reference](#object-reference)
- **Description:** Statistics nodes carried by this collection (inline or `@id`-ref). `cdif:` namespaced and target-suffixed because the DDI-CDI `cdi:has` association is polymorphic.


### cdi:hasWeight
- **Cardinality:** Optional
- **Content:** [CdifInstanceVariable](#sec-cdifinstancevariable) or [object reference](#object-reference)
- **Description:** The InstanceVariable whose values were used as weights when computing the statistics in this collection.


### cdif:indexedBy
- **Cardinality:** Optional, Repeatable
- **Content:** [CdifInstanceVariable](#sec-cdifinstancevariable) or [object reference](#object-reference)
- **Description:** CDIF addition (not in canonical DDI-CDI): the InstanceVariable(s) the contained Statistics index -- the collection-level coordinate space.


(sec-cdistatistics)=
## class--cdi:Statistics
A named bundle of one or more Statistic value objects for an instance variable, optionally weighted, optionally broken down by Category.

### \@id
- **Cardinality:** Optional
- **Content:** string.uri
- **Description:** Identifier for this Statistics node.


### \@type
- **Cardinality:** Required -- 'cdi:Statistics', Repeatable
- **Content:** string.uri
- **Description:** MUST include `cdi:Statistics`.


### cdi:statistic
- **Cardinality:** Required, Repeatable
- **Content:** Array of Statistic value objects
- **Description:** Ordered list of Statistic value objects carried by this bundle. Order is significant -- consumers MAY rely on array position.


### cdi:typeOfStatistic
- **Cardinality:** Optional
- **Content:** [DefinedTerm](#sec-definedterm), [skos:Concept](#sec-skosconcept), or string
- **Description:** Controlled-vocabulary entry naming the kind of statistic -- e.g. mean, median, count, sum, stdDev.


### cdi:hasWeight
- **Cardinality:** Optional
- **Content:** [CdifInstanceVariable](#sec-cdifinstancevariable) or [object reference](#object-reference)
- **Description:** The InstanceVariable whose values were used as weights when computing the Statistic entries.


### cdif:appliesTo
- **Cardinality:** Optional, Repeatable
- **Content:** [CdifInstanceVariable](#sec-cdifinstancevariable) or [object reference](#object-reference)
- **Description:** CDIF addition (not in canonical DDI-CDI): the InstanceVariable(s) this Statistics bundle summarizes -- the per-bundle "what these numbers describe" link.


### cdif:has_CategoryStatistics
- **Cardinality:** Optional, Repeatable
- **Content:** [cdi:CategoryStatistics](#sec-cdicategorystatistics)
- **Description:** CategoryStatistics entries breaking this Statistics bundle down by Category. `cdif:` namespaced and target-suffixed because the DDI-CDI `cdi:has` association is polymorphic.


(sec-cdicategorystatistics)=
## class--cdi:CategoryStatistics
Statistics for a specific Category of an instance variable within a dataset.

### \@id
- **Cardinality:** Optional
- **Content:** string.uri
- **Description:** Identifier for this CategoryStatistics node.


### \@type
- **Cardinality:** Required -- 'cdi:CategoryStatistics', Repeatable
- **Content:** string.uri
- **Description:** MUST include `cdi:CategoryStatistics`.


### cdi:for
- **Cardinality:** Required
- **Content:** [skos:Concept](#sec-skosconcept) or [object reference](#object-reference)
- **Description:** The Category this CategoryStatistics is for (inline Category node or an `@id`-reference).


### cdi:statistic
- **Cardinality:** Required, Repeatable
- **Content:** Array of Statistic value objects
- **Description:** Per-category Statistic value objects.


### cdi:typeOfStatistic
- **Cardinality:** Optional
- **Content:** [DefinedTerm](#sec-definedterm), [skos:Concept](#sec-skosconcept), or string
- **Description:** Controlled-vocabulary entry naming the kind of statistic.


### cdi:hasWeight
- **Cardinality:** Optional
- **Content:** [CdifInstanceVariable](#sec-cdifinstancevariable) or [object reference](#object-reference)
- **Description:** The InstanceVariable whose values were used as weights.
