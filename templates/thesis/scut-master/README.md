# SCUT Master's Thesis Template

This directory is a placeholder for the SCUT (South China University of Technology) master's thesis LaTeX template.

## Setup

To use this template with gen-thesis:

1. Clone the SCUT thesis template: https://github.com/mengchaoheng/SCUT_thesis
2. Copy the `.cls`, font files, and base `.tex` structure into this directory
3. Run gen-thesis with `--template scut-master`

## Template Structure (Expected)

```
scut-master/
├── scutthesis.cls          # Document class
├── fonts/                  # Bundled fonts (Times New Roman, SimSun, etc.)
├── chapter/                # Sample chapter structure
└── biblibrary/             # Bibliography style config
```

## Notes

- Requires XeLaTeX compiler (not pdfLaTeX)
- Uses biber with biblatex gb7714-2015 citation style
- Fonts must be self-contained for cross-platform compilation

## Contributing Your School's Template

See [docs/adding-templates.md](../../../docs/adding-templates.md) for instructions.
