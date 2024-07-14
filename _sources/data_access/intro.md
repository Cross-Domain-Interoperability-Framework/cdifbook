# Data Access Introduction

This section of the framework makes recommendations for describing and documenting policies related to the retrieval of a digital object for research, and subsequent operations performed on that digital object. In the CDIF framework, we use the term 'access' to include the activities connected to the initial retrieval of a digital object, and 'usage' to include all subsequent operations performed on that digital object.  Access and usage policies (herein 'access policies'), when defined, are typically unstructured and bespoke. Data providers may not make access policies explicit and when they do, they tend to re-invent new policies locally. Therefore data users experience new data access policy content and structure at every access-related interaction across the science system. Any kind of aggregation or orchestration of data across providers is stymied by an incoherent data access policy environment in terms of existence, coverage, content, and machine-actionability.

## Objective
The objective of the Data Access Profile in the Cross Domain Interoperability Framework (CDIF) is to progress to a more structured and standardised, machine-actionable approach. The benefits and convenience for data requestors accrue through:
- transparency and efficiency in requesting data;
- consistency of access experience across data custodians;
- automated processes and services across those providers;
- federated permissions query and access.

For data custodians, the structured and standardised approach to data asset permissions through a structured ontology (e.g., ODRL) provides:
- Default minimum good practice guidance and design patterns for new systems development;
- Efficiencies, equity, and transparency in processing access requests;
- Ability to track and evaluate access requests to improve processes and address inequities in access and reuse;
- Opportunities for repository or archive providers to participate in multi-organisational collaborations to support collaborative science.

The goal is interoperable, machine-actionable, expression of data access policies. It is important to note this is not the entirety of 'Access' from a plain English or FAIR perspective. Accessing sensitive data is a complicated, multi-faceted, multi-party process involving for example:
- Clarifying intellectual property rights, and, where appropriate, copyright.
- Negotiation with data producers
- Specifying data reuse licences
- Establishing bespoke legal agreements
- Ensuring cybersecurity
- Assessing data sensitivity and identifying potential secondary disclosure risks
- Preventing reverse engineering of the data
- Establishing the ethical and legal constraints pertaining to the nature of the data in question
- Clarifying the nature of a specific access, including the requestor and the data request purpose
- Identifying roles or attributes upon which access may be granted
- Provision of secure access environments with or without analytics
This recommendation acknowledges the importance of all those (and related) elements but focuses on standard ways of capturing data access requests in structured, machine-actionable policy statements at the point of access, as well as prescribing what subsequent operations can (or cannot) be performed until the end of the research lifecycle. CDIF does not offer guidance on how to classify data sensitivity. It does provide a recommendation on how to express access constraints based on a data provider's pre-existing data sensitivity classification. It doesn't explain when, why or how an ethics approval is needed. It does provide a recommendation for the standard machine-readable policy that would tell a data requester that an ethics approval is needed in order to retrieve the data.

In scope for this recommendation are:
- The standard expression of access policies
- The “publishing” of those policies
- The automated execution of machine-readable access policies.

Useful enablers of an access policy but 'out of scope' for this recommendation are:
- How access policies are derived
- How to classify of the sensitive nature of the data
- How to assess the risk of secondary disclosure
- Licences / Legal Agreements /IP / Copyright
- Good practice IT Security/ cybersecurity
- Consent/ Ethics Approval.

## High-level recommendation
To promote interoperability and mutual intelligibility around access conditions, CDIF recommends use of the [Open Digital Rights Language (ODRL)](https://www.w3.org/TR/odrl-model/) to describe data asset access policies. ODRL is a RDF-based, 'widely adopted language for expressing permissions, obligations, and conditions related to digital rights' ([Policy Patterns for Usage Control in Data Spaces, 2023](https://arxiv.org/pdf/2309.11289.pdf) ) that can be serialised in JSON and XML. While minimal in terms of classes and relationships, it nonetheless allows sufficient flexibility through the use of constraints and refinements and descriptive logic. ODRL also allows straightforward extensions to the core model and vocabulary with ODRL Profiles.

## Risks and enablers for CDIF using ODRL
The existence of ODRL as an existing, well-supported W3C standard is a key 'enabler' for CDIF, based on the the stated [principle](..\background\principles.html#mainstream) committed to using existing, well-supported standards wherever possible. The first risk with ODRL is that it doesn’t cover all scenarios. The scenario-based discussion in following sections aim to tease out what can be done with ODRL immediately and what scenarios might need further extensions or new approaches. Our conclusion is that  ODRL and its extension/profiling capability is quite useful as is. The second risk is the barrier to adoption of ODRL. There is an urgent need for tooling to simplify how domain scientists and data curators use this mature W3C standard. The information model, ontology, and linked data implementation are beyond the capability of many scientists and infrastructure service providers who can benefit from standardised access policies at scale. This risk is both high and likely and currently has no mitigation.

