# CDIF Core Profile — Schema.org Implementation

This page documents the mapping between CDIF content items and their schema.org implementation. Some example metadata documents are accessible in the [Core Github repository](https://github.com/Cross-Domain-Interoperability-Framework/profile-core/tree/main/examples). The \'Obl.\' column specifies the cardinality obligation for the property; \'1\' means one value required; 1..\* means at least one value is required; 0..\* means the property is optional and more that one value can be provided. Properties with path from "subjectOf" describe the metadata.

All property names use namespace prefixes as declared in the `@context` (e.g. `schema:`, `dcterms:`). The `schema:` prefix is required for all schema.org properties. The CDIF JSON-LD implementation uses a hierarchical JSON structure, and CURIE syntax to abbreviate URIs using prefixes defined in the JSON-LD context.  The implementation does not map un-prefixed JSON keys to URIs, rather prefixes a namespace abbreviation on the key label to represent the URI.  This enables using standard JSON schema to validate documents and avoids confusion about the vocabulary origin of keys used in the JSON.

Each item lists its obligation, JSON encoding, and a scope note explaining usage. 

## Metadata identifier
- **Obligation:** mandatory
- **JSON:** `"schema:subjectOf" / "@id": "{URI}"`
- **Scope note:** The URI for the metadata record should be the `@id` value for the `schema:subjectOf` node. This node has `@type ["schema:Dataset"]` with `schema:additionalType ["dcat:CatalogRecord"]`, and a `schema:about` property referencing the `@id` of the root resource node.

## Resource identifier
- **Obligation:** mandatory
- **JSON:** `"schema:identifier": {PropertyValue or string}`
- **Scope note:** The primary identifier for the resource. Can be a simple string (ideally a resolvable URI), or a `schema:PropertyValue` with `propertyID` (identifier scheme, e.g. from [identifiers.org](https://registry.identifiers.org/registry/)), `value` (the identifier string), and `url` (resolvable link). The PropertyValue approach is strongly recommended following the [ESIP Science on Schema.org guidance](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#identifier).

## Title
- **Obligation:** mandatory
- **JSON:** `"schema:name": {string}`
- **Scope note:** A set of words that should identify the described resource for human use. Ideally, should be unique in the scope of the metadata catalog containing this metadata record.

## Distribution
- **Obligation**: mandatory.*Either a `schema:url` or a `contentUrl` inside `schema:distribution` is required to indicate how to get the resource*.
- *Landing page URL*
  - **JSON:** `"schema:url": {URL}`
  - **Scope note:** This URL is generally expected to GET an HTML landing page about the resource.
- *Distribution array*
  - **JSON:**
    ```json
    "schema:distribution": [
      {"@type": ["schema:DataDownload"], "schema:contentUrl": {URL}, ... },
      {"@type": ["schema:WebAPI"], "schema:serviceType": ..., ... }
    ]
    ```
- **Scope note:** An array of distribution objects. Items may be DataDownload (file-based access) or WebAPI (service-based access). A DataDownload must include `schema:contentUrl`, and should include `schema:encodingFormat` and `dcterms:conformsTo`. The `@type` is encoded as an array (e.g. `["schema:DataDownload"]`).

## Rights
- **Obligation:** 1..*
- **JSON:** `"schema:license": [{text or URI or CreativeWork}, ...]` or `"schema:conditionsOfAccess": [{text or URI}, ...]`
- **Scope note:** At least one of `schema:license` or `schema:conditionsOfAccess` must be provided (as arrays). URL to license document or text explanation of restrictions on use. There might be multiple links to documents specifying related security, privacy, usage, sharing, etc. concerns.

## Metadata profile identifier
- **Obligation:** 1..*
- **JSON:**
  ```json
  "schema:subjectOf" / "dcterms:conformsTo": [
    {"@id": "https://w3id.org/cdif/core/1.0/"},
    {"@id": "https://w3id.org/cdif/discovery/1.0/"}
  ]
  ```
- **Scope note:** An array of objects, each with an `@id` property whose value is a conformance URI. For CDIFDiscovery, both the core and discovery URIs are required. Extended profiles add their own conformance URIs to this array.

## Metadata date
- **Obligation:** 0..1
- **JSON:** `"schema:subjectOf" / "schema:sdDatePublished": {Date}`
- **Scope note:** Use ISO 8601 format. The most recent publication date for the metadata content. Harvesters use this to determine if they have already harvested and processed this record.

## Metadata contact
- **Obligation:** 0..1
- **JSON:** `"schema:subjectOf" / "schema:maintainer": {Person or Organization}`
- **Scope note:** Should include a name and contact point (institutional e-mail is best) for the agent responsible for metadata content. This is the contact point to report problems with metadata content. Person and Organization are Agent objects with various properties.

## Metadata catalog
- **Obligation:** 0..1
- **JSON:**
  ```json
  "schema:subjectOf" / "schema:includedInDataCatalog": {
    "@type": "schema:DataCatalog",
    "schema:name": ...,
    "schema:url": ...
  }
  ```
- **Scope note:** Identifies the data catalog or repository containing this metadata record. Value is a `schema:DataCatalog` with at least a name and URL.

## Resource type
- **Primary type — `@type`**
  - **Obligation:** mandatory
  - **JSON:** `"@type": ["schema:Dataset", ...]`
  - **Scope note:** An array of schema.org type values using the `schema:` prefix. Must include `"schema:Dataset"`. Additional allowed types: `schema:CreativeWork`, `schema:SoftwareApplication`, `schema:SoftwareSourceCode`, `schema:Product`, `schema:WebAPI`, `schema:DigitalDocument`, `schema:Collection`, `schema:ImageObject`, `schema:DataCatalog`, `schema:DefinedTermSet`, `schema:MediaObject`.
- **Additional type — `schema:additionalType`**
  - **Obligation:** 0..*
  - **JSON:** `"schema:additionalType": [{DefinedTerm or string}, ...]`
  - **Scope note:** If a more specific resource type needs to be specified using a vocabulary other than schema.org, add a text or URI value here. Must be consistent with the `@type`. Always encode as an array.

## Description
- **Obligation:** 0..1
- **JSON:** `"schema:description": {string}`
- **Scope note:** Free text, with as much detail as is feasible.

## Originators
- **Obligation:** 0..*
- **JSON:** `"schema:creator": {"@list": [{Person or Organization}, ...]}`
- **Scope note:** Author or originator of intellectual content. Uses the JSON-LD `@list` construct to preserve author order. Each item can be a Person, Organization, or an object reference (`{"@id": "..."}`) to an agent defined elsewhere. Use ORCID or other PID to identify persons where possible.

## Publication Date
- **Obligation:** 0..1
- **JSON:** `"schema:datePublished": {date time}`
- **Scope note:** Date on which the resource was made publicly accessible. Use ISO 8601 format.

## Modification Date
- **Obligation:** mandatory
- **JSON:** `"schema:dateModified": {date time}`
- **Scope note:** Date of most recent update to resource content. If Publication Date is not provided, defaults to the Modification Date. Use ISO 8601 format.

## Other identifiers
- **Obligation:** 0..*
- **JSON:** `"schema:sameAs": [{URI or PropertyValue}, ...]`
- **Scope note:** Other identifiers for the same resource, as IRI reference strings, object references (`{"@id": "..."}`), or structured identifiers using `schema:PropertyValue`.

## Version
- **Obligation:** 0..1
- **JSON:** `"schema:version": {string or number}`
- **Scope note:** The version number or identifier for this resource. Values should sort from oldest to newest using an alphanumeric sort on version strings.

## Language
- **Obligation:** 0..1
- **JSON:** `"schema:inLanguage": {string}`
- **Scope note:** The language of the dataset content (e.g. `"en"`, `"fr"`).

## Keyword
- **Obligation:** 0..*
- **JSON:**
  ```json
  "schema:keywords": [
    {string},
    {
      "@type": "schema:DefinedTerm",
      "schema:name": "OCEANS",
      "schema:inDefinedTermSet": "gcmd:sciencekeywords",
      "schema:identifier": {...}
    },
    ...
  ]
  ```
- **Scope note:** Implement with text for tags, and `schema:DefinedTerm` for keywords from a controlled vocabulary. The DefinedTerm approach is used to represent concepts with links to their defining vocabulary. Recommend using DefinedTerm for all keywords if any are from a known vocabulary.

## Distribution Agent
- **Single provider**
  - **Obligation:** 0..*
  - **JSON:** `"schema:provider": [{Person or Organization}, ...]`
  - **Scope note:** Contact point for the provider of a distribution. For a simple digital object with a download URL, or a resource with multiple distributions all from the same provider.
- **Per-distribution provider**
  - **Obligation:** 0..*
  - **JSON:** `"schema:distribution": [{"@type": ["schema:DataDownload"], "schema:provider": [{Person or Organization}]}, ...]`
  - **Scope note:** If there are multiple distributions with different providers, each distribution can have a separate provider array.

## Related agents (contributor role)
- **Obligation:** 0..*
- **JSON:** `"schema:contributor": [{Person or Organization}, ...]`
- **Scope note:** Recognition for others who have contributed to the production of the resource but are not recognized as authors/creators.

## Related agent (other role)
- **Obligation:** 0..*
- **JSON:**
  ```json
  "schema:contributor": [{
    "@type": "schema:Role",
    "schema:roleName": "Principal Investigator",
    "schema:contributor": {
      "@type": "schema:Person",
      "@id": "https://orcid.org/...",
      "schema:name": "John Doe",
      "schema:affiliation": {
        "@type": "schema:Organization",
        "@id": "https://ror.org/...",
        "schema:name": "..."
      },
      "schema:contactPoint": {
        "@type": "schema:ContactPoint",
        "schema:email": "john.doe@example.org"
      }
    }
  }]
  ```
- **Scope note:** To assign roles to contributors like editor, maintainer, publisher, point of contact, copyright holder (e.g. DataCite contributor types), use the [role construction defined by schema.org](http://blog.schema.org/2014/06/introducing-role.html).

## Related resources
- **Obligation:** 0..*
- **JSON:**
  ```json
  "schema:relatedLink": [{
    "@type": "schema:LinkRole",
    "schema:linkRelationship": "...",
    "schema:target": {
      "@type": "schema:EntryPoint",
      "schema:encodingFormat": "text/html",
      "schema:name": "...",
      "schema:url": "https://example.org/data/stations"
    }
  }]
  ```
- **Scope note:** Use schema.org `relatedLink` with a `LinkRole` value, and the link URL in a `target` EntryPoint object. These properties expect WebPage and Action as their domain, so the [schema.org validator](https://validator.schema.org/) will throw a warning (not an error). Related-resource links are useful for evaluation and use of data, but because of the wide variety of relationship possibilities they are difficult to use in general search scenarios. Use a soft-type implementation, with a link-relationship type using a `schema:DefinedTerm`, and a resolvable identifier for the relationship target.

## Funding
- **Obligation:** 0..*
- **JSON:**
  ```json
  "schema:funding": [{
    "@type": "schema:MonetaryGrant",
    "schema:identifier": {
      "@type": "schema:PropertyValue",
      "schema:propertyID": "grant-id",
      "schema:value": "..."
    },
    "schema:name": "grant title",
    "schema:funder": {
      "@id": "https://ror.org/...",
      "@type": "schema:Organization",
      "schema:name": "org name"
    }
  }]
  ```
- **Scope note:** Use schema.org encoding and the [Science on Schema.org pattern](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/Dataset.md#funding). Other organization properties can be included in the funder Organization.

## Policies
- **Obligation:** 0..*
- **JSON:**
  ```json
  "schema:publishingPrinciples": [{
    "@type": "schema:CreativeWork",
    "schema:name": "...",
    "schema:url": "..."
  }, ...]
  ```
- **Scope note:** FDOF `digitalObjectMutability`, RDA `digitalObjectPolicy`, FDOF `PersistencyPolicy`. Policies related to maintenance, update, and expected time to live.

## Checksum
- **Obligation:** 0..1
- **JSON:**
  ```json
  "schema:distribution": [{
    "@type": ["schema:DataDownload"],
    "spdx:checksum": {
      "@type": "spdx:Checksum",
      "spdx:algorithm": "SHA256",
      "spdx:checksumValue": "abc123..."
    },
    ...
  }, ...]
  ```
- **Scope note:** A string value calculated from the content of the resource representation, used to test if content has been modified. No schema.org property; follow DCAT v3 adoption of the [Software Package Data Exchange (SPDX)](https://spdx.org/rdf/terms/) property. The `spdx:Checksum` object has two properties: `algorithm` and `checksumValue`. The checksum is a property of each distribution / DataDownload.

## Provenance for discovery
Provenance for discovery is limited to documenting technology used in the creation of the dataset and documenting other datasets that were inputs to the content of the described resource. The cdifDiscovery profile specifies only that `prov:wasGeneratedBy` has a `prov:Activity` with `prov:used` items that are strings or `@id` references. Any additional structure under `prov:used` is optional and defined by extended profiles.

### *Provenance (instruments, software, etc.)*
- **Obligation:** 0..*
- **JSON:**
  ```json
  "prov:wasGeneratedBy": [{
    "@type": ["prov:Activity"],
    "prov:used": [
      "nerc:collection/L05/current/134",
      {"@id": "nerc:collection/B76/current/B7600031"}
    ]
  }]
  ```
- **Scope note:** Identify sensors, instruments, platforms, software, algorithms, etc. used in the creation of the described resource. The `prov:used` array accepts strings (URIs or labels) or object reference (`{"@id": "..."}`).

### *Provenance (input datasets)*
- **Obligation:** 0..*
- **JSON:**
  ```json
  "prov:wasDerivedFrom": [
    "http://doi.org/10.547/347848",
    {"@id": "http://doi.org/10.3578/h5ls"},
    {"@type": "schema:CreativeWork", "schema:name": "...", "schema:url": "..."}
  ]
  ```
- **Scope note:** Identify datasets that were inputs to the content of the described resource. Items can be strings (URIs), object reference (`{"@id": "..."}`), or CreativeWork objects with name and URL.




## Service-based distribution
An API builds on a basic communication protocol (e.g. HTTP) by defining functionality and formatting to enable providing the specific data a user requires. This might involve filtering, subsetting, or various transformations for e.g. schema mapping, aggregating or anonymizing data. The focus here is on Web APIs that provide data using a URL for the endpoint location (the server that implements the data access protocol), with parameters to specify the particular data requested. The query parameters might be appended to this base URL as part of the URL, or provided as a message with the request. The implementation is based on the schema.org Action patterns. A WebAPI distribution is included as an item in the `schema:distribution` array alongside DataDownload items.

Implementation of metadata to describe a service-based (API) distribution:

## Service type
- **Obligation:** 1
- **JSON:**
```json
  "schema:distribution": [{
    "@type": ["schema:WebAPI"],
    "schema:serviceType": "{string or DefinedTerm}"
  }]
```
- **Scope note**: Specify the kind of service. Ideally this should be a resolvable identifier. Currently there is no widely adopted registry for serviceType identifiers. For interoperability, there must be an external arrangement between data providers and consumers on the strings that will be used to specify service types.
## Service description document
- **Obligation**: 0..1
- **JSON**:`"schema:documentation": "{string or CreativeWork}"`
- **Scope note**: Document that provides a machine-actionable description of a service instance. Examples include OpenAPI documents, OGC Capabilities documents.
## Endpoint URL
- **Obligation**: 1
- **JSON**:
```"schema:potentialAction": [{
  "@type": ["schema:Action"],
  "schema:target": {
    "@type": "schema:EntryPoint",
    "schema:urlTemplate": "..."
  }
}]
```
- **Scope note**: Web location to invoke service; if there are parameters on the URL, the URL template construct enables description of the parameters.
## Access constraints
- **Obligation**: 1
- **JSON**:`"schema:termsOfService": "{string or CreativeWork}"`
- **Scope note**: Description of access privileges required to use the API, e.g. registration, licensing, payments.