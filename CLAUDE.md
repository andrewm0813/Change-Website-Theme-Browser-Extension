# Theme Force — Claude Code Guidelines

## Repository
Browser extension that forces websites to use dark mode, light mode, or the system default. Works in both Chrome and Firefox (Manifest V3).

## Branch
Always commit and push changes to the `main` branch unless otherwise instructed.

## Project Structure
```
extension/
  manifest.json     # MV3 manifest (Chrome + Firefox compatible)
  background.js     # Service worker / background script
  content.js        # Injected into every page at document_start
  popup.html        # Extension popup UI
  popup.css         # Popup styles
  popup.js          # Popup logic
  icons/            # PNG icons (16, 32, 48, 128px)
  gen_icons.py      # Script to regenerate icons (stdlib only, no Pillow)
```

## Cross-Browser Compatibility
- The manifest includes both `background.service_worker` (Chrome) and `background.scripts` (Firefox)
- Both content and background scripts use a `const api = (typeof browser !== 'undefined' ? browser : chrome)` shim
- One ZIP file works for both browsers — do not create separate builds

## Extension Versioning
- Version is set in `extension/manifest.json` under `"version"`
- Follow semver: bump patch (x.x.1) for fixes, minor (x.1.0) for new features
- Current version: 1.0.1

## Key Implementation Notes
- `content.js` runs at `document_start` and sets `color-scheme` on `:root`
- For sites that ignore `color-scheme`, a CSS `filter: invert + hue-rotate(180deg)` fallback is applied via `@media` query
- Images, videos, canvas, and SVG images are re-inverted to preserve true colour
- Theme preference is stored in `storage.local` and broadcast to all tabs via `background.js`
- The `__tf-invert` class is added to `<html>` as a hook for the CSS fallback

## Do Not
- Do not commit binary files (ZIPs, compiled assets) to the repo
- Do not create separate Firefox and Chrome builds — the manifest supports both
- Do not add `gen_icons.py` to release ZIPs
