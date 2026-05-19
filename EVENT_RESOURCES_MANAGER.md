# Event Resources Manager
**Last Updated:** 2026-05-19  
**Maintained by:** Event Discovery Board scanning system  
**Purpose:** Master list of all event sources scanned weekly, with status, access notes, and update history

---

## 📋 Organization

Resources are organized by:
1. **Category** — Platform type or venue class
2. **City** — Geographic focus (Barcelona, Amsterdam, Antwerp, Brussels, Ghent)
3. **Status** — Active ✓, Stale ⚠, Defunct ✗
4. **Last Checked** — When we last pulled data from this source
5. **Access Method** — How to retrieve data (WebFetch, Chrome MCP, manual, API)

---

## 🔴 MANDATORY — Check Every Run

### PRYMAL Events
| Source | URL | City | Status | Last Checked | Access | Notes |
|--------|-----|------|--------|--------------|--------|-------|
| PRYMAL | https://www.prymal.world | Multi | ✓ | 2026-05-19 | WebFetch | Check /wildflow, /events pages. Always use prymal.world URL, never Eventbrite for PRYMAL events |

---

## 🎵 Resident Advisor (RA.co) — Venue Pages

**Access Method:** Chrome MCP + javascript_tool (RA uses full JS rendering)  
**Last Systematic Scan:** 2026-05-19

### Barcelona (9 venues)
| Venue | RA URL | Alt URL | Status | Last Checked |
|-------|--------|---------|--------|--------------|
| Luz de Gas (Nacar electronic) | https://ra.co/clubs/20462 | - | ✓ | 2026-05-19 |
| BORIS CLUB | https://ra.co/clubs/277679 | - | ✓ | 2026-05-19 |
| LAUT | https://ra.co/clubs/129273 | - | ✓ | 2026-05-19 |
| Nitsa Club | https://ra.co/clubs/2072 | - | ✓ | 2026-05-19 |
| Macarena Club | https://ra.co/clubs/3818 | - | ✓ | 2026-05-19 |
| Moog Club | https://ra.co/clubs/2253 | - | ✓ | 2026-05-19 |
| DETROIT CLUB | https://ra.co/clubs/163338 | - | ✓ | 2026-05-19 |
| Razzmatazz (main + The Loft) | https://ra.co/clubs/2071 | https://www.salarazzmatazz.com/agenda | ✓ | 2026-05-19 |
| La [2] de Apolo | - | https://www.sala-apolo.com/en/agenda | ✓ | 2026-05-19 |
| INPUT High Fidelity | - | https://www.inputbarcelona.com/ | ✓ | 2026-05-19 |
| City Hall Barcelona | - | https://www.cityhallbarcelona.com/ | ✓ | 2026-05-19 |
| Full BCN listing | https://ra.co/clubs/es/barcelona | - | ✓ | 2026-05-19 |

### Amsterdam (13 venues)
| Venue | RA URL | Alt URL | Status | Last Checked |
|-------|--------|---------|--------|--------------|
| Shelter | https://ra.co/clubs/124413 | - | ✓ | 2026-05-19 |
| RADION | https://ra.co/clubs/91202 | - | ✓ | 2026-05-19 |
| BRET | https://ra.co/clubs/108549 | - | ✓ | 2026-05-19 |
| Lovelee | https://ra.co/clubs/177303 | - | ✓ | 2026-05-19 |
| CONTACT | https://ra.co/clubs/225039 | - | ✓ | 2026-05-19 |
| Garage Noord | https://ra.co/clubs/172289 | https://garagenoord.com/agenda | ✓ | 2026-05-19 |
| De School | https://ra.co/clubs/115681 | - | ⚠ | 2026-05-19 |
| Marktkantine | https://ra.co/clubs/47892 | https://www.marktkantine.nl/ | ✓ | 2026-05-19 |
| Disco Dolly | https://ra.co/clubs/137068 | - | ✓ | 2026-05-19 |
| Thuishaven | https://ra.co/clubs/151456 | - | ✓ | 2026-05-19 |
| Doka (Volkshotel) | https://ra.co/clubs/86905 | - | ✓ | 2026-05-19 |
| Q-Factory | - | https://www.q-factory.nl/agenda | ✓ | 2026-05-19 |
| Paradiso Noord (Tolhuistuin) | - | https://paradiso.nl/agenda | ✓ | 2026-05-19 |
| Westerunie / Westergasterras | - | https://www.westergas.nl/agenda | ✓ | 2026-05-19 |
| Full AMS listing | https://ra.co/clubs/nl/amsterdam | - | ✓ | 2026-05-19 |

### Antwerp (5 venues)
| Venue | RA URL | Alt URL | Status | Last Checked |
|-------|--------|---------|--------|--------------|
| Ampere | https://ra.co/clubs/98955 | - | ✓ | 2026-05-19 |
| Club Vaag | https://ra.co/clubs/116869 | https://www.clubvaag.be/agenda | ✓ | 2026-05-19 |
| Petrol | https://ra.co/clubs/3097 | https://www.petrolclub.be/ | ✓ | 2026-05-19 |
| Trix | - | https://www.trixonline.be/agenda | ✓ | 2026-05-19 |
| Cinema Cartoons / Hangar 41 | - | https://www.hangar41.be/agenda | ✓ | 2026-05-19 |
| Full Antwerp listing | https://ra.co/clubs/be/antwerp | - | ✓ | 2026-05-19 |

### Brussels (5 venues — Added May 2026)
| Venue | RA URL | Alt URL | Status | Last Checked |
|-------|--------|---------|--------|--------------|
| Fuse Brussels | https://ra.co/clubs/2069 | https://www.fuse.be/agenda | ✓ | 2026-05-19 |
| C12 Brussels | https://ra.co/clubs/188107 | https://www.c12.brussels/agenda | ✓ | 2026-05-19 |
| Spirit of 66 | - | https://www.spiritof66.be/agenda | ✓ | 2026-05-19 |
| Recyclart | - | https://www.recyclart.be/agenda | ✓ | 2026-05-19 |
| Bonnefooi | - | https://www.bonnefooi.be/ | ✓ | 2026-05-19 |
| Full Brussels listing | https://ra.co/clubs/be/brussels | - | ✓ | 2026-05-19 |

### Ghent / Other Belgium (5 venues — Added May 2026)
| Venue | RA URL | Alt URL | Status | Last Checked |
|-------|--------|---------|--------|--------------|
| Kompass Klub | https://ra.co/clubs/154028 | https://www.kompassklub.com/agenda | ✓ | 2026-05-19 |
| Vooruit / VIERNULVIER | - | https://viernulvier.gent/agenda | ✓ | 2026-05-19 |
| Decadance | - | https://www.decadance.be/agenda | ✓ | 2026-05-19 |
| Charlatan | - | https://www.charlatan.be/agenda | ✓ | 2026-05-19 |
| La Rocca (Lier) | - | https://www.larocca.be/agenda | ✓ | 2026-05-19 |
| Full Ghent listing | https://ra.co/clubs/be/ghent | - | ✓ | 2026-05-19 |

---

## 🎪 Recurring Community Events — Specific Pages

| Event | URL | City | Frequency | Status | Last Checked |
|-------|-----|------|-----------|--------|--------------|
| Ecstatic Dance Amsterdam (Loods12) | https://www.ecstaticdanceamsterdam.com/ | Amsterdam | Tuesday evenings | ✓ | 2026-05-19 |
| Ecstatic Dance Amsterdam (The Other Side) | https://www.ecstaticdanceamsterdam.com/ | Amsterdam | Sunday mornings | ✓ | 2026-05-19 |
| The Conscious Club (Friday Ecstatic + Cacao) | https://theconsciousclub.com/events | Amsterdam | Monthly Friday | ✓ | 2026-05-19 |
| Odessa Amsterdam | https://odessa.amsterdam/ | Amsterdam | Multiple weekly | ✓ | 2026-05-19 |
| Barcelona Entrepreneurs (Tuesday + Friday) | https://www.meetup.com/pro/barcelona-entrepreneurs/ | Barcelona | Tue & Fri | ✓ | 2026-05-19 |
| AI Engineers Barcelona | https://www.meetup.com/ai-engineers-barcelona/ | Barcelona | Every Mon | ✓ | 2026-05-19 |

---

## 🌐 Platforms — Full Search Capability

### Meetup
| Region | Search URL | Access Method | Last Checked | Notes |
|--------|-----------|----------------|--------------|-------|
| Amsterdam (networking) | meetup.com/find/nl--amsterdam/business-entrepreneur-networking/ | Chrome MCP | 2026-05-19 | Filter for in-person, 20+ attendees |
| Barcelona (general) | meetup.com/find/events/?location=Barcelona | Chrome MCP | 2026-05-19 | Search: AI, networking, entrepreneurship |
| Antwerp (general) | meetup.com/find/events/?location=Antwerp | Chrome MCP | 2026-05-19 | Search: tech, networking |
| Brussels (general) | meetup.com/find/events/?location=Brussels | Chrome MCP | 2026-05-19 | Search: tech, entrepreneurship |

**Keywords to search:** "AI builders", "AI meetup", "professional networking", "startup networking", "founders meetup", "indie makers", "tech networking"

### Eventbrite
| Region | Search URL | Access Method | Last Checked | Notes |
|--------|-----------|----------------|--------------|-------|
| Amsterdam | eventbrite.com/d/netherlands--amsterdam/networking | WebFetch | 2026-05-19 | Filter by date range |
| Barcelona | eventbrite.com/d/spain--barcelona/networking | WebFetch | 2026-05-19 | Filter by date range |
| Antwerp | eventbrite.com/d/belgium--antwerp/events | WebFetch | 2026-05-19 | Filter by date range |

### Luma (lu.ma)
| Region/Focus | URL/Query | Access Method | Last Checked | Notes |
|--------|-----------|----------------|--------------|-------|
| Amsterdam (general) | lu.ma/search?q=amsterdam | WebSearch | 2026-05-19 | JS-rendered; use WebSearch for city-specific results |
| Barcelona (general) | lu.ma/search?q=barcelona | WebSearch | 2026-05-19 | JS-rendered; supplement with WebSearch |
| AI Builders Amsterdam | luma.com/aibuilders_amsterdam | WebFetch | 2026-05-19 | First Thursday monthly |
| Amsterdam Startup | luma.com/AmsterdamStartup | WebFetch | 2026-05-19 | Networking focus |
| Wellness (generic) | lu.ma/search?q=breathwork+OR+sound+bath+OR+ecstatic | WebSearch | 2026-05-19 | Search for wellness keywords |

### Hipsy
| Region | URL | Access Method | Last Checked | Notes |
|--------|-----|----------------|--------------|-------|
| Amsterdam | hipsy.eu | Chrome MCP + filter | 2026-05-19 | Client-side filters only; use Chrome or WebSearch |
| Barcelona | hipsy.eu | Chrome MCP + filter | 2026-05-19 | Client-side filters only; use Chrome or WebSearch |

### Shotgun (shotgun.live)
| Region/Genre | URL | Access Method | Last Checked | Notes |
|--------|-----|----------------|--------------|-------|
| Barcelona (techno) | shotgun.live/en/cities/barcelona/techno | WebFetch | 2026-05-19 | Limited coverage; city-only URL returns 404 |
| Barcelona (house) | shotgun.live/en/cities/barcelona/house | WebFetch | 2026-05-19 | Limited coverage |
| Antwerp (techno) | shotgun.live/en/cities/antwerp/techno | WebFetch | 2026-05-19 | Limited coverage |

---

## 📍 Local City Aggregators

### Antwerp
| Source | URL | Status | Last Checked | Access |
|--------|-----|--------|--------------|--------|
| MyGuide Antwerp | https://myguideantwerp.com/en/events | ✓ | 2026-05-19 | WebFetch |
| Uit in Vlaanderen (Antwerp filter) | https://www.uitinvlaanderen.be/agenda/c/antwerpen | ✓ | 2026-05-19 | WebFetch |

### Belgium (Broader)
| Source | URL | Status | Last Checked | Access |
|--------|-----|--------|--------------|--------|
| Uit met Muziek | https://uitmetmuziek.be | ✓ | 2026-05-19 | WebFetch |
| Visit Brussels | https://visit.brussels/en/article/events-in-brussels | ✓ | 2026-05-19 | WebFetch |
| Dansio | https://dansio.be/agenda | ✓ | 2026-05-19 | WebFetch |

### Amsterdam
| Source | URL | Status | Last Checked | Access |
|--------|-----|--------|--------------|--------|
| I Amsterdam | https://www.iamsterdam.com/en/whats-on/events | ✓ | 2026-05-19 | WebFetch |
| Amsterdam.nl Uitagenda | https://www.amsterdam.nl/uitagenda/ | ✓ | 2026-05-19 | WebFetch |

### Barcelona
| Source | URL | Status | Last Checked | Access |
|--------|-----|--------|--------------|--------|
| Time Out Barcelona | https://www.timeout.com/barcelona/events | ✓ | 2026-05-19 | WebFetch |
| BCN Mes | https://bcnmes.com/agenda | ✓ | 2026-05-19 | WebFetch |

---

## 📊 Resource Summary Stats
- **Total RA Venues:** 40+ (across 5 regions)
- **Meetup Groups:** 10+ (by city/category)
- **Platforms:** 6 (Meetup, Eventbrite, Luma, Hipsy, Shotgun, + RA)
- **Recurring Events:** 6 (tracked separately)
- **Local Aggregators:** 8 (by region)
- **Total Unique Sources:** 60+

---

## 📝 Update Log

| Date | Update | Reason |
|------|--------|--------|
| 2026-05-19 | Created Event Resources Manager | Systematic tracking for weekly scans |
| 2026-05-19 | Added all 40+ RA venues | MANDATORY source per daily-scan SKILL |
| 2026-05-19 | Added 6 recurring community events | Direct organizer pages (Ecstatic Dance, Conscious Club, Odessa, Barcelona Entrepreneurs, AI Engineers BCN) |
| 2026-05-19 | Documented 6 platforms | WebFetch vs. Chrome MCP vs. WebSearch access patterns |

---

## 🔄 Update Protocol

**When to update this document:**
1. **After every weekly scan** — Mark "Last Checked" dates as current
2. **When a source moves or breaks** — Mark status as ⚠ (stale) or ✗ (defunct), note what changed
3. **When Henk gives feedback** — Add notes ("Added May 2026 per feedback") like existing Brussels/Ghent entries
4. **When a new source is discovered** — Add it to the appropriate category with full details
5. **When access methods change** — Update "Access Method" column (e.g., "WebFetch moved to Chrome MCP")

**Who updates it:**
- Primary: Event Board scanning system (weekly)
- Secondary: Henk when discovering new resources or noting defunct sources

**Where it lives:**
`/Users/OPTIBIZ/Documents/Claude/Projects/Things to do - Planning Board/EVENT_RESOURCES_MANAGER.md`

---

## 🎯 Next Steps for Future Scans

1. **Open this document FIRST** before each scan
2. **Update "Last Checked" dates** as you process each section
3. **Note any sources that failed or returned no events**
4. **Add new discoveries** to the appropriate section with [Date Added] tag
5. **Mark dead sources** as ✗ and note when they died
