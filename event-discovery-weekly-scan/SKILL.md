---
name: event-discovery-weekly-scan
description: |
  Execute the weekly Event Discovery Board scan using a three-tier model that puts RA.co city event pages into the weekly run, not the monthly one.

  **When to use:** Every Monday, or whenever the user asks to "run the weekly event scan", "update the events for this week", "refresh the event board", or "complete this week's scan".

  **What it does:**
  - **Tier 1 (weekly):** Fetches recurring community sources (Odessa, Ecstatic Dance, Conscious Club, Barcelona Entrepreneurs, AI Engineers Barcelona, AI Tinkerers Amsterdam)
  - **Tier 2a (weekly) — RA.co city pages:** Fetches Antwerp, Brussels, Ghent, Amsterdam, Barcelona event pages via Chrome MCP. This is the source of truth for club programming and the main change from the prior protocol.
  - **Tier 2b (weekly) — Meetup + Luma:** Keyword searches for wellness, networking, AI across the four primary cities.
  - **Tier 3 (ad-hoc, ~quarterly):** Deep venue-by-venue Chrome MCP sweep, only when RA city pages clearly missed something (e.g. a venue pulled its RA listing).
  - **Scoring & Merging:** Score new events (7.0+ minimum), merge into event_board_data.json, validate coverage.
  - **Digest & Git:** Generate/update the weekly digest with explicit coverage table, prepare and push the commit per the project's commit-and-push-after-every-update rule.

  **Token cost (realistic):** ~€1/month for Tier 1, ~€0.80/month for Tier 2a RA city pages, ~€0.10/month for Tier 2b Meetup/Luma, ~€0.07/month for occasional Tier 3 = **~€2/month sustainable**. Replaces both the prior "weekly Tier 1+2 + monthly Tier 3" model (which missed club programming three weeks out of four) and the old €15/week full-scan model.

compatibility:
  tools:
    - WebFetch
    - Chrome MCP (required for RA.co city pages and Meetup/Luma JS-rendered content)
  dependencies:
    - event_board_data.json (in /Users/OPTIBIZ/Documents/Claude/Projects/Things to do - Planning Board/)
    - EVENT_RESOURCES_MANAGER.md (reference for all sources)
    - WEEKLY_SCAN_PROTOCOL.md (reference for workflow details)
---

# Event Discovery Weekly Scan

## Overview

This skill automates the weekly event board scan using a revised three-tier model. The key change from the prior version: **RA.co city event pages run every week as part of Tier 2**, not monthly as Tier 3. This closes the club-programming gap that left Antwerp/Brussels/Ghent under-covered three weeks out of four.

The new model targets **~€2/month** for a complete weekly run.

---

## Tier 1: Recurring Community Sources (Weekly — ~5K tokens, ~€0.02/run)

Six direct-URL, high-signal sources. Check every Monday.

| Source | URL | Frequency | Category |
|--------|-----|-----------|----------|
| Odessa Amsterdam | https://odessa.amsterdam/ | Daily | wellness |
| Ecstatic Dance Amsterdam | https://www.ecstaticdanceamsterdam.com/ | Tue + Sun | wellness |
| The Conscious Club | https://theconsciousclub.com/events | Monthly Friday | wellness |
| Barcelona Entrepreneurs | https://www.meetup.com/pro/barcelona-entrepreneurs/ | Tue + Fri | networking |
| AI Engineers Barcelona | https://www.meetup.com/ai-engineers-barcelona/ | Every Mon | networking |
| AI Tinkerers Amsterdam | https://lu.ma/aibuilders | First Thu monthly | networking |

For each event found, build an event object using the schema in *"Updating event_board_data.json"* below.

---

## Tier 2a: RA.co City Event Pages (Weekly — ~75K tokens, ~€0.20/run) — NEW WEEKLY ITEM

This is the main change in the revised protocol. RA.co has one event page per city that lists upcoming events across all venues in that city. A single fetch surfaces 40–800 events. This replaces what the prior protocol called "Tier 3 monthly venue-by-venue sweep" — which was both too infrequent and unnecessarily expensive.

### How to do it

Use Chrome MCP (`navigate` + `get_page_text`) for each of these URLs in a single browser_batch where possible:

| City | URL | Typical upcoming count |
|------|-----|------------------------|
| Antwerp | https://ra.co/events/be/antwerp | ~75 |
| Brussels | https://ra.co/events/be/brussels | ~160 |
| Ghent | https://ra.co/events/be/ghent | ~45 |
| Amsterdam | https://ra.co/events/nl/amsterdam | ~700 |
| Barcelona | https://ra.co/events/es/barcelona | ~800 |

WebFetch returns shells on RA.co because the listings are JS-rendered — Chrome MCP is required.

### How to filter the output

The page text dumps every upcoming event for that city. To keep this manageable:

1. Filter to events whose date falls within `[today, week_end]`.
2. For Amsterdam and Barcelona, additionally filter to events with **≥100 RA attendee count** OR RA Pick designation — those two cities are too large to sweep top-to-bottom.
3. For Antwerp, Brussels, Ghent — include the full week's listings; volume is manageable.
4. Skip events already on the board (check `id` field).

### Scoring from RA

- **RA Pick** events: 8.5 baseline (raise to 8.7+ if also high attendance)
- **150+ attendees**: 8.0 baseline
- **50-149 attendees**: 7.5 baseline
- **<50 attendees, no Pick**: 7.0 baseline; include if it's at a venue we trust (Fuse, C12, Garage Noord, Shelter, Nitsa, LAUT, etc.) or has unique sound (RA Pick subgenres)

---

## Tier 2b: Meetup + Luma Keyword Searches (Weekly — ~5K tokens, ~€0.02/run)

Same as the prior protocol's Tier 2. Wellness, networking, AI keywords across the four primary cities.

**Amsterdam:** Search "ecstatic", "breathwork", "sound bath", "meditation", "conscious", "movement" on Meetup + Luma.

**Barcelona:** Same wellness keywords + entrepreneurship/product-management for the existing networking cluster.

**Antwerp / Brussels:** "tech", "AI", "startup", "wellness" — these cities are partially handled by RA Tier 2a now, but Meetup catches the non-club networking events RA doesn't list.

JS-rendered pages: use Chrome MCP if WebFetch returns shells.

---

## Tier 3: Ad-Hoc Deep Venue Sweep (Quarterly or as-needed — ~50K tokens, ~€0.20/run)

**When to run:** Not on a calendar. Trigger only when:

- RA's city page is suspiciously quiet for a venue you'd expect to have programming (e.g. Fuse on a Friday with no listing — go check fuse.be directly).
- A venue has pulled its RA listing or is using a different platform (e.g. some Brussels venues use Resident Advisor's competitor PaylogiC or their own ticketing).
- A new venue is announced and not yet aggregated by RA.

**How to do it:** Chrome MCP navigate to each venue's official agenda page. Extract dates + lineup. Add events directly to the board.

Default venue list (from EVENT_RESOURCES_MANAGER.md):
- Barcelona: Nitsa, LAUT, BORIS CLUB, Nacar, DETROIT CLUB, Moog, Razzmatazz, La [Apolo], Luz de Gas, City Hall
- Amsterdam: Shelter, RADION, BRET, Lovelee, Garage Noord, Marktkantine, Disco Dolly, Thuishaven, Paradiso, Q-Factory
- Antwerp: Ampere, Club Vaag, Petrol, Trix, Hangar 41 (note: Hangar 41 is a bar, not a club — verify)
- Brussels: Fuse, C12, Spirit of 66, Recyclart, Bonnefooi
- Ghent: Kompass Klub, Vooruit, Decadance

Don't run all 40+ in one pass. Pick the 3–5 that RA seemed quiet on this week and verify those.

---

## Completeness Checklist (Validate Before Finalizing)

| City | Target this-week count | Categories required |
|------|------------------------|---------------------|
| Amsterdam | ≥20 | ≥3 wellness, ≥2 networking, ≥1 hidden gem |
| Barcelona | ≥20 | ≥2 wellness (Odessa/Ecstatic Dance/Conscious Club + others), ≥3 networking, ≥1 hidden gem |
| Antwerp | ≥5 | ≥1 club/venue event from RA Tier 2a, ≥1 networking |
| Brussels | ≥10 | ≥1 wellness, ≥3 club events from RA Tier 2a |
| Ghent | ≥3 | ≥1 club event from RA Tier 2a (Ghent was at zero before the new model) |

If any city/category is below target, log what was checked and why — distinguish between *"not checked"* (a scan miss) and *"checked, genuinely no events this week"* (e.g. Club Vaag dark Pentecost weekend).

---

## Updating event_board_data.json

1. Open `/Users/OPTIBIZ/Documents/Claude/Projects/Things to do - Planning Board/event_board_data.json`
2. Update metadata: `lastUpdated`, `week_start`, `week_end`.
3. Append new events to the `events` array (do not delete old ones).
4. Schema for each event:
   ```json
   {
     "id": "unique-kebab-case-id",
     "name": "Event Name",
     "organizer": "Organizer Name",
     "date": "YYYY-MM-DD",
     "time": "HH:MM",
     "endTime": "HH:MM",
     "city": "Amsterdam|Barcelona|Antwerp|Brussels|Ghent|Houthalen-Helchteren",
     "venue": "Venue Name",
     "category": "wellness|music|networking|community",
     "subcategory": "ecstatic dance|breathwork|techno|house|AI builders|...",
     "cost": "€X-Y" or "Free" or "€?",
     "source": "Odessa|Meetup|Luma|Resident Advisor|venue site|...",
     "url": "https://...",
     "hook": "One-sentence appeal (what makes this event special)",
     "score": 7.0,
     "hidden_gem": true|false,
     "is_festival": true|false,
     "prymal": true|false,
     "reasons": ["alignment", "uniqueness", "community", "ra_pick", "networking", "free", ...]
   }
   ```

---

## Updating the Weekly Digest

1. Open / create `/Users/OPTIBIZ/Documents/Claude/Projects/Things to do - Planning Board/Weekly Digests/digest-YYYY-MM-DD.md`.
2. Sections (in order):
   - **Top 5 picks** (highest scores, 1-2 sentences each)
   - **Don't miss by city** (≥7.5 score, grouped by city)
   - **Hidden gems** (hidden_gem: true)
   - **Sources scanned** (count by source — explicit so completeness is auditable)
   - **Board status** with city/category tally
   - **Confirmed gaps & non-coverage** — list venues genuinely dark this week vs. venues not yet checked. This section is non-optional.

---

## Git Workflow

After updating event_board_data.json and digest:

```bash
cd "/Users/OPTIBIZ/Documents/Claude/Projects/Things to do - Planning Board/"
git add event_board_data.json "Weekly Digests/digest-*.md" EVENT_RESOURCES_MANAGER.md
git commit -m "Weekly scan [YYYY-MM-DD]: +X events across Y cities

Tier 1: [recurring sources checked]
Tier 2a (RA city pages): [list cities + counts found]
Tier 2b (Meetup/Luma): [keywords run]
Tier 3 (ad-hoc): [only mention if it ran]

City totals: AMS X, BCN X, ANT X, BRX X, GENT X"
git push origin main
```

Per the project's `feedback_github_push` rule, commit + push happens after every board update unless the user explicitly says to hold off.

---

## Success Criteria

A successful weekly run produces:

1. ✅ Tier 1 sources fetched (or each logged as "no events this week")
2. ✅ Tier 2a — all 5 RA city event pages read via Chrome MCP
3. ✅ Tier 2b — Meetup/Luma searches run for the four primary cities
4. ✅ Completeness checklist reviewed; any gaps explicitly logged as *not-checked* vs. *checked-and-empty*
5. ✅ event_board_data.json updated; JSON validates
6. ✅ Weekly digest written with the explicit coverage table and gaps section
7. ✅ Commit pushed to GitHub
8. ✅ Live board verified at https://prymal-digital.github.io/event-discovery-board/

**Time investment:** ~20–25 minutes per weekly run. The bulk of that is reading RA city page output and scoring events; the actual tool calls are <5 minutes.

---

## Cost Model (Revised)

| Tier | Cadence | Tokens/run | Cost/run | Per month |
|------|---------|------------|----------|-----------|
| 1 | Weekly | ~5K | ~€0.02 | ~€0.08 |
| 2a (RA city pages) | Weekly | ~75K | ~€0.20 | ~€0.80 |
| 2b (Meetup/Luma) | Weekly | ~5K | ~€0.02 | ~€0.08 |
| 3 (ad-hoc venue sweep) | ~quarterly | ~50K | ~€0.20 | ~€0.07 |

**Total: ~€1/month** at the floor, **~€2/month** with normal Tier 3 trigger rate. Well inside the €2-5/month ceiling.

---

## Why this is different from the prior version

The prior protocol claimed the same ~€4/month figure but produced an incomplete board three weeks out of four, because club programming only came from the monthly Tier 3. This revision moves club coverage into the weekly Tier 2 by switching from venue-by-venue scraping to RA.co city event pages — a single page per city that aggregates all listed venues. Faster, cheaper, more current, and weekly.

The old Tier 3 is preserved as an ad-hoc tool for when RA misses something specific, not as a calendar event.

---

## Notes

- Chrome MCP is required (not optional) — WebFetch returns empty shells on RA.co, Meetup, and Luma because their content is JS-rendered.
- Score events on the basis of *alignment with Henk's clusters* (conscious/wellness, music, networking, community) plus signal quality (RA Pick, attendee count, unique venue). The rubric is a guide, not a hard rule.
- Recurring events (Odessa, Ecstatic Dance, etc.) are high-signal — include them weekly even if the same facilitator runs them every week.
- If a venue / city is dark for a specific week (e.g. Club Vaag during Pentecost weekend 2026), log that explicitly in the digest's gaps section. *Dark for a real reason* is not the same as *unchecked*.
