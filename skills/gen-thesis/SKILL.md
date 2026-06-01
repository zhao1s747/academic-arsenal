---
name: gen-thesis
version: 1.1.0
description: "Assemble a structured thesis or academic report from existing papers and documents. Outputs compilable LaTeX (primary) or Word .docx (secondary). NEVER fabricates data or citations. Use when: user asks to write/assemble a thesis, dissertation, research proposal, literature survey, or says '写大论文/生成论文/开题报告/调研报告/整合论文'."
---

# gen-thesis

Assemble a structured academic document (thesis, proposal, or survey report) from existing papers, proposals, and notes. Integrates content into a compilable LaTeX project or Word document.

<HARD-RULE>
## Academic Integrity — Non-Negotiable

1. **NEVER fabricate experimental data, results, or citations.** If data is not in the source material, leave a `% TODO: [description]` placeholder.
2. **NEVER invent references.** Only use citation keys that exist in the merged .bib file.
3. **NEVER guess numbers.** If a metric, parameter, or count is not explicitly stated in the source, mark it `% TODO`.
4. **Preserve original figures** unless user explicitly asks for redraw.
5. **English→target-language rewrite** must preserve technical accuracy. When uncertain about a translation, keep the English term in parentheses.

Violation of these rules produces academic fraud. There are no exceptions.
</HARD-RULE>

## Input

The user provides:
1. **Source papers**: one or more papers (PDF / LaTeX source / Markdown)
2. **Supporting docs** (optional): research proposal, notes, previous drafts
3. **Output type**:
   - `thesis` — degree thesis (default)
   - `proposal` — research proposal / 开题报告
   - `survey` — literature survey / 调研报告
4. **Template** (optional): a LaTeX template directory, or "word" for .docx output
5. **Language**: target language for the output (default: Chinese)

Optional arguments:
- `--template <path|name>`: LaTeX template (default: generic; e.g. `scut-master`)
- `--format latex|word`: output format (default: latex)
- `--lang zh|en`: output language (default: zh)
- `--output <dir>`: output directory (default: `./thesis-output/`)

## Process

### Phase 1: Analyze Source Material

For each source paper/document:
1. Read the full content (prefer LaTeX source over PDF for accuracy)
2. Extract and catalog:
   - Title, authors, abstract
   - Section headings (potential chapter mapping)
   - All figures (file paths and captions)
   - All tables (content and captions)
   - All equations (LaTeX source)
   - Experimental setup (datasets, metrics, hyperparameters, hardware)
   - Results (tables, comparisons, numbers)
   - References (BibTeX entries)
3. Note the language of each source

### Phase 2: Plan Document Structure

Based on output type:

**thesis (学位论文):**
```
Abstract (中/英)
Chapter 1: Introduction (绪论)
  1.1 Research background
  1.2 Research status (国内外研究现状)
  1.3 Main contributions
  1.4 Thesis organization
Chapter 2: Theoretical foundations (相关理论基础)
  (Shared background: problem definition, base architectures, key techniques)
Chapter 3..N: One chapter per paper/contribution
  X.1 Chapter introduction
  X.2 Method
  X.3 Experiments
  X.4 Chapter summary
Chapter N+1: Conclusion & future work
References
Acknowledgments
```

**proposal (开题报告):**
```
1. Research background & significance
2. Literature review (国内外研究现状)
3. Research objectives & content
4. Proposed approach / technical route
5. Expected outcomes
6. Timeline / milestones
7. References
```

**survey (调研报告):**
```
1. Introduction & scope
2. Taxonomy / classification of approaches
3. Detailed review by category
4. Comparison & analysis
5. Open problems & future directions
6. References
```

**🔴 CHECKPOINT — 章节结构确认：**
向用户展示规划的文档结构（章节标题 + 每章内容来源 + 哪些地方会留 TODO），确认后再开始生成。用户可能要求调整章节顺序、合并/拆分章节、或指定某些内容的处理方式。

### Phase 3: Map Content to Structure

For each chapter/section:
1. Identify which source paper(s) contribute content
2. Plan English→target-language rewriting
3. Mark MISSING content that needs `% TODO` placeholders:
   - Abstract (user customization needed)
   - Acknowledgments (personal content)
   - Conclusion future-work specifics
   - Any data not present in source papers
4. Plan figure placement; flag unclear figures for potential gen-diagram redraw
5. For multi-paper theses: shared content (problem definition, datasets, metrics) in Ch.2; paper-specific in subsequent chapters to avoid repetition

### Phase 4: Generate Output

#### LaTeX Path (Primary)

1. **Create project structure:**
   ```
   output-dir/
   ├── main.tex              # Root document with \include{} calls
   ├── chapter/
   │   ├── abstract.tex
   │   ├── chapter01.tex
   │   ├── ...
   │   ├── conclusion.tex
   │   └── ack.tex
   ├── biblibrary/
   │   └── refs.bib          # Merged, deduplicated bibliography
   ├── Fig/                   # All figures copied here
   └── fonts/                 # Self-contained fonts (if template requires)
   ```

2. **Main .tex file**: configure document class, packages, bibliography resource, and \include statements.

3. **Chapter files**: write with:
   - Proper LaTeX formatting (equations, figures, tables, citations)
   - `\cite{key}` only for keys present in refs.bib
   - `% TODO: <specific description>` for any gap
   - Figures referenced as `Fig/<filename>`

4. **Bibliography**: merge all .bib entries from source papers, deduplicate by citation key, verify every `\cite{key}` has a matching entry.

5. **Self-contained fonts** (if template supports): copy required font files into the project so it compiles on any machine without system font installation.

#### Word Path (Secondary / Best-Effort)

1. Write a python-docx script:
   - Structured .docx with heading levels matching chapters
   - Insert figures at appropriate positions
   - Format tables with borders
   - `[TODO: ...]` markers for gaps
2. Run the script to produce .docx
3. Warn: "Word output is best-effort. For publication quality, use the LaTeX path."

```bash
python3 -c "import docx" 2>/dev/null || pip install python-docx -i https://pypi.tuna.tsinghua.edu.cn/simple
python3 build_thesis.py
```

### Phase 5: Verify Compilation (LaTeX only)

```bash
# Check LaTeX availability
which latexmk || echo "WARNING: latexmk not found. Install TeX Live / MacTeX / MiKTeX."

# Compile
cd output-dir
latexmk -xelatex main.tex

# Verify
if [ $? -eq 0 ]; then
  echo "Compilation successful"
  grep -o "Output written on.*pages" main.log
else
  echo "Compilation failed — reviewing errors..."
  # Fix common issues: undefined citations, missing packages, font errors
fi
```

Fix any compilation errors before reporting success. Common fixes:
- Undefined citation: check .bib key spelling
- Missing package: add to preamble
- Font not found: verify fonts/ directory and cls Path configuration

### Phase 6: Report & Offer Diagram Redraw

Report to user:
- Output directory path and key files
- Page count (if compiled)
- List of all `% TODO` placeholders with their locations (file:line)
- Any figures flagged as unclear

If unclear figures exist:
- "Figure X appears low-quality. Want me to redraw it using gen-diagram?"
- If yes: invoke gen-diagram, export PNG, replace in project, recompile

## Template System

Templates stored in `templates/thesis/<name>/` provide:
- Document class (`.cls`) or style files (`.sty`)
- Package configuration
- Font requirements and bundled fonts
- Sample chapter structure
- Cover page format

**Built-in: `scut-master`** (SCUT master's thesis)
- XeLaTeX + biber + biblatex (gb7714-2015 style)
- Self-contained fonts (Times New Roman, SimSun, SimHei, KaiTi, FangSong)
- Chinese cover page with title/author/advisor fields

**Custom template usage:**
- User specifies `--template /path/to/template/`
- Skill reads template structure and adapts output accordingly
- If template has example .tex files: follow their conventions for includes, packages, etc.

## Important Notes

- This skill produces a SKELETON with real content filled in. It is NOT a finished document.
- Users MUST review and edit. The skill saves structural work, not intellectual work.
- Every `% TODO` must be specific: `% TODO: 填写摘要(200-300字)` not just `% TODO`
- Citation integrity: verify every \cite{key} has a .bib match before declaring success
- For multi-paper theses: avoid duplicating shared methodology across chapters
- Prefer LaTeX source input over PDF (more accurate equation/figure extraction)
