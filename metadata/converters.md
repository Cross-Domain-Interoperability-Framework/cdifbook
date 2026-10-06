# Metadata Converters

The following is a list of prototype converters that translate metadata between CDIF and other widely used metadata formats, created with the assistance of Anthropic's Claude Code. These are intended to demonstrate how common standard metadata formats can be used to generate CDIF-compliant metadata. CDIF is intended to function as an integration layer - a "lingua franca" - between domains and infrastructures, so being able to go to and from common standard formats which may be used in any particular domain is a fundamental approach. These tools show how that can be achieved.

The converters here are not necessarily sufficient for use in a production setting, and should be tested carefully before being deployed. They are all accompanied by sample metadata instances to illustrate what kinds of information are handled. Further, the mappings which are supported can be found in the [Simple Standard for Sharing Ontological Mappings (SSSOM)](https://mapping-commons.github.io/sssom/) format. They are tested against the example corpora in the repository, but the mappings are still being reviewed and output should be validated before it is published. Feedback and contributions are welcome through the repository's issue tracker. These converters are intended primarily as a demonstration of what is possible, and as an initial step in building a production solution.

The conversion tools live in the [CDIF converters repository](https://github.com/Cross-Domain-Interoperability-Framework/converters) on GitHub. They are Python command-line tools; most also expose their conversion as a function that can be imported into other code. All of the tools are licensed under the [Apache License Version 2.0](https://www.apache.org/licenses/LICENSE-2.0). The SSSOM mappings are licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).



## Converters at a glance

### [Science-on-Schema.org](https://github.com/ESIPFed/science-on-schema.org) (SOSO)

SOSO and CDIF are both schema.org profiles, so this conversion is structural alignment rather than vocabulary translation. The converters are plain Python with no third-party dependencies, and the correspondences are coded directly. The SSSOM tables `cdif-to-soso` and `soso-to-cdif` document the mapping rather than drive it; a drift-checker ([`soso/check\_soso\_mappings.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/soso/check_soso_mappings.py)) runs in CI to confirm that the tables and the code still agree on the example records. Properties with no counterpart are passed through unchanged, since both sides are open-world.

* SOSO → CDIF code: [`soso2cdif.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/soso2cdif.py), [`soso/ConvertFromSOSO.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/soso/ConvertFromSOSO.py). Accepts a local file or a URL; extracts embedded JSON-LD from an HTML landing page. Adds the `schema:` prefix to property names, rewrites the `@context`, fills CDIF-required fields where they can be derived (for example the identifier from the `@id`), wraps creators in an ordered `@list`, and adds the CDIF catalog record with `conformsTo` [detected from content](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/detect_conformance.py).
* CDIF → SOSO code: [`soso/ConvertToSOSO.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/soso/ConvertToSOSO.py). Rewrites the `@context` to SOSO style and strips the `schema:` prefixes; drops the CDIF catalog record, which SOSO has no equivalent for; warns about SOSO-required gaps rather than inventing values.

### [MLCommons Croissant](https://docs.mlcommons.org/croissant/)

Croissant describes machine-learning datasets with nested `RecordSet` and `Field` objects. Both converters read their SSSOM table at runtime through the shared [`sssom\_engine.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/sssom_engine.py). The table records which Croissant term maps to which CDIF term and on which class (`sc:name` means different things on a dataset, a file and a field); a short set of Python shapers, named in the table's `transform` column, builds the nested structures. An aliases table normalises variant source IRIs. Because much of the structural work is still in code, a drift-checker ([`croissant/check\_croissant\_mappings.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/croissant/check_croissant_mappings.py)) also runs in CI.

* CDIF → Croissant 1.1 code: [`croissant/ConvertToCroissant.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/croissant/ConvertToCroissant.py). Distributions become `cr:FileObject`; variables and physical mappings become `cr:RecordSet` / `cr:Field`. The conversion is lossy, since Croissant has no vocabulary for provenance or spatial and temporal extent; every CDIF property that cannot be carried has an explicit `unmapped` row, so the loss is documented. Where Croissant-required fields are missing, the converter falls back and warns.
* Croissant 1.0/1.1 → CDIF code: [`croissant/ConvertFromCroissant.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/croissant/ConvertFromCroissant.py). Lossy inverse, producing CDIF Data Description (or Discovery when there is no `recordSet`), with `conformsTo` [detected from content](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/detect_conformance.py).

### [W3C DCAT](https://www.w3.org/TR/vocab-dcat-3/)

DCAT metadata is typically serialized as a graph: datasets, distributions, agents and locations are separate nodes linked by identifiers, so the converter indexes every node and follows those links before mapping. It is fully table-driven: [`dcat-to-cdif.sssom.tsv`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/mappings/dcat-to-cdif.sssom.tsv) is read at runtime, with a `subject\_class` column saying which class each property belongs to and a `transform` column naming the shaper for structured values such as agents, qualified attributions, spatial and temporal coverage, and distributions. A second table, [`dcat-aliases.sssom.tsv`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/mappings/dcat-aliases.sssom.tsv), maps IRIs that publishers use by mistake to the ones they meant. Unmapped DCAT properties are preserved in the output.

* DCAT → CDIF code: [`DCAT/dcat\_to\_cdif.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/DCAT/dcat_to_cdif.py). Reads a DCAT JSON-LD catalog or dataset; can list the datasets in a catalog and convert all of them or a selection. Covers DCAT 3, DCAT-AP and its extensions (HVD, GeoDCAT-AP, mobilityDCAT-AP, HealthDCAT-AP, MLDCAT-AP), the German, Spanish and Norwegian national profiles, and DCAT-US 3.0 and 1.1. [`DCAT/build\_corpus.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/DCAT/build_corpus.py) converts the full example corpus as a regression test.

### [NASA Common Metadata Repository (CMR) Unified Metadata Model-Collections (UMM-C)](https://www.earthdata.nasa.gov/about/esdis/eosdis/cmr)

UMM-C is the JSON collection metadata model of NASA's Common Metadata Repository (CMR). The converter follows the DCAT pattern: it reads [`ummc-to-cdif.sssom.tsv`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/mappings/ummc-to-cdif.sssom.tsv) at runtime, where each row gives a UMM-C JSON path, the CDIF target, and a `transform` naming one of the converter's shaper functions (for contacts, GCMD keywords, spatial geometries, temporal extents, related URLs and so on). Row order sets precedence. Related URLs that match no mapping row are kept under a `ummc:` namespace rather than dropped. The mapping rows are tool-suggested and **still under curator review**.

* UMM-C → CDIF code: [`UMM/umm\_to\_cdif.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/UMM/umm_to_cdif.py). Accepts a CMR search result, a single record, or a CMR search URL, and writes one CDIF Discovery record per collection. Optionally fetches the collection's associated UMM-Var records to describe its variables, and adds GCMD Keyword Management System (KMS) concept URIs to keywords.

### [DDI Codebook](https://ddialliance.org/Specification/DDI-Codebook/) 1.2.2 and 2.5

DDI Codebook 2.5 is an element-level superset of 1.2.2, so one data-driven engine handles both. The SSSOM worksheets list every literal-valued element in the DDI Codebook XML Schemas, whether or not CDIF has a target for it, factored into a common table plus version-specific extras. [`mappings/sync\_ddi\_mappings.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/mappings/sync_ddi_mappings.py) checks the worksheets against the XSDs and compiles the mapped rows into [`ddi\_mappings.json`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/mappings/ddi_mappings.json), which the engine applies. Shapering code builds what a table cannot express, such as variable value domains, shared code lists and summary statistics. Study metadata becomes the CDIF dataset, variables become `cdi:InstanceVariable`, and data files become distributions.

* DDI → CDIF code: [`DDI/ddi2cdif.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/DDI/ddi2cdif.py). Single entry point: detects the DDI flavour and version and routes the file to the right converter, including DDI-CDI XML (below). The engine is [`DDI/ddi\_sssom\_to\_cdif.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/DDI/ddi_sssom_to_cdif.py); [`DDICodebook/ddi25\_to\_cdif.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/DDICodebook/ddi25_to_cdif.py) calls it for 2.5, and [`DDI/ddi122\_to\_cdif.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/DDI/ddi122_to_cdif.py) provides the claude-coded value-domain and statistics build it delegates to.
* DDI Codebook 2.5 (Harvard Dataverse) → CDIF code: [`DDI/ddi\_to\_cdif.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/DDI/ddi_to_cdif.py). A thin layer over the engine that adds file size, checksum and column headers from the Dataverse API.

### [DDI-CDI](https://ddialliance.org/Specification/DDI-CDI/) 1.0

A DDI-CDI XML instance is a flat list of typed objects linked by references (basically a graph), with associations represented as objects of their own. The converter indexes every object by its identifier, resolves the references to rebuild the graph, and then down-shifts the verbose DDI-CDI model to the simpler CDIF profile form, following the class and attribute crosswalk maintained in the CDIF `ucmism2m` project. It is claude-coded and was built in six phases: dataset skeleton, value domains and code lists, data structure, physical mappings, provenance, and discovery enrichment.

* DDI-CDI XML → CDIF code: [`DDI-CDI/ddicdi\_to\_cdif.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/DDI-CDI/ddicdi_to_cdif.py), also reached through [`DDI/ddi2cdif.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/DDI/ddi2cdif.py). The RDF / JSON-LD serialisation of DDI-CDI is not yet supported.

### [RO-Crate](https://www.researchobject.org/ro-crate/) 1.2

RO-Crate and CDIF both use schema.org, but RO-Crate is a flat `@graph` with unprefixed terms, while CDIF is a nested tree with prefixed terms. These converters use no mapping table: they are pipelines of standard JSON-LD operations (expand, flatten, compact, frame) built on the `pyld` library, followed by small fix-ups for each format's conventions.

* CDIF → RO-Crate code: [`ROCrate/ConvertToROCrate.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/ROCrate/ConvertToROCrate.py). Flattens and compacts with the RO-Crate 1.2 context, adds the `ro-crate-metadata.json` descriptor, sets the root dataset identifier to `./`, and makes sure the license and the root `hasPart` list are present.
* RO-Crate → CDIF code: [`ROCrate/ROCrateToCDIF.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/ROCrate/ROCrateToCDIF.py). Frames the crate into a nested CDIF tree, moves downloadable files into distributions, and can validate the result.
* Validator: [`ROCrate/ValidateROCrate.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/ROCrate/ValidateROCrate.py) checks RO-Crate 1.2 structural requirements and, optionally, the RO-Crate SHACL shapes.

## How the converters are implemented

### A common target shape

Every converter that produces CDIF writes **JSON-LD built on schema.org**, rooted at a `schema:Dataset`, with `schema:`-prefixed property names and a `schema:subjectOf` **catalog record** that carries the metadata about the metadata. This is the framed tree that the CDIF JSON Schemas validate. The source formats differ from it mainly in structure: SOSO uses bare schema.org terms and has no catalog record; Croissant nests `RecordSet` and `Field`; DCAT is a graph linked by identifiers; DDI uses its own XML vocabularies. Each converter reconciles those structural differences as well as translating terms.

### Mapping tables plus shapers

The property-level mappings for each converter are recorded as [SSSOM](https://mapping-commons.github.io/sssom/) mapping sets in the repository's [`mappings/`](https://github.com/Cross-Domain-Interoperability-Framework/converters/tree/main/mappings) directory, one per source → target direction. An SSSOM table is a flat list of term-to-term correspondences. The project extends it with a few columns: the class the source property sits on, where the value lands in the CDIF record, and a named **transform**.

A flat table cannot express all the structural work a conversion needs, such as resolving references between graph nodes, collapsing reified associations, reshaping code lists into `skos:ConceptScheme`, or creating the catalog record. The converters therefore follow a **hybrid** pattern:

* The **table** carries the term correspondences: the bulk of the mapping.
* A short list of **shaper functions**, named in the table's `transform` column, builds the structured values.

The DCAT, UMM-C, Croissant and DDI Codebook converters **read their table at runtime**, so editing the table changes the conversion and the table cannot drift away from the code. For DDI, the worksheets are compiled into a single [`ddi\_mappings.json`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/mappings/ddi_mappings.json) that the converter reads. The SOSO tables, and parts of the Croissant ones, still document the conversion rather than drive it. Automated drift-checkers compare those tables with the converter's actual behaviour on the example corpus and fail the repository's CI when they disagree.

### Declaring conformance from content

A CDIF record declares the profiles it conforms to with `dcterms:conformsTo` in its catalog record. Rather than hard-coding that list, the converters to CDIF test each record's **actual content** using the [`detect\_conformance.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/detect_conformance.py) module from the CDIF [validation](https://github.com/Cross-Domain-Interoperability-Framework/validation) repository: `detect\_conformance()` works out which profiles the record meets, and `apply\_conformance()` writes them into the catalog record's `dcterms:conformsTo`. For each CDIF profile it runs a SPARQL check for the elements the profile introduces, followed by a SHACL validity check. Only the profiles the content actually meets are declared. A record that meets none declares none, because a false claim would select schemas and shapes the record cannot satisfy. A `--static-conformance` option keeps a converter's built-in default list instead.

### Validation

Most converters can validate their output (`--validate`), framing the JSON-LD and checking it against the CDIF JSON Schemas. The `validation` repository's [`FrameAndValidate.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/tools/FrameAndValidate.py) tool does the same for any CDIF record. Croissant output can be checked with the MLCommons `mlcroissant` validator, and SOSO output with the SOSO SHACL shapes.

## What is in the repository

|**Path**|**Contents**|
|-|-|
|[`soso2cdif.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/soso2cdif.py)|Front end for SOSO → CDIF from a file or URL|
|[`sssom\_engine.py`](https://github.com/Cross-Domain-Interoperability-Framework/converters/blob/main/sssom_engine.py)|Shared engine that applies an SSSOM mapping table to a document|
|[`soso/`](https://github.com/Cross-Domain-Interoperability-Framework/converters/tree/main/soso)|CDIF ↔ SOSO converters, the SOSO mapping drift-checker and an example|
|[`croissant/`](https://github.com/Cross-Domain-Interoperability-Framework/converters/tree/main/croissant)|CDIF ↔ Croissant converters, the mapping documents, a drift-checker, and example corpora (Croissant exports harvested from Dataverse and from MLCommons) with their CDIF conversions|
|[`DCAT/`](https://github.com/Cross-Domain-Interoperability-Framework/converters/tree/main/DCAT)|The DCAT converter, a regression harness, a profile-coverage workbook, a DCAT-AP vs DCAT-US comparison, and a corpus of about 780 upstream DCAT examples. The 240 that describe a `dcat:Dataset` are converted in `cdifOK/`|
|[`UMM/`](https://github.com/Cross-Domain-Interoperability-Framework/converters/tree/main/UMM)|The UMM-C converter, the UMM-C and UMM-Var JSON Schemas it is written against, and example CMR records with their CDIF conversions|
|[`DDI/`](https://github.com/Cross-Domain-Interoperability-Framework/converters/tree/main/DDI)|The DDI flavour dispatcher, the data-driven DDI Codebook engine, the DDI 1.2.2 and Harvard Dataverse converters, the DDI 1.2.2 schema, and examples|
|[`DDICodebook/`](https://github.com/Cross-Domain-Interoperability-Framework/converters/tree/main/DDICodebook)|The DDI Codebook 2.5 converter, the 2.5 schema, notes on the 2.5 additions, and examples|
|[`DDI-CDI/`](https://github.com/Cross-Domain-Interoperability-Framework/converters/tree/main/DDI-CDI)|The DDI-CDI 1.0 XML converter and examples|
|[`ROCrate/`](https://github.com/Cross-Domain-Interoperability-Framework/converters/tree/main/ROCrate)|The RO-Crate converters and validator, with example RO-Crate and CDIF records|
|[`mappings/`](https://github.com/Cross-Domain-Interoperability-Framework/converters/tree/main/mappings)|The SSSOM mapping tables (`\*.sssom.tsv`) and their metadata sidecars (`\*.sssom.yml`) for every converter, alias tables, and the scripts that keep the sidecars and the DDI mapping file in sync|
|[`validation/`](https://github.com/Cross-Domain-Interoperability-Framework/validation)|A git submodule containing the CDIF validation repository: `detect\_conformance`, the CDIF schemas and frame, and validation tools|

Each directory has its own `README.md` with usage and mapping details.

## Getting started

```bash
git clone https://github.com/Cross-Domain-Interoperability-Framework/converters.git
cd converters
git submodule update --init      # optional: content-based conformance detection and local schemas
pip install -r requirements.txt

python soso2cdif.py https://example.org/dataset -o out.json
python DCAT/dcat\_to\_cdif.py catalog.jsonld --output ./out --validate
```

The `validation` submodule is optional. Without it the converters still run, but they fall back to a default `conformsTo` list, and validation fetches the schemas from GitHub.

## Licensing

The converter software is licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0). The mapping tables, documentation and converted example metadata are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Third-party examples and schemas bundled for testing keep their original licenses.

<!-- cdif-footer-include -->

:::{include} ../\_static/footer.md
:::

