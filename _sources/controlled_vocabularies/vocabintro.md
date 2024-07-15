# Controlled Vocabularies 

Terminology-based semantic resources are a key element in information systems, establishing the binding between the symbols (strings) manipulated by computers and human-intelligible meaning of properties, types, values, or any other element in a volume of data. These a critical component in scenarios involving (but not limited to) data integration and harmonisation.

For the purposes of this document, we use the term ‘controlled vocabularies’ to cover all of the related set of terminology-based semantic resources, even though this may not be technically exact (ontologies, for instance, are often seen as a different kind of resource). Here, our use of the term potentially includes codelists, classifications, thesaurus, taxonomies, glossaries, ontologies, etc. In places where we specifically mean ontologies, classifications, or other types, these terms are explicitly used.
 
We envision two primary scenarios for FAIR usage of controlled vocabularies. In the first scenario (data-centric) an agent (human or machine) encounters a term, code, or symbol in a data set and needs to understand the meaning of that symbol, or to determine if its meaning is the same as some other term, code, or symbol. The navigation is from the individual symbol (code, term) to its meaning within the system of which it is part. 
In the second scenario a vocabulary as a whole is published as a reusable resource for use outside the context of a particular field in a data set. Navigation is from the vocabulary resource into a component parts (term).

Both scenarios require description of the ability to navigate between the controlled vocabulary as a managed whole and its component parts.

To meet these use cases, each member of the vocabulary must have its own globally, unique, web-resovable identifier. These unique identifiers are used by machines to detect when the same concept is being used in two different data sets. 

Persistent, resolvable identifiers (PIDs) are required, with a globally unique mapping from a URI to a concept, and a URI that can be resolvable on the Web to obtain a useful representation. How these identifier strings are formulated will vary widely across user communities. CDIF only recommends that they be included in the definition of a controlled vocabulary.
The goal is that identified concepts can be reused to ease the burden of data harmonisation: if two data sets use the same concept, by referencing the same PID, then there is no ambiguity. 

CDIF recommends the use of the Simple Knowledge Organisation System (SKOS) for representing concept vocabularies.  SKOS is a RDF vocabulary that includes predicates to assign an identifier to a concept,  provide a definition, and assign preferred, language-localized labels (strings) for human use to identify the concept. A vocabulary service exposing the SKOS content on the web is necesary to make the identifiers resolvable. 

CDIF recommends following the guidance provided by [Cox et al. (2021) ‘Ten Simple Rules for making a Vocabulary FAIR’](https://doi.org/10.1371/journal.pcbi.1009041). The CDIF recommendation to use SKOS (as described in this section) aligns with Rule 6 (Cox et al., 2021) regarding machine-readable formats for CVs.