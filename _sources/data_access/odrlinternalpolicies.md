#  Representing internal access policies with ODRL

A standalone repository can hold sensitive personal data requiring mediated access. The target persona might be a data manager in a high profile research group that conducts health studies. The group is committed to providing 'secondary access' to other research groups to build collaborative networks and optimise reuse and impact of data they have collected. The data manager wants make the access processes visible with policies that ensure consistent, efficient processing of access requests, and provide standardised policy types that support automation of access workflows. The research group wants to start with a single policy for its clinical trial datasets (assets) that makes it clear that these datasets are available for secondary use by other academic users. The research group also wishes to specify actions that can be performed on those assets by end users (e.g. access for academic researchers for a specific research question to the full dataset in a trusted research environment (TRE) ).

## Recommendation
In this scenario, the data custodians would use ODRL to express the data asset access policy decomposed into specific access conditions, assets, actors, and actions.  This example is based on research environment in Australia.

- **Actor**: Registered academic researchers (ANU_Registered_Users), condition: demonstrate accreditation by ANU (Accredited_Researcher_Property)
- **Action**: analyse dataset in a TRE environment (Analyse_In_TRE)
- **Asset**: dataset 6123 (http://example.com/dataset_6123)
- **Asset**: Pseudonymized data product (http://example.com/dataset_6123; not identified separately)
- **Action**: Secondary use of asset (SecondaryUse)

```
{
"@context": "http://www.w3.org/ns/odrl.jsonld",
"@type": "Set",
"uid": "http://example.com/policy:1",
"permission": [
	{
	"target": "http://example.com/dataset_6123",
	"action": "Analyse_In_TRE",
	"assignee": {
		"@type": "PartyCollection",
		"source": "ANU_Registered_Users",
		"refinement": [
			{
			"leftOperand": "Accredited_Researcher_Property",
			"operator": "eq",
			"rightOperand": "true"
			}
		]
	  }
	},
	{
	"target": "http://example.com/dataset_6123",
	"action": "SecondaryUse",
	"assignee": {
		"@type": "PartyCollection",
		"source": "ANU_Registered_Users",
		"refinement": [
			{
			"leftOperand": "Researcher_Location_Property",
			"operator": "eq",
			"rightOperand": "Australia"
			}
			]
		}
	}
	]
}
```
Example ODRL Policy.
 
Note that in this example Actions, PartyCollections and Refinements only have local meaning and can only be executed locally. 'Analyse_In_TRE' and 'SecondaryUse' are labels that have no potential for machine-actionability beyond the local repository environment.

## Conventions for ODRL Parties and Actions

Conventions for ODRL policies to express access conditions could support more granular groupings of datasets based on their common access features. Re-usable ODRL policies could be deployed across multiple repositories enabling a metadata harvesters to aggregate content about access and make access conditions transparent for users searching the catalogue.

ODRL Policy - template for referencing ODRL ‘Parties’ and ‘Actions’ by URI

```
"permission": [
{
"target": "http://example.com/example_digital_object",
"action": _Actions_,
"assignee": {
	"@type": "PartyCollection",
	"source": _genericParty_,
	"refinement": [
		{
			"leftOperand": _constraintProperty_,
			"operator": _constraintPredicate_,
			"rightOperand": _constraint_value_
		}
	]
	}
}
]
```
Example ODRL Policy - template for re-usable ‘Parties’ and ‘Actions’

This approach uses a vocabulary of common, reusable Actions defined in the [ODRL Core Vocabulary](https://www.w3.org/TR/odrl-vocab/#actionsCommon) to populate the 'action' element. Additional terms can be defined in 'ODRL Profiles' (outside of the scope of this document), an extension mechanism for the core model and vocabulary.

The ODRL syntax is used to assert a generic Party in the 'source' element, and then refines it to a particular subset of parties with attributes in the 'refinement' section.  In practice, there is a finite number of 'refinement' permutations that could be pre-defined and assigned a persistent URI to be used as the 'source' key above alongside the dereferenced {leftOperand, operator, rightOperand} array.

Access policies typically define types of users (parties) and what actions they can, cannot or must perform. This depends on referencing a number of standard ODRL 'Parties' or 'Actions' by URI.

```
"permission": [
{
"target": "http://example.com/example_digital_object",
"action": "http://cdif.org/odrl/1/download",
"assignee": {
	"@type": "PartyCollection",
	"source": "https://cessda.eu/partycollection:312",
	"refinement": [
		{
			"leftOperand": "http://www.w3.org/ns/odrl/2/spatial",
			"operator": "http://www.w3.org/ns/odrl/2/eq",
			"rightOperand": "de"
		}
	]
	}
}
]
```
Example ODRL Policy statement, concrete example. This policy asserts that users located in Germany have the ‘Download’ permission. The reusable 'PartyCollection' item https://cessda.eu/partycollection:312 (signifying users located in Germany) is re-used from an existing vocabulary used by multiple repositories.

## Workflow scenario:
The workflow outlined in the table below illustrates a message exchange initiated by a researcher requesting a dataset stored in a specific repository with a central brokerage mediating the access on behalf of this and many other repositories. Note that here, the Broker is not acting as a portal for finding the data, but as a mediator of access to a resource known to be at an identified repository.

| **step** | **Message** | **Message pseudo-description** |
| --- | --- | --- |
1 |Request CDIF Resource|User locates dataset resource on repository catalogue with DOI https://dx.doi.org:12345 and clicks on ‘Access Data’ button
2 |CDIF ODRL Policy|Repository posts new RequestFulfilment object to Brokerage API including location of CDIF record for https://dx.doi.org:12345, a JSON-LD artefact which encapsulates ODRL Policy #2b
3 |Request AgentProperties|Brokerage parses ODRL Policy #2b and determines that the user must be located in Germany. This is a very reductive example but more complex real-world examples might be the collection of additional information through an online form managed by the broker.
4 |Supply AgentProperties|The user will supply any information requested by the broker.
5 |Verify AgentProperties|The brokerage will perform the necessary validation and verification of any information supplied by the user. This might involve auxiliary processes, such as seeking third party approval.
6 |ODRL Request|Once all conditions outlined by the ODRL Policy have been met and satisfied by the broker, an ODRL Request will be sent to the repository on behalf of the user.<br>{ "@context": "http://www.w3.org/ns/odrl.jsonld",<br>"@type":"Request",<br>"uid":"http://brokerage.com/odrlrequest/9a112bc6-d93d-4a59-8384-0ac65399ef94", <br>"permission": [<br>&emsp;{"target": "http://repository7987.org/dataset_6123",<br>&emsp;"action": "http://cdif.org/odrl/1/download", <br>&emsp;"assignee": "http://orciduser675765"}<br>&emsp;]}
7 |ODRL Offer|The repository will respond with an ODRL Offer to the broker.<br>{"@context": "http://www.w3.org/ns/odrl.jsonld", <br>"@type": "Offer",<br>"uid":"http://brokerage.com/odrloffer/2f93930b-93f7-418f-a045-aa49d09faf2b", <br>"permission": [<br>&emsp;{"target": "http://repository7987.org/dataset_6123", <br>&emsp;"action": "http://cdif.org/odrl/1/download", <br>&emsp;"assigner": "http://repository7987.org"}<br>&emsp;]}
8 	|AccessToken and ODRL Agreement |	Assuming the broker accepts the offer on behalf of the user, an encrypted Access token is sent to the user and an ODRL Agreement is filed for audit purposes.<br>{<br>"@context": "http://www.w3.org/ns/odrl.jsonld",<br>"@type": "Agreement",<br>"uid":"		http://example.com/odrlagreement/d91d4663-f9bd-4bd5-8a81-c331e19bf987",<br>	"dcterms:references":[<br>&emsp;"http://brokerage.com/odrlrequest/9a112bc6-d93d-4a59-8384-0ac65399ef94",<br>		&emsp;"http://brokerage.com/odrloffer/2f93930b-93f7-418f-a045-aa49d09faf2b"<br>&emsp;],<br>	"permission": [<br>	&emsp;{"target":"http://repository7987.org/dataset_6123",<br>&emsp;"action": "http://cdif.org/odrl/1/download",<br>&emsp;"assigner": "http://repository7987.org",<br>&emsp;"assignee": "http://orciduser675765"}<br>&emsp;]}
9 |Present Access Token|The user presents the token to the Repository
10|Retrieve Resource|The repository makes the file available for immediate download to the user.


This is over-engineered for a simple download where the only condition, as per ODRL Policy in the concrete example above, is that the researcher be based in Germany, However, in the wild, access policies are multi-step processes with complex criteria, often with asynchronous handoffs to third parties for approval. The framework above, while necessarily brief for the purposes of this document, illustrates how a more complex stateful negotiation process could be performed using ODRL as the basis for defining conditions,  executing those conditions through a centralised brokerage, and generating a rich set of accounting information for access requests and fulfilment across the supported repositories.

The example here disambiguates prose representation of access policies by using URIs as references to (i) pre-defined process actors (ORDL 'Parties') and (ii) pre-defined procedural steps (ODRL 'Actions'), framed in consistent syntactical way. These examples do not in themselves provide sufficient detail or semantics for automated implementation of complex rule-based workflows across multiple parties. To enable repeatable and consistent implementation of request fulfillments framed by ODRL policies, conventions to invoke machine-actionable workflows are needed, for example using [Common Workflow Language (CWL)](https://www.commonwl.org/), including downstream invocations such as creating on-demand secure analytics environments (Infrastructure-as-code or IaaC).  Taking CDIF to the next level of maturity in the access arena will mean committing to the creation of a CDIF ODRL Profile that extends the core ODRL ontology and vocabulary so that a widely applicable set of cross-domain Actions are available for re-use as well as being associated with unambiguous workflow implementations, expressed with additional classes and attributes in the CDIF ODRL Profile.