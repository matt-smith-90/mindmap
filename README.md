# Mind Map

A single-file, no-install mind mapping app. Open `mindmap.html` in your browser and start mapping — no build step, no server, no accounts.

## Features

- **Add nodes** — click **+ Node** in the toolbar to drop a new node onto the canvas, ready to type into immediately.
- **Quick branching** — hover any node to reveal small **+** buttons on its top, right, bottom, and left edges. Click one to instantly create a new node already connected to it, positioned in that direction. This is the fastest way to grow a map.
- **Edit node text** — click into any node and type. Text is centered in the node.
- **Drag to arrange** — click and drag any node to reposition it; connected lines follow automatically.
- **Connect existing nodes** — toggle **Connect Nodes**, then click one node and another to link them. Click again on either end to cancel a pending connection.
- **Label connections** — click any connection line to type a label on it. Leave it blank and it just renders as a plain line (no placeholder text). A small **×** next to each line deletes that connection.
- **Delete nodes** — hover a node and click the red **×** in its corner. Deleting a node also removes any connections attached to it.
- **New** — clears the canvas back to a single starter node and unlinks from whatever file was loaded, so your next save creates a fresh file.
- **Clear** — wipes all nodes and connections but keeps you linked to the currently loaded file (the emptied map is what gets saved).

## Saving and loading

This app saves to **real files on disk**, not just downloads, using the browser's File System Access API:

- **Load** opens a file picker and reads the exact `.json` file you choose.
- **Save** writes back to that same file in its original location. If no file is loaded yet, the first Save opens a "save as" picker — after that, the file is linked and stays in sync.
- **First-save naming** — the very first time you save an unlinked map, the suggested file name is taken from whatever you typed into the first node you created.
- **Autosave** — once a file is loaded or first saved, every change (adding/editing/deleting a node or connection, dragging a node) is written back to that file automatically about half a second after you stop. The toolbar shows the current file name and a status dot ("Saving…" / "Saved").

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
    { "id": "n1", "x": 1500, "y": 1000, "text": "Central Idea" }
  ],
  "connections": [
    { "id": "c1", "from": "n1", "to": "n2", "text": "leads to" }
  ],
  "idCounter": 3
}
```

This makes maps easy to inspect, version, back up, or generate programmatically.

## Tech

Everything — HTML, CSS, and JavaScript — lives in one file (`mindmap.html`). There are no dependencies beyond a Google Fonts import for the toolbar typeface. No frameworks, no build tools, no external services.
