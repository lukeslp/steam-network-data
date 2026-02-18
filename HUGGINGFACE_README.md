---
license: cc-by-4.0
task_categories:
  - graph-ml
  - feature-extraction
language:
  - en
tags:
  - steam
  - games
  - network-analysis
  - co-review
  - graph-data
  - gaming
  - social-network
pretty_name: Steam Co-Review Network
size_categories:
  - 1M<n<10M
dataset_info:
  features:
    - name: nodes
      dtype: list
    - name: links
      dtype: list
    - name: meta
      dtype: dict
---

# Steam Co-Review Network

Two Steam games share an edge when multiple users reviewed both. Built from 128 million user reviews across 80,000 games (2012-2024), this dataset maps how the Steam catalog is connected through player overlap.

## Files

### steam_network_full.json

The complete co-review graph with minimal filtering:

- 48,362 game nodes (every game with 10+ reviews)
- 33,041,298 weighted edges (2+ shared reviewers per pair)
- Per-node cap of 50 neighbors (densest connections preserved)
- Edge weights range from 2 to 420,410 shared reviewers

**Node format:**
```json
{
  "id": "620",
  "title": "Portal 2",
  "year": "2011",
  "rating": "Overwhelmingly Positive",
  "ratio": 97,
  "reviews": 263842,
  "price": 9.99
}
```

**Link format:**
```json
{"source": 0, "target": 42, "weight": 1523}
```

Source and target are indices into the nodes array. Weight is the number of users who reviewed both games.

### steam_all_2005.json

82,928 games released 2005-2025. Packed as arrays for compact JSON:

```
[name, year, ratio, reviews, price, ratingIdx, genreIdxs, tagIdxs, developer]
 [0]    [1]   [2]    [3]      [4]    [5]        [6]        [7]       [8]
```

Genres and tags are stored as index arrays referencing top-level `genres[]` and `tags[]` lookup tables in the same file.

### steam_force_layout.json

Genre-aware pre-computed layout positions for the top ~9K nodes, clustered by primary genre with hub games as anchors. Use as warm-start coordinates for force-directed visualization.

## Sources

- **Game metadata**: [FronkonGames Steam Games Dataset](https://huggingface.co/datasets/FronkonGames/steam-games-dataset) — Jan 2026 snapshot, 122K games
- **User reviews**: [artermiloff Steam Reviews 2024](https://www.kaggle.com/datasets/artermiloff/steam-games-reviews-2024) — 128M reviews across 80K games, one CSV per game, 2012-June 2024

## Pipeline

1. Load game metadata from FronkonGames enriched CSV
2. Scan 30K+ per-game review CSVs, extract steamid-to-game mappings
3. For each user who reviewed 2+ games, generate all game pairs
4. Count shared reviewers per pair to produce edge weights
5. Filter: minimum 2 shared reviewers (no neighbor cap)

Full pipeline: [github.com/lukeslp/steam-network-data](https://github.com/lukeslp/steam-network-data)

## Use Cases

- **Graph ML**: Node classification (predict genre/rating from network position), link prediction, community detection
- **Recommendation systems**: Games connected by high edge weights share audiences
- **Market analysis**: Which genres cluster together? Where are the gaps?
- **Visualization**: Force-directed layouts, chord diagrams, genre timelines of the Steam ecosystem

## Live Visualization

[dr.eamer.dev/datavis/interactive/steam/](https://dr.eamer.dev/datavis/interactive/steam/)

Four interactive Canvas-rendered views: universe scatter, chord diagram, force-directed network, and genre timeline.

## Distribution

- **GitHub**: [lukeslp/steam-network-data](https://github.com/lukeslp/steam-network-data)
- **Kaggle**: [lucassteuber/steam-universe-network](https://www.kaggle.com/datasets/lucassteuber/steam-universe-network)

## Author

Luke Steuber — [lukesteuber.com](https://lukesteuber.com) — [@lukesteuber.com on Bluesky](https://bsky.app/profile/lukesteuber.com)
