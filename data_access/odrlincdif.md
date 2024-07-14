# ODRL implementation

## Policy example
Ahe data custodians can use ODRL to express a data asset access policy decomposed into specific access conditions, assets, actors, and actions.  This example is based on research environment in Australia.

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

## Policy template

Conventions for ODRL policies to express access conditions could support more granular groupings of datasets based on their common access features. Re-usable ODRL policies could be deployed across multiple repositories enabling a metadata harvesters to aggregate content about access and make access conditions transparent for users searching the catalogue.

This approach uses a vocabulary of common, reusable Actions defined in the [ODRL Core Vocabulary](https://www.w3.org/TR/odrl-vocab/#actionsCommon) to populate the 'action' element. Additional terms can be defined in 'ODRL Profiles' (outside of the scope of this document), an extension mechanism for the core model and vocabulary.

The ODRL syntax is used to assert a generic Party in the 'source' element, and then refines it to a particular subset of parties with attributes in the 'refinement' section.  In practice, there is a finite number of 'refinement' permutations that could be pre-defined and assigned a persistent URI to be used as the 'source' key above alongside the dereferenced {leftOperand, operator, rightOperand} array.

Access policies typically define types of users (parties) and what actions they can, cannot or must perform. This depends on referencing a number of standard ODRL 'Parties' or 'Actions' by URI.

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

Example. ODRL Policy template for re-usable ‘Parties’ and ‘Actions’

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

<div id="exampleconcretepolicy">Example ODRL Policy statement<div>. This policy asserts that users located in Germany have the ‘Download’ permission. The reusable 'PartyCollection' item https://cessda.eu/partycollection:312 (signifying users located in Germany) is re-used from an existing vocabulary used by multiple repositories.

The example here disambiguates prose representation of access policies by using URIs as references to (i) pre-defined process actors (ORDL 'Parties') and (ii) pre-defined procedural steps (ODRL 'Actions'), framed in consistent syntactical way. These examples do not in themselves provide sufficient detail or semantics for automated implementation of complex rule-based workflows across multiple parties. To enable repeatable and consistent implementation of request fulfillments framed by ODRL policies, conventions to invoke machine-actionable workflows are needed, for example using [Common Workflow Language (CWL)](https://www.commonwl.org/), including downstream invocations such as creating on-demand secure analytics environments (Infrastructure-as-code or IaaC).  

A common language for expressing data access policies across the cooperative of data providers using the clearing house is a necessary but insufficient first step. A shared understanding of how policies interface with business processes is also required so that both data provider and the clearing house can be satisfied that request fulfilment was efficient, consistent and auditable.

Taking CDIF to the next level of maturity in the access arena will mean committing to the creation of a CDIF ODRL Profile that extends the core ODRL ontology and vocabulary so that a widely applicable set of cross-domain Actions are available for re-use as well as being associated with unambiguous workflow implementations, expressed with additional classes and attributes in the CDIF ODRL Profile.

##  Connecting CDIF and ODRL

(under construction)
CDIF recommends that a DCAT record be created, describing a dcat:distribution or, in lay parlance, a digital object, corresponding to the data set or resources to be used. For queryable data sources, this would instead be a dcat:service. (The dcat:dataset class is not used, because different distributions of it may have different access conditions.) An ODRL Rule has a mandatory ‘target’ attribute which specifies the ODRL Asset to which the Rule applies. For example:

```
"permission": [
	{
		"target": "http://repository7987.org/dataset_6123",
		"action": "http://cdif.org/odrl/1/download",
		"assignee": "http://orciduser675765",
	}
```

Conversely, a DCAT record could express a predicate http://www.w3.org/ns/odrl/2/hasPolicy with the URI of the ODRL Policy as a target.

An ODRL policy might apply to multiple ODRL Assets. One possible solution is to leverage the ODRL AssetCollection class, which can group multiple Assets under one URI as the target of an ODRL rule. This requires policies defining persistent grouping rules that define an AssetCollection, determining if and how the membership of the AssetCollection can change. 