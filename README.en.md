# Academic Arsenal

> A Claude Code plugin for the academic writing workflow — generate diagrams, slides, and thesis documents from your papers.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Plugin-purple.svg)](https://claude.ai/claude-code)

## What It Does

```
📄 Your Papers + Notes
        │
        ▼
┌─────────────────────────────────────────┐
│         Academic Arsenal                 │
├──────────┬──────────┬───────────────────┤
│gen-diagram│gen-slides│    gen-thesis     │
│  .drawio │  .pptx   │  LaTeX / Word     │
└──────────┴──────────┴───────────────────┘
        │          │          │
        ▼          ▼          ▼
   Clean diagrams  Editable   Compilable
   for any platform PPT       thesis skeleton
```

Three skills, one plugin. Each works independently or together:

| Skill | Input | Output | Use Case |
|-------|-------|--------|----------|
| **gen-diagram** | Description / paper / code | `.drawio` XML | Architecture diagrams, flowcharts, taxonomies |
| **gen-slides** | Paper(s) + presentation type | `.pptx` | Seminar, proposal, midterm, defense presentations |
| **gen-thesis** | Paper(s) + template | LaTeX project / `.docx` | Degree thesis, research proposal, literature survey |

## Key Features

- **Academic Integrity**: Never fabricates data, results, or citations. Gaps are marked with `% TODO` placeholders.
- **Truly Editable Output**: `.pptx` with real text (not images), `.drawio` XML (not PNG), `.tex` source (not PDF).
- **Platform-Agnostic Diagrams**: `.drawio` files open in [diagrams.net](https://app.diagrams.net) on any OS — no desktop app required.
- **Template-Extensible**: Contribute your university's LaTeX template; everyone benefits.
- **Composable**: gen-slides and gen-thesis can call gen-diagram to redraw unclear figures.

## Installation

```bash
# In Claude Code, install the plugin:
claude plugin add academic-arsenal
```

Or manually clone and link:
```bash
git clone https://github.com/zhao1s747/academic-arsenal.git
# Add the path to your Claude Code plugin configuration
```

## Usage

### Generate a Diagram

```
/gen-diagram "architecture of a CNN-based image super-resolution network"
/gen-diagram --type taxonomy "classification of SR methods: CNN-based, Transformer-based, GAN-based"
/gen-diagram ./my-paper.pdf --type pipeline
```

### Generate Slides

```
/gen-slides ./paper.pdf --type defense --logo ./school-logo.png
/gen-slides ./paper1.tex ./paper2.tex --type seminar --lang zh
/gen-slides ./thesis-project/ --type midterm --colors "#1F4E79" "#2E75B6"
```

### Generate Thesis

```
/gen-thesis ./paper1.pdf ./paper2.pdf --template scut-master --type thesis
/gen-thesis ./paper.tex --type proposal --format word
/gen-thesis ./papers/ --type survey --lang en
```

## Presentation Types

| Type | Slides | Best For |
|------|--------|----------|
| `seminar` | 8-12 | Lab meetings, journal clubs |
| `proposal` | 12-15 | Research proposals |
| `midterm` | 15-20 | Progress reports |
| `defense` | 18-25 | Thesis defense |
| `group-project` | 8-15 | Course projects, team presentations |

## Requirements

- **Claude Code** (any version with plugin support)
- **Python 3.8+** with `python-pptx` (for slides) and `python-docx` (for Word output)
- **LaTeX distribution** (for thesis compilation): TeX Live / MacTeX / MiKTeX
- **draw.io desktop** (optional, for PNG/PDF export from diagrams)

The skills will guide you through installing any missing dependencies.

## Contributing

We welcome contributions! The highest-impact way to help:

- **Add your university's LaTeX template** → See [docs/adding-templates.md](docs/adding-templates.md)
- **Add slide themes** → New `.py` theme in `templates/slides/`
- **Report issues** → Open a GitHub issue with your use case

See [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md) for full guidelines.

## License

[MIT](LICENSE)

---

[中文说明](README.md)
