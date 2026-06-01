# Contributing to Academic Arsenal

Thank you for considering a contribution! Here's how to help.

## Ways to Contribute

### 1. Add Your School's LaTeX Template

The most impactful contribution! See [adding-templates.md](./adding-templates.md) for detailed instructions.

### 2. Improve Skill Prompts

If a skill produces suboptimal output for your use case:
1. Open an issue describing the input, expected output, and actual output
2. If you have a fix, submit a PR modifying the relevant `skills/<name>/SKILL.md`

### 3. Add Slide Themes

Create a new Python theme file in `templates/slides/`:
- Follow the structure of `academic_blue.py`
- Use descriptive color variable names
- Include a docstring explaining when to use the theme

### 4. Report Bugs

Open a GitHub issue with:
- Which skill you used
- What input you provided (or a minimal reproducible example)
- What went wrong
- Your OS and Claude Code version

## Development Setup

1. Clone the repo
2. Install the plugin in Claude Code (add the path to plugin configuration)
3. Test by invoking skills: `/gen-diagram`, `/gen-slides`, `/gen-thesis`

## Code Style

- Skills are Markdown files — keep formatting clean and consistent
- Python templates: self-contained, no deps beyond python-pptx / python-docx
- Use meaningful variable names in all code examples

## Pull Request Process

1. Fork the repo and create a feature branch
2. Make your changes
3. Test the affected skill(s) in Claude Code
4. Submit a PR with a clear description of what changed and why
