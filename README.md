# d2-conflux-data

Data snapshots for [D2 Conflux](https://d2conflux.com), published with GitHub Pages so the site can read them without using its own hosting.

## perk-popularity.json

How many copies of each legendary weapon carry each perk, and each left + right perk combo. A GitHub Action (`.github/workflows/crawl.yml`) runs every 3 hours and spends 2 hours reading players' profiles through Bungie's public API: new players found through PvE matches that ended in the last 15 minutes, and players already in the store it hasn't read for a week. Each read gives the perks on every legendary they have equipped, plus their vault and inventories when they've made them public, and when they last played.

Each copy counts once, by its item instance id, for as long as its owner keeps playing: a player who hasn't played for 14 days drops out with their copies, and a read that sees a player's whole vault drops the copies they no longer have. Those copies, and the players they belong to, are kept in a store: a binary file, too big for git, kept as an asset of the [`copies` release](../../releases/tag/copies) and rewritten by every run. Because it lists players' Bungie membership ids, the file is encrypted (AES-256-GCM) with the `POPULARITY_STORE_KEY` secret; only the Action can read it. `perk-popularity.json`, the store tallied, has no player in it.

The crawler and merge live in the app repo (`scripts/crawl-perk-popularity.ts`, `scripts/merge-perk-popularity.ts`), and the formats are described in `src/lib/weapons/perk-popularity.ts` and `src/lib/weapons/popularity-copies.ts` there. The app repo branch the Action runs is the `APP_REF` repository variable (default `main`).

Run it by hand from the Actions tab (Crawl perk popularity → Run workflow). It needs the `BUNGIE_CRAWLER_API_KEY` secret (an API key from a Bungie application with OAuth client type "Not Applicable" and no origin header) and the `POPULARITY_STORE_KEY` secret (32 random bytes, base64: `openssl rand -base64 32`). A new key can't read the old store, so changing it starts the store over.
