# AGENTS.md

## Repo structure

Single-file PWA. Everything lives in `index.html` (~5280 lines of embedded HTML, CSS, and JS). No build step, no bundler, no transpiler — just open `index.html` in a browser.

| File | Purpose |
|------|---------|
| `index.html` | Entire app (UI, styles, logic, data model) |
| `changelog.txt` | Plain-text changelog (all version entries) |
| `sw.js` | Service worker — offline cache only. Bump the `CACHE` string (e.g. `nexgame-v31`) whenever `index.html` changes. |
| `manifest.webmanifest` | PWA install manifest |
| `icon-192.png`, `icon-512.png` | App icons |

## Branches

- `main` — production / stable
- `dev` — active development (current default)

Commits are short imperative descriptions (never include version numbers). Each bug fix or feature change should be committed individually (one commit per fix). Never push to origin unless explicitly asked to.

## Key conventions inside `index.html`

- **Version:** `APP_VERSION` constant (line ~3841). Also referenced in `sw.js` cache key.
- **Dev banner:** `DEV_BUILD` boolean — shows/hides a red "Dev Build" banner. Set to `false` before shipping.
- **Data model:** `games` array — each game has fields: `id, name, date, tba, status, type, hype, price, priceEstimated, steamAppId, color, image, notes, reminder`.
- **Archived games:** separate `archived` array, same shape.
- **Persistence:** `localStorage` keys all prefixed `nexgame.`. `save()` serialises the `games` array; `saveArchived()` does the same for archived.
- **Restore / backup:** `applyBackupData()` calls `cleanGame()` on every entry — **add any new game field there** or it will be stripped on restore.
- **Modals/overlays:** each is a `.overlay` div toggled via `.classList.add('show')` / `.remove('show')`. Escape key handler (line ~3542) must include `closeNewOverlay()` if you add a new overlay.
- **Filter pickers:** each filter (Type, Status, Hype, Price) is a `<details>` dropdown with checkbox multi-select. Filter state persists to localStorage with its own key.
- **Toast notifications:** `toast(message)` — auto-dismiss after 2.6s.
- **SVG icons:** all hand-inlined. Global SVG rule (line ~18) sets `fill:none; stroke:currentColor; stroke-width:2`. Define new icons as JS template-literal constants.
- **CSS patterns:** `.card` (list view), `.tile` (grid view), `.cal-cell` (calendar). Action buttons are hidden by default, revealed on hover via `.actions { opacity: 0 }` -> `.card:hover .actions { opacity: 1 }`.
- **Wikidata / Steam lookups:** use `fetch()` with proxy fallback arrays (`PROXIES`). No API keys needed.
- **Bulk operations:** each gets its own overlay (HTML) + JS section (e.g. `steamBulkOverlay`, `bulkEditOverlay`). Pattern: close settings -> build rows -> render list -> show overlay -> apply -> toast -> close.

## Gotchas

- The file is one long script. CSS is in a `<style>` block, JS in a `<script>` block, HTML in the `<body>`. Line numbers shift frequently — always verify line numbers before editing.
- `sw.js` cache version must be bumped when `index.html` changes, otherwise users get stale PWA caches.
- `applyBackupData()` / `cleanGame()` must list every game field explicitly — missing a field silently strips it on restore.
- There is no linter, formatter, or test suite. Verify changes by opening `index.html` in a browser.
