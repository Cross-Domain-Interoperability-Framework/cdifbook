# Concept Scheme

A CDIF Concept Scheme is a controlled vocabulary or terminology represented as a [SKOS](https://www.w3.org/TR/skos-reference/) `ConceptScheme` in JSON-LD. It uses the same SKOS building blocks as the [Codelist](codelistprofile.md) profile, but `skos:Concept` has broader usage here: a concept can represent a possible value for a categorical variable, or an entity or property in a data model. Where the Codelist profile expects a simple, often flat list of coded values, the Concept Scheme profile accommodates richer terminologies with definitions, sources, and hierarchy.  

All property names use namespace prefixes declared in the `@context`:

```json
"@context": {
  "skos": "http://www.w3.org/2004/02/skos/core#",
  "schema": "http://schema.org/",
  "dcterms": "http://purl.org/dc/terms/"
}
```

## skos:ConceptScheme

The root object representing the concept scheme, typed as `skos:ConceptScheme`. It carries the scheme-level descriptive properties together with the mandatory CDIF Core metadata (see the [Codelist](codelistprofile.md) profile for the full required-property set).

### \@id
- **Obligation:** mandatory
- **JSON:** `"@id": "{URI}"`
- **Scope note:** Globally unique, resolvable URI for the concept scheme.

### \@type
- **Obligation:** mandatory
- **JSON:** `"@type": ["skos:ConceptScheme"]`
- **Scope note:** Must include `skos:ConceptScheme`.

### skos:prefLabel
- **Obligation:** mandatory
- **JSON:** `"skos:prefLabel": "{string}"` or an array of [LanguageTaggedValue](#languagetaggedvalue)
- **Scope note:** Preferred human-readable label that identifies the scheme. At most one per language.

### skos:definition
- **Obligation:** mandatory
- **JSON:** `"skos:definition": "{string}"`
- **Scope note:** An unambiguous statement of the meaning or purpose of the scheme.

### dcterms:source
- **Obligation:** mandatory
- **JSON:** `"dcterms:source": "{string or URI}"`
- **Scope note:** The authority for the origin of the scheme's definitions.

(sec-skosconcept)=
## skos:Concept

A `skos:Concept` within the scheme. In the Concept Scheme profile a concept may represent a possible value for a categorical variable, or an entity or property in a data model. `skos:prefLabel`, a `skos:definition`, and a source citation are required; `skos:notation` is optional and used identically to the Codelist profile. The requirements for each concept in the RDF implementation:

### \@type
- **Obligation:** mandatory
- **JSON:** `"@type": ["skos:Concept"]`
- **Scope note:** Must include `skos:Concept`.

### schema:identifier
- **Obligation:** mandatory
- **JSON:** `"@id": "{URI}"` or `"schema:identifier": "{string}"` (see [PropertyValue](#sec-propertyvalue-id))
- **Scope note:** Each concept must have a globally unique, resolvable identifier.

### skos:inScheme
- **Obligation:** mandatory
- **JSON:** `"skos:inScheme": {"@id": "{scheme URI}"}`
- **Scope note:** [Object reference](#object-reference) to the containing `skos:ConceptScheme`.

### skos:prefLabel
- **Obligation:** mandatory, repeatable
- **JSON:** `"skos:prefLabel": "{string}"` or an array of [LanguageTaggedValue](#languagetaggedvalue)
- **Scope note:** At least one preferred label; multiples are allowed but only one per language-locale (SKOS does not permit more than one `skos:prefLabel` per language).

### skos:definition
- **Obligation:** mandatory
- **JSON:** `"skos:definition": "{string}"` or [LanguageTaggedValue](#languagetaggedvalue)
- **Scope note:** A definition of the concept, also in language-specific form.

### skos:broader / skos:narrower
- **Obligation:** required for hierarchy (see scope note)
- **JSON:** `"skos:broader": [{"@id": "{parent URI}"}]`
- **Scope note:** If the scheme is hierarchical, use `skos:broader` to indicate a concept's parent. `skos:narrower` (the inverse) can also be provided, supporting navigation both up and down the hierarchy. See [Bidirectional hierarchy](#bidirectional-hierarchy).

### skos:notation
- **Obligation:** optional
- **JSON:** `"skos:notation": ["{code}"]`
- **Scope note:** A code or abbreviation, unique within the scope of the vocabulary, that denotes the concept in data instances. Not required if unique URIs are used instead; the convention must be defined in a vocabulary profile. Notation values are commonly short strings that are easier to interpret than the concept identifier.

### skos:altLabel
- **Obligation:** optional
- **JSON:** `"skos:altLabel": "{string or LanguageTaggedValue}"`
- **Scope note:** Other labels, also in language-specific form.

## Data types

This profile uses the shared [LanguageTaggedValue](#languagetaggedvalue), [object reference](#object-reference), and [PropertyValue](#sec-propertyvalue-id) patterns defined on the [Common data types](../metadata/datatypes.md) page.
