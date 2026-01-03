# Apartment 3D Synoptic (Canvas)

A lightweight, browser-only **3D apartment synoptic** built with **HTML + CSS + JavaScript (Canvas 2D)**.

It renders a neon/wireframe floorplan in a pseudo‑3D view, paired with a **clickable 2D minimap** (smart‑home style UI).  
**No build step. No dependencies.**

---

## Demo

- Open `index.html` in a browser (Chrome/Edge/Firefox) [https://gabrieleangeli.github.io/apartment-3d-synoptic/](https://gabrieleangeli.github.io/apartment-3d-synoptic/).
- Interact with the 3D view and the minimap (selection is synced).

---

## Features

- **Pseudo‑3D wireframe rendering** on `<canvas>` (neon/glow style)
- **2D minimap** (phone‑style UI) with room labels + click‑to‑select
- **3D room picking** (click rooms in the 3D view to select/highlight)
- “CAD‑like” controls:
  - **Left‑drag**: rotate (yaw + pitch)
  - **Right‑drag**: rotate **only horizontally** (yaw around Y axis)
  - **Shift + drag**: pan
  - **Mouse wheel**: zoom
  - **R**: toggle auto‑rotate
- **Performance‑oriented rendering**
  - Decorative elements are **not regenerated every frame**
  - Heavier visuals can be rebuilt **after interaction ends**
  - Optional FPS/quality controls (if enabled in the UI)

---

## Getting Started

### Option A — Open directly
You can open the file directly in the browser:

- Double click `index.html`
- or drag & drop into Chrome/Edge/Firefox

### Option B — Serve locally (recommended)
Serving locally helps if you later add assets or split files:

```bash
# Python
python -m http.server 8000
```

Then open:

- `http://localhost:8000/`

---

## Controls

### Selection
- Click a room in the **3D view** or in the **2D minimap**  
  The selection is synchronized between both views.

### Camera / Navigation
- **Left mouse drag**: rotate (yaw + pitch)
- **Right mouse drag**: yaw only (horizontal rotation around Y)
- **Shift + drag**: pan
- **Wheel**: zoom
- **R**: toggle auto‑rotation

---

## Project Structure

This project is intentionally simple and can live as a single file:

- `index.html` — everything (HTML, CSS, JS) in one place

If you want to split it:

- `styles.css` — UI + neon styling
- `app.js` — rendering, picking, controls

---

## Floorplan Data Model

Rooms are defined as polygons in a plain JavaScript array:

- Each room has:
  - `id` (string)
  - `name` (label)
  - `info` (subtitle/description)
  - `poly` (list of 2D points in meters): `[[x,z], [x,z], ...]`
  - optional:
    - `h` (height, for balconies/low walls)
    - `kind` (e.g. `"outdoor"`)

Example:

```js
const ROOMS = [
  {
    id: "kitchen",
    name: "Kitchen",
    info: "Cooking / dining",
    poly: [[0.8,0.8],[3.6,0.8],[3.6,3.2],[0.8,3.2]]
  }
];
```

> Tip: keep polygons ordered (clockwise or counter‑clockwise) and closed implicitly (the last point connects to the first).

---

## Performance Notes

Canvas 2D rendering is often **CPU‑bound**, especially if you use:

- many strokes per frame
- `shadowBlur`
- additive blending (`globalCompositeOperation = "lighter"`)

To keep the scene responsive:

- Avoid rebuilding decorative layers every frame
- Rebuild grid/decoration **after** user interactions (drag/zoom) end
- Use an FPS limiter when needed (e.g., 30–45 FPS)
- Prefer caching static layers into offscreen canvases

---

## Customization

### Colors / style
Look for CSS variables near the top of the file, for example:

- `--cyan`, `--mag`, `--bg0`

### Add “devices” and overlays
A common next step is adding overlay objects:

- sensors / switches
- doors / windows
- lights / alarm zones

Suggested approach:

- keep overlays in their own arrays (`DEVICES`, `DOORS`, …)
- draw overlays in separate cached layers when possible
- keep picking logic separate for rooms vs overlays

---

## Roadmap Ideas

- Multiple floors / floor selector
- Room status (alarm, occupancy, temperature, etc.)
- Import/export layouts as JSON
- Device layers with filtering and per‑room aggregation

---

## License

Choose what fits your use case:

- **MIT** (recommended for open source)
- or keep it private/internal

---

## Credits

Built as a compact experiment to explore:

- canvas‑based pseudo‑3D rendering
- interaction patterns (CAD‑like controls)
- UI composition with a synchronized minimap
