# TerraVol

A free, browser-based surface volume calculator for earthworks and site surveys — no license, no install, no lab booking.

Developed by **Mlibatisi Dlamini** · BSc Geomatics (University of Botswana) · © 2026

## Why

Cut-and-fill and surface volume work usually means a licensed seat of ArcGIS or Civil 3D — often only available in a campus lab, and rarely available off campus. TerraVol covers the specific, everyday task that trips people up most: turning a set of surveyed X, Y, Z points into a triangulated surface and getting plan area, surface area, and cut/fill volumes against a reference elevation, entirely client-side in a browser tab.

## Features

- **CSV upload** with automatic X/Y/Z column detection, or a built-in sample dataset to try it instantly.
- **Point filtering** by survey code (e.g. drop control points or blunders before triangulating).
- **Constrained triangulation** — an optional breakline/string column forces those edges into the mesh (tops and toes of slope, kerbs, drains) instead of letting the triangulation cut across them. This is the same idea ArcGIS and Civil 3D use, and the biggest single source of divergence from those tools when it's missing. Implemented with [`@kninnug/constrainautor`](https://github.com/kninnug/Constrainautor) (Sloan's edge-insertion algorithm) on top of `d3-delaunay`.
- **Plan and 3D perspective views** of the triangulated surface — draggable/pannable in both, with a "Open larger" fullscreen view and a "Download image" (PNG) export for either.
- **Five elevation symbologies**: Terrain, Viridis, Spectral, Grayscale, and Hillshade (pure relief shading from triangle normals, independent of elevation colour).
- **Dark / light mode** toggle.
- **Cut / fill / net volume**, plan area, surface area, coordinate extent, and the lowest/highest surveyed points — all computed live against a reference elevation you set (or pick from the data's min/max).
- **Downloadable report** (`.txt`) recording every input, setting, extent, and result, with credit and an accuracy disclaimer, for a lab writeup or client email.
- **Built-in onboarding** — a short, student-friendly "how to use this" panel on first load, reopenable any time from the header.

## Limitations

TerraVol is a teaching and quick-check tool, not a replacement for ArcGIS or Civil 3D on a real project:

- It does not clip triangles that straddle the reference plane, so figures very close to that elevation can differ slightly from professional GIS software even with breaklines enforced.
- Breakline detection assumes rows sharing the same value in the breakline column, in file order, form a connected line — this matches most survey exports (a "String"/"Line" ID column) but won't reconstruct an out-of-order or multi-branch breakline correctly.
- Results are not guaranteed to match licensed GIS/CAD software to full accuracy. Where the numbers matter for grading or a client, verify against licensed software. Where they don't, this should save you the wait for a lab seat.

## Using it

Open `index.html` in a browser — that's the whole app, no server or build step required. Upload a CSV with X, Y, Z columns (and optionally a code and/or breakline column), set a reference elevation, and read the results.

## Deploying

This repo is a single static file, so any static host works:

**GitHub Pages**
1. Push `index.html` to this repo's default branch.
2. Repo **Settings → Pages → Source**, select the branch and `/ (root)`, save.
3. Live at `https://<username>.github.io/<repo>/` within a minute or two.

**Netlify / Vercel**: drag-and-drop the repo (or connect it) — no build command needed, since `index.html` is already a complete, self-contained bundle.

## Development

The shipped `index.html` is a pre-built, minified bundle so it deploys with zero setup. To modify the app itself:

```bash
npm install
# edit src/App.jsx
npx esbuild src/main.jsx --bundle --minify --target=es2018 --loader:.jsx=jsx --jsx=automatic --outfile=bundle.js
# then re-embed bundle.js into index.html, or serve src/ with your own dev setup (e.g. Vite)
```

Runtime dependencies: `react`, `react-dom`, `d3` (Delaunay triangulation), `papaparse` (CSV parsing), `lucide-react` (icons), `@kninnug/constrainautor` (constrained triangulation).

## License

MIT — free to use, fork, and extend. If you improve something, a pull request is welcome.
