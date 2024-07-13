# How was the CDIF developed?

The idea for CDIF came initially from a series of workshops held at Schloss Dagstuhl in Wadern, Germany,
starting in 2018. The focus was on the alignment of standards from different domains, and the events were
co-hosted by the Data Documentation Initiative (DDI) Alliance, building on a long-running series of Dagstuhl
events. Each year subsequently, [further discussions were held](https://codata.org/initiatives/decadal-programme2/dagstuhl-workshops/).

Participants in the workshops included standards developers and implementers from a wide range of
backgrounds, and a general effort was made to keep the work grounded by including participants from a
range of scientific domains, as well as computer scientists and standards and infrastructure experts. While
sometimes exploratory or theoretical, use cases from the real world were used as a basis for the work.

As the subject of how standards could be used together was explored further, it became clear that
substantial interoperability across domain boundaries would be possible if some basic agreements could be
forged on the content and expression of key metadata.

These discussions provided input to the planning for CODATA’s ‘Making Data Work for Cross-Domain Grand Challenges’ programme (Sponsored by the International Science Council, as part of the [ISC’s Action Plan](https://council.science/actionplan/) ), in which the ideas for a standards-based interoperability framework became more fully formalised, in line with the recommendations from the EU’s [‘Turning FAIR into Reality’ report](https://data.europa.eu/doi/10.2777/1524). While similar work such as the EOSC Interoperability Framework was ongoing, none of this work specifically addressed the field-level alignments across metadata standards which could produce machine-to-machine interoperability in a cross-domain scenario.

With the WorldFAIR project, it became possible to access a range of domain expertise around FAIR, and to ground the work on CDIF more broadly. Each of the WorldFAIR Case Studies completed an initial FAIR Implementation Profile, and this served as input to provide an overview of what standards were currently in use and which were being considered. The FIPs also provided a view into the degree of standards adoption across different communities. Subsequent meetings were held with each group to look at how well the domain-neutral standards being considered for inclusion in CDIF served to describe FAIR data and resources from those communities. These inputs proved to be valuable. To cite two examples:

- In Work Package 07 on Population Health, Schema.org was used extensively to describe the context of an experiment for the purposes of exchange, in line with work done by the Observational Health Data Sciences and Informatics (OHDSI) group (which provided the Observational Medical Outcomes Partnership (OMOP) Common Data Model used as a standard data description in that community). As in many other examples, the implementation of Schema.org relied on JSON-LD.
- The Work Package 11 Oceans Science and Sustainable Development Case Study provided a view of the way a large-scale knowledge graph could be developed on the basis of a common core of metadata fields, again using Schema.org embedded in landing pages as JSON-LD. More information on WP11’s relation to CDIF is available in [WorldFAIR D11.2](https://zenodo.org/records/7682399)

Synergies such as these emerged from the work, and helped to guide the development of the CDIF recommendations. Other connections to the Case Studies may be less evident, because the CDIF functionality is truly broad in scope, and attempts to provide a solution for all domains, and not just those represented in the project. This is reflected in the use of standards and technologies which are seen as widely used across all domains. The issues being addressed by CDIF apply to all domains, and so may not derive directly from specific statements reflected in any individual Case Study.

The work on CDIF was primarily conducted by a group of 30 invited experts, divided into a Working Group and an Advisory Group. Representatives from many different FAIR initiatives, implementations and standards bodies were included, many of them having attended the Dagstuhl workshops in preceding years. The Working Group met every two weeks over the duration of the project, discussing and drafting specific recommendations which were then passed to the Advisory Group for review. Public review of initial drafts was also informally conducted in some cases.

An effort was made to consider additional successful examples in related fields. While there is no existing cross-domain network for FAIR data today, there are initiatives from the scientific world and the world of official statistics which proved valuable in showing which approaches could be employed. Among these was the [Statistical Data and Metadata eXchange (SDMX) Initiative](https://sdmx.org/), which provides a standards-based framework for the exchange of statistical data and metadata among national and international statistical agencies and central banks in a broad global network. Significantly, SDMX standards are employed in the collection and publication of the [Sustainable Development Goals (SDG) Indicators](https://unstats.un.org/sdgs/indicators/indicators-list/). The Helmholtz Metadata Collaboration (HMC) likewise served as an example of how a large number of disparate research centres in Germany could exchange data and metadata in a practical fashion, leveraging the ODIS approach. Several such examples were considered, with an eye toward identifying practical implementation approaches.

The criteria for selecting standards for cross-domain use include:
1. they should be domain-agnostic — that is, not based on assumptions specific to a particular domain;
2. they must be open;
3. they should be already in use for the intended purpose, to the greatest extent possible;
4. they should be adoptable — that is, there is a community which can support them, provide tools and knowledge, etc.; 
5. they should support anticipated functions in the future, at least to a reasonable extent.

These criteria limit the number of candidates to a relatively narrow field, as much of the work on standardisation has been grounded in specific domains. Specifications intended for use on the Web are prominent, as these frequently meet the criteria given above.

CDIF has been developed with a knowledge that there cannot be, and likely never will be, a single set of standards useful for all FAIR exchanges in all situations and across all domains. It is clear that a much higher degree of convergence is possible. Within domains, standards-based approaches have proven to be successful in many different areas. CDIF attempts to start an ongoing process defining what such a convergence can look like, based in approaches shown to work, and using standards and technologies fit for the purposes of cross-domain exchange.