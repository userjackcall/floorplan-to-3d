# AGENTS.md

## Project

Single-file HTML web app: `floorplan-to-3d.html` — open directly in browser, no build step. Three.js loaded from CDN (`r0.164`). Chinese UI.

## Key facts

- **Edit the main file**, then create versioned copies (`floorplan-to-3d_v0.2.html`) for release snapshots.
- No tests, no linter, no typecheck. Manual testing only: open in Chrome/Edge.
- No package.json, no bundler. All logic in one HTML file (~1400 lines).
- State lives in a global `S` object. 3D meshes tracked in `S.meshes` Map.
- 2D canvas uses world coords → screen via `cx.translate(offsetX,offsetY); cx.scale(zoom,zoom)`.
- 3D scene is Y-up; STL export is Z-up.
- Walls always solid (no hollow, no infill). Door = gap in wall. Window = `wall.windowCuts[]` array.

## Coordinate system

- `wpos(e)` converts screen→world: `(clientX - rect.left - offsetX) / zoom`
- 2D draw coords are in mm. 3D preview multiplies by `S.scale * printScale`.
- Wall coordinates: `sX,sY` start, `eX,eY` end. Window cuts store `t1,t2` as projection along wall normal.

## Common edit patterns

- Tools are defined in `data-tool` buttons (select, wall, door, window, stairs, room, furniture, dimension, eraser, pan).
- Two-click tools (door, window) snap click to nearest wall via `snapToWall()`.
- Window parameters (`winSill`, `winHt`) are global on `S`, not per-window. Changing them updates all windows.
- Door threshold `> 1` (was `> 5`) for short wall segments after cut.

## GitHub workflow

- `git add -A && git commit -m "msg" && git push`
- Screenshots go in `screenshots/` folder. Root `*.png` is gitignored.
