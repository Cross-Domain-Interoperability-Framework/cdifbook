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

The handbook is organized around five core profiles:

1.  **Discovery:** Patterns for metadata content, serialization, and publication.
2.  **Data Access:** Documentation of access conditions and permitted use.
3.  **Controlled Vocabularies:** Practices for publishing semantic artifacts.
4.  **Data Integration:** Structural and semantic documentation to make data "integration-ready."
5.  **Universals:** Description of universal elements like time, geography, and units of measurement.

---

## 🛠 Local Development

This book is built using [Jupyter Book](https://jupyterbook.org/). You can build it locally to preview changes.

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

### Building the Book

To build the HTML version of the book:

```bash
jupyter-book build --site --html
```

The output will be generated in `_build/html/` (with site assets in `_build/site/`). You can open `_build/html/index.html` in your browser to view the book.

### Development Workflow

Use these commands while iterating on content:

- **Build after edits:**
    ```bash
    jupyter-book build --site --html
    ```
- **Force a full rebuild (when project/config changes):**
    ```bash
    jupyter-book clean --all --yes
    jupyter-book build --site --html
    ```
- **Check links:**
    ```bash
    jupyter-book build --site --html --check-links
    ```

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
