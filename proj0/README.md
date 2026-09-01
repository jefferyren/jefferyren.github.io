# CS 180 · Project 0 — Becoming Friends with Your Camera

Course webpage for Project 0: perspective, focal length, and the center of projection.

## Design

"Cyanotype table" — the page is treated as a drafting sheet. Cool paper (`#EFF3F7`) over a
24 px blueline grid, navy ink, and one reserved accent: orange (`#E8590C`) is used *only* for
measurement, never for emphasis. Type is Instrument Serif (display), Inter Tight (body), and
Space Mono (labels and data), loaded from Google Fonts.

Two structural devices carry meaning rather than decoration:

- **Registration marks.** Every photo sits inside blueprint corner ticks, so each image reads
  as a plate in a drawing set (`Plate 1.1`, `1.2`, `2.1`, …).
- **Dimension lines.** Under each plate, an orange dimension line states where the camera
  stood. Line *length* is proportional to distance — the 0.3 m selfie gets a stub, the 3 m
  one spans the plate, and Part 2's pair is drawn `d` against `d/2`.

Motion is limited to a single scroll-triggered reveal: plates fade up and their dimension
lines draw left-to-right, as if being annotated. It is disabled under
`prefers-reduced-motion`, and no content depends on JavaScript to become visible.

There is a print stylesheet — each part breaks onto its own page, so the site prints as a
drawing set.

## Structure

```
index.html                    the whole page
style.css                     styles
media/
  part1-close.jpg             Part 1 — 24 mm equiv., shot close
  part1-far.jpg               Part 1 — 88 mm equiv., stepped back
  part2-far-zoomed.jpg        Part 2 — 53 mm equiv., shot from far
  part2-near-wide.jpg         Part 2 — 24 mm equiv., walked closer
  part3-dollyzoom.gif         Part 3 — the animated dolly zoom
  dolly/frame01..09.jpg       Part 3 — individual stills
part1a.HEIC, part1b.HEIC      camera originals (kept for reference)
part2a.HEIC, part2b.HEIC      camera originals (kept for reference)
part3.gif                     original GIF
```

## Image pipeline

Camera originals are HEIC. Each was converted to progressive JPEG, resized to 1600 px on the
long edge, and had its EXIF rotation baked into the pixels:

```bash
sips -s format jpeg -s formatOptions 100 part2a.HEIC --out /tmp/p2a.jpg
python3 -c "
from PIL import Image, ImageOps
im = ImageOps.exif_transpose(Image.open('/tmp/p2a.jpg'))
im.thumbnail((1600,1600), Image.LANCZOS)
im.convert('RGB').save('media/part2-far-zoomed.jpg','JPEG',quality=86,optimize=True,progressive=True)
"
```

No cropping, retouching, or color grading was applied.

## Publishing to GitHub Pages

```bash
git init
git add .
git commit -m "CS 180 Project 0"
git branch -M main
git remote add origin https://github.com/<user>/<repo>.git
git push -u origin main
```

Then in the repo: **Settings → Pages → Source: Deploy from a branch → `main` / `(root)`**.
The site appears at `https://<user>.github.io/<repo>/` within a minute or two.

## Local preview

```bash
python3 -m http.server 8000
```
