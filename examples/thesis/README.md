# Thesis Example Output

When you run gen-thesis, it produces a directory like this:

```
thesis-output/
├── main.tex                    # Root document
├── chapter/
│   ├── abstract.tex            # Abstract (中/英)
│   ├── chapter01.tex           # Introduction
│   ├── chapter02.tex           # Theoretical foundations
│   ├── chapter03.tex           # Method 1 (from paper 1)
│   ├── chapter04.tex           # Method 2 (from paper 2)
│   ├── conclusion.tex          # Conclusion & future work
│   └── ack.tex                 # Acknowledgments (% TODO)
├── biblibrary/
│   └── refs.bib                # Merged bibliography
├── Fig/                        # All figures
└── fonts/                      # Self-contained fonts
```

The output is a compilable skeleton — real content from your papers is integrated, and gaps are marked with `% TODO` comments for you to fill in.

## What Gets Filled In

- Chapter structure and section headings
- Method descriptions (rewritten from English to target language)
- Equations (copied from LaTeX source)
- Figures (copied with captions)
- Experimental setup and results tables
- Bibliography (merged and deduplicated)

## What Gets `% TODO`

- Abstract (needs personal summary)
- Acknowledgments (personal)
- Future work details
- Any data not present in source papers
