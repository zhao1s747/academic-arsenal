---
name: gen-diagram
version: 1.0.0
description: "Generate editable draw.io diagrams from descriptions, papers, or code. Outputs platform-agnostic .drawio XML that opens in diagrams.net (web/desktop) on any OS."
---

# gen-diagram

Generate publication-quality draw.io diagrams from natural language descriptions, academic papers, or code analysis.

## Input

The user provides ONE of:
1. A natural language description of what to diagram
2. A paper or document path to analyze and visualize
3. A code directory to map as an architecture diagram

Optional arguments (parsed from user message):
- `--type <type>`: architecture | flowchart | pipeline | taxonomy | comparison | sequence
- `--output <path>`: where to save (default: current directory)
- `--export png|pdf`: also export rendered image (requires draw.io CLI)
- `--style academic|minimal|colorful`: visual theme (default: academic)

## Process

### Phase 1: Analyze Input

1. If input is a **description**: extract entities, relationships, and flow direction
2. If input is a **document/paper**: read it, identify key concepts, methods, data flow, or system components worth diagramming. Ask the user which aspect to visualize if multiple options exist.
3. If input is a **code directory**: scan structure (ignore node_modules, .git, __pycache__, dist, build), identify modules, entry points, data flow, and key abstractions.

### Phase 2: Determine Diagram Type

If `--type` not specified, auto-detect:

| Input Pattern | Diagram Type |
|---|---|
| Steps, decisions, "if/then" | flowchart |
| Components, services, layers | architecture |
| Input → process → output | pipeline |
| Categories, hierarchy, "types of" | taxonomy |
| A vs B, pros/cons | comparison |
| Messages between actors | sequence |

### Phase 3: Plan Layout

Choose layout strategy based on diagram type:

| Type | Layout | Direction |
|---|---|---|
| flowchart | Snake/S-shape (max 4 per row) | Top-to-bottom with wrapping |
| architecture | Layered blocks | Top-to-bottom or left-to-right |
| pipeline | Linear with branches | Left-to-right |
| taxonomy | Tree | Top-to-bottom |
| comparison | Grid/table | N/A |
| sequence | Lifelines | Top-to-bottom |

**Layout constraints:**
- Aspect ratio between 1:1 and 2:1 (never a single column or single row)
- Node spacing: 80px horizontal gap, 80px vertical gap
- Standard node size: 160x60 (steps), 140x80 (decisions), 120x50 (start/end)
- Max nodes: 8-20 for readability; if more needed, split into sub-diagrams

### Phase 4: Generate draw.io XML

Use this XML skeleton:

```xml
<mxfile host="app.diagrams.net" modified="YYYY-MM-DDTHH:mm:ss.000Z" agent="academic-arsenal/gen-diagram" version="21.0.0">
  <diagram name="Diagram" id="diagram-1">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="1169" pageHeight="827" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- nodes and edges here -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

**Color scheme (academic style — default):**

| Element | fillColor | strokeColor |
|---|---|---|
| Start/End | #d5e8d4 | #82b366 |
| Process step | #dae8fc | #6c8ebf |
| Decision | #fff2cc | #d6b656 |
| Data/IO | #f5f5f5 | #999999 |
| Emphasis/Key | #e1d5e7 | #9673a6 |

**Node style templates:**
- Rectangle: `rounded=1;whiteSpace=wrap;html=1;fillColor=X;strokeColor=Y;`
- Diamond: `rhombus;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;`
- Ellipse: `ellipse;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;`
- Cylinder: `shape=cylinder3;whiteSpace=wrap;html=1;fillColor=#f8cecc;strokeColor=#b85450;`

**Edge style:**
- Standard: `edgeStyle=orthogonalEdgeStyle;rounded=1;orthogonalLoop=1;`
- With label: add `value="label text"` attribute
- Dashed (annotation): add `dashed=1;`

**LaTeX in labels:** Use `\(formula\)` syntax in node values when math notation is needed. Set `math="1"` on the mxGraphModel.

**Snake/S-shape layout algorithm (for flowcharts):**
1. Compute columns per row: `C = ceil(sqrt(N))`, max 4
2. Odd rows (1, 3, 5...): left-to-right, x increases
3. Even rows (2, 4, 6...): right-to-left, x decreases
4. Row transitions: vertical edge from last node of row to first node of next row
5. Column spacing: 240px (node 160px + gap 80px)
6. Row spacing: 140px (node 60px + gap 80px)
7. Canvas margin: 60px top and left

### Phase 5: Write File

1. Determine filename:
   - If user specified `--output`: use that path
   - Otherwise: `<descriptive-name>.drawio` in current directory
2. Write the XML file using the Write tool
3. Report: diagram type, node count, file path, and how to open it

### Phase 6: Optional Export

If user specified `--export` OR if draw.io CLI is detected:

```bash
# macOS
if [ -f "/Applications/draw.io.app/Contents/MacOS/draw.io" ]; then
  /Applications/draw.io.app/Contents/MacOS/draw.io --export --format png --output output.png input.drawio
# Linux / Windows (if in PATH)
elif command -v drawio &> /dev/null; then
  drawio --export --format png --output output.png input.drawio
fi
```

If CLI not available, inform user: "To export as PNG/PDF, open the .drawio file in diagrams.net (free, web-based) or install the draw.io desktop app."

## Style Variations

### academic (default)
Clean, muted colors. Suitable for papers and theses. Uses the color scheme above.

### minimal
All nodes white with thin gray borders. Emphasis through bold text only.
- fillColor: #ffffff, strokeColor: #333333 for all nodes

### colorful
Vibrant, distinct colors per category. Good for presentations.
- Uses a broader palette: blues, greens, purples, oranges — each category gets its own hue.

## Examples

**Invocation:**
- "Draw an architecture diagram of a CNN-based super-resolution network"
- "Visualize the pipeline from this paper: ./paper.pdf"
- "Create a taxonomy of image super-resolution methods"
- `/gen-diagram --type flowchart "training pipeline: load data, augment, forward pass, compute loss, backprop, update weights"`

## Important Notes

- All IDs must be unique. Use semantic prefixes: `node_`, `edge_`, `group_`
- Keep labels concise (max 4-5 words per node). Add detail in tooltips if needed.
- Before writing XML: mentally verify the layout won't produce a single-column or single-row diagram
- The .drawio file is the PRIMARY output. It can be opened and edited on ANY platform.
- Do NOT require draw.io desktop to be installed. The XML format is the deliverable.
- Fill in the current date in the XML `modified` attribute (format: `YYYY-MM-DDTHH:mm:ss.000Z`)
