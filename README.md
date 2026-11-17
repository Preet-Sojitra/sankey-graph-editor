# Sankey Graph Editor

**A free-form, zero-install editor for Sankey diagrams. Drag every node, ribbon and curve by hand, then export vector SVG / PDF for your paper.**

![Sankey Graph Editor](docs/screenshot.png)

**▶ Try it:** open [`index.html`](index.html) in your browser, or use the live version at **https://preet-sojitra.github.io/sankey-graph-editor/** (once GitHub Pages is enabled).

---

## The story

It was 3 AM and a CVPR deadline was getting close. The paper needed a Sankey diagram showing how data flowed through our pipeline: which inputs went where, how much of each, and how it all came together at the end.

That should have been the easy part, but it wasn't.

Every Sankey tool I tried did the layout for me. You feed it numbers and it decides where the nodes go, how the ribbons bend and what order they stack in. That's great for a dashboard, but a paper figure has to look *exactly* right: a ribbon has to curve around a label, two flows need to line up with the figure next to it, and a node has to sit precisely where the caption says it is. The tools either didn't let me touch any of that, or fought me every time I tried.

So I stopped looking and wrote my own: one HTML file where everything is just a shape you can grab. Nodes are rectangles you drag. Ribbons are curves whose ends you pin wherever you want. It was rough, ugly and held together with tape, but it made the figure.

After the deadline I cleaned it up, fixed the bugs, added what I wished I'd had that night, and put it here. If you've ever wanted to just *grab the ribbon and move it*, this is for you.

— Preet

> The original 3 AM version is kept in [`archive/v1-3am.html`](archive/v1-3am.html), for history (and humility).

---

## Why use this

- **You're in control.** No automatic layout fighting you. Every position, width and curve is yours.
- **Free curves.** Each ribbon has two Bézier handles you can drag, or use a one-slider "curviness" for quick, clean flows.
- **Smart helpers when you want them.** Ribbon ends snap to node edges and stack neatly against each other. *Stack ribbons* orders flows to minimise crossings. There are also align, distribute and match-size tools.
- **Paper-ready output.** Tight-cropped SVG, vector PDF, or PNG at 1–8×, in your paper's font (e.g. Times).
- **Zero install.** A single `index.html` with no dependencies, no build and no server. It works offline and runs in any modern browser.
- **Your data stays local.** Autosaved in your browser, plus plain-JSON project files you can keep next to your LaTeX source.

## Quick start

1. Open `index.html` (or the live link above).
2. A small demo loads the first time. Press **New** for a blank canvas, or **Open…** [`examples/demo.json`](examples/demo.json).
3. Click **+ Node**, select it, and drag the **green dot** to pull out a ribbon. Drop the end on another node's left edge and it snaps into place.
4. Shape it: drag the **orange dots** (ends), **blue squares** (width) and **purple diamonds** (curve).
5. **Export → SVG / PDF / PNG.** **Save** keeps a `.json` you can reopen any time.

## Features

| | |
|---|---|
| **Nodes** | Move, resize, corner radius, fill, opacity, border. Multi-line labels on the right / left / top / bottom / inside, each with its own font size, colour, bold, and a draggable offset. |
| **Ribbons** | Independent start/end widths (or locked equal), free Bézier handles, smooth-curve mode, solid or gradient fill, opacity, border, optional centred label. |
| **Snapping** | Ribbon ends snap to node edges, then stack against the node's top/bottom and neighbouring ribbons. Optional grid snap. Hold `Alt` to turn snapping off. |
| **Layout helpers** | Stack ribbons (crossing-minimising order), fit node height to ribbons, align, distribute, same width/height. |
| **Editing** | Multi-select (Shift+click, Shift+drag box), group move, copy/cut/paste, duplicate, z-order, arrow-key nudge, undo/redo (300 steps). |
| **View** | Zoom at cursor (wheel / pinch), pan (drag / Space / middle mouse), fit to content, trackpad-friendly wheel-pans mode. |
| **Export** | SVG with no editor clutter, vector PDF via the print dialog, PNG 1–8×, copy SVG to clipboard. Transparent or white background, adjustable padding. |
| **Files** | JSON project save/open, drag-and-drop `.json` onto the canvas. Older project files still open. |

## Mouse & keyboard

| Action | How |
|---|---|
| Move | Drag a node or ribbon (ribbons attached to a node move with it) |
| New ribbon from a node | Drag the green dot on the selected node |
| Move a ribbon end | Drag an orange dot (`Alt` = no snapping) |
| Ribbon width | Drag a blue square |
| Bend a ribbon | Drag a purple diamond (`Shift` = keep horizontal tangent) |
| Move a label | Drag the label text |
| Edit a label | Double-click the node |
| Box select | `Shift` + drag on empty canvas |
| Pan | Drag empty canvas, hold `Space`, or middle mouse |
| Zoom | Wheel / pinch, `+` / `−`, `0` = fit |
| Nudge | Arrow keys (`Shift` = 10 units) |
| Undo / redo | `Ctrl/⌘+Z`, `Ctrl/⌘+Shift+Z` or `Ctrl+Y` |
| Duplicate / copy / cut / paste | `Ctrl/⌘ + D / C / X / V` |
| Select all / save | `Ctrl/⌘ + A / S` |
| New node / ribbon | `N` / `R` |
| Delete / deselect | `Del` or `Backspace` / `Esc` (also cancels a drag) |
| Help | `?` |

## Using it in a paper (LaTeX)

- **PDF:** click **PDF**, choose *Save as PDF* in the print dialog and set margins to *None*. The page is sized to the figure and stays fully vector:
  ```latex
  \includegraphics[width=\linewidth]{figures/sankey.pdf}
  ```
- **SVG → PDF from the command line:** `inkscape sankey.svg --export-type=pdf` or `rsvg-convert -f pdf -o sankey.pdf sankey.svg`.
- **Match the paper font:** in the Project panel, set the font to `"Times New Roman", Times, serif` (or Latin Modern / CMU Serif if installed).
- **Keep the `.json` with your figures** so a reviewer's "can you swap these two flows?" takes two minutes, not a night.

## Project file format

Projects are plain JSON, so it's easy to generate one from a Python script and fine-tune it by hand:

```json
{
  "version": 2,
  "settings": {"name": "sankey", "font": "Arial, Helvetica, sans-serif", "bg": "none", "pad": 10},
  "nodes":   [{"id": 1, "label": "Input", "x": 60, "y": 50, "w": 24, "h": 300, "color": "#a8dcc4"}],
  "ribbons": [{"id": 2, "sx": 84, "sy": 50, "tx": 640, "ty": 400, "w1": 110, "w2": 110, "color": "#dcdce3"}]
}
```

- A ribbon's top edge runs from `(sx, sy)` to `(tx, ty)`. `w1` / `w2` are its thickness at each end.
- `curv` (0–1) controls the smooth curve. With `"cmode": "custom"`, `c1x, c1y` / `c2x, c2y` are Bézier control offsets relative to the start / end.
- Any missing field falls back to a default, so generated files only need the basics.

## Repository layout

```
index.html          the whole app (one commented file)
examples/demo.json  sample project
docs/screenshot.png README image
archive/v1-3am.html the original 3 AM version
CHANGELOG.md        what changed and what was fixed
```

## Contributing

Issues and pull requests are very welcome, especially from anyone who has fought a Sankey figure before a deadline.

- The app lives in `index.html`, split into commented sections: model → geometry → rendering → interaction → panel → export.
- Please keep it **dependency-free and single-file**, so "download and open" keeps working.
- If you change the project format, make sure older `.json` files still open.

**Roadmap ideas:** vertical (top-to-bottom) flows · value-driven widths (type a number → width) · auto-colour ribbons from their source node · SVG import · themes / colour palettes.

## Credits

Created by **[Preet Sojitra](https://github.com/Preet-Sojitra)**.
Refactor, bug fixes and new features with **Claude** (Anthropic).

If this saved you a late night, a ⭐ helps others find it.

## License

[MIT](LICENSE) © 2026 Preet Sojitra and contributors
