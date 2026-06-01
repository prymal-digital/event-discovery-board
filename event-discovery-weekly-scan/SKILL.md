---
name: event-discovery-weekly-scan
description: |
  Run the weekly Event Discovery Board scan + digest. Use when the user says "run the weekly event scan", "update the events for this week", "refresh the event board", or "complete this week's scan".

  All protocol details — tiers, sources, scoring, digest path, bg rotation, git workflow — live in `WEEKLY_SCAN_PROTOCOL.md` at the project root. Read that file and execute what it says.
compatibility:
  tools:
    - WebFetch
    - Chrome MCP (required for RA.co, Meetup, Luma)
---

# Event Discovery Weekly Scan

This skill is a thin pointer. The single source of truth is
**`/Users/OPTIBIZ/Documents/Claude/Projects/Things to do - Planning Board/WEEKLY_SCAN_PROTOCOL.md`**.

Read it, then execute it. Do not duplicate instructions in this file or anywhere else — update only the protocol.
