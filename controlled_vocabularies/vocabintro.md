# Controlled Vocabularies: Codelists and Concept Schemes 

Terminology-based semantic resources are a key element in information systems, establishing the binding between the symbols (strings) manipulated by computers and human-intelligible meaning of properties, types, values, or any other element in a volume of data. These are a critical component in scenarios involving (but not limited to) data integration and harmonisation.

For the purposes of this document, we use the term ‘controlled vocabularies’ to cover all of the related set of terminology-based semantic resources, even though this may not be technically exact (ontologies, for instance, are often seen as a different kind of resource). Here, our use of the term potentially includes codelists, classifications, thesaurus, taxonomies, glossaries, ontologies, etc. In places where we specifically mean ontologies, classifications, or other types, these terms are explicitly used.
 
We envision two primary scenarios for FAIR usage of controlled vocabularies. In the first scenario (data-centric) an agent (human or machine) encounters a term, code, or symbol in a data set and needs to understand the meaning of that symbol, or to determine if its meaning is the same as some other term, code, or symbol. The navigation is from the individual symbol (code, term) to its meaning within the system of which it is part. 
In the second scenario a vocabulary as a whole is published as a reusable resource for use outside the context of a particular field in a data set. Navigation is from the vocabulary resource into a component part(term).

Both scenarios require description of the ability to navigate between the controlled vocabulary as a managed whole and its component parts.

To meet these use cases, each member of the vocabulary must have its own globally, unique, web-resovable identifier. These unique identifiers are used by machines to detect when the same concept is being used in two different data sets. 

Persistent, resolvable identifiers (PIDs) are required, with a globally unique mapping from the identifier to a concept. The identifier must be be resolvable on the Web to obtain a useful representation. How these identifier strings are formulated will vary widely across user communities. CDIF only recommends that they be included in the definition of a controlled vocabulary.
The goal is that identified concepts can be reused to ease the burden of data harmonisation: if two data sets use the same concept, by referencing the same PID, then there is no ambiguity. 

CDIF is recommending profiles for two kinds of controlled vocabulary resources: Codelists and Concept Schemes. A Codelist is a resource that maps short strings (codes) to meaning. At the simplest level meaning can be conveyed by another longer string--a 'label' that is more informative for users. Concept schemes are collections of information objects that represent concepts with a human-intelligible label, a definition that specifies the concept, and auxiliary information typically including relationships between concepts and information about the source of the definition. Codelists are intended for use in constructing user interfaces with pick lists for populating fields in datasets. Concept schemes are more broadly applicable to any situation in which the meaning of some information entity, e.g. class, property, property value, needs to be made clear to avoid misunderstanding in the interpretation of data.  Requirements for the Codelist and Concept Scheme profile are as follows.

### Codelist Requirements

- A codelist object must be documented with the required CDIF core properties: Identifier, Title, Date, License or conditions for use, a URL at which the codelist is accessible, and an identifier for the CDIF profile used to represent the codelist
- Every item in the codelist must have a unique 'code' that is used to represent the code in data instance
- Every item in the codelist must have a human-intelligible label
- Optional: identifiers can be assigned to codelist items; if no identifier is assigned, item identifiers will be assumed to be concatenation of the codelist identifeir, '/' and the unique code assigned to the codelist item. 
- Optional: a definition with a more complete explanation of what the code means.
- Optional: hierarchical links between items encoding 'broader' and 'narrower' relationships between items. Broader,narrower can be interpreted broadly according to the semantics of the codelist entries. To facilitate software applications using the codelist, broader and narrower relations must both be explicit in the codelist representation. 

### Concept Scheme Requirements

- A Concept Scheme object must be documented with the required CDIF core properties: Identifier, Title, Date, License or conditions for use, a URL at which the concept scheme is accessible, and an identifier for the CDIF profile used to represent the concept scheme.
- The concept scheme must identify the most general concepts in the scheme. If there is some hierarchy in the concept scheme, this will be a subset of the concepts; if the scheme is 'flat', then all concepts in the scheme will be listed.
- every item in the scheme must have a globally unique identifier
- every item in the scheme must have a human-intelligible label.
- every item in the scheme must have a text definition that unambiguously defines the meaning of the concept and differentiates it from other concepts in the scheme.
- every item in the scheme must cite the authority for its definition; the authority may be 'this scheme' if definitions are original to the concept scheme.
- Optional: hierarchical links between items encoding 'broader' and 'narrower' relationships between items. Broader,narrower can be interpreted broadly according to the semantics of the concept entries. To facilitate software applications using the concept scheme, broader and narrower relations must both be explicit in the concept representation. 

# Implementation

CDIF recommends the use of the Simple Knowledge Organisation System (SKOS) for representing concept vocabularies.  SKOS is a RDF vocabulary that includes predicates to assign an identifier to a concept,  provide a definition, and assign preferred, language-localized labels (strings) for human use to identify the concept. A vocabulary service exposing the SKOS content on the web is necesary to make the identifiers resolvable. 

This use of SKOS materially aligns with that described in the document [‘Modelling of Eurostat’s Statistical
Classifications in ShowVoc’](https://cros.ec.europa.eu/book-page/modeling-eurostats-statistical-classifications-showvoc) for classification items.

CDIF recommends following the guidance provided by [Cox et al. (2021) ‘Ten Simple Rules for making a Vocabulary FAIR’](https://doi.org/10.1371/journal.pcbi.1009041). The CDIF recommendation to use SKOS (as described in this section) aligns with Rule 6 (Cox et al., 2021) regarding machine-readable formats for CVs.

## Note on formal statistical classifications
Documentation of formal statistical classifications includes additional information, but a detailed profile for CDIF has not been developed. CDIF recommends using the style used at [Eurostat](https://cros.ec.europa.eu/book-page/modeling-eurostats-statistical-classifications-showvoc) and [FAO](https://www.fao.org/statistics/caliper/resources/data-modeling/en). These descriptions include additonal properties, and can include tables documenting mapping between versions of classifications. This information is represented using XKOS, see the [XKOS specification](https://rdf-vocabulary.ddialliance.org/xkos.html) and [user guide](https://linked-statistics.github.io/xkos/xkos-best-practices.html).

<!-- cdif-footer-include -->
:::{include} ../_static/footer.md
:::
