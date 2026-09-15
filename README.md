# Mind Map

A single-file, no-install mind mapping app. Open `mindmap.html` in your browser and start mapping — no build step, no server, no accounts.

## Features

- **Add nodes** — click **+ Node** to drop a new node into the center of your current view, ready to type into immediately. New mind maps start empty — there's no default starter node.
- **Search** — the search box in the toolbar filters nodes live as you type, matching both node text and notes, and lists matches in a dedicated panel on the left edge of the screen. Click a result (or arrow-key to it and hit Enter) to pan/zoom the view straight to that node — it gets a highlighted outline plus a brief pulse animation so it's obvious which one you jumped to, and the list stays open so you can click through several results in a row.
- **Quick branching** — hover any node to reveal small **+** buttons on its top, right, bottom, and left edges. Click one to instantly create a new node already connected to it, positioned in that direction.
- **Edit node text** — click into any node and type. Text is bold, centered, and rendered in a handwriting-style font (Caveat) for a scribbled-note feel.
- **Node color** — select a node (click its body, not its text) and use the color swatch in the toolbar to set its background to any color you like. The picker is disabled unless a node is selected.
- **Node shape** — a node's side panel includes a **Shape** dropdown: Default, Rectangle, Square, Circle, Diamond, Cloud, or Text only (no background/border, just floating text). Custom colors apply correctly to every shape.
- **Checkbox / todo mode** — select a node and click **☑ Checkbox** in the toolbar to add a checkbox to it, turning that node (and, since it can be linked to others, whole branches of the map) into a todo item. Click the checkbox to mark it done — the text dims and gets a strikethrough. Click the toolbar button again to remove the checkbox.
- **Supplemental notes** — clicking a node's body also opens a side panel with a free-form notes text box for that node. A small 📝 badge appears on any node that has a note.
- **Drag to arrange** — click and drag any node to reposition it; connected lines follow automatically, stopping neatly at the node's edge instead of running under it.
- **Move / Select toggle** — the 🖐 Move / ⬚ Select button in the toolbar controls what dragging empty canvas does. **Move** (the default) pans the view. **Select** draws a rubber-band box instead, highlighting any node it touches — drag any highlighted node afterward and the whole group moves together as one. Clicking a highlighted node without dragging collapses the selection to just that one; clicking empty canvas clears it. Scroll/pinch panning and zooming work the same regardless of which mode is active.
- **Connect existing nodes** — toggle **Connect Nodes**, then click one node and another to link them.
- **Label connections** — click any connection line to type a label on it. Leave it blank and it renders as a plain line, with no icons cluttering it.
- **Connection line style & color** — hover any connection and click its 📝 button to open the side panel: a **Line style** dropdown (Solid, Dashed, Single arrow, Double arrow) and a **Line color** picker for that connection, right below it. The arrowhead (if any) matches the chosen color automatically.
- **Supplemental notes on connections** — the same panel's notes box lets you attach a longer note to a connection, once it has a label.
- **Delete affordances on hover** — the red **×** on a connection only appears when you hover that connection's line, keeping plain connections visually clean.
- **Infinite pan & zoom** — the canvas has no boundary. Scroll (trackpad or mouse wheel) to pan freely in any direction — there's always more space, it never stops at the last node. Pinch, or hold Ctrl/Cmd and scroll, to zoom smoothly in or out (roughly 2%–2000%); the toolbar also has **−** / **+** zoom buttons and a percentage readout that resets to 100% when clicked. Dragging empty canvas also pans by default (see Move/Select toggle below).
- **New** — clears the canvas back to a truly empty mind map (no default node), resets the view, and unlinks from whatever file was loaded, so your next save creates a fresh file.
- **Clear** — wipes all nodes and connections but keeps you linked to the currently loaded file.

## Saving and loading

This app saves to **real files on disk**, not just downloads, using the browser's File System Access API:

- **Load** opens a file picker and reads the exact `.json` file you choose.
- **Save** writes back to that same file in its original location. If no file is loaded yet, the first Save opens a "save as" picker — after that, the file is linked and stays in sync.
- **Save As** always opens the file picker so you can save a new copy under a new name/location. Afterward, that new file becomes the active one — Save and autosave switch to it, leaving the original file untouched.
- **First-save naming** — the very first time you save an unlinked map, the suggested file name is taken from whatever you typed into the first node you created.
- **Autosave** — once a file is loaded or first saved, every change (adding/editing/deleting a node or connection, dragging, recoloring, notes, line style) is written back to that file automatically about half a second after you stop. The toolbar shows the current file name and a status dot ("Saving…" / "Saved").

### Browser support

The File System Access API (real save-in-place) is supported in **Chrome, Edge, and other Chromium-based browsers**. In browsers that don't support it (e.g. **Firefox, Safari**), the app falls back to:

- **Load** → opens a standard file picker and reads the file into the app.
- **Save** → downloads a copy as a `.json` file (you'll need to re-select it via Load next time, and autosave won't have anywhere to write to).

The toolbar tells you which mode you're in.

## File format

Mind maps are saved as plain JSON:

```json
{
  "nodes": [
    { "id": "n1", "x": 1500, "y": 1000, "text": "Central Idea", "notes": "", "color": "", "hasCheckbox": false, "checked": false, "shape": "" }
  ],
  "connections": [
    { "id": "c1", "from": "n1", "to": "n2", "text": "leads to", "notes": "", "lineStyle": "solid" }
  ],
  "idCounter": 3
}
```

`color` is empty by default (uses the app's default node background); when set, it's a hex color string. `lineStyle` is one of `solid`, `dashed`, `arrow`, or `arrow-double`.

This makes maps easy to inspect, version, back up, or generate programmatically.

## Tech

Everything — HTML, CSS, and JavaScript — lives in one file (`mindmap.html`). There are no dependencies beyond a Google Fonts import (Space Grotesk, Inter, Caveat). No frameworks, no build tools, no external services. Panning and zooming use a single CSS transform on the canvas layer, so nodes and connection lines stay perfectly in sync at any zoom level.
