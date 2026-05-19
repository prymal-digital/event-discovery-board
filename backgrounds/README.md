# Backgrounds

This folder powers the hero band image at the top of `index.html`.

## Files

- `manifest.json` — curated list of landscape Unsplash photos themed to electronic music festival / summer outdoor (festival crowds, open-air stages, beach/forest festivals, sunset crowds, etc.). Each entry has `id`, `url` (direct `images.unsplash.com` link at `w=1920&q=80`), `credit`, and `theme`.
- `bg-current.jpg` — the image shown this week. Referenced by `index.html` as `backgrounds/bg-current.jpg`.

## Rotation

The weekly scheduled scan rotates `bg-current.jpg` once per week using the ISO week number:

```
index = ISO_week_number % len(manifest.images)
chosen = manifest.images[index]
download(chosen.url) -> backgrounds/bg-current.jpg
git add backgrounds/bg-current.jpg && git commit && git push
```

So the same week of every year picks the same photo, and adding a new image at the end of the manifest does not disturb earlier weeks (modulo the list-length change). `index.html` itself never has to change — it always reads `backgrounds/bg-current.jpg`.

## Adding new images

1. Find a landscape Unsplash photo (≥ 1600px wide, daytime/sunset, outdoor, festival/crowd vibe).
2. Resolve its direct image URL: `https://images.unsplash.com/photo-XXXXXXXXXX?w=1920&q=80`.
3. Append `{ "id": "...", "url": "...", "credit": "Name / Unsplash", "theme": "short tag" }` to `manifest.json`.
4. Commit. Next weekly scan will pick it up automatically when the week index lands on its slot.

All images are used under the Unsplash License (free for commercial and personal use, no attribution required — but we keep `credit` for hygiene).
