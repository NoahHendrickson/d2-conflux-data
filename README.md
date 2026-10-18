# d2-conflux-data

Data snapshots for [D2 Conflux](https://d2conflux.com), published with GitHub Pages so the site can read them without using its own hosting.

## perk-popularity.json

How often players run each perk, and each left + right perk combo, on legendary weapons in PvE. A daily GitHub Action (`.github/workflows/crawl.yml`) samples about 20,000 players from the last 24 hours of PvE matches through Bungie's public API: post-game reports give the players, and their profiles give the perks on the weapons they used in that match. Each day is saved in `daily/`, and `perk-popularity.json` adds up the last 7 days. Only the tallies are kept; nothing identifies a player.

The crawler and merge live in the app repo (`scripts/crawl-perk-popularity.ts`, `scripts/merge-perk-popularity.ts`), and the format is described in `src/lib/weapons/perk-popularity.ts` there. The app repo branch the Action runs is the `APP_REF` repository variable (default `main`).

Run it by hand from the Actions tab (Crawl perk popularity → Run workflow). It needs the `BUNGIE_CRAWLER_API_KEY` secret: an API key from a Bungie application with OAuth client type "Not Applicable" and no origin header.

`daily/2026-10-17.json` is the first crawl (50,000 players, matches from Oct 16–17), from before combos were counted.
