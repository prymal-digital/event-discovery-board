# Weekly Event Discovery Scan Protocol

**Canonical, single source of truth for the Monday scan.**
**Last revised:** 2026-06-01 (consolidated bg rotation + single-digest write; this file is now the only place the protocol lives).

The scheduled task (`event-board-daily-scan`, Mondays 07:38 Europe/Amsterdam) and the `event-discovery-weekly-scan` skill both delegate to this file. Do not duplicate instructions elsewhere — update only here.

---

## What the scan produces

Each Monday run:
1. Updates `event_board_data.json` (appends this week's events, dedup by `id`)
2. Overwrites `Weekly Digests/digest.md` (single file, no dated history)
3. Rotates `backgrounds/bg-current.jpg` via the manifest in `backgrounds/manifest.json`
4. One combined commit + push to `origin main` — board redeploys via GitHub Pages

`index.html` is **never** touched by the scan — it fetches `event_board_data.json` at load and re-renders.

---

## Three-Tier Scanning Model

### Tier 1 — Recurring community sources (weekly, ~5K tokens)

Six direct-URL, high-signal sources. WebFetch first; Chrome MCP fallback if the page is JS-rendered.

| Source | URL | Cadence |
|--------|-----|---------|
| Odessa Amsterdam | https://odessa.amsterdam/ | Daily ecstatic dance |
| Ecstatic Dance Amsterdam | https://www.ecstaticdanceamsterdam.com/ | Tue Loods12 + Sun TOS |
| The Conscious Club | https://theconsciousclub.com/events | Monthly Friday |
| Barcelona Entrepreneurs | https://www.meetup.com/pro/barcelona-entrepreneurs/ | Tue + Fri |
| AI Engineers Barcelona | https://www.meetup.com/ai-engineers-barcelona/ | Every Mon |
| AI Tinkerers Amsterdam | https://lu.ma/aibuilders | Note: slug now resolves to SF — verify before adding |

### Tier 2a — RA.co city event pages (weekly, ~75K tokens) — REQUIRED

Chrome MCP required (WebFetch returns shells on RA). Batch in one `browser_batch` per city, paginate via `?page=N` until you exit the week window.

| City | URL | Filter |
|------|-----|--------|
| Antwerp | https://ra.co/events/be/antwerp | Full week sweep |
| Brussels | https://ra.co/events/be/brussels | Full week sweep |
| Ghent | https://ra.co/events/be/ghent | Full week sweep |
| Amsterdam | https://ra.co/events/nl/amsterdam | ≥100 RA attendees OR RA Pick |
| Barcelona | https://ra.co/events/es/barcelona | ≥100 RA attendees OR RA Pick |

**Scoring from RA:** RA Pick = 8.5+; 150+ attendees = 8.0; 50–149 = 7.5; <50 at trusted venue (Fuse, C12, Garage Noord, Shelter, Nitsa, LAUT, Ampere, etc.) = 7.0.

### Tier 2b — Meetup + Luma keyword searches (weekly, ~5K tokens)

Chrome MCP (JS-rendered).

- **Amsterdam:** "AI", "breathwork", "ecstatic", "meditation", "networking", "startup"
- **Barcelona:** "wellness", "entrepreneurship", "product management", "AI builders"
- **Antwerp:** "tech", "startup", "networking"
- **Brussels:** "tech", "startup", "AI", "wellness"
- **Luma:** `lu.ma/amsterdam`, `lu.ma/barcelona` (scroll for the week)

### Tier 3 — Ad-hoc venue sweep (~quarterly, only as needed)

Trigger only when RA's city page is suspiciously quiet for a venue with known programming, or a venue has pulled its RA listing. Not on a calendar.

Venue checklist (use as needed, do not run all):
- BCN: Nitsa, LAUT, BORIS, Nacar, DETROIT CLUB, Moog, Razzmatazz, Apolo, Luz de Gas, City Hall
- AMS: Shelter, RADION, BRET, Lovelee, Garage Noord, Marktkantine, Disco Dolly, Thuishaven, Paradiso, Q-Factory
- ANT: Ampere, Club Vaag, Petrol, Trix
- BRX: Fuse, C12, Spirit of 66, Recyclart, Bonnefooi
- GENT: Kompass Klub, Vooruit, Decadance

---

## Event schema (in `event_board_data.json` under `events[]`)

```json
{
  "id": "unique-kebab-case-id",
  "name": "Event Name",
  "organizer": "Organizer Name",
  "date": "YYYY-MM-DD",
  "time": "HH:MM",
  "endTime": "HH:MM",
  "city": "Amsterdam|Barcelona|Antwerp|Brussels|Ghent|Houthalen-Helchteren|Mechelen",
  "venue": "Venue Name",
  "category": "wellness|music|networking|community",
  "subcategory": "ecstatic dance|breathwork|techno|house|AI builders|...",
  "cost": "€X-Y" or "Free" or "€?",
  "source": "Odessa|Meetup|Luma|Resident Advisor|venue site|...",
  "url": "https://...",
  "hook": "One-sentence appeal",
  "score": 7.0,
  "hidden_gem": true|false,
  "is_festival": true|false,
  "prymal": false,
  "reasons": ["alignment","uniqueness","community","ra_pick","networking","free", ...]
}
```

### `prymal` flag — read carefully

`prymal: true` is **only** for events that PRYMAL is running, hosting, or formally part of. It is **not** a "this event is relevant to a PRYMAL audience" flag. Default to `false`. If you are not sure whether PRYMAL is the organiser, it is `false`.

---

## Decision rules

**Add an event when:**
- Date falls in `[week_start, week_end]`
- Score ≥ 7.0
- Source is verifiable (URL + platform)
- Not a duplicate (check `id`)

**Skip when:**
- Score < 7.0
- Date >3 weeks out (defer to next scan)
- Duplicate (use first source found)

---

## Background rotation (weekly, ~1 sec)

Pick from `backgrounds/manifest.json` by ISO week:

```python
import json, datetime, urllib.request
manifest = json.load(open('backgrounds/manifest.json'))
imgs = manifest['images']
iso_week = datetime.date.today().isocalendar()[1]
chosen = imgs[iso_week % len(imgs)]
req = urllib.request.Request(chosen['url'], headers={'User-Agent':'Mozilla/5.0'})
with urllib.request.urlopen(req, timeout=30) as r:
    open('backgrounds/bg-current.jpg','wb').write(r.read())
```

`index.html` reads `backgrounds/bg-current.jpg` directly — no code change needed. To add new images, append entries to `backgrounds/manifest.json`.

---

## Digest

**Path:** `Weekly Digests/digest.md` — **overwrite** each Monday. No dated archive (history lives in git).

**Structure (under 600 words):**

```
# Where to be this week — Mon DD–DD MMM YYYY

## The 5 picks
Top 5 by score within this week. Each: **Day Time** — Event Name *(City)* — Score X.X
One-line why-it-matters. [Source ↗](url)

## Don't miss if you're in...
Grouped by city, ≥7.0 score, excluding the top 5. One line each.

## Hidden gems
Events flagged `hidden_gem: true`. One line each.

## What I skipped
Low-score-but-prominent events considered and rejected, with reason.

## Sources scanned
Tier-by-tier tally — RA-Antwerp X, RA-Brussels X, RA-Ghent X, RA-AMS X, RA-BCN X, Meetup X, Luma X, Odessa X.

## Confirmed gaps & non-coverage
Venues genuinely dark this week vs. sources not yet checked (Hipsy, Eventbrite, AllEvents, FB Events — known gaps).

## Board status
City totals (this week only): AMS X, BCN X, ANT X, BRX X, GENT X, MECH X
By category: music X, wellness X, networking X
```

Tone: direct, opinionated, no hedging. Sommelier's pick, not a wine list.

---

## Git workflow

Commit only the four files the scan touches. Never `git add .` or `git add -A`.

```bash
PB="/Users/OPTIBIZ/Documents/Claude/Projects/Things to do - Planning Board"
cd "$PB"
git add event_board_data.json "Weekly Digests/digest.md" backgrounds/bg-current.jpg
git -c user.name="Henk" -c user.email="henk@optibiz.be" commit -m "Weekly scan ${MONDAY}: +X events across Y cities

Tier 1: [sources checked]
Tier 2a (RA): AMS X, BCN X, ANT X, BRX X, GENT X
Tier 2b (Meetup/Luma): [keywords run]
Bg: ISO week ${WEEK} → ${PHOTOGRAPHER}

City totals after merge: AMS X, BCN X, ANT X, BRX X, GENT X"
git push origin main
```

If the push is rejected, bail out and report — do not cherry-pick / rebase / merge.

---

## Final report (one line at end of run)

`Weekly scan ${MONDAY} complete — added X events, pushed as commit ${SHA}. Live board: https://prymal-digital.github.io/event-discovery-board/`
