# NexGame — Release Tracker

Track upcoming game releases with a live days-until-release countdown. A single-file PWA — no build tools, no backend, no API keys required.

## Features

- **Track games** with release dates, type (DLC/Patch/EAR), status (Queued/Prequel/Link/Ready), hype level, price, and custom card backgrounds
- **Live countdown** for every tracked game
- **List & grid views** with grouping by month and alphabetical sorting
- **Filters** — filter by type, status, hype, price, and full-text search
- **Wikidata lookup** — search for any game title and auto-fill its release date and artwork
- **Bulk import** — paste a list of game titles and have each one looked up automatically
- **Dropbox sync** — optional sync across devices using your own Dropbox app (client-side OAuth2 PKCE, no external server)
- **Analytics** — release calendar, quarterly trends, lead-time distribution, and release-day patterns
- **Themes** — Nebula, Ocean, Sunset, Forest, Midnight
- **PWA** — installable, works offline, background updates
- **Backup & restore** — export/import your data as JSON

## Usage

Open `index.html` in a browser. That's it.

No server needed — localStorage keeps your data on-device. For cross-device sync, connect Dropbox in Settings > Sync.

## Tech

Pure HTML, CSS, and vanilla JS in a single file. Data lives in `localStorage`. Release dates come from [Wikidata](https://www.wikidata.org/) (free, no key). Dropbox sync uses the [Dropbox API](https://www.dropbox.com/developers) with PKCE OAuth2.
