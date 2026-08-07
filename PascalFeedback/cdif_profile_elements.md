# CDIF Profile Element Master Index

This master index aggregates all metadata classes and properties defined across the 7 Cross-Domain Interoperability Framework (CDIF) profiles: **Core, Discovery, Data Description, Data Structure, Codelist, Provenance, and Manifest (Packaging)**.

---

## Master Element Reference Table

| Element (Class / Property) | Profile(s) | Type / Expected Value | Cardinality / Requirement | Description & Context |
| :--- | :--- | :--- | :--- | :--- |
| [`schema:Dataset`](https://schema.org/Dataset) | Core, Discovery, Data Description, Data Structure, Provenance, Manifest | Class | **Required** | The root entity representing the dataset or digital resource. |
| ↳ [`schema:identifier`](https://schema.org/identifier) | Core, Codelist | `string.uri` OR [`schema:PropertyValue`](https://schema.org/PropertyValue) | **Required (1)** | Unique, resolvable primary identifier (e.g. DOI URL, ORCID). |
| ↳ [`schema:name`](https://schema.org/name) | Core, Discovery, Codelist | `string` | **Required (1)** | Title or human-readable name of the entity. |
| ↳ [`schema:dateModified`](https://schema.org/dateModified) | Core, Codelist | `string` (ISO 8601) | **Required (1)** | Last update timestamp for the dataset or vocabulary. |
| ↳ [`schema:subjectOf`](https://schema.org/subjectOf) | Core, Manifest | [`dcat:CatalogRecord`](https://www.w3.org/TR/vocab-dcat-3/#Class:Catalog_Record) | **Required (1)** | Links dataset to its metadata management record. |
| ↳ [`schema:license`](https://schema.org/license) | Core, Codelist | `string` OR `@id` reference | **Conditional Required** | Choice: supply `license` OR `conditionsOfAccess`. |
| ↳ [`schema:conditionsOfAccess`](https://schema.org/conditionsOfAccess) | Core, Codelist | `string` OR `@id` reference | **Conditional Required** | Choice: supply `license` OR `conditionsOfAccess`. |
| ↳ [`schema:url`](https://schema.org/url) | Core | `string.uri` | **Conditional Required** | Choice: supply landing page `url` OR a `distribution`. |
| ↳ [`schema:distribution`](https://schema.org/distribution) | Core, Data Description, Manifest | [`schema:DataDownload`](https://schema.org/DataDownload) OR [`schema:WebAPI`](https://schema.org/WebAPI) | **Conditional Required** | Choice: supply landing page `url` OR a `distribution` (1..*). |
| ↳ [`schema:description`](https://schema.org/description) | Core | `string` | Recommended (0..1) | Detailed summary of the dataset. Warns on check if missing. |
| ↳ [`schema:creator`](https://schema.org/creator) | Core | `@list` of `Person`/`Org` | Recommended (0..*) | Ordered list of creators/authors. Warns on check if missing. |
| ↳ [`schema:keywords`](https://schema.org/keywords) | Core | `string` or `DefinedTerm` | Optional (0..*) | Tags or terms classifying the resource. |
| ↳ [`schema:funding`](https://schema.org/funding) | Core | [`schema:MonetaryGrant`](https://schema.org/MonetaryGrant) | Optional (0..*) | Linked grant details and funding bodies. |
| ↳ [`schema:version`](https://schema.org/version) | Core | `string` or `number` | Optional (0..1) | Sortable version label of the dataset. |
| ↳ [`schema:inLanguage`](https://schema.org/inLanguage) | Core | `string` (ISO 639) | Optional (0..1) | Primary language of the content. |
| ↳ [`schema:datePublished`](https://schema.org/datePublished) | Core | `string` (ISO 8601) | Optional (0..1) | Date when the dataset was made public. |
| ↳ [`schema:sameAs`](https://schema.org/sameAs) | Core, Discovery | `string.uri` | Optional (0..*) | Alternate identifier URLs. |
| ↳ [`schema:relatedLink`](https://schema.org/relatedLink) | Core | [`schema:LinkRole`](https://schema.org/LinkRole) | Optional (0..*) | Links to publications, software, or tools. |
| ↳ [`schema:publishingPrinciples`](https://schema.org/publishingPrinciples) | Core | `string` OR `@id` reference | Optional (0..*) | Link to update policy/maintenance descriptions. |
| ↳ [`schema:additionalType`](https://schema.org/additionalType) | Core | `string` or `DefinedTerm` | Optional (0..*) | Secondary/semantic classification types. |
| ↳ [`prov:wasDerivedFrom`](https://www.w3.org/TR/prov-o/#wasDerivedFrom) | Core, Provenance | `@id` reference | Optional (0..*) | References upstream datasets used as input. |
| ↳ [`prov:wasGeneratedBy`](https://www.w3.org/TR/prov-o/#wasGeneratedBy) | Core, Provenance | [`prov:Activity`](https://www.w3.org/TR/prov-o/#Activity) | Optional (0..*) | Process or script that generated this dataset. |
| ↳ [`schema:variableMeasured`](https://schema.org/variableMeasured) | Discovery | [`schema:PropertyValue`](https://schema.org/PropertyValue) or [`cdi:InstanceVariable`](http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/#InstanceVariable) | Optional (0..*) | List of variables (Discovery Properties or CDI Variables). |
| ↳ [`schema:spatialCoverage`](https://schema.org/spatialCoverage) | Discovery | [`schema:Place`](https://schema.org/Place) | Optional (0..*) | Geographic boundaries of dataset collection. |
| ↳ [`schema:temporalCoverage`](https://schema.org/temporalCoverage) | Discovery | `string` OR [`time:ProperInterval`](https://www.w3.org/TR/owl-time/#class-proper-interval) | Optional (0..*) | Temporal range (ISO 8601 interval or OWL interval). |
| ↳ [`schema:measurementTechnique`](https://schema.org/measurementTechnique) | Discovery | `string` or `DefinedTerm` | Optional (0..*) | Methodology used to collect values. |
| ↳ [`dqv:hasQualityMeasurement`](https://www.w3.org/TR/vocab-dqv/#dqv:hasQualityMeasurement) | Discovery | [`dqv:QualityMeasurement`](https://www.w3.org/TR/vocab-dqv/#dqv:QualityMeasurement) | Optional (0..*) | Measured data quality parameters. |
| [`dcat:CatalogRecord`](https://www.w3.org/TR/vocab-dcat-3/#Class:Catalog_Record) | Core | Class | **Required** | Nested metadata management record. |
| ↳ [`schema:about`](https://schema.org/about) | Core | `@id` reference | **Required (1)** | Points back to the parent `schema:Dataset`. |
| ↳ [`dcterms:conformsTo`](http://purl.org/dc/terms/conformsTo) | Core, Manifest | `@id` reference | **Required (1..*)** | Conformance profile URIs (e.g. Core, Discovery). |
| [`schema:DataDownload`](https://schema.org/DataDownload) | Core, Data Description, Manifest | Class | **Required** | Digital distribution (file download). |
| ↳ [`schema:contentUrl`](https://schema.org/contentUrl) | Core | `string.uri` | **Required (1)** | Direct download URL. |
| ↳ [`schema:encodingFormat`](https://schema.org/encodingFormat) | Core | `string` | Optional (0..*) | MIME format of the file. |
| ↳ [`spdx:checksum`](http://spdx.org/rdf/terms#checksum) | Core | [`spdx:Checksum`](http://spdx.org/rdf/terms#Checksum) | Optional (0..1) | File integrity hash object. |
| ↳ [`csvw:delimiter`](https://www.w3.org/TR/tabular-metadata/#dfn-delimiter) | Data Description | `string` | **Required (1 for Tabular)** | Character separating values (e.g., `","`, `"\t"`). |
| ↳ [`csvw:header`](https://www.w3.org/TR/tabular-metadata/#dfn-header) | Data Description | `boolean` | **Required (1 for Tabular)** | Indicates if a header row is present in the file. |
| ↳ [`cdi:characterSet`](http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/#PhysicalDataSet-characterSet) | Data Description | `string` | Optional (0..1) | Character encoding (e.g., `"UTF-8"`). |
| ↳ [`cdif:hasPhysicalMapping`](https://w3id.org/cdif/data_description/1.0) | Data Description | Array of mappings | **Required (1..* for maps)** | Column-to-variable structural mappings. |
| ↳ [`schema:hasPart`](https://schema.org/hasPart) | Manifest | Array of `MediaObject`/`Work` | Optional (0..*) | Files nested inside an archive distribution. |
| [`schema:WebAPI`](https://schema.org/WebAPI) | Core | Class | **Required** | Digital distribution (service-based API). |
| ↳ [`schema:serviceType`](https://schema.org/serviceType) | Core | `string` or `DefinedTerm` | **Required (1)** | Protocol standard identifying name (e.g. OpenAPI). |
| ↳ [`schema:termsOfService`](https://schema.org/termsOfService) | Core | `string` or `LabeledLink` | **Required (1..*)** | API access conditions/credentials. |
| ↳ [`schema:potentialAction`](https://schema.org/potentialAction) | Core | [`schema:Action`](https://schema.org/Action) | **Required (1..*)** | Executable API actions. |
| [`schema:Place`](https://schema.org/Place) | Discovery | Class | **Required (if spatial)** | Spatial boundary location. |
| ↳ [`schema:geo`](https://schema.org/geo) | Discovery | Point / Bounding Box | **Required (1)** | Geographic bounds in decimal degrees on WGS 84. |
| [`time:ProperInterval`](https://www.w3.org/TR/owl-time/#class-proper-interval) | Discovery | Class | **Required (if temporal)** | Non-standard calendar temporal bounds. |
| ↳ [`time:hasBeginning`](https://www.w3.org/TR/owl-time/#property-hasBeginning) | Discovery | `@id` or value | **Required (1)** | Start boundary of the interval. |
| ↳ [`time:hasEnd`](https://www.w3.org/TR/owl-time/#property-hasEnd) | Discovery | `@id` or value | **Required (1)** | End boundary of the interval. |
| [`cdi:InstanceVariable`](http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/#InstanceVariable) | Data Description, Data Structure | Class | Optional (0..*) | A semantic variable populated in the dataset. |
| ↳ [`cdif:physicalDataType`](https://w3id.org/cdif/data_description/1.0) | Data Description | `string` | **Required (1..*)** | Physical representation data type (e.g., `float`). |
| ↳ [`cdi:takesSubstantiveValuesFrom`](http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/#RepresentedVariable-takesSubstantiveValuesFrom) | Data Description | Value Domain | Optional (0..1) | Domain defining valid values. |
| ↳ [`cdi:takesSentinelValuesFrom`](http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/#RepresentedVariable-takesSentinelValuesFrom) | Data Description | Value Domain | Optional (0..*) | Domain defining missing/refusal values. |
| [`cdi:DataStructure`](http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/#DataStructure) | Data Structure | Class | **Required (if structure)** | Physical structural layout container. |
| ↳ [`cdi:has_DataStructureComponent`](http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/#DataStructure-has_DataStructureComponent) | Data Structure | Array of Components | **Required (1..*)** | Mapped component columns. |
| [`skos:ConceptScheme`](https://www.w3.org/TR/skos-reference/#ConceptScheme) | Codelist | Class | **Required** | The vocabulary scheme definition. |
| ↳ [`skos:prefLabel`](https://www.w3.org/TR/skos-reference/#prefLabel) | Codelist | `string` | **Required (1..*)** | Title/label for vocabulary scheme. |
| ↳ [`skos:hasTopConcept`](https://www.w3.org/TR/skos-reference/#hasTopConcept) | Codelist | Array of `@id` | **Required (1..*)** | Root terms in the taxonomy. |
| [`skos:Concept`](https://www.w3.org/TR/skos-reference/#Concept) | Codelist | Class | **Required** | Individual term/code in codelist. |
| ↳ [`skos:inScheme`](https://www.w3.org/TR/skos-reference/#inScheme) | Codelist | `@id` reference | **Required (1)** | Links term back to parent ConceptScheme. |
| ↳ [`skos:definition`](https://www.w3.org/TR/skos-reference/#definition) | Codelist | `string` | **Required (1..*)** | Semantic definition of term. |
| ↳ [`skos:broader`](https://www.w3.org/TR/skos-reference/#broader) | Codelist | `@id` reference | **Conditional Required** | Required (1..*) if term has parent term. |
| ↳ [`skos:narrower`](https://www.w3.org/TR/skos-reference/#narrower) | Codelist | `@id` reference | **Conditional Required** | Required (1..*) if term has child terms. |
| [`prov:Activity`](https://www.w3.org/TR/prov-o/#Activity) | Core, Provenance | Class | **Required** | Workflow run, processing event or acquisition step. |
| ↳ [`prov:used`](https://www.w3.org/TR/prov-o/#used) | Provenance | Array of `@id` | Optional (0..*) | Datasets/files consumed by the activity. |
| ↳ [`prov:wasAssociatedWith`](https://www.w3.org/TR/prov-o/#wasAssociatedWith) | Provenance | Array of `@id` (Person/Org) | Optional (0..*) | Agents (Person/Organization) running the activity. |
| ↳ [`schema:actionProcess`](https://schema.org/actionProcess) | Provenance | [`schema:HowTo`](https://schema.org/HowTo) | Optional (0..1) | Methodology workflow script template. |
| [`prov:Entity`](https://www.w3.org/TR/prov-o/#Entity) | Provenance | Class | **Required** | Artifacts generated or consumed by activities. |
| ↳ [`prov:wasGeneratedBy`](https://www.w3.org/TR/prov-o/#wasGeneratedBy) | Provenance | `@id` reference | Optional (0..1) | The activity that produced the entity. |
| ↳ [`prov:wasDerivedFrom`](https://www.w3.org/TR/prov-o/#wasDerivedFrom) | Provenance | Array of `@id` | Optional (0..*) | Direct lineage reference to parent inputs. |
