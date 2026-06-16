# Chicago Safety & Commute Atlas

An interactive dashboard that analyzes **475,651** reported Chicago crime incidents (June 2024–June 2026) to answer a single practical question: **where should someone relocating to Chicago for a job in the Loop live to balance safety with a reasonable commute?**

🔗 **Live site:** https://techwithvictoria.github.io/chicago-crime-dashboard/

---

## The business problem

A SkillBridge participant is relocating to Illinois for a role at **Krasan Consulting** (111 W Jackson Blvd, Suite 1700, in the Loop). Safety is their top priority, but they're new to Chicago and hearing conflicting claims about which areas are safe. This project turns the city's public crime data into a clear, defensible recommendation — and an interactive tool to vet any specific address.

## What it does

A single interactive map plus supporting charts that let you:

- **Shade the city** by violent crime or all crime, at the district, ward, or beat level
- **Drop incident icons or a hotspot heatmap** to see where crime physically clusters
- **Show live CTA / Metra transit** near any location (pulled from OpenStreetMap)
- **Search any home address** to see its ½-mile crime count, nearest station, and a side-by-side **transit vs. driving commute** to the Loop office (live driving route via OSRM, transit time via Google Maps)
- Read the analysis behind it: a quadrant scatter, district rankings, time-of-day patterns, day-of-week, and crime-type breakdowns

## Key findings

- **Edgewater (District 20)** is the safest district on **both** total crime (~420/month) and violent crime (~129/month).
- **Living closer to the office means more crime, not less** — the Loop (District 1) runs ~295 violent incidents a month, more than **2× Edgewater's**.
- **Total crime count alone is misleading.** Austin (District 15) has below-median total crime, yet **44% of its incidents are violent** (vs ~35% citywide) — the clearest example of why we rank on *violent* crime, not raw volume.
- **Time of day matters more than the day of week.** Crime is lowest overnight (~2–7 AM) and peaks midday through evening; day-of-week varies by only ~1 point, so it's too weak to act on.
- **No hard safety-vs-commute tradeoff** — several safe neighborhoods (Edgewater, Beverly, Jefferson Park, Albany Park, Logan Square) have a one-seat transit ride to the Loop.

## Recommendations

| Rank | Neighborhood | District | Commute to the Loop |
|------|--------------|----------|---------------------|
| 1 | Edgewater | 20 | Red Line direct, ~35 min |
| 2 | Beverly | 22 | Metra Rock Island → LaSalle St, ~25–30 min |
| 3 | Logan Square | 14 | Blue Line direct, ~20 min |
| 4 | Jefferson Park | 16 | Blue Line direct, ~40 min |
| 5 | Albany Park | 17 | Brown Line, ~45 min |

## Data

- **Source:** City of Chicago "Crimes – 2001 to Present" (Chicago Data Portal, dataset `ijzp-q8t2`), drawn from the Chicago Police CLEAR system.
- **Scope:** 475,651 incidents, June 2024–June 2026, across 22 police districts, 50 wards, and ~275 beats.
- **Violent crime** is defined as: arson, assault, battery, criminal sexual assault, homicide, human trafficking, intimidation, kidnapping, offenses involving children, robbery, sex offense, stalking, and weapons violation.
- Monthly averages are computed over the 24-month period. District 31 (a non-residential administrative district with only ~21 incidents) is excluded. Day-of-week is computed directly from each incident's date.

## How it was built

1. **Extract** — downloaded the source data and filtered to the analysis window.
2. **Clean & prepare** — standardized columns, split the timestamp into date / hour / day-of-week, and defined the violent-crime flag.
3. **Analyze** — aggregated by district, ward, beat, hour, day, and crime type; built the volume-vs-violence comparison.
4. **Map** — joined the crime metrics to official police-district and beat boundaries (ward outlines derived from the incident points).
5. **Build** — assembled a single self-contained HTML dashboard.

## Tech

- [Leaflet](https://leafletjs.com/) + CARTO basemap tiles for the map
- [Chart.js](https://www.chartjs.org/) for the charts
- [OpenStreetMap Nominatim](https://nominatim.org/) (geocoding) and [Overpass API](https://overpass-api.de/) (live transit stops)
- [OSRM](http://project-osrm.org/) for live driving routes
- Everything is baked into one `index.html` — no build step, no server.

## Running it locally

It's a single file. Either:

- Open `index.html` directly in a browser, **or**
- Serve it (recommended, so the map services behave):
  ```bash
  python -m http.server 8000
  # then visit http://localhost:8000
  ```

An internet connection is required, since the map tiles, geocoding, transit, and routing load from public services.

## Notes & limitations

- District rankings use **raw incident counts**, not per-capita rates (population by district isn't in the dataset), so very small or large districts could shift slightly under a per-resident measure.
- Transit travel **time** opens in Google Maps (live transit routing needs a paid API key); the in-app driving route and distance are computed live.
- This is an analytical tool, not a navigation app.

## License & attribution

Crime data © City of Chicago, provided under the [Chicago Data Portal Terms of Use](https://www.chicago.gov/city/en/narr/foia/data_disclaimer.html). Built for an educational data-analysis project.
