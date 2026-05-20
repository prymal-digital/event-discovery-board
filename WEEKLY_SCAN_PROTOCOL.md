# Weekly Event Discovery Scan Protocol
**Established:** 2026-05-19, revised 2026-05-20
**Purpose:** Efficient weekly scanning that actually catches club programming, not just recurring community events. The 2026-05-20 revision moves RA.co city event pages into the weekly Tier 2 after the prior monthly cadence was found to miss 30+ events per week.

---

## Three-Tier Scanning Model — REVISED 2026-05-20

The key change: **RA.co city event pages run weekly in Tier 2a**, not monthly in Tier 3. The prior monthly cadence missed ~30 events/week of club programming and left Ghent at zero coverage. Switching from "venue-by-venue scraping monthly" to "5 RA city pages weekly" is both cheaper and more complete.

### Tier 1: Recurring Community Sources (WEEKLY — ~5K tokens, ~€0.02/run)

Six direct-URL, high-signal sources. Check every Monday.

| Source | URL | Frequency |
|--------|-----|-----------|
| Odessa Amsterdam | https://odessa.amsterdam/ | Daily |
| Ecstatic Dance Amsterdam | https://www.ecstaticdanceamsterdam.com/ | Tue + Sun |
| The Conscious Club | https://theconsciousclub.com/events | Monthly Friday |
| Barcelona Entrepreneurs | https://www.meetup.com/pro/barcelona-entrepreneurs/ | Tue + Fri |
| AI Engineers Barcelona | https://www.meetup.com/ai-engineers-barcelona/ | Every Mon |
| AI Tinkerers Amsterdam | https://lu.ma/aibuilders | First Thu monthly |

**Method:** WebFetch each page; some are JS-rendered and need Chrome MCP fallback.

---

### Tier 2a: RA.co City Event Pages (WEEKLY — ~75K tokens, ~€0.20/run) — NEW WEEKLY

Single page per city. Each surfaces 40–800 upcoming events across all listed venues. Replaces the prior monthly venue-by-venue Tier 3.

| City | URL | Typical upcoming |
|------|-----|------------------|
| Antwerp | https://ra.co/events/be/antwerp | ~75 events |
| Brussels | https://ra.co/events/be/brussels | ~160 events |
| Ghent | https://ra.co/events/be/ghent | ~45 events |
| Amsterdam | https://ra.co/events/nl/amsterdam | ~700 events |
| Barcelona | https://ra.co/events/es/barcelona | ~800 events |

**Method:** Chrome MCP `navigate` + `get_page_text` (required — WebFetch returns shells on RA.co).

**Filtering:**
- For Antwerp / Brussels / Ghent — include every event whose date falls in `[today, week_end]`.
- For Amsterdam / Barcelona — filter to ≥100 RA attendees OR RA Pick designation (too large to sweep fully).
- Always skip events already on the board (check `id`).

---

### Tier 2b: Meetup + Luma Keyword Searches (WEEKLY — ~5K tokens, ~€0.02/run)

Existing keyword searches across the four primary cities.

**Meetup Keywords (search each region):**
- Amsterdam: "ecstatic", "breathwork", "sound bath", "meditation", "AI", "networking", "startup"
- Barcelona: "wellness", "entrepreneurship", "product management", "AI builders"
- Antwerp: "tech", "startup", "networking" (club programming now in Tier 2a)
- Brussels: "tech", "startup", "AI", "wellness" (club programming now in Tier 2a)

**Luma Searches:**
- `lu.ma/search?q=amsterdam+breathwork`
- `lu.ma/search?q=barcelona+wellness`
- `lu.ma/search?q=tech+antwerp`

**Method:** Chrome MCP for both Meetup and Luma (JS-rendered).

---

### Tier 3: Ad-Hoc Deep Venue Sweep (QUARTERLY or as-needed — ~50K tokens, ~€0.20/run)

Not on a calendar. Trigger only when RA's city page is suspiciously quiet for a venue with known programming, or a venue has pulled its RA listing.

**Venue list** (use as a checklist when running):
- Barcelona: Nitsa, LAUT, BORIS CLUB, Nacar, DETROIT CLUB, Moog, Razzmatazz, La [Apolo], Luz de Gas, City Hall
- Amsterdam: Shelter, RADION, BRET, Lovelee, Garage Noord, Marktkantine, Disco Dolly, Thuishaven, Paradiso, Q-Factory
- Antwerp: Ampere, Club Vaag, Petrol, Trix (Hangar 41 is a bar — exclude)
- Brussels: Fuse, C12, Spirit of 66, Recyclart, Bonnefooi
- Ghent: Kompass Klub, Vooruit, Decadance

**Method:** Chrome MCP navigate to each venue's official agenda. Pick the 3–5 most likely to have been missed by RA, not all 40+.

---

## Weekly Workflow (EST. 20-25 mins, ~85K tokens, ~€0.24)

1. **Open EVENT_RESOURCES_MANAGER** — Check "Last Checked" dates
2. **Run Tier 1** — fetch the 6 recurring community sources
3. **Run Tier 2a** — Chrome MCP through the 5 RA city event pages (batch in one `browser_batch` call). Extract this-week events per the filtering rules above.
4. **Run Tier 2b** — Meetup + Luma keyword searches for the four primary cities
5. **Merge into event_board_data.json**
   - Deduplicate by `id`
   - Validate dates within `[week_start, week_end]`
   - Score using the rubric in SKILL.md
6. **Update digest** — include the explicit "Sources scanned" tally and "Confirmed gaps & non-coverage" sections
7. **Commit + push to GitHub** with the format below

```
Weekly scan [YYYY-MM-DD]: +X events across Y cities

Tier 1: [recurring sources checked]
Tier 2a (RA city pages): [list cities + counts found]
Tier 2b (Meetup/Luma): [keywords run]
Tier 3 (ad-hoc): [only mention if it ran this week]

City totals: AMS X, BCN X, ANT X, BRX X, GENT X
```

8. **Mark EVENT_RESOURCES_MANAGER "Last Checked"** with today's date.

---

## Ad-Hoc Tier 3 Workflow

Run only when RA city pages clearly missed a venue. Process:

1. Identify 3–5 specific venues that look quiet on RA but should have programming.
2. Chrome MCP navigate to each venue's official agenda page.
3. Extract events into the schema, score, merge, push.

Do not run all 40+ venues on a schedule. That's the model we replaced.

---

## Decision Rules

**When to add an event:**
- Date falls in [week_start - 3 weeks, week_end]
- Score ≥ 7.0 (see scoring rubric in EVENT_RESOURCES_MANAGER)
- Source is verifiable (URL + platform)
- Not duplicate (check id field)

**When to skip:**
- Score < 7.0
- Date is >3 weeks in future (save for next weekly scan)
- Duplicate of existing event (different source, same actual event → use first source found)
- Club/venue hasn't announced yet (note in digest as "pending")

**When to refresh digest:**
- If ≥3 new high-scoring (≥8.0) events added
- Otherwise, keep existing digest and note "Updated [date] with [X] new additions"

---

## Token Budget — REVISED

| Tier | Cadence | Tokens/run | Cost/run | Per month |
|------|---------|------------|----------|-----------|
| 1 | Weekly | ~5K | ~€0.02 | ~€0.08 |
| 2a (RA city pages) | Weekly | ~75K | ~€0.20 | ~€0.80 |
| 2b (Meetup/Luma) | Weekly | ~5K | ~€0.02 | ~€0.08 |
| 3 (ad-hoc venue sweep) | ~quarterly | ~50K | ~€0.20 | ~€0.07 |

**Floor:** ~€1/month (no Tier 3 needed any given month).
**Typical:** ~€2/month (one Tier 3 trigger every 2–3 months).

This sits inside the €2-5/month ceiling chosen on 2026-05-20.

### Why this is cheaper than the prior estimate

The original Tier 3 line item budgeted ~€3.33/month for a venue-by-venue Chrome MCP sweep of 40+ pages. Switching to 5 RA city pages (one per city, each surfacing every event at every venue in that city) replaces ~95% of that work with ~5% of the token cost. The savings are not theoretical — today's actual Tier-3 run used ~75K tokens to cover what the old protocol estimated at ~2000 tokens per venue × 40 venues = 80K tokens.

---

## Future Improvements

- **Auto-scraping:** Build a cron job to hit Odessa + Ecstatic Dance + Luma weekly
- **Venue API:** Contact major clubs for direct event feeds (Shelter, RADION, Fuse, C12)
- **Webhook alerts:** Subscribe to Meetup + Luma webhooks for new event notifications
- **Scan template:** Create daily-scan SKILL with this protocol built-in

---

## Last Updated
- **2026-05-19** — Protocol established after May 18-24 scan complete with 55 events + cost review
- **2026-05-20** — Revised. Moved RA.co city pages from monthly Tier 3 into weekly Tier 2a after Pentecost-week scan revealed 30+ missed events (Klub Dramatik 4-day festival in Antwerp, Audiodise Park / Mura Masa / Rainbow Disco Club, full Ghent gap). Cost model corrected — actual Tier-3 run cost ~€0.20, not €3.33.

