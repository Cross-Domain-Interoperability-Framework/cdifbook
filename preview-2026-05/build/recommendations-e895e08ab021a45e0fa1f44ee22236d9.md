# Metadata Recommendations

CDIF metadata recommendations are packaged in a [set of profiles](https://cross-domain-interoperability-framework.github.io/cdifbook/preview-2026-05/background/development/#production-of-cdif-profiles). Each profile includes:
1. A Content model that specifies the information expected to be included in any metadata record, with required, recommended and optional content items. Each profile description starts with a description of its content model, e.g. for the [core profile](https://cross-domain-interoperability-framework.github.io/cdifbook/preview-2026-05/metadata/core/#information-model)
2. A JSON-LD serialization for that content using the Schema.org vocabulary to define the fields in a metadata record, e.g. [Core profile JSON-LD serialization](./coreSchemaImplementationNew.md), and an [implementation using the DCAT rdf vocabulary](./dcat.md)
3. Tools for validating metadata instance documents, including JSON schema, SHACL rules, a JSON-LD framing document, and python code to run validation. 

Each CDIF profile is housed in a github repository containing an implementation guidance document, JSON schema, SHACL rules, a JSON-LD framing document, and a subdirectory containing examples. 

An additional critical part of the recommendations is description of [Workflows to publish CDIF metadata](./publication.md) so that is can be found and indexed by search providers using standard web technology.


