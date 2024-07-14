# Data Catalog Vocabulary (DCAT) vocabulary

DCAT is an important specification for exposing discovery metadata. To support those who use DCAT, we recommend that the DCAT metadata be expressed in JSON-LD as recommended for Schema.org, and the equivalent fields be used.

DCAT has become very common in some areas: DCAT-AP and GeoDCAT have gained a lot of traction in Europe. DCAT-US promises to gain similar traction. While this varies domain-by-domain, the fact that DCAT itself is designed for describing data, while Schema.org is immensely broad in scope (it describes basically everything for the purposes of the Web!), means that DCAT has some features which are specifically useful for CDIF’s purposes.

There are several different mappings between Schema.org and DCAT available, and some may be more appropriate than others for any particular implementation. By default, however, we recommend the mapping from the W3C group which has developed DCAT:

1. DCAT - Schema.org mapping in the context of DCAT version 2:
- https://ec-jrc.github.io/dcat-ap-to-schema-org/ or https://www.w3.org/TR/vocab-dcat-2/#dcat-sdo 
- https://w3c.github.io/dxwg/dcat/rdf/dcat-schema.ttl is a mapping between DCAT2 and SDO 3.4. "...This mapping is axiomatized using the predicates rdfs:subClassOf, rdfs:subPropertyOf, owl:equivalentClass, owl:equivalentProperty, skos:closeMatch, and also using the annotation properties sdo:domainIncludes and sdo:rangeIncludes to match [SCHEMA-ORG](https://www.w3.org/TR/vocab-dcat-2/#bib-schema-org) semantics. …" .

2. Update in the DCAT version 3 documentation: https://www.w3.org/TR/vocab-dcat-3/#dcat-sdo