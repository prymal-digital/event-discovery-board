# Weekly Event Discovery Scan Protocol
**Established:** 2026-05-19  
**Purpose:** Efficient, sustainable scanning ensuring completeness without excessive token consumption

---

## Three-Tier Scanning Model (Sustainable Approach)

### Tier 1: Recurring Community Events (EVERY WEEK — Cheap, ~50 tokens)
**Always pull from these direct organizer pages — events are usually posted 1-2 weeks ahead:**

| Event | URL | Frequency |
|-------|-----|-----------|
| Odessa Amsterdam | https://odessa.amsterdam/ | Daily |
| Ecstatic Dance Amsterdam | https://www.ecstaticdanceamsterdam.com/ | Tue + Sun |
| The Conscious Club | https://theconsciousclub.com/events | Monthly |
| Barcelona Entrepreneurs | https://www.meetup.com/pro/barcelona-entrepreneurs/ | Tue + Fri |
| AI Engineers Barcelona | https://www.meetup.com/ai-engineers-barcelona/ | Every Mon |
| AI Tinkerers Amsterdam | https://lu.ma/aibuilders | First Thu monthly |

**Method:** WebFetch each page, check for dates in target week. Takes 5-10 min, ~30-40 tokens.

---

### Tier 2: Platform Automated Alerts (WEEKLY — Free/Cheap, ~100 tokens)
**Check Meetup + Luma — these platforms email/notify closest to event dates**

**Meetup Keywords (search each region):**
- Amsterdam: "AI", "networking", "startup", "founders", "tech"
- Barcelona: "AI", "entrepreneurship", "product management", "builders"
- Antwerp: "tech", "startup", "networking"
- Brussels: "tech", "startup", "AI"

**Luma Searches (WebSearch):**
- `lu.ma/search?q=amsterdam+network`
- `lu.ma/search?q=barcelona+wellness`
- `lu.ma/search?q=tech+antwerp`

**Method:** WebFetch/WebSearch these pages weekly. Events appear 2-3 weeks out.

---

### Tier 3: Full Venue Scan (MONTHLY, not weekly — ~2,000 tokens, block for once/month)
**Once per month (e.g., last Sunday of month), systematically visit RA.co venue pages + club websites**

**Venues to scan (40+):**
- Barcelona: Nitsa, LAUT, BORIS CLUB, Nacar, DETROIT CLUB, Moog, Razzmatazz, La Apolo, Luz de Gas, City Hall
- Amsterdam: Shelter, RADION, BRET, Lovelee, Garage Noord, Marktkantine, Disco Dolly, Thuishaven, Paradiso, Q-Factory
- Antwerp: Ampere, Club Vaag, Petrol, Trix, Hangar 41
- Brussels: Fuse, C12, Spirit of 66, Recyclart, Bonnefooi
- Ghent: Kompass Klub, Vooruit, Decadance

**Method:** Chrome MCP + javascript_tool to extract events 2-3 weeks out, build event objects, merge into database.

**Timing:** Run last Sunday of month to capture events for weeks 2-5 of next month. This prevents weekly re-scans.

---

## Weekly Workflow (EST. 20 mins, ~150 tokens)

1. **Open EVENT_RESOURCES_MANAGER** — Check "Last Checked" dates
2. **Run Tier 1** (recurring community events)
   - Odessa: WebFetch, extract this week + next week events
   - Ecstatic Dance: WebFetch, check Tue/Sun schedules
   - All others: Quick WebFetch + manual note
3. **Run Tier 2** (Meetup/Luma keyword searches)
   - 5 WebSearch queries for major cities
   - Scan for new events in target week + future weeks
4. **Merge into event_board_data.json**
   - Check for duplicates by id + name
   - Validate dates within [week_start, week_end] + 2 weeks ahead
   - Score events
5. **Update digest** if >2 new events
6. **Push to GitHub** with message: "Weekly scan: [X] new events from [sources]"
7. **Mark EVENT_RESOURCES_MANAGER "Last Checked"** with today's date

---

## Monthly Workflow (TIER 3 — Last Sunday)

1. **Load all 40+ RA.co venue pages** using Chrome MCP
2. **Extract events** from 2-4 weeks out (will be booked by then)
3. **Build event objects** following schema
4. **Merge into JSON** (validate, deduplicate)
5. **Update digest** with full refresh
6. **Push to GitHub** with message: "Monthly full venue scan: [X] new events"
7. **Update EVENT_RESOURCES_MANAGER** "Last Checked" dates

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

## Token Budget

**Weekly cost (Tier 1 + 2):** ~150 tokens = €0.25
**Monthly cost (Tier 3):** ~2,000 tokens = €3.33
**Quarterly cost (FULL rescans if needed):** ~5,000 tokens = €8.33

**Total sustainable:** ~€12/month vs. €15/week for old approach = **80% cost reduction**

---

## Future Improvements

- **Auto-scraping:** Build a cron job to hit Odessa + Ecstatic Dance + Luma weekly
- **Venue API:** Contact major clubs for direct event feeds (Shelter, RADION, Fuse, C12)
- **Webhook alerts:** Subscribe to Meetup + Luma webhooks for new event notifications
- **Scan template:** Create daily-scan SKILL with this protocol built-in

---

## Last Updated
2026-05-19 — Protocol established after May 18-24 scan complete with 55 events + cost review

