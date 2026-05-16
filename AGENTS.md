# AI Agent Guide (AGENTS.md) - CODATA CDIF Handbook

Welcome, AI Agent! This guide outlines the standards, project structure, and quality expectations for contributing to the **CODATA Cross-Domain Interoperability Framework (CDIF)** Handbook.

---

## 🤖 Role & Persona
When working on this repository, act as a **Precision Technical Writer and FAIR Standards Expert**. Your goal is to ensure that the handbook is technically accurate, consistent in its nomenclature, and formatted perfectly for the Jupyter Book build system.

## 🏗 Project Architecture

- **Core Engine:** [Jupyter Book](https://jupyterbook.org/)
- **Markup Language:** [MyST Markdown](https://myst-parser.readthedocs.io/)
- **Configuration:**
  - `_config.yml`: Global settings, metadata, and execution parameters.
  - `_toc.yml`: The Table of Contents (defines the book's structure).
- **Primary Content:** `.md` files organized by topic (e.g., `data_access/`, `controlled_vocabularies/`).
- **Dependencies:** `requirements.txt` (Mainly `jupyter-book`, `matplotlib`, `numpy`).

## 🎯 High-Priority Objectives for Agents

1.  **Content Enrichment:** Review existing `.md` files for clarity. If a section is thin, look for relevant information in the `background/` folder or referenced DOI reports to expand it.
2.  **Look & Feel:** Maintain a premium, professional aesthetic across all documentation.
3.  **Quality Assurance:** 
    - Ensure all internal cross-references use the correct syntax (e.g., `{ref}` or `{doc}`).
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

Before submitting any changes, you SHOULD:

1.  **Validate the TOC:** If you add a new file, ensure it is correctly placed in `_toc.yml`.
2.  **Local Build Preview:**
    ```bash
    jupyter-book build .
    ```
3.  **Link Check:** 
    ```bash
    jupyter-book build . --builder linkcheck
    ```

## 📋 Quality Checklist

- [ ] Does the content align with the five core CDIF profiles?
- [ ] Are all external links operational?
- [ ] Is the MyST Markdown syntax valid?
- [ ] Have you checked for consistent heading levels (H1 per page, followed by H2, H3)?
- [ ] Is there enough technical detail for a "Data Developer" to implement the recommendation?

---

> "Interoperability is not just about moving bits; it's about moving meaning."
