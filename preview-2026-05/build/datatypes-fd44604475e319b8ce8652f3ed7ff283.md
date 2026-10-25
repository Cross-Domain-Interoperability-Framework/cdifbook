# Common Data Types

Several JSON-LD encoding patterns recur across the CDIF profiles. They are defined once here and referenced from the individual profile pages. All property names use namespace prefixes declared in each document's `@context` (`schema:`, `skos:`, `dcterms:`, `cdi:`, `xsd:`, etc.).

(object-reference)=
## Object Reference

A reference to another node by its `@id`, used to link to an object defined elsewhere in the same document, in another CDIF document, or externally. An object reference carries no other properties — it is resolved by matching its `@id` to the full node definition.

- **JSON:** `{"@id": "uri for target"}`
- **Scope note:** An 'object reference' indicates that a property value is specified with a URI rather than an inline object. The `@id` value must be a resolvable identifier that yields a JSON object of the type the property expects. For some elements the target is an `@id` node defined elsewhere in the same document; otherwise it should be a resolvable HTTP URI for an external resource.

(languagetaggedvalue)=
## LanguageTaggedValue

An RDF literal with a language tag, serialized as a JSON-LD value object.

- **JSON:** `{"@value": "Natural Solid Material", "@language": "en"}`
- **Scope note:** `@value` is the text content; `@language` is a [BCP 47](https://www.rfc-editor.org/info/bcp47) language tag (e.g. `en`, `fr`, `de`, `sv`). Use an array of LanguageTaggedValue objects to provide a label or text in multiple languages.

(sec-definedterm)=
## DefinedTerm

A `schema:DefinedTerm` represents a concept drawn from a controlled vocabulary, providing a human-readable name together with a resolvable identifier and a link to the vocabulary that defines it.

- **JSON:**
  ```json
  {
    "@type": ["schema:DefinedTerm"],
    "schema:name": "{string}",
    "schema:identifier": "{URI}",
    "schema:inDefinedTermSet": "{URI}",
    "schema:termCode": "{string}"
  }
  ```
- **Scope note:** Use a DefinedTerm (rather than a plain string) whenever a value comes from a known vocabulary. `schema:name` is the label, `schema:identifier` the concept URI, `schema:inDefinedTermSet` the vocabulary URI, and `schema:termCode` an optional short code. A [skos:Concept](#sec-skosconcept) may be used instead where a fuller SKOS description is available.

(sec-xsddatatype)=
## xsdDataType

The XML Schema datatype that constrains the lexical form of a value, given as a CURIE in the `xsd:` namespace.

- **JSON:** `"xsd:double"` (e.g. `xsd:string`, `xsd:integer`, `xsd:double`, `xsd:date`, `xsd:dateTime`, `xsd:boolean`, `xsd:anyURI`)
- **Scope note:** Identifies the primitive datatype used to interpret values, drawn from the [XML Schema built-in datatypes](https://www.w3.org/TR/xmlschema11-2/#built-in-datatypes). Use where a variable or value domain needs an unambiguous machine-readable datatype.

(sec-propertyvalue-vm)=
## PropertyValue (variableMeasured)

The Discovery-profile shape for a `schema:variableMeasured` item: a `schema:PropertyValue` describing one variable in a dataset. The Data Description profile extends this base with additional CDIF properties.

- **JSON:**
  ```json
  "schema:variableMeasured": [{
    "@type": ["schema:PropertyValue"],
    "@id": "astm:var0011",
    "schema:propertyID": ["pato:PATO_0000025", "astm:prop/0405"],
    "schema:name": "hostMineral",
    "schema:description": "..."
  }]
  ```
- **Scope note:** Base properties available on every `variableMeasured` item: `@id`, `schema:name`, `schema:description`, `schema:alternateName`, `schema:propertyID` (URI(s) for the represented concept — the semantic linkage for the variable's meaning), `schema:measurementTechnique`, `schema:unitText`, `schema:unitCode`, `schema:minValue`, `schema:maxValue`, `schema:url`. Follows the [ESIP Science on Schema.org](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#variables) recommendation.

(sec-propertyvalue-id)=
## PropertyValue (identifier)

A `schema:PropertyValue` used to express a structured identifier when a plain resolvable URI does not make the identifier scheme or resolution process clear.

- **JSON:**
  ```json
  {
    "@type": ["schema:PropertyValue"],
    "schema:propertyID": "https://registry.identifiers.org/registry/doi",
    "schema:value": "10.5683/SP2/TTJNIU",
    "schema:url": "https://doi.org/10.5683/SP2/TTJNIU"
  }
  ```
- **Scope note:** The ESIP Science on Schema.org guidance recommends serializing identifiers as `schema:PropertyValue`. CDIF suggests using this approach only for identifiers whose URI does not make the authority clear or whose resolution process is not well known; otherwise a simple URI string is sufficient.

<!-- cdif-footer-include -->
:::{include} ../_static/footer.md
:::
