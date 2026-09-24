# cyberchef-live

Static mirror of [GCHQ CyberChef](https://github.com/gchq/CyberChef), built for
GitHub Pages. Live at https://coattails-droid.github.io/cyberchef-live/

## Build provenance

- **Source repo:** gchq/CyberChef
- **Commit SHA:** fb95859b4e57acf42c448ec1a07a4b9221a4e9e9
- **Build command:** `npm install && npm run build (npx grunt prod)`
- **Build date:** 2026-09-24 (UTC)
- **License:** Apache-2.0 (see `LICENSE`, copied verbatim from upstream)

## What this is

This repo contains ONLY the production build output (`build/prod` from the
source tree) plus the upstream license and this README. No `node_modules`,
no build tooling.

The webpack build uses an empty `publicPath`, so all asset URLs are relative
and the app works from the `/cyberchef-live/` subpath. Hash-based deep links
(`#recipe=...`) work without any server rewrites.

Excluded from the upstream `build/prod` output (not needed to serve the app):
`BundleAnalyzerReport.html` (dev-only bundle analysis) and the pre-compressed
`*.br` / `*.gz` duplicates (GitHub Pages does not content-negotiate them).

One deliberate deviation in `index.html`: the in-app "Download ZIP file"
button pointed at `CyberChef_v11.5.0.zip`, the 91 MB offline-download archive.
That file is too large for the GitHub blob API (upload 502'd), and release
assets can't be used from this automation, so the archive is not hosted here
and the button was replaced with a disabled note saying so. Everything else
in the app works as upstream built it.

## Build note

The stock `npx grunt prod` (terser, parallel workers) was OOM-killed
repeatedly on the small build VM, so the *local-only* `Gruntfile.js` (not
shipped here) was patched to minify with esbuild via
`minimizer-webpack-plugin` (`{ minify: esbuildMinify }`). Output remains
minified; only the minifier implementation differs. The upstream source,
including `webpack.config.js` (`publicPath: ""`), is otherwise untouched.

## Verify locally

```bash
python3 -m http.server 8000
# open http://localhost:8000/
```
