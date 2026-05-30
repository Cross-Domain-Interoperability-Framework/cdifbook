# The structure of CDIF

The organisation of the CDIF recommendations follows a basic functional breakdown emerging from the need to support certain exchanges of information as dictated by the FAIR Principles. ‘Findable’ requires that we be able to make our resources searchable, and enable them to be catalogued. ‘Accessible’ requires that we can retrieve or link to resources in a technical sense, but also that we can understand what conditions are in the case that access is limited. ‘Interoperable’ means that we can load resources into our systems for processing once acquired, and operate on them in a meaningful way. ‘Reusable’ means that we have enough information to understand the data and the uses to which it can legally be put.

There is not a point-for-point alignment between the FAIR Principles and the functional requirements that support them. The information needed to support one principle may be required also to support another, and there may be no clear distinction made between these sets of information. CDIF organises FAIR ‘functions’ in a fashion that maps to the principles, but more between systems needed for implementation and the information they require:

- Discovery, Cataloguing, and Dissemination (F)
- Controlling Data Access (A)
- Data Description, Use, and Integration: Structure and Semantics (F, A, I, R)
- Characterizing Data/Provenance and Process, Quality (I, R)
- Universals: Time, Geography, and Units of Measure (F, I, R)

This organisation is not strictly functional, but attempts to describe the information needed for each major function (Discovery, Access, Integration, Reuse) as well as to address some of the common information needs (concept schemes, codelists, and mappings, "universals"). Not all topics are covered in equal depth at this time: some are supported more thoroughly in this version of the guidelines than the others, which will receive more attention in future. This reflects the current state of play within the FAIR community, and the perceived relative importance of these functions based on current and planned implementations.

These areas were identified through examination of FAIR implementations in many domains, and are driven by the relative maturity of the standards and practice within communities engaging proactively with FAIR. Discovery is perhaps the most common subject of FAIR implementation, as it is both less demanding in terms of metadata (and therefore resources) as well as being logically a first step: if you can’t find it, you can’t use it! Access to open data is in some senses a 'solved' problem, so the attention of the FAIR community is turning towards the need to better support access to controlled data. Currently, support for providing access to controlled data is often strictly manual, presenting a practical bottleneck for reuse. 

While we are early in our development of standards and systems for automating access to controlled data, there are some initial steps which can be easily taken. Data interoperability and reuse have been receiving an increasing amount of attention in many domains: these are arguably the most metadata-intensive aspects of FAIR, but they also hold a huge potential in terms of efficiency gains: if we can ease the problems of integration and harmonisation ('data wrangling') through automation, the potential resource savings are large. Data integration necessarily raises the question of how semantics are exposed and mapped. These topics provide the focus of the current document and the CDIF profiles.

# Summary of CDIF profiles and recommendations

**General**: CDIF metadata should be embedded in landing pages or linked stand-alone files, encoded in JSON-LD. The supported profiles will be indicated as part of the metadata.

**Core Profile**: The Core profile identifies a set of fields from [Schema.org](https://schema.org) for use in all other profiles, covering such basic functions as management of the metadata and statements of conformance.

**Discovery Profile**: This profile recommends the use of a set of key [Schema.org](https://schema.org) fields for describing static datasets and queryable data sources, with the [DCAT](https://www.w3.org/TR/vocab-dcat-3/) equivalent recognised as an acceptable alternative.

**Manifest Profile**: This profile is used to describe packages of metadata and FAIR resources, to form bundles for dissemination, archiving, etc. It can be used to render “webby” FDOs. The recommended implementation uses RO Crate.

**Access Profile**: This profile recommends that [ODRL](https://www.w3.org/TR/odrl-model/) Actions and Entities be used to describe policies and conditions for the use of data. At this time, the utility of this approach is limited by the lack of shared vocabularies for conditions of use, user qualifications, legal constraints, and similar important items. ODRL is thus limited to describing policies in terms of the disseminating institution, but provides a basis for expansion in future when the needed vocabularies are developed.

**Concept Scheme Profile**: This profile recommends the use of [SKOS](https://www.w3.org/TR/2009/REC-skos-reference-20090818/) for describing controlled vocabularies, understood to mean any terminological resource. The use of [OWL](https://www.w3.org/TR/owl2-overview/) as a linked extension towhat is presented in SKOS is also recommended, as is the use of [XKOS](https://ddialliance.org/Specification/RDF/XKOS) for formal statistical classifications.

**Data Description Profile**: This profile recommends the use of [Schema.org](https://schema.org) and [DDI-CDI](https://ddialliance.org/Specification/ddi-cdi) to provide a granular description of quantitative data sets, and how the logical content of those datasets relates to their physical encoding. Both text-based (i.e., CSV, fixed-width) and binary (i.e., NetCDF, HDF5, Parquet) are supported. The recommendations cover description of individual data sets to make them ‘integration-ready’.

**Codelist Profile**: This profile recommends the use of [SKOS](https://www.w3.org/TR/2009/REC-skos-reference-20090818/) for describing the enumerated sets of values used to populate the fields in data sets. These are used in the description of data sets and their structures in the Data Description and Data Structure profiles.

**Data Structure Profile**: This profile uses [DDI-CDI](https://ddialliance.org/Specification/ddi-cdi) to describe reusable data structures and variables. In future, this capaility will be extended to data formats.

**Universals Profile**: This section recommends the information which should be provided when describingtime, geography, and units of measurement in other metadata sets. Some standards for this purpose arerecommended in each area.


The CDIF includes recommendation for specific implementation approaches in each profile, based on web technology. While many standards and vocabularies require the use of RDF, it is not a technology that is commonly used in every domain. The solution to this is to advocate the use of JSON-LD, which allows the expression of RDF vocabularies in the common JSON syntax.

In each profile a minimum set of required fields are specified to support common cases. Other optional fields are suggested, and the path forward toward support of more complex scenarios is indicated. While FAIR implementation is demanding, it is hoped that consistent use of a common core of metadata can minimize the effort required.

Users only need adopt those profiles that are useful to them. There is no requirement for the adoption of optional profile content. For example, it is possible to describe data to make it ‘integration ready’ at a detailed level, but not to support profiles for data discovery or access, to give but one example. CDIF profiles are intended to be a toolkit for implementation, with the needed functions being addressed in any specific setting according to implementer priorities.

For some common combinations of profiles, implementation artefacts will also be provided as a convenience. **[FILLTHIS IN WHEN WE KNOW WHERE IN THE BOOK THESE WILL BE PROVIDED]**