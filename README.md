# Dearborn ALPR Surveillance Map

An interactive map visualizing Automated License Plate Recognition (ALPR) cameras and surveillance infrastructure in Dearborn, Michigan.

![Dearborn Surveillance Map](https://img.shields.io/badge/cameras-78-red) ![Police Stations](https://img.shields.io/badge/police%20stations-6-blue) ![Built with Astro](https://img.shields.io/badge/built%20with-Astro-purple)

## Overview

This project maps publicly documented surveillance cameras in the Dearborn, MI area using data from [OpenStreetMap](https://www.openstreetmap.org). Similar to projects like [DeFlock](https://deflock.me) and [LPR Maps](https://lprmaps.com), it provides transparency into the growing network of automated surveillance systems.

## Features

- **Interactive Mapbox map** with dark theme
- **78 ALPR cameras** with direction cones showing camera orientation
- **6 Police stations** in the surrounding area
- **Dearborn city boundary** overlay
- **Filter & search** by operator, manufacturer
- **Toggleable layers** - show/hide different data types
- **Data table view** with fly-to functionality
- **Mobile responsive** design
- **Click popups** with camera details (operator, manufacturer, mount type, power source)

## Data Sources

All data is sourced from [OpenStreetMap](https://www.openstreetmap.org) under the [ODbL license](https://opendatacommons.org/licenses/odbl/).

### Key OSM Tags Used

| Tag | Description |
|-----|-------------|
| `man_made=surveillance` | Surveillance device |
| `surveillance:type=ALPR` | Automated License Plate Recognition |
| `operator=*` | Operating organization |
| `manufacturer=*` | Camera manufacturer (Flock Safety, Motorola, etc.) |
| `direction=*` | Camera pointing direction (degrees) |
| `camera:mount=*` | Mount type (pole, wall, street_lamp) |

## Tech Stack

- **[Astro](https://astro.build)** - Static site framework
- **[Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/)** - Interactive mapping
- **[OpenStreetMap](https://www.openstreetmap.org)** - Data source
- **[Overpass API](https://overpass-api.de)** - OSM data queries

## Setup

### Prerequisites

- [Bun](https://bun.sh) or Node.js
- [Mapbox account](https://www.mapbox.com) (free tier works)

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/dearborn-surveillance-map.git
cd dearborn-surveillance-map

# Install dependencies
bun install

# Create environment file
cp .env.example .env

# Add your Mapbox token to .env
# PUBLIC_MAPBOX_TOKEN=pk.your_token_here

# Start development server
bun run dev
```

Open [http://localhost:4321](http://localhost:4321)

### Build for Production

```bash
bun run build
```

Output is in the `dist/` folder.

## Updating Data

To refresh camera data from OpenStreetMap:

1. Go to [Overpass Turbo](https://overpass-turbo.eu)
2. Run this query:

```
[out:json][timeout:120];

{{geocodeArea:Dearborn, Michigan, USA}}->.dearborn;

(
  node["man_made"="surveillance"](area.dearborn);
  way["man_made"="surveillance"](area.dearborn);
);

out body;
>;
out skel qt;
```

3. Export as GeoJSON
4. Replace `src/data/cameras.json`

## Contributing Data to OpenStreetMap

If you spot surveillance cameras not on the map, you can add them to OpenStreetMap:

1. Create an account at [openstreetmap.org](https://www.openstreetmap.org)
2. Use the iD editor or [JOSM](https://josm.openstreetmap.de)
3. Add a node with these tags:
   - `man_made=surveillance`
   - `surveillance:type=ALPR` (or `camera` for general CCTV)
   - `operator=*` (e.g., "Dearborn Police Department")
   - `manufacturer=*` (e.g., "Flock Safety")
   - `direction=*` (0-360 degrees from north)
   - `camera:mount=*` (pole, wall, street_lamp)

### Identifying Flock Safety Cameras

Flock Safety ALPR cameras are common and recognizable:
- Solar panel on top
- Small camera housing
- Usually mounted on dedicated poles
- Often at neighborhood entrances and major intersections

## Project Structure

```
dearborn-surveillance-map/
├── src/
│   ├── pages/
│   │   └── index.astro          # Main page
│   ├── components/
│   │   └── Map.astro            # Map component
│   ├── data/
│   │   ├── cameras.json         # ALPR camera data
│   │   ├── boundary.json        # Dearborn city boundary
│   │   └── police.json          # Police station data
│   └── styles/
│       └── global.css           # Styles
├── .env.example
├── astro.config.mjs
├── package.json
└── README.md
```

## Color Legend

| Color | Meaning |
|-------|---------|
| Red | Police department cameras |
| Orange | Private operators (retail, HOA) |
| Blue (dots) | University cameras |
| Blue (large) | Police stations |
| Red dashed line | City boundary |

## Privacy & Purpose

This project aims to increase transparency around public surveillance infrastructure. All data shown is already publicly documented in OpenStreetMap. The goal is to help residents understand the extent of automated surveillance in their community.

## License

- **Code**: MIT License
- **Map Data**: [OpenStreetMap](https://www.openstreetmap.org/copyright) - ODbL
- **Map Tiles**: [Mapbox](https://www.mapbox.com) - See their terms

## Acknowledgments

- [OpenStreetMap contributors](https://www.openstreetmap.org)
- [DeFlock](https://deflock.me) and [LPR Maps](https://lprmaps.com) for inspiration
- [EFF's Atlas of Surveillance](https://atlasofsurveillance.org)

---

**Note**: This is a civic transparency project. Camera locations shown are based on community-contributed OpenStreetMap data and may not be complete or fully accurate.
