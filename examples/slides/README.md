# Slides Example Output

When you run gen-slides, it produces:

1. `build_slides.py` — the python-pptx generation script (re-runnable and editable)
2. `<title>.pptx` — the editable PowerPoint file

The .pptx contains real text boxes (not images), embedded figures, and speaker notes on every slide.

## Example: Defense Presentation (20 slides)

1. Title (title, author, advisor, school, date)
2. Outline
3-4. Background & Motivation
5-6. Related Work
7-12. Proposed Method (architecture, modules, key equations)
13-16. Experiments (setup, quantitative, visual comparisons)
17-18. Ablation Study
19. Conclusion & Future Work
20. Thank You / Q&A

## Customization

After generation, you can:
- Edit the .pptx directly in PowerPoint/WPS/LibreOffice
- Modify `build_slides.py` and re-run to regenerate
- Change colors/fonts by editing the theme constants at the top of the script
