# Event Board — how it works (reference)

_Last set up: 2026-06-22. One process, one calendar, one repo._

## The pieces

| Thing | Where | Role |
|-------|-------|------|
| **Live board** | https://prymal-digital.github.io/event-discovery-board/ | GitHub Pages site. Fetches the data file on load and re-renders. |
| **The calendar** | `event_board_data.json` | Single source of event data. **This is the only file that changes the board.** |
| **The digest** | `Weekly Digests/digest.md` | Single human-readable weekly brief (overwritten each week; git history is the archive). |
| **The UI** | `index.html` | Never edited by the scan. Reads the JSON. |
| **The protocol** | `WEEKLY_SCAN_PROTOCOL.md` | Single source of truth for how the scan runs (sources, scoring, schema, git). |
| **Manual trigger** | `event-discovery-weekly-scan/` skill | Run on demand; it just delegates to the protocol. |
| **The repo** | `prymal-digital/event-discovery-board` | One repo. The board folder is its working tree. |

## The one process

Scheduled task **`event-board-weekly-digest`** runs **Mondays ~07:43**. In one run it: scans (Tier 1 recurring + RA city pages via Chrome) → updates `event_board_data.json` → writes `digest.md` → makes **one commit + push of those two files** from a fresh `/tmp` clone. It never touches `index.html`.

Pushing the **data file** is what updates the board — pushing only the digest does nothing visible. (That was the old bug.)

## Tokens (two places, by necessity)

- **Manual pushes from this Mac:** stored in the macOS keychain (`credential.helper osxkeychain`). git asks once, remembers after.
- **The automated Monday task:** can't reach the keychain (runs sandboxed), so the token lives in its clone URL inside `~/Documents/Claude/Scheduled/event-board-weekly-digest/SKILL.md`. Keep that file private; it's not in the repo.

Use a **no-expiry** PAT with `repo` scope so this doesn't silently break again. If a token ever leaks, revoke it on GitHub and update both places.

## Troubleshooting

- **`fatal: Unable to create '.git/index.lock'` / `HEAD.lock`** — stale lock from a crashed run. Fix: `find .git -name '*.lock' -delete` then retry.
- **`push rejected (fetch first)`** — the remote moved. Integrate keeping your scan: `git merge -X ours origin/main -m "merge" && git push`.
- **Auth failed** — token expired/typo (watch for a doubled `ghp_` prefix). Regenerate, then `git remote set-url origin https://github.com/prymal-digital/event-discovery-board.git` and let the keychain prompt take it.
- **Board still shows old data after a successful push** — GitHub Pages CDN cache; wait a few minutes and hard-refresh (Cmd+Shift+R). Verify the true content at `https://raw.githubusercontent.com/prymal-digital/event-discovery-board/main/event_board_data.json`.

## Publishing manually (if you ever need to)

```bash
cd ~/Documents/Claude/Projects/"Things to do - Planning Board"
find .git -name '*.lock' -delete
git add event_board_data.json "Weekly Digests/digest.md"
git commit -m "Manual update $(date +%F)"
git push origin main
```

_`CLEANUP_PLAN.md` was a one-off helper — safe to delete now that the consolidation is done._
