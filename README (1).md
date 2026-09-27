# Warehouse Inventory Visualization Tool

A two-part tool for tracking exactly where inventory sits on warehouse shelves, how much dead space is being wasted, and who moved what and when.

Built in two phases: a **2D web app** first, to get the data model and workflow right, followed by a **fully interactive 3D floor plan** once real use showed that a flat view wasn't enough to reason about actual shelf space.

---

## Live Tools

| Version | What it's for |
|---|---|
| **2D — Yard & Rack Locator** | Fast, detailed bin-by-bin editing. Best for day-to-day data entry. |
| **3D — Warehouse Floor Plan** | Spatial overview of the whole room. Best for understanding layout, aisles, and overall utilization at a glance. |

Both versions share the same underlying data model and bin-editing panel — the 3D tool simply opens the same panel when you click a bin in the 3D scene.

---

## Core Features

### Rack & Level Configuration
- Each rack's width, depth, height, and number of levels is configured independently.
- Two level types:
  - **Small Box** — subdivided into 4 bins per level, for individually-boxed items.
  - **Skid** — one full-width slot per level, since only one skid fits per level.
- Level heights don't have to match — one rack can have 6 short shelves, another 4 tall ones.

### Real Dimensions, Real Dead Space
- Every box is entered with its actual width × height × depth — not an estimated fill percentage.
- The tool calculates exact dead space on every axis (H / D / W) and tells you exactly how much room is left, and what could still fit there.
- Adding a box that physically won't fit is blocked at entry time, with a clear explanation of which dimension is the problem — your form inputs are preserved so you can adjust and retry.

### Multi-Item Bins & Stacking
- A single bin can hold several boxes.
- Each box is explicitly marked as stacked **"on top"** (adds to height) or **"behind"** (adds to depth).
- Manual reordering (▲ / ▼) within each stack — the top item in the list is the physically topmost (or frontmost) box.
- Each box gets its own distinct color, consistent across the item list and the visual preview.

### Full Audit Trail
- Every box records **who received it** and **when**.
- Removing a box asks who removed it and when, then moves it into that bin's **removal history** rather than deleting it outright.
- The 3D tool adds a consolidated **Removal Archive** — every removal, across every rack and bin, in one list.

### 3D Floor Plan
- Racks are placed into an empty room one at a time, each with its own position, rotation, and color.
- Full camera control: drag to orbit, scroll to zoom, right-drag to pan.
- Real box dimensions are rendered to scale inside each bin, using the same stacking logic as the 2D tool.
- Hovering over any box shows its full record (SKU, dimensions, received by/date) as a tooltip — an "ID card" for that item without opening the edit panel.
- Hovering over an empty or occupied bin shows a "click to edit" cursor and tooltip, so it's clear what's interactive.

---

## Tech Stack

- **Plain HTML / CSS / JavaScript** — no build step, no framework, runs as a single self-contained file.
- **Three.js (r128)** for the 3D scene, loaded via CDN, with a hand-rolled orbit/pan camera control (no external controls addon).
- All state lives in memory in the browser tab — refreshing the page resets the data. There is no backend or persistent storage yet (see Roadmap).

---

## How to Use

1. Open the tool in a browser (double-click the `index.html` file, or host it anywhere).
2. **2D tool:** define a rack's dimensions and levels in the setup screen, click "Build," then click any bin to add boxes.
3. **3D tool:** click "+ Add Rack," adjust its position/size/levels in the sidebar, then click any bin in the 3D scene to open the same editing panel.

---

## Roadmap / Known Limitations

- **No persistence.** Data resets on page reload. A real deployment would need a backend (even a simple key-value store) to save state between sessions.
- **Simplified "behind" stacking in 3D.** The 3D view renders depth-stacked boxes directly behind the height stack; the fine-grained positioning available in the 2D preview isn't fully replicated yet.
- **Single shared room in 3D.** All racks currently live in one un-named room; multi-room / multi-floor support isn't built yet.

---

## Project History

This tool was built iteratively based on direct, hands-on feedback while actually using it to plan real warehouse shelving — see `Warehouse_Tool_Project_Summary_Report.docx` for the fuller narrative, including the original hand-drawn sketch that defined the 3D redesign.
