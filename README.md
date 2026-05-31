# CODATA Cross-Domain Interoperability Framework (CDIF) Handbook

[![Jupyter Book Badge](https://jupyterbook.org/badge.svg)](https://cross-domain-interoperability-framework.github.io/cdifbook)
[![License: CC0-1.0](https://img.shields.io/badge/License-CC0_1.0-lightgrey.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

The **CODATA Cross-Domain Interoperability Framework (CDIF)** Handbook is the primary source of documentation for CDIF. It provides implementation recommendations based on profiles of common, domain-neutral metadata standards aligned to support the core functions required by FAIR.

🌐 **Live Book:** [https://cross-domain-interoperability-framework.github.io/cdifbook](https://cross-domain-interoperability-framework.github.io/cdifbook)

---

## 🎯 Objectives

CDIF is designed to support multi-disciplinary research by establishing a "lingua franca" for metadata exchange. Our primary goals include:

- **Support FAIR implementation:** Providing practical, machine-actionable recommendations for FAIR data exchange.
- **Cross-domain interoperability:** Bridging the gap between domain-specific standards using domain-neutral profiles.
- **Scalability:** Reducing the "many-to-many" mapping problem into a manageable "many-to-one" dynamic.
- **Practicality:** Leveraging existing standards (metadata, web standards) that are already in common use.

## 📖 Framework Structure

The handbook is organized around these core profiles:

1.  **Core:** Shared metadata content model and schema.org implementation underpinning the other profiles.
2.  **Discovery:** Patterns for metadata content, serialization, and publication.
3.  **Controlled Vocabularies:** Practices for publishing semantic artifacts.
4.  **Data Description (for Integration):** Structural and semantic documentation to make data "integration-ready."
5.  **Data Access:** Documentation of access conditions and permitted use.
6.  **Universals:** Description of universal elements like time, geography, and units of measurement.

---

## 🛠 Local Development

This book is built using [Jupyter Book 2](https://jupyterbook.org/) (the MyST `mystmd`-based engine, pinned to `2.1.2` in `requirements.txt`). You can build it locally to preview changes.

### Prerequisites

- Python 3.8+
- Either [pip](https://pip.pypa.io/en/stable/installation/) or [uv](https://docs.astral.sh/uv/)

### Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Cross-Domain-Interoperability-Framework/cdifbook.git
    cd cdifbook
    ```

2.  **Install dependencies (Option A: pip):**
    ```bash
    python -m venv .venv
    source .venv/bin/activate
    pip install -r requirements.txt
    ```

3.  **Install dependencies (Option B: uv):**
    ```bash
    uv venv
    source .venv/bin/activate
    uv pip install -r requirements.txt
    ```

4.  **Install dependencies (Option C: conda):**
    ```bash
    conda create -n cdifbook python=3.12
    conda activate cdifbook
    pip install -r requirements.txt
    ```

### Building the Book

To build the HTML version of the book:

```bash
jupyter-book build --html
```

The output is written to `_build/html/`. Open `_build/html/index.html` in your browser to view the book.

### Development Workflow

Use these commands while iterating on content:

- **Live preview (auto-rebuilds on save, recommended):**
    ```bash
    jupyter-book start
    ```
    Serves the book at [http://localhost:3000](http://localhost:3000).
- **Build after edits:**
    ```bash
    jupyter-book build --html
    ```
- **Check links:**
    ```bash
    jupyter-book build --html --check-links
    ```

> ⚠️ **Do not run `jupyter-book clean` or delete `_build/`.** The ~130 MB site theme is cached only inside `_build/` (no global cache under `~/.myst` or `%LOCALAPPDATA%`). Wiping it forces a fresh download from the network and will break offline builds entirely.

> **Working offline?** Run a build once while online to populate the theme cache, then leave `_build/` alone. Link checking also requires a connection.

### Branch Preview Deploys

Pushes to the `cdifBookUpdates2026-05` branch are auto-built by a GitHub Actions workflow (`.github/workflows/preview-cdifBookUpdates2026-05.yml`) and published to a subpath of the GitHub Pages site without disturbing the main-branch book at root:

- **Preview URL:** [https://cross-domain-interoperability-framework.github.io/cdifbook/preview-2026-05/](https://cross-domain-interoperability-framework.github.io/cdifbook/preview-2026-05/)
- **Main book (from `main`):** [https://cross-domain-interoperability-framework.github.io/cdifbook/](https://cross-domain-interoperability-framework.github.io/cdifbook/)

The workflow builds with `BASE_URL=/cdifbook/preview-2026-05` so internal links resolve under the subpath, and deploys to `gh-pages/preview-2026-05/` with `keep_files: true` (the root site is preserved).

---

## 🤝 How to Contribute

We welcome contributions from the community!

### Joining the Working Group
The CDIF Working Group meets virtually every two weeks.
- **Join us:** [Application Form](https://docs.google.com/forms/d/e/1FAIpQLSfC6BoxSnAZOMWZ65pNTiljnsBXjfv80NgEeALPcY5DPaESHA/viewform)
- **Community List:** [Register here](https://bit.ly/cdif-community-list)

### Specific Tasks Needing Help
- **JSON-LD Examples:** Creating detailed metadata for data integration and access.
- **Validation Rules:** Developing SHACL rules to validate CDIF metadata.
- **Use Cases:** Documenting data descriptions for AI/training data (e.g., using Croissant).
- **Format Support:** Metadata examples for gridded data or time series (NetCDF/HDF).

### Content Contributions
1. Fork the repository.
2. Create a new branch (e.g., `feature/new-section`).
3. Make your changes in the relevant `.md` files.
4. Verify the build locally.
5. Submit a Pull Request.

---

## 📜 License

This work is dedicated to the public domain under the **CC0 1.0 Universal (CC0 1.0)** license. See the [LICENSE](LICENSE) file for the full legal text.

## 🙏 Acknowledgments

CDIF emerged from the WorldFAIR project and is maintained by the **World Fair CDIF Working Group**. We acknowledge the contribution of the 30+ experts from various FAIR initiatives and standards bodies who have helped shape this framework.
