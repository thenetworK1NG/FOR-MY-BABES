# Shadow Plushie

A small, private 3D page for a plushie holding a birthday cake lit by candles.
Plain HTML + Three.js, no build step, no tracking, no network calls — open it and it runs.

## Run it

Any static file server works:

```bash
python -m http.server 8000
```

Then open <http://localhost:8000>. Opening `index.html` directly from the
filesystem will not work: browsers block ES module imports on `file://` URLs.

On Windows there is also `starthost.bat`, which starts the server and opens the page.

## Personalise it

Everything you want to change is in one object near the top of `index.html`, at
line 254:

```js
const CONFIG = {
  name:    "My Love",
  message: "a little something I made just for you",
  date:    "",
  model:   "./model/shadow_plushie.glb"
};
```

- `name` — the big line at the top.
- `message` — the line under it.
- `date` — optional; leave `""` to hide it.
- `model` — path to the GLB, if you swap in a different one.

Edit, save, refresh. No rebuild.

## Controls

| Action | Result |
| --- | --- |
| Drag | Orbit the camera |
| Pinch / scroll | Zoom |
| Tap the plushie | Hearts burst out of it |
| Camera button | Toggle slow auto-spin |
| Photo button | Hide the interface and save a full-resolution PNG |
| Fullscreen button | Go fullscreen |

## Quality

Quality is chosen automatically from device memory, CPU cores, screen size and
pixel ratio. Override it with a query parameter:

```
index.html?q=low
index.html?q=high
```

Low caps the pixel ratio and thins out the glow sprites and point lights, which
helps older phones.

## Layout

```
index.html            the whole page — markup, styles and script
model/                shadow_plushie.glb (934 KB, textures as WebP)
source/               the untouched original model
vendor/three/         Three.js r169, vendored for offline use
.nojekyll             tells GitHub Pages to serve files as-is
starthost.bat         local server helper for Windows
```

## Publishing to GitHub Pages

Push these files to a repository and turn Pages on under
**Settings → Pages → Deploy from a branch**, choosing the branch and the root
folder. `.nojekyll` is already there, so nothing will be processed out from
underneath it.

Note that everything here is in one folder, so the repository root *is* the site
root. If you would rather keep the project files out of the published branch,
copy the contents into a `docs/` folder and point Pages at `/docs` instead.

## A note on the model

`source/shadow_plushie.glb` is the original file, kept unmodified.
`model/shadow_plushie.glb` is the same model with its textures re-encoded to
WebP at quality 90, which takes it from 5.24 MB to 934 KB. Geometry is
byte-for-byte equivalent in triangle count and bounding box; the only loss is a
slight softening of the texture detail, which is not visible at the size this
model is displayed.

The model ships without normal attributes, because it was exported as an
unlit asset. `prepareModel()` calls `computeVertexNormals()` on the meshes that
need to receive light, otherwise the lit shading has nothing to work from and the
model renders black.
