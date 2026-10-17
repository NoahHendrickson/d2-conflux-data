# d2-conflux-data

Data snapshots for [D2 Conflux](https://d2conflux.com), published with GitHub Pages so the site can read them without using its own hosting.

## perk-popularity.json

How often players run each perk on legendary weapons, split by PvE and PvP. A weekly GitHub Action (`.github/workflows/crawl.yml`) samples recently active players through Bungie's public API: recent post-game reports give the players, and their profiles give the perks on the weapons they had equipped. Only the tallies are kept; nothing identifies a player.

The crawler lives in the app repo at `scripts/crawl-perk-popularity.ts`, and the format is described in `src/lib/weapons/perk-popularity.ts` there.

Run it by hand from the Actions tab (Crawl perk popularity → Run workflow). It needs the `BUNGIE_CRAWLER_API_KEY` secret: an API key from a Bungie application with OAuth client type "Not Applicable" and no origin header.
