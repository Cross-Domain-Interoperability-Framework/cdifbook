# Introducing CDIF

Many important research questions demand a multi-disciplinary approach in which data and resources are used across domain and infrastructure boundaries. In such scenarios, domain-specific community standards fall short of the requirements for FAIR exchange of the critical metadata and other information needed. The **Cross-Domain Interoperability Framework** (CDIF) is designed to support FAIR implementation for these projects by establishing a ‘lingua franca’ for this information, based on existing standards and technology to support interoperability, in both human- and machine-actionable fashion. CDIF is a set of implementation recommendations, based on profiles of common, domain-neutral metadata standards which are aligned to work together to support core functions required by FAIR.  

The idea for CDIF first emerged from workshops and discussions at conferences prior to the WorldFAIR project, beginning in 2018. The WorldFAIR project provided an opportunity to advance that vision, through aset of 11 case studies across many domains, allowing the needs and practices around FAIR within such domains to be summarised in the form of FAIR Implementation Profiles (FIPs). Based on the FIPs and focused meetings, the requirements for CDIF were established. A group of 30 invited experts from different FAIR initiatives and standards bodies made up a Working Group and an Advisory Group to synthesise the findings from WorldFAIR and to produce the current CDIF draft. 

The framework is based on a set of five core profiles that address the most important functions for cross-domain FAIR implementation.
1. Discovery (patterns for metadata content, serialization and publication)
2. Data access (documentation of access conditions and permitted use)
3. Controlled vocabularies (practices for the publication of controlled vocabularies and semantic artefacts)
4. Data integration (documentation of the structural and semantic aspects of data to make it integration-ready)
5. Universals (description of ‘universal’ elements -- time, geography, and units of measurement).

Each of these profiles is supported by specific recommendations, including the set of metadata fields in specific standards to use, and the method of implementation to be employed for machine-level interoperability.


CDIF is designed to leverage the work of other FAIR initiatives such as FAIR-Impact and the work in EOSC. It is designed to be implementable with existing tools, standards, and technologies but, as a set of recommended practices, must be maintained as FAIR implementations develop and evolution occurs in the technology sphere. CDIF leverages methodologies such as FIPs from the [GO FAIR Foundation](https://www.go-fair.org/how-to-go-fair/fair-implementation-profile/). Importantly, it aligns with efforts such as the [EOSC Interoperability Framework](https://op.europa.eu/en/publication-detail/-/publication/d787ea54-6a87-11eb-aeb5-01aa75ed71a1/language-en), and developments such as [Signposting](https://signposting.org/FAIR/) and reference implementation of the [FAIR Digital Object Framework](https://fairdo.org/). Work on semantic mapping and in some other areas is informed by on-going developments in other fora such as [RDA](https://www.rd-alliance.org/). CDIF is designed to enable the practical implementation of FAIR by supporting these frameworks and approaches in cross-domain scenarios. 

In any given domain, the standards used should be to a considerable extent mappable to and from their corresponding CDIF profiles, reducing the volume of mappings needed to interoperate effectively for core FAIR functions in multi-disciplinary scenarios. Broadly speaking, FAIR demands an increase in the metadata provided by the disseminators of data, especially if we are to automate resource-intensive data integration tasks, which today are largely manual. In a cross-domain scenario, the sheer number of mappings needed is not supportable. CDIF provides a solution by changing a many-to-many dynamic into a many-to-one dynamic. 

# What is CDIF
 
**CDIF is not intended to replace existing community standards**, but to supplement them for communication across domain and infrastructure boundaries. It does not aim to replace the specific models needed within different domains, but it does aim to establish a foundation of common metadata which can support a core set of FAIR functionality. Real-world examples of large-scale standards-based exchange networks, such as the [Statistical Data and Metadata Exchange](https://sdmx.org/) (SDMX) and the [Ocean InfoHub](https://oceaninfohub.org/odis/) (ODIS) have been used as inspiration for the overall approach, to ensure its feasibility for practical implementation. This draft includes links to early prototypes for such data as the Sustainable Development Goal Indicators and some of their source data, showing how the mining of the native standard descriptions of the source, to produce its equivalent in CDIF, can support disaggregation and integration of that data with other sources. 

There is a wealth of research on how FAIR can be implemented, and investigations into this subject show no sign of abating. CDIF is not a research project, nor does it intend to outline the best possible solution to the challenges of FAIR implementation. Instead, it is first and foremost a practical exercise: how can we find a set of common standards and technology approaches that will enable us to implement FAIR for cross-domain scenarios using things which exist today? Standards are only useful if they are agreed and implemented, so CDIF tries to advocate those standards and technology already in common use. There are gaps in current practice that require additional information and standards, but these are minority cases. Web standards already exist for supporting most of the needed functions, but they are not always used in ways which are interoperable, requiring a common approach for at least some core aspects. The CDIF recommendations are based on practical considerations: we must agree on a body of practice, and whether it is the best possible solution is a secondary concern. More important is that it be implemented widely, so that FAIR exchange can become as easy as possible. Once laid, such a foundation can be perfected. 

To this end, CDIF identifies a set of common functions which are needed to implement FAIR: describing data for discovery and assessment, supporting access to data by describing the licensing and conditions of use, laying out the structure and semantics of data to support automated integration, and providing information regarding the provenance of data and resources. In each area, practical recommendations are made regarding what standard or standards should be used, and how they should be implemented. In those areas where there is no clear practice which can be recommended, or which require further investigation, this is noted. For the provision of basic discovery metadata, the description of licence conditions, the publication of controlled vocabularies, and the description of data to make it ‘integration ready’, specific steps are described which can be used to guide immediate implementation.

# Who can use the CDIF?

CDIF is aimed primarily at data infrastructures, i.e. those organisations which develop, maintain, and disseminate FAIR resources for reuse, often as centralised points of access within their communities or area(s) of interest. While data stewardship by research organisations is an important element of FAIR, not all research organisations perform this dissemination, instead relying on data archives or other dedicated repositories. FAIR reuse is most effective when authoritative producers, or those acting on their behalf, provide their data and metadata to others for reuse, so the authoritative versions of such resources are the ones which get reused. Such organisations are often motivated to be the point of dissemination, as it is their mission, and they bear the responsibility, both legal and reputational, for those resources. CDIF is a tool which they can use to better support this mission.


# The CDIF workgroup

The History section describes the development of this specification.  Many people have contributed and we'd like acknowledge their time and effort. 



# How to contribute 

The CDIF Working Group meets virtually every two weeks and collaborates online.  The focus of work will be on developing the new profiles, as well as renewing the existing profiles based on feedback.

The CDIF Advisory Group meets 2-4 times a year, as required and receives profile documentation to review.

To apply to join the WG or AG, please [use this form](https://docs.google.com/forms/d/e/1FAIpQLSfC6BoxSnAZOMWZ65pNTiljnsBXjfv80NgEeALPcY5DPaESHA/viewform) and specify which group you wish to join!

If you simply want to keep up to date with developments, [please register to join the CDIF community list](https://bit.ly/cdif-community-list).

To find out more about CDIF, please consult the [presentations and recording from our last webinar](https://codata.org/the-cross-domain-interoperability-framework-cdif-practical-guidelines-for-fair-interoperability-webinar-25-july/), or the CDIF report [https://doi.org/10.5281/zenodo.11236871](https://doi.org/10.5281/zenodo.11236871).  Project development is online in Github in the [Cross-Domain Interoperability Framework organization](https://github.com/Cross-Domain-Interoperability-Framework)

A major goal of CDIF is to streamline workflow from needing particular data to having the data you need accessible to the software you're writing or using in the environment where you work. If you have experience or ideas for how this can be achieved, please get in touch!

Some work items we need help with:

* Example CDIF JSON-LD metadata with detailed information for data integration. Data integration information using CDI-DDI vocabulary to describe more complex datasets, with objects, relationships, sentinel values, and controlled vocabularies.  

* Example CDIF JSON-LD Metadata for Data Access, examples using ODRL to document various workflows and requirements to obtain permission for data access or necessary aggregation to de-personalize data.

* Example Metadata describing gridded data or time series using NetCDF or HDF.

* SHACL rules to validate CDIF JSON-LD

* Use cases and example for describing data for use by AI system e.g. for training data. Use of Croissant.