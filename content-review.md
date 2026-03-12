# Content Review Report

Date: 2026-03-12  
Repository: `Cross-Domain-Interoperability-Framework/cdifbook`

## Purpose

This report summarizes current content-quality diagnostics from the migrated Jupyter Book v2 workflow, with focus on broken links, missing assets, citation parse issues, and practical recommendations for maintainer discussion.

## Validation Method

Diagnostics were collected from:

```bash
jupyter-book build --site --html --check-links
```

Log snapshot source: `/tmp/cdif-content-check.log` (extracted issue lines in `/tmp/cdif-content-issues.log`).

## Executive Summary

- Build command exits successfully (`EXIT:0`), but diagnostics include **72** warning/error lines.
- Breakdown:
  - **30 warnings** (`⚠️`)
  - **42 errors** (`⛔️`)
- Main issue classes:
  - **39** unresolved links
  - **29** unresolved citation-label parses
  - **2** placeholder links (`[TBD]()`)
  - **1** missing image asset
  - **1** legacy internal link syntax warning

This is not a build-system failure; it is primarily a **content quality and link hygiene** backlog.

## Issue Inventory

## 1) High-priority content defects

### A. Placeholder links with empty targets
- File: `universals/univuom.md`
- Symptoms:
  - `Link has no URL: TBD`
  - `Link for "" did not resolve`
- Likely cause: inline placeholders such as `[TBD]()`.

### B. Missing image asset
- File: `data_integration/ddidescriptiondatastructure.md`
- Symptom:
  - `Cannot find image "./figures/datacubeexample.jpg" in data_integration`
- Current folder contents indicate this file does not exist in `data_integration/figures/`.

## 2) Repeated parse/citation warnings

Top contributor:
- `data_access/odrlsharedservices.md` (22 diagnostics)

Related files:
- `metadata/schemaorgimplementation.md`
- `metadata/dcat.md`
- `metadata/signpostinglinkrel.md`
- `metadata/pidkernel.md`

Symptom pattern:
- `Could not link citation with label "context"|"type"|"id"|"lan"|"en"|"languageCode"`

Interpretation:
- MyST is interpreting bracketed terms in prose/table pseudo-JSON as citation/reference labels.
- This commonly happens when JSON-like content is embedded inline instead of fenced as code.

## 3) External link failures (non-placeholder)

Most frequent status classes in this run:
- `404 Not Found`: 18
- `429 Too Many Requests`: 6
- `403 Forbidden`: 1
- `401 Unauthorized`: 1
- `500`: 1
- unresolved without explicit status in output: 12

Examples include:
- outdated spec URLs (e.g., some DDI, OGC, CODATA pages)
- GitHub links rate-limited during check (`429`)
- private/auth-required links (Google Doc in `metadata/pidkernel.md`)
- intentionally fake demonstration URLs in ODRL workflow examples

## 4) Legacy internal link syntax

- File: `data_access/intro.md`
- Symptom:
  - legacy syntax warning for link target `mainstream` (needs `#mainstream` style anchor target)

## Files with Highest Diagnostic Volume

From the extracted issue set:

1. `data_access/odrlsharedservices.md` (22)
2. `metadata/schemaorgimplementation.md` (11)
3. `metadata/dcat.md` (11)
4. `metadata/signpostinglinkrel.md` (7)
5. `universals/univuom.md` (6)
6. `metadata/pidkernel.md` (5)

## Recommendations (for maintainer discussion)

## Priority 1 — Fix obvious content blockers now

1. Replace `[TBD]()` placeholders with either:
   - concrete links, or
   - plain text TODO markers that are not parsed as links.
2. Restore or replace missing image reference in `data_integration/ddidescriptiondatastructure.md`.
3. Update legacy anchor syntax in `data_access/intro.md` to modern `#anchor` style.

## Priority 2 — Reduce parser noise in pseudo-JSON examples

1. In files with heavy inline JSON/ODRL examples (especially `data_access/odrlsharedservices.md`), move pseudo-JSON into fenced code blocks.
2. Where bracketed tokens are prose-only, escape or rewrite to avoid MyST citation parsing.
3. In tables, prefer short code literals over inline structured blobs when possible.

## Priority 3 — Curate outbound links

1. Classify each failing external URL as one of:
   - replace with current canonical URL,
   - intentionally illustrative example URL,
   - access-restricted/internal reference.
2. For intentionally illustrative URLs, use reserved domains (`example.org`) and render as code/non-clickable text.
3. For rate-limited GitHub references (`429`), consider replacing with stable docs pages or pinning to raw/permalink resources.

## Priority 4 — Establish ongoing link/content hygiene

1. Add a recurring maintenance pass for external links.
2. Keep `jupyter-book build --site --html --check-links` as a required pre-PR check for content-heavy changes.
3. Consider splitting “known noisy links/examples” into an explicit policy so checks remain actionable.

## Suggested Resolution Path

For a pragmatic maintainer workflow:

1. **Quick cleanup PR** (small scope):
   - fix `[TBD]()` links,
   - fix missing image,
   - fix legacy anchor syntax.
2. **Parser-noise PR**:
   - refactor pseudo-JSON in `data_access/odrlsharedservices.md` and related metadata pages.
3. **External-link curation PR**:
   - replace dead links, normalize illustrative links, and document policy for non-resolvable example URLs.

This phased approach reduces noise quickly while avoiding one very large editorial change-set.