# Steam Co-Review Network

Graph dataset connecting 82,000+ Steam games through shared user reviews. Two games share an edge when multiple users reviewed both.

Built from 128 million user reviews across 80,000 games (2012-2024), sourced from the [artermiloff Steam Reviews 2024](https://www.kaggle.com/datasets/artermiloff/steam-games-reviews-2024) dataset and [FronkonGames](https://huggingface.co/datasets/FronkonGames/steam-games-dataset) game metadata.

## Files

| File | Description | Size |
|------|-------------|------|
| `steam_network_full.json` | Full co-review graph (48K nodes, 33M weighted edges) | 1.4 GB |
| `steam_all_2005.json` | 82,928 game catalog with genres, tags, ratings | 6.2 MB |
| `steam_force_layout.json` | Genre-aware pre-computed layout positions (9K nodes) | 252 KB |

## Live Visualization

**[dr.eamer.dev/datavis/interactive/steam/](https://dr.eamer.dev/datavis/interactive/steam/)**

Four interactive Canvas-rendered views: universe scatter plot, chord diagram, force-directed network, and genre timeline.

## Pipeline

```bash
python3 build_network_full.py   # Scan 128M reviews, build co-review edges
python3 enrich_data.py          # Build game catalog from FronkonGames CSV
python3 compute_layout.py       # Genre-aware force layout (needs networkx)
```

## Platforms

- **GitHub**: [lukeslp/steam-network-data](https://github.com/lukeslp/steam-network-data)
- **Kaggle**: [lucassteuber/steam-universe-network](https://www.kaggle.com/datasets/lucassteuber/steam-universe-network)
- **HuggingFace**: [lukeslp/steam-co-review-network](https://huggingface.co/datasets/lukeslp/steam-co-review-network)

## License

CC-BY-4.0

## Author

Luke Steuber — [lukesteuber.com](https://lukesteuber.com)
