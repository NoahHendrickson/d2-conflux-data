# d2-conflux-data

Data snapshots for [D2 Conflux](https://d2conflux.com), published with GitHub Pages so the site can read them without using its own hosting.

## perk-popularity.json

How many copies of each legendary weapon carry each perk, and each left + right perk combo. A GitHub Action (`.github/workflows/crawl.yml`) runs every 3 hours and spends 25 minutes finding players through PvE matches that ended in the last 15 minutes, through Bungie's public API. For each player, one profile read gives the perks on every legendary they have equipped, plus their vault and inventories when they've made them public.

Each copy counts once, by its item instance id, and drops out once it hasn't been seen for 14 days. Those copies are kept in a copy store: a gzipped binary file, too big for git, kept as an asset of the [`copies` release](../../releases/tag/copies) and rewritten by every run. `perk-popularity.json` is that store tallied. Nothing identifies a player: the store has instance ids and perks, all of it public on the profile it came from.

The crawler and merge live in the app repo (`scripts/crawl-perk-popularity.ts`, `scripts/merge-perk-popularity.ts`), and the formats are described in `src/lib/weapons/perk-popularity.ts` and `src/lib/weapons/popularity-copies.ts` there. The app repo branch the Action runs is the `APP_REF` repository variable (default `main`).

Run it by hand from the Actions tab (Crawl perk popularity → Run workflow). It needs the `BUNGIE_CRAWLER_API_KEY` secret: an API key from a Bungie application with OAuth client type "Not Applicable" and no origin header.
