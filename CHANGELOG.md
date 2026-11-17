# Changelog

## 2.0.0 — first open-source release

Rewritten into one readable, commented file. Project files and autosaves from 1.x still open, with the same defaults.

### Bug fixes
- **Redo never worked from the toolbar.** Clicking *Undo* recorded a fresh history step afterwards, which wiped the redo stack. Fixed.
- **Panel edits weren't undoable.** Typing a label, X/Y or other numbers never reached history. All panel edits are undoable now.
- **Space / Delete could re-trigger the last clicked button.** Buttons now drop focus after a click.
- **Ribbon ends snapped to node edges anywhere on the canvas**, even far above or below the node. The magnet now requires the end to be next to the node.
- **Clicking the green "new ribbon" dot without dragging** left a zero-length ribbon behind. It's now discarded.
- **Opening a broken or foreign JSON** crashed silently and could corrupt state. Files are now validated, with a clear message.
- **Untrusted project files could inject SVG attributes** (e.g. through a crafted colour string). Colours and other fields are sanitised on load.
- `pointercancel` / lost pointer capture (touch, alt-tab mid-drag) left the editor stuck in a drag.
- Moving a node through the X/Y/Width fields detached its ribbons. They now stay attached.
- PNG export leaked object URLs; Copy SVG gave no feedback and failed silently.
- Firefox line-based wheel deltas zoomed far too slowly.

### New
- Free Bézier curve handles per ribbon (`Shift` keeps horizontal tangents), plus *Reset to smooth curve*.
- Gradient ribbons, ribbon labels, node corner radius, node opacity.
- Node labels: left/right/top/bottom/inside/none, multi-line, font size, colour, bold, draggable offset.
- Vertical stacking magnet: ribbon ends snap to node top/bottom and neighbouring ribbons.
- *Stack ribbons* (crossing-minimising order), *Fit height to ribbons*, *Stack all*.
- Multi-selection (Shift+click, Shift+drag box), group move, align / distribute / match size.
- Copy / cut / paste, select all, z-order, `N` / `R` shortcuts, `Esc` cancels a drag.
- Export: vector PDF via print, PNG 1–8×, transparent/white background, padding, project name as file name.
- Project font setting (e.g. Times for LaTeX papers).
- Drag-and-drop `.json` to open; wheel-pans mode for trackpads; zoom % and cursor coordinates.
