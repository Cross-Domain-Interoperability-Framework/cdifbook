# Concept Schema

## skos:ConceptScheme
 
### skos:prefLabel
- **Obligation:** mandatory
- **JSON:** `"skos:prefLabel":{string}`
- **Scope note:** A string that identifies the codelist item for human users..

### skos:definition
- **Obligation:** mandatory
- **JSON:** `"skos:definition":{string}`
- **Scope note:** A string that provides an unambigous definiton of what the contact quantifies.   .

### dcterms:source
- **Obligation:** mandatory
- **JSON:** `"dcterms:source":{string}`
- **Scope note:** A string that specifies the authority for the origin of the definition.

(sec-skosconcept)=
## skos:Concept
skos:concept has more general usage in the Concept Scheme profile than in the Codelist profile. Skos:prefLabel is required; a definition and source citation are required.  The skos:notation is optional, but its usage is identical to Codelist. A skos:Concept can represent a possible value for a categorical variable, or an entity or property in a data model. In the RDF implementation of a skos:ConceptScheme, these are the requirements for each concept:

### rdf:type skos:Concept.

### schema:Identifier
must have a globally unique, resolvable identifier.
### skos:inScheme 
relationship to the containing skos:ConceptScheme.
### skos:prefLabel
must have at least one, but may have multiples as these are language specific (SKOS does not permit more than one skos:prefLabel per language-locale).
### skos:definition
 must provide a definition of the concept with skos:definition, also in language-specific form.
### skos:broader, skos:narrower
 must use the  relationship to indicate its parent in the hierarchy if the concept scheme is hierarchical. skos:narrower relationships (inverse of skos:broader) can be provided, supporting navigation both up and down the concept hierarchy.
### skos:notation 
- optional. a unique (in the scope of the vocabulary)  must be provided if code abbreviations are used to denote the skos:Concept in data instances. This is not necessary if unique URIs are used instead of a skos:notation in data instances. The convention must be defined in a vocabulary profile. skos:notation values are commonly short strings or abbreviations that are easier for users to interpret than the concept identifier.
### skos:altLabel
- optional. Other labels may be provided, also in language-specific form.
