# Backgrounds

This folder powers the hero band image at the top of `index.html`.

## Files

- `manifest.json` — curated list of landscape Unsplash photos. Each entry has `id`, `url` (direct `images.unsplash.com` link at `w=1920&q=80`), `credit`, `theme` and `setting`:
  - `"setting": "outdoor"` — summer festival / open-air photos (festival crowds, open-air stages, sunset crowds).
  - `"setting": "indoor"` — club photos (dark rooms, lasers, DJ booths, club crowds).
- `bg-current.jpg` — the image shown this week. Referenced by `index.html` as `backgrounds/bg-current.jpg`.

## Rotation (seasonal)

The weekly scheduled scan rotates `bg-current.jpg` once per week. It first picks the pool that matches the season, then uses the ISO week number within that pool:

```
setting = "outdoor" if April <= month <= September else "indoor"
pool    = [img for img in manifest.images if img.setting == setting]
chosen  = pool[ISO_week_number % len(pool)]
download(chosen.url) -> backgrounds/bg-current.jpg
```

So the board shows festivals in the summer half of the year and clubs from October to March, when most events are inside. `index.html` itself never has to change — it always reads `backgrounds/bg-current.jpg`.

## Adding new images

1. Find a landscape Unsplash photo (≥ 1600px wide) under the free Unsplash License, not Unsplash+.
2. Resolve its direct image URL: `https://images.unsplash.com/photo-XXXXXXXXXX?w=1920&q=80`.
3. Append `{ "id": "...", "url": "...", "credit": "Name / Unsplash", "theme": "short tag", "setting": "outdoor" | "indoor" }` to `manifest.json`.
4. Commit. The next weekly scan picks it up when the week index lands on its slot in that season's pool.

All images are used under the Unsplash License (free for commercial and personal use, no attribution required — but we keep `credit` for hygiene).
