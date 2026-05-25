# Codelist

A CDIF codelist is a controlled vocabulary or classification scheme represented as a [SKOS](https://www.w3.org/TR/skos-reference/) `ConceptScheme` serialized in JSON-LD. The profile composes the base SKOS ConceptScheme and Concept building blocks with CDIF-specific constraints: resolvable identifiers, required definitions, bidirectional hierarchy, and the mandatory CDIF Core metadata properties. It aligns with the approach described in ['Modelling of Eurostat's Statistical Classifications in ShowVoc'](https://cros.ec.europa.eu/book-page/modeling-eurostats-statistical-classifications-showvoc).

All property names use namespace prefixes declared in the `@context`:

```json
"@context": {
  "skos": "http://www.w3.org/2004/02/skos/core#",
  "schema": "http://schema.org/",
  "dcterms": "http://purl.org/dc/terms/"
}
```

Additional prefixes may be added for concept URIs (e.g. `"sf": "https://w3id.org/isample/vocabulary/sampledfeature/"`).

## Codelist concept scheme

The root object representing the controlled vocabulary or classification scheme, typed as `skos:ConceptScheme`. It carries the scheme-level properties below together with the mandatory CDIF Core metadata.

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
- **JSON:** `"skos:prefLabel": "Sampled Feature Type vocabulary"` or an array of [LanguageTaggedValue](#languagetaggedvalue)
- **Scope note:** Preferred human-readable label for the scheme. At most one per language.

### skos:hasTopConcept
- **Obligation:** 1..*
- **JSON:**
  ```json
  "skos:hasTopConcept": [
    {	"@id": "id for concept node", 
		"@type": ["skos:Concept"], 
		"skos:prefLabel": "{{string}}",
        "skos:notation": "{{code string for concept}}",
        "skos:inScheme": [
			{"@id": "{{id of this ConceptScheme}}"}
	},
	{.. possibly other codelist concept instances}
  ]
  ```
- **Scope note:** Top-level concepts that have no `skos:broader` within this scheme. The JSON-LD hierarchy is rooted here — all child concepts are reached by traversing `skos:narrower` from these top concepts. Items may be inline concept objects or [object reference](#object-reference)s.

### schema:identifier
- **Obligation:** mandatory
- **JSON:** `"schema:identifier": "https://w3id.org/isample/vocabulary/sampledfeature/"` or a [PropertyValue](#propertyvalue-identifier)
- **Scope note:** Primary identifier for the codelist (a CDIF Core metadata property). Takes precedence over `dcterms:identifier`.

### schema:dateModified
- **Obligation:** mandatory
- **JSON:** `"schema:dateModified": "2024-04-19"`
- **Scope note:** Date (ISO 8601) when the codelist was last modified. Takes precedence over `dcterms:modified`.

### schema:license / schema:conditionsOfAccess
- **Obligation:** mandatory — at least one
- **JSON:** `"schema:license": [{"@id": "https://creativecommons.org/licenses/by/4.0/legalcode"}]` or `"schema:conditionsOfAccess": ["{text}"]`
- **Scope note:** A license (URI or [object reference](#object-reference)) or a text statement of access conditions; at least one is required. `schema:license` takes precedence over `dcterms:license`.

### Optional scheme properties
- **Obligation:** optional
- **JSON -- Scope note:**
  - `schema:url` -- web page describing the codelist.
  - `schema:creator` -- Person, Organization, or `@list` of agents (author/maintainer); `dcterms:created` — original creation date.
  - `schema:version` -- version identifier for the scheme.
  - `skos:definition` -- formal explanation of the scheme's meaning or purpose.
  - `skos:altLabel`, `skos:hiddenLabel` -- alternative and search-only labels.
  - Documentation notes: `skos:note`.

(codelist-concept)=
## Codelist concept
A `skos:Concept` with CDIF constraints, representing a single term or category within the scheme.

### \@id
- **Obligation:** mandatory
- **JSON:** `"@id": "{URI}"`
- **Scope note:** Globally unique, resolvable URI for this concept.

### \@type
- **Obligation:** mandatory
- **JSON:** `"@type": ["skos:Concept"]`
- **Scope note:** Must include `skos:Concept`.

### skos:prefLabel
- **Obligation:** mandatory
- **JSON:** `"skos:prefLabel": "Natural Solid Material"` or an array of [LanguageTaggedValue](#languagetaggedvalue)
- **Scope note:** Preferred label that identifies the codelist item for human users. At most one per language (enforced by SHACL `sh:uniqueLang`).

### skos:inScheme
- **Obligation:** mandatory
- **JSON:** `"skos:inScheme": {"@id": "sf:sampledfeaturevocabulary"}`
- **Scope note:** The concept scheme(s) this concept belongs to, each given as an [object reference](#object-reference).

### skos:definition
- **Obligation:** optional
- **JSON:** `"skos:definition": "A naturally occurring solid material."`
- **Scope note:** Formal definition of this concept.

### skos:broader
- **Obligation:** required if the concept is a value of `skos:narrower` on another concept
- **JSON:** `"skos:broader": [{"@id": "sf:anysampledfeature"}]`
- **Scope note:** Broader (parent) concept(s), given as [object reference](#object-reference)s. See [Bidirectional hierarchy](#bidirectional-hierarchy). Top concepts must **not** declare `skos:broader` within the scheme.

### skos:narrower
- **Obligation:** optional, repeatable
- **JSON:** array of inline [Codelist concept](#codelist-concept) objects or [object reference](#object-reference)s
- **Scope note:** Narrower (child) concepts. Each inline child must declare `skos:broader` pointing back to this concept. Use inline objects to build the JSON tree, or `{"@id": "..."}` references.

### skos:notation
- **Obligation:** mandatory
- **JSON:** `"skos:notation": ["{{string}}"]`
- **Scope note:** Classification code(s) that identify the item for use in datasets / computer consumption. Should be unique within the scheme.

## Data types

(languagetaggedvalue)=
### LanguageTaggedValue
- **JSON:** `{"@value": "{{string}}", "@language": "{{language code}}"}`
- **Scope note:** An RDF literal with a language tag, serialized as a JSON-LD value object. `@value` is the text content; `@language` is a BCP 47 language tag (e.g. `en`, `fr`, `de`, `sv`).

(propertyvalue-identifier)=
### PropertyValue (for schema:identifier)
The ESIPfed Science On Schema.org guidance recommends serializing identifiers using the schema:PropertyValue element. CDIF suggests only using this approach for identifiers for which the URI does not make the authority clear or for which the resolution process is not well known.

- **JSON:**
  ```json
  {
    "@type": ["schema:PropertyValue"],
    "schema:propertyID": "https://registry.identifiers.org/registry/doi",
    "schema:value": "10.5683/SP2/TTJNIU",
    "schema:url": "https://doi.org/10.5683/SP2/TTJNIU"
  }
  ```
- **Scope note:** Use when the identifier is not a simple resolvable URI.

(bidirectional-hierarchy)=
## Bidirectional hierarchy
CDIF codelists require concept hierarchies to be expressed in **both** directions:

- **`skos:narrower`** is needed because the JSON-LD tree is rooted at `skos:hasTopConcept`; without it, child concepts cannot be reached by traversing the document from the root.
- **`skos:broader`** is needed for upward navigation and for display trees in vocabulary browsers and classification tools.

Any concept that appears as a value of `skos:narrower` **must** also declare `skos:broader` pointing back to its parent. Top concepts (those in `skos:hasTopConcept`) must **not** have `skos:broader` within the scheme.

```json
{
  "@id": "sf:anysampledfeature",
  "@type": ["skos:Concept"],
  "skos:prefLabel": "Any sampled feature",
  "skos:definition": "Top concept",
  "skos:inScheme": {"@id": "sf:sampledfeaturevocabulary"},
  "skos:narrower": [
    {
      "@id": "sf:earthmaterial",
      "@type": ["skos:Concept"],
      "skos:prefLabel": "Natural Solid Material",
      "skos:definition": "A naturally occurring solid material.",
      "skos:inScheme": {"@id": "sf:sampledfeaturevocabulary"},
      "skos:broader": [{"@id": "sf:anysampledfeature"}]
    }
  ]
}
```

## Array convention
Unlike other CDIF profiles, the Codelist profile does **not** require repeatable properties to always be serialized as arrays. This follows standard SKOS practice, which allows either a single string or an array for literal values. Both of these are valid:

```json
"skos:prefLabel": "Material"
```

```json
"skos:prefLabel": [
  {"@value": "Material", "@language": "en"},
  {"@value": "Matériau", "@language": "fr"}
]
```

Consumers of CDIF codelist documents should test whether a value is a string or an array before iterating.

## Validation
- **JSON Schema** validates structure: required scheme properties (`@id`, `skos:prefLabel`, `skos:hasTopConcept`, `schema:identifier`, `schema:dateModified`, and license/access), concept requirements (`@id`, `skos:inScheme`, `skos:definition`), and bidirectional hierarchy (inline narrower concepts must have `skos:broader`).
- **SHACL** validates RDF constraints: `sh:uniqueLang` on `skos:prefLabel`, `sh:class skos:ConceptScheme` on `skos:inScheme`, `sh:class skos:Concept` on `skos:broader`, and the `narrowerImpliesBroader` rule.
