# CNC Pro Studio v5.9 — Imported real STL terrain preview

Standalone local HTML. Open `index.html` in Chrome, or host it with GitHub Pages.

## What's fixed

- Imported STL/OBJ mesh surfaces are immediately shown as a **3D target relief**, not an uncarved flat stock board. The blue top view shows a color-coded topographic heightmap with contour lines. Drag the 3D view to orbit and scroll to zoom.
- Many terrain exporters include a **thick, flat bottom slab** below the real elevations. The older importer normalized against the slab's entire thickness, compressing mountain detail. v5.9 uses sampled **top-surface** height variations and (by default) fits them to the configured carving depth, preserving relative elevation differences.
- New "Real STL terrain — 3D preview" controls under Maps > 3D model let you choose carving depth, turn full-depth normalization on/off, preview, and generate actual cutter passes.
- Full-depth height fitting is **vertical exaggeration** to suit the relief depth, not a claim of physically accurate topographic scale. STL itself does not encode a unit of measure; verify actual XY dimensions.
- Software-rendered 3D preview now includes contour lines so terrain doesn't appear like a smooth flat tray on browsers without WebGL.
- Changing relief depth resets old toolpath validation. New G-code is generated only when requested.

## Steps

1. Open CNC Pro v5.9.
2. Click **Open / upload file**, select your real Colorado or other **`.stl`** (or triangulated `.obj`).
3. CNC Pro opens **3 Maps > 3D model**, displaying a color elevation map and **3D TARGET TERRAIN — not machined yet**. This is the intended target geometry, not the cutter simulation.
4. Scroll to **Real STL terrain — 3D preview** and choose **Carving depth**, for example 4 mm (less than your stock thickness). Leave **Use full carving depth for measured top-surface heights** selected if your terrain STL includes a thick base.
5. Select **Show terrain in 3D** to update the preview, or **Generate cutter passes** to create and validate router G-code and simulate actual removal.
6. Check bit diameter/type, feed, depth per pass, step-over, stock setup, clamps and fixture clearance. Confirm the final relief is not cut too deep. Perform an air-cut before using a verified matching controller.

## Limitations and troubleshooting

- For STL/OBJ, this is **top-visible 2.5D machining**. Undercuts, sides, backside geometry, separate parts and machine collisions are not modeled. Geometry above the model's sampled top surface is not reconstructed.
- Files with a truly flat top surface cannot be converted into mountains automatically. Likewise, a terrain mesh tilted onto its side may show a flat or incorrect top. Rotate it so elevation is along +Z in a mesh editor before importing.
- A real U.S. state **outline** or geographic silhouette is not an elevation map. Download a terrain STL or measured elevation raster (DEM) for topography.
- This version accepts STL and triangulated OBJ as 3D meshes, not GeoTIFF DEM files directly. Photos become *artistic brightness reliefs* unless separately calibrated.
- This app generates **router bit** toolpaths, **not laser-ready G-code**. Imported Makera NC programs must not be treated as verified code for a FoxAlien machine.
- When a mesh appears flat, confirm its file extension and that the imported elevation status says a nonzero Z span. If that still fails, send the original file and the Download error report for inspection.

## Browser checks

The v5.9 import and preview path was tested in Chromium using a **synthetic terrain slab** (not real Colorado terrain) with a thick base and shallow upper relief. The tested sample was remapped to the selected depth and rendered with 2D contours and 3D relief. A separate 228,800-triangle synthetic STL also loaded and displayed without JavaScript errors. On a smaller 90 × 75 mm test stock, it generated a 5,293-movement validated router toolpath and completed the finished-cut simulation. These checks do **not** prove that an unprovided user STL, a specific FoxAlien controller, or a physical cut is correct.
