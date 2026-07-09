# CellDynaMo Suite

A single-file, browser-based tool for building and visualizing mitotic spindle simulations. Open `config_tool11.html` (or your renamed copy) in a modern browser — no installation, no server required.

From the launcher screen you can go to:
- **Configurator** — build a starting cell/spindle setup and export it as a `.conf` file for the simulation engine.
- **Trajectory Visualizer** — load PSF + DCD simulation output and play it back in 3D, with optional motor and force-vector overlays.

Use the **← back arrow** (top-left) to return to the launcher at any time, and the **sun/moon toggle** (top-right) to switch light/dark mode.

---

## 1. Configurator

### Getting started
1. From the launcher, click **Configurator**.
2. Pick a case at the top of the left panel:
   - **Prometaphase** — fully supported.
   - **Anaphase** / **One Centrosome** — placeholders, marked "under development."
3. Once Prometaphase is selected, the parameter panel and the live 3D preview appear.

### Parameter sections (left panel)
Click a section header to expand/collapse it.

| Section | What it controls |
|---|---|
| **Ellipsoid & Poles** | Cell shape (`ellipse_a/b/c`), spindle pole separation and size |
| **Microtubules (MT)** | Number/thickness/length/color intensity of MT fibers per pole, and number of K-fibers (length and color intensity are only for the visualizaton purposes and won't affect the configuration file) |
| **Chromatid Core** | Number of chromatids (must be even), kinetochore radius, contour length |
| **Chromosome Kinetics** | One sub-panel per chromosome — position (X/Y/Z) and orientation (X/Y/Z angle in radians). Panels rebuild automatically when you change the chromatid count |
| **Regulatory Proteins** | Toggle chromokinesins, CENP-E (+ phosphorylated state), dynein, Ndc80, Aurora A/B kinase clouds on/off |

The 3D viewport updates live as you edit values. A legend appears bottom-left whenever any regulatory protein is enabled.

### Viewport controls
- **Left-drag**: rotate • **Right-drag**: pan • **Scroll**: zoom
- **⟲ Update & Align View** (top button): re-centers and reframes the camera on the current configuration.

### Exporting
- **Download .conf**: saves your current parameters as a tab-separated `config.conf` file, ready for the simulation engine.
- **Reset Configuration**: restores all Prometaphase parameters to their defaults.

---

## 2. Trajectory Visualizer

### Getting started
1. From the launcher, click **Trajectory Visualizer**.
2. In the left panel, under **Load Files**, load your **PSF** (topology) file, then your **DCD** (trajectory) file. Playback and the 3D scene populate once both are loaded.

### Optional overlays
- **Motors** — load a separate PSF + DCD pair containing motor-protein atoms (Dynein, Chromokinesin, CENP-E, Ndc80, identified by atom name). These appear as extra entries in the Components list.
- **Forces** — load a separate PSF + DCD pair of TAIL/HEAD atom pairs to show force vectors in a small picture-in-picture window.
  - **Chromosomes** field: type `all`, a single number (`2`), a range (`1-3`), or a list (`1,3,6`) to choose which chromosomes' force vectors are shown.
  - **Scale bar axis — Dynamic vs Stable**:
    - *Dynamic* re-fits the zoom to whatever is visible every frame (vector sizes look consistent, but the pN scale bar shifts frame to frame).
    - *Stable* keeps the zoom fixed for a given chromosome selection (no flicker while playing), and automatically re-sizes when you change *which* chromosomes are selected.
  - Check **Show force inset window** to display it; drag it by its header, resize it with the corner handle or the Size fields, and minimize it with the `–` button.

### Components panel
Every atom type found in the PSF (MT, kinetochores, chromatid bodies, poles, plus any loaded motors) gets its own row:
- Checkbox to show/hide
- Color swatch (click to open the color/brightness picker)
- Atom index ranges shown (e.g. `Idx 1-40`)
- **Range** field to render only a subset (`all`, `5`, `1-10`, `1,4,9`)
- **Material** dropdown (Standard/Phong/Toon/Wireframe/Glass)
- **Radius** (nm)
- **Duplicate** — makes an independent copy of the component (useful for coloring a subset differently); duplicated rows can be **Remove**d
- **Spheres/Surface** toggle (kinetochores only)

### Playback
Bottom-left overlay (drag by its header, minimize with `–`):
- Frame slider, frame counter
- ⏮ / ◀ / ▶▶(reverse play) / ▶▶(forward play) / ▶ / ⏭ / 🔁 (loop) buttons
- **Speed** dropdown (0.25×–100×)
- **Stride**: play/step every Nth frame. **Clear** permanently deletes the skipped frames from the loaded trajectory (main + motors + forces, if loaded) — this cannot be undone. It does not rewrite the original files, only the visualization scene. 

### Tools (measurements)
Pick a tool, then click atoms in the 3D view:
- **Label** — click one atom to tag it
- **Distance** — click two atoms; shows live distance, updates every frame
- **Angle** — click three atoms (vertex is the 2nd click); shows live angle, updates every frame
- **Cancel** — deselects the current tool

Each label/distance/angle appears in its own list with **show/hide** and **delete** controls. Distances and angles also have a **plot** link that opens a chart of the value across every frame, exportable as PNG, CSV, or TXT.

### Lighting & Scene
- Background color presets (Dark/Light/Black/White) or a custom color picker
- Six independent lights (Ambient, Main, Fill, Back, Rim, Top) — toggle and set intensity individually
- **Cell membrane**: optional ellipsoid overlay with adjustable a/b/c radii
- **Save Hi-Res PNG**: renders the current frame at 2×–8× resolution (composites the Forces inset window too, if visible)

### Export Video
1. Pick a format (WebM recommended; MP4 depends on browser support), frame range, stride, FPS, and bitrate.
2. Click **Export Video**. Export runs in two phases (capture, then encode) with a progress bar; **Cancel** stops it early.
3. The finished video downloads automatically when done.

### Camera controls (top toolbar)
- **⟲ Align View** — frames the camera on the currently loaded trajectory
- **⟲ Reset Camera** — returns to the default camera position
- **⟲Z / Z⟲** — roll the camera (or hold **Q**/**E**)
- **🗑 Clear Scene** — wipes everything (main trajectory, motors, forces, components, annotations) so you can load a different system from scratch; asks for confirmation first

---

## Tips
- The Configurator and Visualizer are independent — nothing carries over between them; export a `.conf` from the Configurator and feed the resulting simulation's PSF/DCD output into the Visualizer separately.
- All 3D viewports use standard **rotate / pan / zoom** mouse controls (see above).
- Values described as "purely symbolic" in the Configurator's disclaimer (protein counts, etc.) are illustrative, not quantitatively accurate — scales, sizes, and orientations are the reliable part.
