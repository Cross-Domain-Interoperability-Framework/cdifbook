# AI Agent Guide (AGENTS.md) - CODATA CDIF Handbook

Welcome, AI Agent! This guide outlines the standards, project structure, and quality expectations for contributing to the **CODATA Cross-Domain Interoperability Framework (CDIF)** Handbook.

---

## 🤖 Role & Persona
When working on this repository, act as a **Precision Technical Writer and FAIR Standards Expert**. Your goal is to ensure that the handbook is technically accurate, consistent in its nomenclature, and formatted perfectly for the Jupyter Book build system.

## 🏗 Project Architecture

- **Core Engine:** [Jupyter Book 2](https://jupyterbook.org/) — v2.1.2, the MyST `mystmd`-based engine (not the legacy Sphinx v1).
- **Markup Language:** [MyST Markdown](https://mystmd.org/)
- **Configuration:**
  - `myst.yml`: Single config file holding project metadata, the table of contents (`toc:`), and site/theme options. (There is no `_config.yml`/`_toc.yml` — those are legacy v1.)
- **Primary Content:** `.md` files organized by topic (e.g., `data_access/`, `controlled_vocabularies/`, `data_description/`, `universals/`).
- **Shared Datatypes:** `metadata/datatypes.md` defines common types (`object-reference`, `LanguageTaggedValue`, `DefinedTerm`, `xsdDataType`, `PropertyValue` variants) referenced from multiple profiles. Link to those targets — do not redefine them in individual profile docs.
- **Dependencies:** `requirements.txt` (`jupyter-book==2.1.2`, `matplotlib`, `numpy`).

## 🎯 High-Priority Objectives for Agents

1.  **Content Enrichment:** Review existing `.md` files for clarity. If a section is thin, look for relevant information in the `background/` folder or referenced DOI reports to expand it.
2.  **Look & Feel:** Maintain a premium, professional aesthetic across all documentation.
3.  **Quality Assurance:** 
    - Ensure internal cross-references use MyST v2 syntax: link with `[text](#target)` and define the target with a `(target)=` label on the line *directly above* the heading. ⚠️ The `{#id}` heading-attribute syntax does **not** register as a cross-reference target in this build — links to it resolve as "No target for internal reference". Always use the `(label)=` form. Targets are project-global; each label may be defined only once across the whole book.
    - **Escape bare `@` in prose:** mystmd parses `@foo` in body text as a citation reference. Property headings like `### @type` will emit "Could not link citation" warnings — write `### \@type` instead. (Code blocks and inline code don't need escaping.)
    - **One H1 per page.** If a doc has multiple `# Heading` lines, mystmd shifts ALL body heading levels +1 (source `###` renders `<h4>`), which breaks the underline CSS rule that targets `article h3`. Use a single `# Title` and demote the rest to `##`.
    - Verify that any added metadata or code snippets (JSON-LD, CDIF profiles) are valid.
    - Check that images have descriptive alt-text.

## ✍️ Writing Styles & Standards

- **Tone:** Academic yet accessible; authoritative and practical.
- **Terminology:** Stick strictly to [FAIR Principles](https://www.go-fair.org/fair-principles/) and CDIF-specific terminology (Profiles, Recommendations, Implementation Patterns).
- **Formatting:**
  - Use **Bolding** for emphasis on key terms.
  - Use `backticks` for technical terms, file names, and metadata fields.
  - Use callout blocks (Notes, Tips, Warnings) to highlight important information:
    ```markdown
    :::{note}
    This is a recommendation for machine-actionable metadata.
    :::
    ```

## 🛠 Tools & Commands for Agents

The build environment is the **`cdifbook` conda env** (Jupyter Book 2.1.2). Conda activation does not persist between separate shell invocations, so prefix every command with `conda run -n cdifbook`.

Before submitting any changes, you SHOULD:

1.  **Validate the TOC:** If you add a new file, ensure it is correctly placed under `toc:` in `myst.yml`. Mind YAML indentation — each `- title:` group must have a `children:` key before its `- file:` entries.
2.  **Local Build Preview:**
    ```bash
    conda run -n cdifbook jupyter-book build --html
    ```
    Output is written to `_build/html/` (open `_build/html/index.html`).
3.  **Live Preview (auto-rebuild while editing):**
    ```bash
    conda run -n cdifbook jupyter-book start
    ```
    Serves at http://localhost:3000.

> ⚠️ Do not run `jupyter-book clean` or delete `_build/` — it holds the only local copy of the ~130 MB site theme cache (there is no global cache), which is required for offline builds.

## 🚀 Branch Preview Deploys

The repo has a GitHub Actions workflow at `.github/workflows/preview-cdifBookUpdates2026-05.yml` that auto-builds the `cdifBookUpdates2026-05` branch on every push and publishes it to a GitHub Pages subpath without touching the main-branch book:

- **Preview URL:** https://cross-domain-interoperability-framework.github.io/cdifbook/preview-2026-05/
- **Main book (from `main`):** https://cross-domain-interoperability-framework.github.io/cdifbook/

Implementation notes for agents who need to modify this workflow:
- Build runs with `BASE_URL=/cdifbook/preview-2026-05` so generated absolute paths resolve under the subpath.
- Deploy uses `peaceiris/actions-gh-pages@v4` with `destination_dir: preview-2026-05` and `keep_files: true` — this is what preserves the root site. Never remove `keep_files: true`.
- To add a similar preview for another branch, copy the file, change the workflow name, the `branches:` trigger, the `BASE_URL`, the `destination_dir`, and the concurrency group.

## 📋 Quality Checklist

- [ ] Does the content align with the five core CDIF profiles?
- [ ] Are all external links operational?
- [ ] Is the MyST Markdown syntax valid?
- [ ] Have you checked for consistent heading levels (H1 per page, followed by H2, H3)?
- [ ] Is there enough technical detail for a "Data Developer" to implement the recommendation?

---

> "Interoperability is not just about moving bits; it's about moving meaning."
