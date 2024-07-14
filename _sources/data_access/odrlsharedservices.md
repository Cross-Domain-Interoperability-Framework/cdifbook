# Conventions for ODRL Parties and Actions

A data clearing house can aggregate metadata from many repositories and broker access to resources in those repositores. Conventions for representing data access policies are necessary to support groupings of datasets based on their common access policies. Re-usable ODRL policies could be deployed across multiple repositories enabling a metadata harvester to aggregate content about access and make access conditions transparent for users searching the catalogue. The benefits of such a clearing house approach could include:
- Streamlining processes for data users
- Support for combined interdependent requests across multiple data providers for complex studies
- Efficiencies of resourcing and specialisation
- Natural combined reporting and tracking of usage and impact

Standardised, machine-actionable data access policies necessary to deliver on two assumptions of a shared clearing house:
- that the cost of sharing the information about requests does not overwhelm the savings in staff and systems.
- That a central capability can action things and lighten the load of the network rather than just re-routing jobs for fulfilment back to the participating node for 'local implementation'.

The basic approach to representing policies that can be implemented with existing ODRL technology is described in [ODRL implementation](./odrlincdif.md).


# Workflow scenario:
The workflow outlined in the table below illustrates a message exchange initiated by a researcher requesting a dataset stored in a specific repository with a central clearing house mediating access on behalf of this and many other repositories. Note that here, the Broker (clearing house) is not acting as a portal for finding the data, but as a mediator of access to a resource known to be at an identified repository.

| **step** | **Message** | **Message pseudo-description** |
| --- | --- | --- |
1 |Request CDIF Resource|User locates dataset resource on repository catalogue with DOI https://dx.doi.org:12345 and clicks on ‘Access Data’ button
2 |CDIF ODRL Policy|Repository posts new RequestFulfilment object to Brokerage API including location of CDIF record for https://dx.doi.org:12345, a JSON-LD artefact which encapsulates ODRL Policy #2b
3 |Request AgentProperties|Brokerage parses the ODRL Policy statement and determines that the user must be located in Germany. More complex real-world examples might be the collection of additional information through an online form managed by the broker.
4 |Supply AgentProperties|The user will supply any information requested by the broker.
5 |Verify AgentProperties|The brokerage will perform the necessary validation and verification of any information supplied by the user. This might involve auxiliary processes, such as seeking third party approval.
6 |ODRL Request|Once all conditions outlined by the ODRL Policy have been met and satisfied by the broker, an ODRL Request will be sent to the repository on behalf of the user.<br>{ "@context": "http://www.w3.org/ns/odrl.jsonld",<br>"@type":"Request",<br>"uid":"http://brokerage.com/odrlrequest/9a112bc6-d93d-4a59-8384-0ac65399ef94", <br>"permission": [<br>&emsp;{"target": "http://repository7987.org/dataset_6123",<br>&emsp;"action": "http://cdif.org/odrl/1/download", <br>&emsp;"assignee": "http://orciduser675765"}<br>&emsp;]}
7 |ODRL Offer|The repository will respond with an ODRL Offer to the broker.<br>{"@context": "http://www.w3.org/ns/odrl.jsonld", <br>"@type": "Offer",<br>"uid":"http://brokerage.com/odrloffer/2f93930b-93f7-418f-a045-aa49d09faf2b", <br>"permission": [<br>&emsp;{"target": "http://repository7987.org/dataset_6123", <br>&emsp;"action": "http://cdif.org/odrl/1/download", <br>&emsp;"assigner": "http://repository7987.org"}<br>&emsp;]}
8 	|AccessToken and ODRL Agreement |	Assuming the broker accepts the offer on behalf of the user, an encrypted Access token is sent to the user and an ODRL Agreement is filed for audit purposes.<br>{<br>"@context": "http://www.w3.org/ns/odrl.jsonld",<br>"@type": "Agreement",<br>"uid":"		http://example.com/odrlagreement/d91d4663-f9bd-4bd5-8a81-c331e19bf987",<br>	"dcterms:references":[<br>&emsp;"http://brokerage.com/odrlrequest/9a112bc6-d93d-4a59-8384-0ac65399ef94",<br>		&emsp;"http://brokerage.com/odrloffer/2f93930b-93f7-418f-a045-aa49d09faf2b"<br>&emsp;],<br>	"permission": [<br>	&emsp;{"target":"http://repository7987.org/dataset_6123",<br>&emsp;"action": "http://cdif.org/odrl/1/download",<br>&emsp;"assigner": "http://repository7987.org",<br>&emsp;"assignee": "http://orciduser675765"}<br>&emsp;]}
9 |Present Access Token|The user presents the token to the Repository
10|Retrieve Resource|The repository makes the file available for immediate download to the user.


This is over-engineered for a simple download where the only condition is that the researcher be based in Germany. Practical access policies are likely to require multi-step processes with complex criteria, often with asynchronous handoffs to third parties for approval. The framework above, while necessarily brief for the purposes of this document, illustrates how a more complex stateful negotiation process could be performed using ODRL as the basis for defining conditions,  executing those conditions through a centralised brokerage, and generating a rich set of accounting information for access requests and fulfilment across the supported repositories.


