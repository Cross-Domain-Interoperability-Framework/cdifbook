# Concept Scheme Profile

A CDIF Concept Scheme is a controlled vocabulary or terminology represented as a [SKOS](https://www.w3.org/TR/skos-reference/) `ConceptScheme` in JSON-LD. It uses the same SKOS building blocks as the [Codelist](codelistprofile.md) profile, but `skos:Concept` has broader usage here: a concept can represent a possible value for a categorical variable, or an entity or property in a data model. Where the Codelist profile expects a simple, often flat list of coded values, the Concept Scheme profile accommodates richer terminologies with definitions, sources, and hierarchy.  

All property names use namespace prefixes declared in the `@context`:

```json
"@context": {
  "skos": "http://www.w3.org/2004/02/skos/core#",
  "schema": "http://schema.org/",
  "dcterms": "http://purl.org/dc/terms/"
}
```

[Graphical presentation of Concept Scheme Profile](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/cdif-uml-model/cdifConceptScheme/index.html)

## skos:ConceptScheme

The root object representing the concept scheme, typed as `skos:ConceptScheme`. It carries the scheme-level descriptive properties together with the mandatory CDIF Core metadata (see the [Codelist](codelistprofile.md) profile for the full required-property set).

### \@id
- **Cardinality:** mandatory
- **JSON:** `"@id": "{URI}"`
- **Description:** Globally unique, resolvable URI for the concept scheme.

### \@type
- **Cardinality:** mandatory
- **JSON:** `"@type": ["skos:ConceptScheme"]`
- **Description:** Must include `skos:ConceptScheme`.

### skos:prefLabel
- **Cardinality:** mandatory
- **JSON:** `"skos:prefLabel": "{string}"` or an array of [LanguageTaggedValue](#languagetaggedvalue)
- **Description:** Preferred human-readable label that identifies the scheme. At most one per language.

### skos:definition
- **Cardinality:** mandatory
- **JSON:** `"skos:definition": "{string}"`
- **Description:** An unambiguous statement of the meaning or purpose of the scheme.

### dcterms:source
- **Cardinality:** mandatory
- **JSON:** `"dcterms:source": "{string or URI}"`
- **Description:** The authority for the origin of the scheme's definitions.

(sec-skosconcept)=
## skos:Concept

A `skos:Concept` within the scheme. In the Concept Scheme profile a concept may represent a possible value for a categorical variable, or an entity or property in a data model. `skos:prefLabel`, a `skos:definition`, and a source citation are required; `skos:notation` is optional and used identically to the Codelist profile. The requirements for each concept in the RDF implementation:

### \@type
- **Cardinality:** mandatory
- **JSON:** `"@type": ["skos:Concept"]`
- **Description:** Must include `skos:Concept`.

### schema:identifier
- **Cardinality:** mandatory
- **JSON:** `"@id": "{URI}"` or `"schema:identifier": "{string}"` (see [PropertyValue](#sec-propertyvalue-id))
- **Description:** Each concept must have a globally unique, resolvable identifier.

### skos:inScheme
- **Cardinality:** mandatory
- **JSON:** `"skos:inScheme": {"@id": "{scheme URI}"}`
- **Description:** [Object reference](#object-reference) to the containing `skos:ConceptScheme`.

### skos:prefLabel
- **Cardinality:** mandatory, repeatable
- **JSON:** `"skos:prefLabel": "{string}"` or an array of [LanguageTaggedValue](#languagetaggedvalue)
- **Description:** At least one preferred label; multiples are allowed but only one per language-locale (SKOS does not permit more than one `skos:prefLabel` per language).

### skos:definition
- **Cardinality:** mandatory
- **JSON:** `"skos:definition": "{string}"` or [LanguageTaggedValue](#languagetaggedvalue)
- **Description:** A definition of the concept, also in language-specific form.

### skos:broader / skos:narrower
- **Cardinality:** required for hierarchy (see Description)
- **JSON:** `"skos:broader": [{"@id": "{parent URI}"}]`
- **Description:** If the scheme is hierarchical, use `skos:broader` to indicate a concept's parent. `skos:narrower` (the inverse) can also be provided, supporting navigation both up and down the hierarchy. See [Bidirectional hierarchy](#bidirectional-hierarchy).

### skos:notation
- **Cardinality:** optional
- **JSON:** `"skos:notation": ["{code}"]`
- **Description:** A code or abbreviation, unique within the scope of the vocabulary, that denotes the concept in data instances. Not required if unique URIs are used instead; the convention must be defined in a vocabulary profile. Notation values are commonly short strings that are easier to interpret than the concept identifier.

### skos:altLabel
- **Cardinality:** optional
- **JSON:** `"skos:altLabel": "{string or LanguageTaggedValue}"`
- **Description:** Other labels, also in language-specific form.

## Data types

This profile uses the shared [LanguageTaggedValue](#languagetaggedvalue), [object reference](#object-reference), and [PropertyValue](#sec-propertyvalue-id) patterns defined on the [Common data types](../metadata/datatypes.md) page.
