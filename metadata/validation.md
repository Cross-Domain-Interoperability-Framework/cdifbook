# Validation of CDIF metadata documents

One of the requirements for all CDIF conformant metadata records is the inclusion of a conformance declaration that is part of the record. This is implemented in the schema.org JSON-LD implementation with a `schema:subjectOf` key that has `@type` `schema:Dataset` and `schema:additionalType` `dcat:CatalogRecord`. We refer to this as the catalog record part of the metadata — metadata about the metadata. The catalog record includes a `dcterms:conformsTo` statement that is a list of object references for the profiles the document conforms to.

```json
"schema:subjectOf": {
  "@id": "ex:gom-water-quality-wide-2025/catalog-record",
  "@type": ["schema:Dataset"],
  "schema:additionalType": ["dcat:CatalogRecord"],
  "...": "...",
  "dcterms:conformsTo": [
    {"@id": "https://w3id.org/cdif/core/1.1"},
    {"@id": "https://w3id.org/cdif/discovery/1.1"},
    {"@id": "https://w3id.org/cdif/data_description/1.1"}
  ]
}
```

The conformance URIs are set up to resolve as follows:

- The bare URI (e.g. `https://w3id.org/cdif/core/1.1`) resolves to the implementation guide in the release repository.
- The URI with `/schema` appended resolves to the JSON Schema for the profile. This schema only validates the classes and properties defined in the profile.
- The URI with `/shacl` appended resolves to a file with SHACL rules to validate the classes and properties defined in the profile.

Thus, to validate an instance document that declares conformance to more than one profile, each profile's validation artefact (schema or SHACL) must be retrieved and used to test the instance. The CDIF team has implemented Python tooling to execute this validation workflow. The tools live in the [CDIF validation repository](https://github.com/Cross-Domain-Interoperability-Framework/validation) on GitHub, created with the assistance of Anthropic's Claude Code. They are Python command-line tools; several also expose their validation logic as a function that can be imported into other code. The software is licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0), and the schemas, SHACL shapes, documentation and example metadata under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) (see [Licensing](#licensing) below).

These tools are prototypes for demonstrating the CDIF validation approach, and should be tested carefully before being relied on in a production setting. Feedback and contributions are welcome through the repository's issue tracker.

## Validation tools at a glance

### Framing and per-profile validation

CDIF metadata is JSON-LD, which is a **graph** format; JSON Schema validates **trees**. So validation is a two-step workflow: first **frame** the document (reshape the graph into the nested tree the schemas expect, using [`CDIF-frame-2026.jsonld`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/CDIF-frame-2026.jsonld)), then validate the framed result against a profile's JSON Schema. The framing step also normalises prefixes, embeds referenced nodes inline, and regularises arrays versus single values.

* [`tools/FrameAndValidate.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/tools/FrameAndValidate.py) — frames a JSON-LD document and validates it against a single profile's framed-tree JSON Schema. You tell it which profile to use with `--schema` (one of [`CDIFDiscoverySchema.json`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/CDIFDiscoverySchema.json), [`CDIFDataDescriptionSchema.json`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/CDIFDataDescriptionSchema.json), or [`CDIFCompleteSchema.json`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/CDIFCompleteSchema.json)). Use it when you know exactly which profile you want to test against. This is also the **normative source** for the per-profile `FrameAndValidate.py` copies shipped in each profile release repository.
* [`validate-cdif.bat`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/validate-cdif.bat) — a Windows batch wrapper around `FrameAndValidate.py` for running validation from inside the [oXygen XML Editor](https://www.oxygenxml.com/) (Tools → External Tools).
* [`validate-cdif.js`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/validate-cdif.js) — a Node.js command-line counterpart to `FrameAndValidate.py` that frames and validates a CDIF document using `jsonld` and `ajv`.

To facilitate simple JSON Schema validation for the common composite profiles, the framed-tree schemas are published for [core + Discovery](https://github.com/Cross-Domain-Interoperability-Framework/doc-corediscovery/blob/v1.1.1/CDIFDiscoveryDocStructuredSchema.json), [core + Discovery + DataDescription](https://github.com/Cross-Domain-Interoperability-Framework/doc-discoverydatadescription/blob/main/CDIFDataDescriptionProfileStructuredSchema.json), and [core + Discovery + DataDescription + DataStructure](https://github.com/Cross-Domain-Interoperability-Framework/doc-discoverydatadescriptionstructure/blob/main/CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json).

### Conformance-URI-driven validation

* [`ConformanceValidate.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/ConformanceValidate.py) — the profile-agnostic validator. Rather than being told which profile to use, it **discovers** the profiles from the document itself: it reads every `dcterms:conformsTo` URI in the catalog record (the `schema:subjectOf` entries tagged `dcat:CatalogRecord`) and validates the document against each profile's JSON Schema **and** SHACL rules. It can resolve those artefacts from the `w3id.org/cdif/` redirector (`--source w3id`, authoritative, needs network) or from a local map file (`--source local`, offline). It also compares what the record **declares** against what [`detect_conformance.py`](#content-derived-conformance-detection) finds in the content, and reports over-claims (a profile declared but not supported by the content — fatal) and under-claims (supported but not declared — advisory). It runs on a single file or a whole directory, and the engine is importable as `run_conformance(...)` so a web application can call it directly. Use it to ask "what does this document actually conform to, and how well?"
* [`conformance-schema-map.json`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/conformance-schema-map.json) — the local URI → schema/SHACL map that `ConformanceValidate.py --source local` reads, so the full validation can be run offline against the schemas and shapes in the repository.

### Content-derived conformance detection

* [`detect_conformance.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/detect_conformance.py) — derives which CDIF profiles a document **actually** conforms to from its content, independent of whatever it declares. For each managed CDIF class it runs a presence SPARQL `ASK` (for the elements the class introduces beyond its base) gated by a content-SHACL validity check; a profile is reported only when the content both shows the evidence and passes the shapes. `apply_conformance()` writes the detected `cdif:` URIs back into the catalog record's `dcterms:conformsTo`, preserving any non-`cdif:` (domain) claims. It is self-contained (it falls back to fetching the building-block SHACL from GitHub when there is no local checkout), and it is the module every `format → CDIF` converter in the [converters repository](https://github.com/Cross-Domain-Interoperability-Framework/converters) imports to set `conformsTo` from content.

### SHACL validation

JSON Schema validates structure; SHACL validates the RDF graph and can express constraints JSON Schema cannot — SPARQL-based targeting, cross-node relationships, and cardinality rules. The composite SHACL shape bundles are compiled from the modular `rules.shacl` files in the [metadataBuildingBlocks](https://github.com/Cross-Domain-Interoperability-Framework/metadataBuildingBlocks) repository, one bundle per profile ([`CDIF-Discovery-Shapes.ttl`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/ShaclValidation/CDIF-Discovery-Shapes.ttl), `CDIF-DataDescription-Shapes.ttl`, `CDIF-DataStructure-Shapes.ttl`, `CDIF-Provenance-Shapes.ttl`, `CDIF-Manifest-Shapes.ttl`, and the aggregate [`CDIF-Complete-Shapes.ttl`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/ShaclValidation/CDIF-Complete-Shapes.ttl)).

* [`ShaclValidation/ShaclJSONLDContext.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/ShaclValidation/ShaclJSONLDContext.py) — runs [pyshacl](https://github.com/RDFLib/pySHACL) validation of a CDIF document against a composite shape bundle.
* [`ShaclValidation/generate_shacl_report.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/ShaclValidation/generate_shacl_report.py) — produces a structured Markdown report of a SHACL run, grouping issues by severity (Violation → Warning → Info) and then by message, with each issue showing the focus node's `@type` and `schema:name` for context.

SHACL severity is aligned with JSON Schema: properties that are optional in the JSON Schema are `sh:Warning` (not `sh:Violation`) in SHACL, so only structurally required properties fail a record.

### Batch validation

* [`batch_validate.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/batch_validate.py) — runs both JSON Schema and SHACL validation across the repository's example corpora (the `testJSONMetadata`, `cdifbook`, and `cdifProfiles` file groups), with severity-aware per-file results and group and overall summaries. The `cdifbook` and `cdifProfiles` groups resolve from sibling GitHub repositories — local clones by default, or HTTPS-fetched artifacts with `CDIF_BATCH_SOURCE=github`.

### Supporting utilities

* [`tools/FlattenCDIF.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/tools/FlattenCDIF.py) — the inverse of framing: flattens a nested CDIF tree into the `@graph` form.
* [`tools/check_w3id_redirects.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/tools/check_w3id_redirects.py) — guards the two version pins that otherwise fail silently: it checks that each `w3id.org/cdif/<profile>/<version>/schema` returns the version it names, and that the CDIF book's links to release artefacts are not a version behind. Run weekly in CI.
* [`geocodes_harvester.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/geocodes_harvester.py) — harvests dataset metadata from the [EarthCube GeoCodes](https://geocodes.earthcube.org/) SPARQL catalog and optionally converts the records to CDIF core/discovery form.

### Generating the validation artefacts

The schemas and SHACL shapes are **generated** from the building-block sources, not hand-maintained, so they track the normative profiles.

* [`generate_validation_schema.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/generate_validation_schema.py) — generates the framed-tree validation schemas (`CDIFDiscoverySchema.json`, `CDIFDataDescriptionSchema.json`, `CDIFCompleteSchema.json`) from the building-block profile resolved schemas.
* [`generate_graph_schema.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/generate_graph_schema.py) — generates [`CDIF-graph-schema-2026.json`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/CDIF-graph-schema-2026.json), a JSON Schema that validates **flattened** JSON-LD (`@graph` arrays) directly, without framing. This graph schema is **indicative, not normative** — it is deliberately more permissive than the building blocks it is generated from, so a pass against it is a shape check, not a conformance verdict.
* [`ShaclValidation/generate_shacl_shapes.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/ShaclValidation/generate_shacl_shapes.py) — compiles the per-profile composite SHACL shape bundles from the building-block `rules.shacl` files.

## Choosing a tool

| If you want to … | Use |
|------------------|-----|
| Validate against **one** profile you already know | [`tools/FrameAndValidate.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/tools/FrameAndValidate.py) |
| Validate against **every** profile a record declares, with SHACL and a declared-vs-detected check | [`ConformanceValidate.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/ConformanceValidate.py) |
| Find out which profiles a record's **content** actually supports | [`detect_conformance.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/detect_conformance.py) |
| Run SHACL on its own | [`ShaclValidation/ShaclJSONLDContext.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/ShaclValidation/ShaclJSONLDContext.py) |
| Validate a whole corpus at once | [`batch_validate.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/batch_validate.py) |

## Getting started

```bash
git clone https://github.com/Cross-Domain-Interoperability-Framework/validation.git
cd validation
pip install PyLD jsonschema rdflib pyshacl requests

# Validate one document against the Discovery profile schema
python tools/FrameAndValidate.py my-metadata.jsonld -v \
    --schema CDIFDiscoverySchema.json --frame CDIF-frame-2026.jsonld

# Validate against every profile the record declares (schema + SHACL)
python ConformanceValidate.py my-metadata.jsonld --source local

# Detect which profiles the content actually supports
python detect_conformance.py my-metadata.jsonld
```

## Licensing

The CDIF validation repository is dual-licensed by content type, matching the [converters repository](converters.md):

| Content | License |
|---------|---------|
| Software: the Python tools and generators (`*.py`), the oXygen batch wrapper (`.bat`), the Node.js CLI (`.js`), and the CI workflows (`.github/`) | [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0) — see [`LICENSE`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/LICENSE) |
| The JSON Schemas (`*.json`), the JSON-LD frame and context (`*.jsonld`), the SHACL shape bundles (`*.ttl`), the documentation (`*.md`), and the example metadata records | [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) — see [`LICENSE-CC-BY-4.0`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/LICENSE-CC-BY-4.0) |

Third-party material bundled in the repository for testing and reference (for example the DDI-CDI normative schemas and the example metadata corpora) keeps its original license.

<!-- cdif-footer-include -->

:::{include} ../_static/footer.md
:::
