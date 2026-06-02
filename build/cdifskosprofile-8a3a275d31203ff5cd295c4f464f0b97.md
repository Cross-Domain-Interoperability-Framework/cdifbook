# CDIF SKOS profile

A **skos:ConceptScheme**: a set of concepts that define the possible values for a categorical variable. The scheme can define a hierarchy of values from general to more specific. This hierarchy is represented using the skos:broader relationship linking more specific concepts to subsuming more general concepts. If a hierarchy is defined in the scheme the top concept(s), i.e. those that do not have any skos:broader associations, must be identified using the skos:hasTopConcept property on the skos:ConceptScheme.

**skos:Concept**: represent the possible values for a categorical variable. In the RDF implementation of a skos:ConceptScheme, these are the requirements for each concept:
- must specify rdf:type skos:Concept.
- must have a globally unique, resolvable identifier.
- must have a skos:inScheme relationship to the containing skos:ConceptScheme.
- must have at least one skos:prefLabel, but may have multiples as these are language specific (SKOS does not permit more than one skos:prefLabel per language-locale).
- must provide a definition of the concept with skos:definition, also in language-specific form.
- must use the skos:broader relationship to indicate its parent in the hierarchy if the concept scheme is hierarchical 
- a unique (in the scope of the vocabulary) skos:Notation must be provided if skos:Notation is used to denote the skos:Concept in data instances. This is not necessary if unique URIs are used instead of a skos:Notation in data instances. The convention must be defined in a vocabulary profile. skos:Notation values are commonly short strings or abbreviations that are easier for users to interpret than the concept identifier.
- Other labels may be provided using skos:altLabel, also in language-specific form.
- skos:narrower relationships (inverse of skos:broader) can be provided, supporting navigation both up and down the concept hierarchy.


This use of SKOS materially aligns with that described in the document [‘Modelling of Eurostat’s Statistical
Classifications in ShowVoc’](https://cros.ec.europa.eu/book-page/modeling-eurostats-statistical-classifications-showvoc) for classification items.