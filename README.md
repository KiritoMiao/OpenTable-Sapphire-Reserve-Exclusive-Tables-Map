# OpenTable Sapphire Reserve Exclusive Tables Map

Interactive map of all Chase Sapphire Reserve Exclusive Tables restaurants across the US.

**[View Live Map](https://csr-et.kirito.moe/)**

## Features

- Interactive map with all Sapphire Reserve restaurants
- Filter by city and cuisine type
- Click-to-zoom with detailed popups and OpenTable links
- Marker clustering for dense areas
- Responsive layout (desktop + mobile)
- Auto-discovers all cities from OpenTable (currently 50+ cities)

## Usage

```bash
npm install          # Install Puppeteer
npm run scrape       # Scrape all cities; reuse saved coordinates
npm run build        # Generate index.html from data
npm run update       # Both in one step
```

The scraper automatically discovers all available cities from OpenTable's city selector dropdown — no hardcoded city list.

## How it works

1. **`scrape.mjs`** — Uses Puppeteer to visit OpenTable's Sapphire Reserve pages, discovers all cities via the city selector dropdown, scrapes restaurant data (name, cuisine, neighborhood, description, URL), handles pagination, and geocodes locations via Nominatim. Saved coordinates in `data/restaurants.json` are reused for the same restaurant URL, city, and neighborhood, preserving manual corrections. Only new or unresolved locations need geocoding; repeated queries are cached within each run. Requests are sequential, spaced by at least 1.1 seconds, and time out after 10 seconds.
2. **`build.mjs`** — Reads `data/restaurants.json` and generates a standalone `index.html` with Leaflet.js map, auto-generating city colors and filter UI
3. **GitHub Actions** — Runs weekly on Ubuntu 24.04 with Node.js 22 to auto-update the data and deploy to GitHub Pages. The scrape step allows 60 minutes for an initial run without saved coordinates; subsequent runs reuse the committed data.

## Tech Stack

- [Leaflet.js](https://leafletjs.com/) + [OpenStreetMap](https://www.openstreetmap.org/) tiles (no API key required)
- [Puppeteer](https://pptr.dev/) for scraping
- [Nominatim](https://nominatim.org/) for geocoding
- GitHub Pages for hosting
