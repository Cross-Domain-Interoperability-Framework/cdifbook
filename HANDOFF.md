# Handoff: Metadata Converters page (updated 2026-10-24)

For whoever continues the converters-page work in this repo — typically a new
Claude Code session opened in `cdifbook`. The work started in a session in the
`converters` repo; this file carries what that session knew. Delete it once
the page is merged into `main`.

## State

- `metadata/converters.md` and its `myst.yml` TOC entry are committed on the
  preview branch **`cdifBookUpdates`**, which publishes to
  `https://cross-domain-interoperability-framework.github.io/cdifbook/preview-2026-05/`
  (the path keeps its old name). They are **not on `main`**, so a checkout on
  `main` has no converters page.
- The branch is 9 commits ahead of `main`. Besides the converters page it
  carries the **validation page rewrite** (`6316dd09`, `09f0e4b5`); merging the
  branch publishes both.
- **Waiting on reviews from Arofan and Simon H.** before merging
  `cdifBookUpdates` into `main`, which deploys the live book.
- The page reflects the converters repo as of converters PR #11 (2026-10-24):
  SOSO, Croissant, FAIR², DCAT, UMM-C, DDI Codebook, DDI-CDI (XML and JSON-LD,
  plus the RepresentedVariable extractor for the CODATA variable index),
  RO-Crate, OHDSI gaiaCatalog, and the GeoCodes harvester.
- `metadata/validation.md` has uncommitted local edits that are not part of
  the converters work. Commit converters-page changes by path, not with
  `git add -A`.

## Open follow-ups

- **UMM-C and FAIR² sections** say the mapping rows are "still under curator
  review" — update after Stephen reviews `mappings/ummc-to-cdif.sssom.tsv`,
  `fair2-to-cdif.sssom.tsv` and `fair2-aliases.sssom.tsv` in `converters`. The
  OHDSI mapping table and its `includedInDataCatalog` question are also
  awaiting curator review.
- **FAIR² section** may need revising once fair-squared/fair2-spec#7 is answered.
- **"How the converters are implemented"** partly repeats the per-converter
  paragraphs; it could be trimmed to the shared ideas (target shape, table +
  shapers, conformance detection, validation).
- **`croissant/*.docx` background papers** are listed as third-party in the
  converters README licence section; move them under CC BY there if they are
  project-authored (a `converters` repo change).

## Decisions and conventions

- **Licensing** (converters PR #2): Apache-2.0 for software; CC BY 4.0 for
  mapping tables, docs and converter-produced example metadata. Third-party
  examples and schemas keep their own licences.
- **Links:** converter file/directory names link to
  `https://github.com/Cross-Domain-Interoperability-Framework/converters/blob|tree/main/...`;
  `detect_conformance.py`, `ConformanceValidate.py` and
  `tools/FrameAndValidate.py` link to the `validation` repo on `main`. Use
  `[text](url)` — a bracketed `[https://...]` renders as literal text.
- **No backslash escapes inside code.** The page came from a document
  converter that wrote `\_` everywhere. In ordinary text that is a harmless
  escape, but inside `` `inline code` `` or a fenced block MyST prints the
  backslash (`ddicdi\_to\_cdif.py`), so write plain underscores there. The
  footer include is `../_static/footer.md` (unescaped), as on every other page.
- **Conformance wording:** the function is `detect_cdif_conformance()`
  (`detect_conformance()` is its deprecated alias). Profiles are declared by
  *use*: discovery only when a record uses `measurementTechnique`,
  `variableMeasured`, `spatialCoverage`, `temporalCoverage` or
  `hasQualityMeasurement` (validation 512ee58); 47 of the 289 DCAT corpus
  records declare it.
- **Source of facts:** the page summarises the converters repo READMEs
  (root `README.md`, each format's `README.md`, `mappings/README.md`). Check
  them before changing a converter's description.

## Practical notes

- Local preview: `conda run -n cdifbook jupyter-book start`, then
  http://localhost:3000/metadata/converters (rebuilds on save).
- Published preview: a push to `cdifBookUpdates` runs "Deploy preview
  (cdifBookUpdates)", which pushes to `gh-pages`, then GitHub's Pages build
  runs. GitHub's CDN can serve the old page for up to ~10 minutes after both
  finish; add a query string (`?v=2`) to the URL to see the new one.
- `_build/` (ignored) holds stale copies of earlier page versions; a search
  over the repo will find their old text. Exclude it.
- Stephen also edits the page in an editor; reload the file before editing to
  avoid overwriting each other's saves.
