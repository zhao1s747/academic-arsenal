# Adding a New Thesis Template

Want gen-thesis to support your university's LaTeX template? Here's how.

## What You Need

1. Your university's LaTeX thesis template (`.cls` or `.sty` files)
2. Required fonts (if the template uses specific fonts)
3. A working example that compiles

## Steps

### 1. Create a Template Directory

```
templates/thesis/<your-school>/
├── README.md              # Brief description + compilation instructions
├── *.cls or *.sty         # Document class / style files
├── fonts/                 # Required fonts (optional, for self-contained builds)
└── sample/                # A minimal working example
    ├── main.tex
    └── chapter/
        └── sample.tex
```

### 2. Write the README

Include:
- University name (in both English and native language)
- Degree level (Bachelor's / Master's / PhD)
- Required compiler (XeLaTeX / pdfLaTeX / LuaLaTeX)
- Bibliography tool (biber / bibtex) and style
- Any special package requirements
- A link to the original template source (if public)

### 3. Verify Compilation

Your template must compile cleanly with the sample content:

```bash
cd templates/thesis/<your-school>/sample
latexmk -xelatex main.tex  # or appropriate command
```

### 4. Submit a PR

- Title: `feat: add <University Name> thesis template`
- Include a screenshot of the compiled cover page
- Confirm it compiles on at least one OS (Mac/Windows/Linux)

## Template Design Guidelines

- **Self-contained fonts preferred**: Bundle fonts so it works without system installation
- **Minimal dependencies**: Don't require obscure packages absent from standard TeX distributions
- **Clear structure**: Chapter directory layout should be obvious
- **UTF-8 encoding**: All .tex files must be UTF-8

## Existing Templates

| Directory | University | Degree | Compiler |
|-----------|-----------|--------|----------|
| `scut-master` | South China University of Technology | Master's | XeLaTeX |

Your template could be next!
