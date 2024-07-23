# Design principles

## Overview
The Cross Domain Interoperability Framework is based on a set of concrete, implementation-level principles intended to enhance any community’s data management approaches and reduce the barriers to reusing data across domains. These principles will guide the selection of CDIF recommended standards and approaches intended to inform project-level planning and to provide practitioners with clarity regarding the work involved and the skills, personnel, and technology required. The guidance offered by these principles should not be read as absolute requirements; they define aspirations that should be pursued as far as feasible. In the discussion that follows we will use the term ‘data’ for any structured information representation that is intended for use by computer information systems. This includes both direct observational or model data and data describing other data, commonly differentiated as metadata, but subject to the same principles.

## List of principles
In this section, we set out the principles that have guided the development of the CDIF to date.

### Pragmatism
CDIF is not just a set of theoretical standards and guidelines, but a recommended approach that can realistically be implemented, taking into account the capabilities and limitations of (meta)data producers and consumers. This implies considering the known time limitations, resources, and technical capabilities of those expected to implement it. The framework must balance between flexibility and rigid requirements, providing helpful guidance and a systematic approach, with room for adaptation to new situations and requirements, but within clearly defined bounds to preserve interoperability.

(mainstream)=
### Mainstream
Technology and standards recommended by CDIF should build on existing systems and the legacy investment of (meta)data providers and consumers. User familiarity with recommended implementations is desirable, and these should be widely recognised and in line with current production systems. There will always be new standards and technology, and CDIF will need to keep pace with these on an implementation level, but the goal should always be to employ existing technology, rather than to try to anticipate what practice will be in the future. Use existing Web architecture51 whenever possible. For example:
- HTTP methods (POST, GET, PUT, PATCH, and DELETE) to operate on resources;
- Resource-oriented, API-driven architecture to support file-based download as well as interactive, query-driven access;
- Ensure identifiers are dereferenceable over the Web using the HTTP(s) protocol;
- Alternate serialisations can be requested using content negotiation or signposting links.

### Atomicity
Manage data at the finest granularity possible. Information is organised into entities that have sets of properties, and each property has a value that is an instance of another entity, or a simple value that is a number or a category (represented by a word). In practice, this can lead to complex, nested data schemas that are difficult to understand, manage and use, so it is necessary to consider the requirements of the application for which the information will be used, and the trade-off between the cost of complexity and the benefits of granularity. Information captured in free text is useful for human reading, and natural language processing is getting better at extracting structured data from text, but it is subject to semantic fuzziness and misinterpretation. The benefit of highly granular, or ‘atomic’ data is composability: it can be disaggregated and reaggregated to accommodate diverse and evolving standards, and support projection into different forms in an integration-on-demand model.

### Data as an asset that is worth investment
View data as an asset that has value outside of its immediate application. In practice, this is implemented by making data and associated documentation (metadata) persistently available on the Web, with identifiers at all levels of granularity that can be dereferenced using standard Web architecture, and representation formats using one or more prevailing domain-specific or domain-neutral standards. Identifiers in the data should provide linkage to other resources on the Web.

The goal is to enable automation of the data-discovery-to-use workflow to the maximum extent possible. CDIF is intended for data producers who put data on the Web with the intention that it be used. The approach is to generate machine-actionable (atomic) documentation that is as expressive as possible, e.g., very precise and clear definitions, complete descriptions, and links to supplementary metadata. CDIF can be understood as recommendations for implementation of the FAIR Data Principles, extending them to address more concrete issues.

Documentation for data should be targeted to future users, human or automated, who are temporally or socially removed from your current community. Background knowledge and assumptions that are currently obvious might not be obvious to those users. The terminology used should be clearly defined in plain language, and the definitions should be updated as needed such that they will be accessible and current for the lifetime of the data.

### Flexibility and tooling to encourage uptake
Create and share the ability to project data and its metadata into serialisation formats that are useful for a variety of data consumers across domains. Offering data content in multiple human languages supports different language-based communities. From a machine-processing point of view, multimodality will usually entail:
- Creation, documentation, exposure, and maintenance of authoritative mappings between syntactic and semantic conventions within and between domains;
- Development of software that leverages these mappings to automate the presentation of data in forms familiar to end users or to developers.

Standards used to interchange data should be as domain-agnostic as possible, with mapping files and documentation, which must qualify any loss of precision where it occurs. In practice, this means that categorical properties should usually be represented using key-value approaches, with conventions for vocabularies specifying both property types, and vocabularies specifying the range of values associated with each property.

### Broadcast system capabilities
The Web is too big for all users to keep up with who does what, who holds what data, and how to access the data. Data providers have to be proactive by presenting a machine-actionable description of their interoperable data offerings and services, defining the datasets offered, service protocols used, vocabularies, access policies, or any other digital resource. The CDIF needs to define conventions for publishing server capabilities.

### Robustness
Trust in data quality and the longevity and reliability of systems in a cross-domain digital ecosystem is critical to user engagement. Maintaining this robustness has a technology aspect and a business aspect. The architecture and implementation of systems in a cross-domain digital ecosystem should support the addition, update, or retirement of their components. This entails avoiding dependence on any single component, whether it be hardware, software, standards, or conventions. A component-abstraction layer in the architecture can enable of-the-moment components to be swapped. Focus on maintaining basic cross-domain interoperability functions using proven off-the-shelf components. Functionality can be extended, but beware of becoming a silo through dependence on bespoke tooling or approaches.

Robustness from a business point of view is in many ways a bigger challenge. We can point at some
necessary factors, but these are not always sufficient. Systems must define their market niche, making sure
that there is a value proposition that is clear to the system stakeholders so they ‘buy in’ and continually
support the system. Effort to increase awareness of the system capabilities — marketing, training, workshops
— is likely to be necessary. Avoid building cross-domain technology ‘just because’: make sure someone
clearly benefits from the technology, and they know about the technology.

### Show and tell
Collect and publish metadata related to the use of CDIF to show what implementers have done and what the results of those investments have been. A living, visible system will generate trust and uptake; and ensure that the CDIF remains fit for purpose as technology and priorities evolve. A framework that is not used will not continue to function. Make it easy to determine what standards are used, where data are shared, how data can be accessed, who is reusing data, and what the effects are of data reuse. This kind of information will also facilitate changes to improve the framework and its implementation.