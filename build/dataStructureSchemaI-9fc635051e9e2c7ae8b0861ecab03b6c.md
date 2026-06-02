# CDIF Data Structure Profile — Schema.org Implementation

This page documents the content items in the CDIF Data Structure profile and how each is encoded in JSON-LD. Some example metadata documents are accessible in the [Data Structure GitHub repository](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/tree/reviewRevision202606/examples). The 'Cardinality' value specifies how many values a property may carry: `1` means one value required; `1..*` means at least one required, repeatable; `0..*` means optional and repeatable; `0..1` means optional, single-valued.

All property names use namespace prefixes as declared in the `@context` (e.g. `schema:`, `dcterms:`, `cdi:`, `cdif:`). The CDIF JSON-LD implementation uses a hierarchical JSON structure, and CURIE syntax to abbreviate URIs using prefixes defined in the JSON-LD context. The implementation does not map un-prefixed JSON keys to URIs; rather, it prefixes a namespace abbreviation on the key label to represent the URI. This enables using standard JSON Schema to validate documents and avoids confusion about the vocabulary origin of keys used in the JSON.

The Data Structure profile builds on the CDIF Data Description profile (which describes what a dataset's variables *are*) and adds *what roles those variables play in the dataset's structure and how records are keyed*. A `schema:DataDownload` distribution gains a `cdi:isStructuredBy` link pointing to one of three concrete structures — `cdi:WideDataStructure`, `cdi:LongDataStructure`, or `cdi:DimensionalDataStructure` — each of which lists the role-typed components that make up the structure.

Each item lists its Cardinality, JSON encoding, and a Description explaining usage.

See also [graphical presentation of the Data Structure profile](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/CDIFDataStructure/index.html)

Artefacts for the Data Structure profile are in this [GitHub repository](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/tree/reviewRevision202606) (TBD — update link to release tag).

The profile's authoritative implementation guide is [CDIFDataStructureImplementationGuide.md](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/blob/reviewRevision202606/CDIFDataStructureImplementationGuide.md); this page is a content-item-focused summary derived from it.


## Profile conformance declaration
- **Cardinality:** 1..*
- **JSON:**
  ```json
  "schema:subjectOf" / "dcterms:conformsTo": [
    {"@id": "https://w3id.org/cdif/data_structure/1.1"}
  ]
  ```
- **Description:** Required URI declaring that the metadata record conforms to the Data Structure profile. Add to the `dcterms:conformsTo` array on the catalog record alongside conformsTo identifiers for any other profiles that are also being asserted (Core, Discovery, Data Description, etc.). Note that the CDIF conformance class URIs are registered such that the base URI (e.g. https://w3id.org/cdif/data_structure/1.1/) resolves to this implementation guidance page; add /schema and the uri will resolve to the JSON schema for validating instance documents using that profile; add /shacl and the shacl rules, encoded in turtle format, will be returned.

## Distribution data structure link
- **Cardinality:** 1 per conforming DataDownload
- **JSON:**
  ```json
  "schema:distribution": [{
    "@type": ["schema:DataDownload"],
    "schema:contentUrl": "...",
    "cdi:isStructuredBy": { /* inline DataStructure */ }
  }]
  ```
- **Description:** Each `schema:DataDownload` distribution that conforms to this profile must carry a `cdi:isStructuredBy` value that is either an inline `cdi:WideDataStructure`, `cdi:LongDataStructure`, or `cdi:DimensionalDataStructure`, **or** an `@id` reference to a DataStructure defined elsewhere (in the same document, or accessible on the web). Using an `@id` reference is the way the same reusable structure is shared across multiple distributions or datasets.

## Wide data structure
- **Cardinality:** referenced
- **JSON:**
  ```json
  {
    "@type": ["cdi:WideDataStructure"],
    "@id": "#wide-structure-1",
    "cdi:has_DataStructureComponent": [ /* IdentifierComponent, MeasureComponent, AttributeComponent */ ],
    "cdi:has_PrimaryKey": { /* PrimaryKey */ }
  }
  ```
- **Description:** Structure of a one-row-per-unit dataset. Each record represents properties of one unit in the population. Components must each be one of `cdif:IdentifierComponent`, `cdif:MeasureComponent`, or `cdif:AttributeComponent`. `cdi:has_PrimaryKey` and `cdi:has_ForeignKey` are optional.

## Long data structure
- **Cardinality:** referenced
- **JSON:**
  ```json
  {
    "@type": ["cdi:LongDataStructure"],
    "@id": "#long-structure-1",
    "cdi:has_DataStructureComponent": [ /* IdentifierComponent, VariableDescriptorComponent, VariableValueComponent, AttributeComponent */ ]
  }
  ```
- **Description:** Structure of an entity-attribute-value ("long") dataset. Each row contains an identifier, a code naming a variable, and the value of that variable for the identified unit. Components must each be one of `cdif:IdentifierComponent`, `cdif:VariableDescriptorComponent`, `cdif:VariableValueComponent`, or `cdif:AttributeComponent`. Primary/foreign keys are optional.

## Dimensional data structure
- **Cardinality:** referenced
- **JSON:**
  ```json
  {
    "@type": ["cdi:DimensionalDataStructure"],
    "@id": "#cube-structure-1",
    "cdi:has_DataStructureComponent": [ /* DimensionComponent, MeasureComponent, AttributeComponent */ ],
    "cdi:has_DimensionGroup": [ /* DimensionGroup */ ]
  }
  ```
- **Description:** Structure of a multidimensional ("cube") dataset. Each record is addressed by a set of dimension values. Components must each be one of `cdif:DimensionComponent`, `cdif:MeasureComponent`, or `cdif:AttributeComponent`. `cdi:has_DimensionGroup` groups dimensions that together address a coordinate position.

## Identifier component
- **Cardinality:** within `cdi:has_DataStructureComponent`, 1..*
- **JSON:**
  ```json
  {
    "@type": ["cdif:IdentifierComponent"],
    "cdif:isDefinedBy_RepresentedVariable": {"@id": "#var-unit-id"}
  }
  ```
- **Description:** Role given to a represented variable that provides identifying values for records. Used in `cdi:WideDataStructure` and `cdi:LongDataStructure`. `cdif:isDefinedBy_RepresentedVariable` is required.

## Measure component
- **Cardinality:** within `cdi:has_DataStructureComponent`, 0..*
- **JSON:**
  ```json
  {
    "@type": ["cdif:MeasureComponent"],
    "cdif:isDefinedBy_RepresentedVariable": {"@id": "#var-temperature"},
    "cdi:semantic": ["http://qudt.org/vocab/quantitykind/Temperature"]
  }
  ```
- **Description:** Role given to a represented variable that holds the observed or derived values of the dataset. Permitted in `cdi:WideDataStructure` and `cdi:DimensionalDataStructure`. (In `cdi:LongDataStructure` the measured value is carried by `cdif:VariableValueComponent` instead.) The optional `cdi:semantic` carries one or more IRIs or `cdifConceptOrTerm` references that qualify the purpose of the measure against an external controlled vocabulary.

## Attribute component
- **Cardinality:** within `cdi:has_DataStructureComponent`, 0..*
- **JSON:**
  ```json
  {
    "@type": ["cdif:AttributeComponent"],
    "cdif:isDefinedBy_RepresentedVariable": {"@id": "#var-uncertainty"},
    "cdi:qualifies": [{"@id": "#component-temperature"}]
  }
  ```
- **Description:** Role given to a represented variable that qualifies observations or provides supplementary information (e.g. uncertainty, quality flag, observation method). Permitted in all three concrete DataStructure subtypes. `cdi:qualifies` optionally points to the component(s) being qualified.

## Dimension component
- **Cardinality:** within `cdi:has_DataStructureComponent`, 1..* (in DimensionalDataStructure)
- **JSON:**
  ```json
  {
    "@type": ["cdif:DimensionComponent"],
    "cdif:isDefinedBy_RepresentedVariable": {"@id": "#var-time-bin"}
  }
  ```
- **Description:** Role given to a represented variable that acts as a coordinate axis in a multidimensional structure. Used only in `cdi:DimensionalDataStructure`. Dimensions are typically categorical (codelist-valued) or quantized continuous variables (e.g. time bins). `cdif:isDefinedBy_RepresentedVariable` is required.

## Variable descriptor component
- **Cardinality:** within `cdi:has_DataStructureComponent`, 1 (in LongDataStructure)
- **JSON:**
  ```json
  {
    "@type": ["cdif:VariableDescriptorComponent"],
    "cdif:isDefinedBy_DescriptorVariable": {
      "@type": ["cdi:DescriptorVariable"],
      "cdif:name": ["variable_name"],
      "cdif:hasValuesFrom": { /* DescriptorValueDomain mapping codes to RepresentedVariables */ }
    }
  }
  ```
- **Description:** Role given to a represented variable that holds codes identifying *which* logical variable a given long-format row records. Used only in `cdi:LongDataStructure`. `cdif:isDefinedBy_DescriptorVariable` is required and carries an inline `cdi:DescriptorVariable` whose `cdif:hasValuesFrom` is a `cdi:DescriptorValueDomain` enumerating the codes that can appear in the descriptor column, each paired (via `cdif:isDefinedBy`) with the represented variable the code names.

## Variable value component
- **Cardinality:** within `cdi:has_DataStructureComponent`, 1 (in LongDataStructure)
- **JSON:**
  ```json
  {
    "@type": ["cdif:VariableValueComponent"],
    "cdif:isDefinedBy_RepresentedVariable": {"@id": "#var-value"}
  }
  ```
- **Description:** Role given to a represented variable that carries the value of whichever logical variable the row's descriptor identifies. Used only in `cdi:LongDataStructure`. Paired with a sibling `cdif:VariableDescriptorComponent` in the same row.

## Dimension group
- **Cardinality:** in DimensionalDataStructure, 0..*
- **JSON:**
  ```json
  "cdi:has_DimensionGroup": [{
    "@type": ["cdi:DimensionGroup"],
    "@id": "#time-group",
    "cdi:has_DimensionComponent": [{"@id": "#year"}, {"@id": "#month"}, {"@id": "#day"}]
  }]
  ```
- **Description:** Groups dimension components that together address a coordinate position (e.g., a `time` group of year/month/day, a `geography` group of country/state/county). `cdi:has_DimensionComponent` references the grouped dimensions.

## Primary key
- **Cardinality:** 0..1 per DataStructure
- **JSON:**
  ```json
  "cdi:has_PrimaryKey": {
    "@type": ["cdif:PrimaryKey"],
    "cdif:isComposedOf": [
      {"cdif:isDefinedBy_RepresentedVariable": {"@id": "#var-county-fips"}},
      {"cdif:isDefinedBy_RepresentedVariable": {"@id": "#var-year"}}
    ]
  }
  ```
- **Description:** Ordered set of represented variables whose values uniquely identify a record. Array order in `cdif:isComposedOf` is the key position (no intermediate ComponentPosition wrapper). Each item references the represented variable that plays that key position.

## Foreign key
- **Cardinality:** 0..*  per DataStructure
- **JSON:**
  ```json
  "cdi:has_ForeignKey": [{
    "@type": ["cdif:ForeignKey"],
    "cdif:isComposedOf": [
      {"cdif:isDefinedBy_RepresentedVariable": {"@id": "#var-county-fips"}}
    ],
    "cdi:references": {"@id": "https://example.org/datasets/census2020#primary-key"}
  }]
  ```
- **Description:** Set of represented variables in this dataset whose values match a primary key in another dataset. `cdi:references` is an `@id` reference to the primary key of the referenced dataset.

## Represented variable
- **Cardinality:** referenced from each component via `cdif:isDefinedBy_RepresentedVariable`
- **JSON:**
  ```json
  {
    "@type": ["cdif:RepresentedVariable"],
    "@id": "#var-temperature",
    "cdif:name": ["air_temperature"],
    "cdif:displayLabel": ["Air temperature"],
    "cdif:definition": "Dry-bulb air temperature measured 2 m above ground.",
    "cdi:hasIntendedDataType": "xsd:double",
    "cdi:simpleUnitOfMeasure": "K",
    "cdi:unitOfMeasureKind": "temperature",
    "cdi:takesSubstantiveValuesFrom": {"@id": "#valuedomain-temperature"}
  }
  ```
- **Description:** A conceptual variable bound to a substantive value domain — *logical* in the sense that it is not tied to a particular physical data type or column position. The same RepresentedVariable can be referenced from components in wide / long / dimensional structures; that's what lets the same dataset be presented in more than one layout. Cardinality of inner properties is mostly `0..1`; see the [Implementation Guide](https://github.com/Cross-Domain-Interoperability-Framework/profile-datastructure/blob/reviewRevision202606/CDIFDataStructureImplementationGuide.md#cdifrepresentedvariable) for the full set.

## Variable substantive value domain
- **Cardinality:** 0..1 per RepresentedVariable
- **JSON:**
  ```json
  "cdi:takesSubstantiveValuesFrom": {
    "@type": ["cdi:SubstantiveValueDomain"],
    "@id": "#valuedomain-temperature",
    "cdif:recommendedDataType": ["xsd:double"],
    "cdi:isDescribedBy": {
      "@type": ["cdif:ValueAndConceptDescription"],
      "cdi:classificationLevel": "Ratio",
      "cdi:minimumValueInclusive": "0"
    }
  }
  ```
- **Description:** The set of valid, meaningful values for this variable. Either references a `cdif:EnumerationDomain` (for codelist-valued variables) via `cdif:takesValuesFrom`, or is described by a `cdif:ValueAndConceptDescription` (for continuous, ordinal, or pattern-constrained variables) via `cdi:isDescribedBy`.

## Variable sentinel value domain
- **Cardinality:** 0..* per RepresentedVariable
- **JSON:**
  ```json
  "cdi:takesSentinelValuesFrom": [{
    "@type": ["cdi:SentinelValueDomain"],
    "cdif:takesValuesFrom": {"@id": "#missing-codes-codelist"}
  }]
  ```
- **Description:** The sentinel (missing / not-applicable / N/A code) value domain of a RepresentedVariable. May reference one or more distinct sentinel domains (e.g., one codelist for "missing", another for "not applicable").

## Enumeration value domain
- **Cardinality:** referenced
- **JSON:**
  ```json
  {
    "@type": ["cdif:EnumerationDomain"],
    "schema:name": "Census 2020 county codes",
    "cdif:references": {"@id": "https://example.org/codelists/county-fips"},
    "cdif:purpose": "Allowed county identifiers for U.S. data"
  }
  ```
- **Description:** A wrapper allowing a CDIF Codelist (a `skos:ConceptScheme` per the Codelist profile) to be documented as an enumerated value domain. `cdif:references` is required and points to the codelist whose `skos:notation` values are the allowed values of this enumeration.

## Value description (non-enumerated)
- **Cardinality:** 0..1 per SubstantiveValueDomain
- **JSON:**
  ```json
  "cdi:isDescribedBy": {
    "@type": ["cdif:ValueAndConceptDescription"],
    "cdi:classificationLevel": "Continuous",
    "cdi:formatPattern": "#,##0.###",
    "cdi:minimumValueInclusive": "0",
    "cdi:maximumValueExclusive": "100",
    "cdi:regularExpression": "^[0-9]+(\\.[0-9]+)?$"
  }
  ```
- **Description:** Formal description of a non-enumerated value space, used when the substantive value domain is characterized by ranges, patterns, expressions, or classification level rather than a discrete list. `cdi:classificationLevel` is one of `Continuous`, `Interval`, `Nominal`, `Ordinal`, `Ratio`.

## Variable unit of measure
- **Cardinality:** 0..1 per RepresentedVariable
- **JSON:**
  ```json
  "cdi:simpleUnitOfMeasure": "K"
  ```
  or
  ```json
  "cdi:describedUnitOfMeasure": {"@id": "http://qudt.org/vocab/unit/K"}
  ```
- **Description:** Unit of measure for the variable's values. Use `cdi:simpleUnitOfMeasure` (string) for a simple label or symbol; use `cdi:describedUnitOfMeasure` (string IRI or `cdifConceptOrTerm` reference) for a structured unit drawn from a controlled vocabulary (e.g. QUDT). `cdi:unitOfMeasureKind` (e.g., "temperature", "salinity") can be added to allow translation between equivalent units.

## Variable intended datatype
- **Cardinality:** 0..1 per RepresentedVariable
- **JSON:** `"cdi:hasIntendedDataType": "xsd:double"`
- **Description:** Intended physical datatype for variable values. Use an `xsd:` datatype IRI, or a `cdifConceptOrTerm` reference for richer typing.

## Component semantic tag
- **Cardinality:** 0..* per component
- **JSON:** `"cdi:semantic": ["http://qudt.org/vocab/quantitykind/Temperature"]`
- **Description:** Qualifies the role-typed purpose of a component using one or more external controlled-vocabulary IRIs or `cdifConceptOrTerm` references. Allowed on `cdif:MeasureComponent`, `cdif:AttributeComponent`, `cdif:VariableDescriptorComponent`, `cdif:VariableValueComponent` (and on `cdif:DimensionComponent` indirectly via its represented variable).

## Component identifier
- **Cardinality:** 0..1 per component
- **JSON:** `"cdi:identifier": {"@id": "https://example.org/idmint/component-12345"}`
- **Description:** Optional reusable identifier for a component, allowing the same component definition to be referenced from multiple data structures. Value is an `@id` reference to a `schema:Identifier` (PropertyValue pattern).
