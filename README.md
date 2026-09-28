# infinite-craft-browser-db

Recipe database for an unreleased TamperMonkey userscript for
[Infinite Craft](https://neal.fun/infinite-craft/).

The script downloads these files into the browser (IndexedDB) and answers
merges/checks locally — no game-API requests for known pairs.

## Contents (v1, built 2026-09-28)

- `manifest.json` — version, build date, counts
- `elements.min.json` — 783,149 elements as compact `[id, name, emoji, cost]`
- `name_index.min.json` — 783,149 lowercase names → id
- `recipes/shard-000.json … shard-255.json` — 23,079,910 recipes as `[a, b, result]` (canonical `a <= b`, conflicts resolved: min cost, then min id)
- `byresult/000.json … 255.json` — reverse index `{resultId: [[a, b], …]}` for "how to make X"

Total ~710 MB on disk (served gzip-compressed over HTTPS).

## Layout contract

- Pair shard file: `recipes/shard-NNN.json`, `NNN = (min(a,b) * 1000003 + max(a,b)) % 256`, zero-padded to 3
- Result shard file: `byresult/NNN.json`, `NNN = resultId & 255`, zero-padded to 3
- Do not rename files or change the JSON shapes — the script addresses them directly.

## Source

Built from the full unified Infinite Craft recipe corpus
(`api_elements/unified`). Rebuild with `build_browser.cjs`.
