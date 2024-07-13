# Summary of CDIF profiles and recommendations

**General**: CDIF metadata should be embedded in landing pages or linked stand-alone files, encoded in JSON-LD. The supported profiles will be indicated as part of the metadata.

**Discovery profile**: This profile recommends the use of a set of key Schema.org fields for describing static datasets and queryable data sources, with the [DCAT](https://www.w3.org/TR/vocab-dcat-3/) equivalent recognised as an acceptable alternative.

**Access profile**: This profile recommends that [ODRL](https://www.w3.org/TR/odrl-model/) Actions and Entities be used to describe policies and conditions for the use of data. At this time, the utility of this approach is limited by the lack of shared vocabularies for conditions of use, user qualifications, legal constraints, and similar important items. ODRL is thus limited to describing policies in terms of the disseminating institution, but provides a basis for expansion in future when the needed vocabularies are developed.

**Controlled vocabularies profile**: This profile recommends the use of [SKOS](https://www.w3.org/TR/2009/REC-skos-reference-20090818/) for describing controlled vocabularies, understood to mean any terminological resource. The use of [OWL](https://www.w3.org/TR/owl2-overview/) as a linked extension towhat is presented in SKOS is also recommended, as is the use of [XKOS](https://ddialliance.org/Specification/RDF/XKOS) for formal statistical classifications.

**Data description profile**: This profile recommends the use of [DDI-CDI](https://ddialliance.org/Specification/ddi-cdi) to provide a granular description of the structure of data sets, and how the logical content of those datasets relates to their physical encoding. Text-based data is supported (CSV and other delimited formats, fixed-width ASCII, etc.), with the intention of expanding support for other types of data in future. The recommendations cover description of individual data sets to make them ‘integration-ready’.

**Universals profile**: This section recommends the information which should be provided when describingtime, geography, and units of measurement in other metadata sets. Some standards for this purpose arerecommended in each area.


The CDIF includes recommendation for specific implementation approaches in each profile, based on web technology. While many standards and vocabularies require the use of RDF, it is not a technology that is commonly used in every domain. The solution to this is to advocate the use of JSON-LD, which allows the expression of RDF vocabularies in the common JSON syntax.

In each profile a minimum set of required fields are specified to support common cases. Other optional fields are suggested, and the path forward toward support of more complex scenarios is indicated. While FAIR implementation is demanding, it is hoped that consistent use of a common core of metadata can minimize the effort required.

Users only need adopt those profiles that are useful to them. There is no requirement for the adoption of optional profile content. For example, it is possible to describe data to make it ‘integration ready’ at a detailed level, but not to support profiles for data discovery or access, to give but one example. CDIF profiles are intended to be a toolkit for implementation, with the needed functions being addressed in any specific setting according to implementer priorities.