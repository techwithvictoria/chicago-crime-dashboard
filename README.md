# Chicago Safety & Commute Atlas

An interactive dashboard that analyzes **475,651** reported Chicago crime incidents (June 2024–June 2026) to answer one practical question: **where should someone relocating to Chicago for a job in the Loop live to balance safety with a reasonable commute?**

🔗 **Live site:** https://techwithvictoria.github.io/chicago-crime-dashboard/

---

## Table of contents
- [The problem](#the-problem)
- [Key findings](#key-findings)
- [Recommendations](#recommendations)
- [How to use the dashboard](#how-to-use-the-dashboard)
- [The 10 questions answered](#the-10-questions-answered)
- [Data & method](#data--method)
- [A data-quality issue I caught (and fixed)](#a-data-quality-issue-i-caught-and-fixed)
- [Tech](#tech)
- [Running it locally](#running-it-locally)
- [Limitations](#limitations)

---

## The problem

A SkillBridge participant is relocating to Illinois for a role at **Krasan Consulting** (111 W Jackson Blvd, Suite 1700, in the Loop). Safety is their top priority, but they're new to Chicago and hearing conflicting claims about which areas are safe. This project turns the city's public crime data into a clear, defensible recommendation - plus an interactive tool to vet any specific address against both safety and commute.

## Key findings

- **Edgewater (District 20) is the safest district** on **both** total crime (~420/month) and violent crime (~129/month).
- **Living closer to the office means *more* crime, not less.** The Loop (District 1) runs ~295 violent incidents a month - more than **2× Edgewater's**.
- **Total crime count alone is misleading.** Austin (District 15) has below-median *total* crime, yet **44% of its incidents are violent** (vs ~35% citywide). It's the clearest reason we rank on *violent* crime, not raw volume.
- **Time of day matters more than day of week.** Crime is lowest overnight (~2–7 AM) and peaks midday through evening; days of the week differ by only ~1 percentage point, so the day you go out doesn't meaningfully change risk.
- **No hard safety-vs-commute tradeoff exists** - several safe neighborhoods have a one-seat transit ride to the Loop.

## Recommendations

| Rank | Neighborhood | District | Violent/mo | Commute to the Loop |
|------|--------------|----------|-----------|---------------------|
| 1 | Edgewater | 20 | ~129 | Red Line direct, ~35 min |
| 2 | Beverly | 22 | ~213 | Metra Rock Island → LaSalle St, ~25–30 min |
| 3 | Logan Square | 14 | ~169 | Blue Line direct, ~20 min |
| 4 | Jefferson Park | 16 | ~192 | Blue Line direct, ~40 min |
| 5 | Albany Park | 17 | ~174 | Brown Line, ~45 min |

*Top pick is Edgewater for all-around safety; Beverly is the easiest commute.*

---

## How to use the dashboard

The page scrolls top-to-bottom: an **interactive map** first, then the **charts and analysis** that back up the findings.

### The interactive map (Section 01)

The control panel on the right of the map drives everything:

- **Tentative home address** - type any Chicago address (e.g., *5300 N Clark St*) and click **Find**. The map drops a 🏠 pin and the panel fills with a full commute + safety readout (see below).
- **Boundary** - switch the shaded areas between **Districts / Wards / Beats / None**. District numbers are labeled directly on the map when Districts is selected.
- **Color by** - shade each area by **Violent crime** or **All crime** (per-area monthly averages). A 5-color scale runs green (safest) → red (highest).
- **Incidents - Off / On** - drop individual crime icons (clustered), filterable by crime type, violent-only, and time of day.
- **Hotspots - Off / All / Violent** - a heatmap showing where incidents physically cluster.
- **Transit - CTA / Metra (live), Off / On** - plots live train/subway stations from OpenStreetMap.

**Hover** any area for a quick readout (its level badge, total/mo, violent/mo, violent %). **Click** for a detailed popup. The 📍 Krasan HQ pin shows its label when you hover over it.

### The address readout (the standout feature)

After you search an address, the panel shows a **commute card**:

- **Best** - a recommendation that picks the smartest mode for *that* address: **Walk** if it's close (≤ ~1.2 mi), otherwise **Transit**.
- **Driving (live)** - real road distance and time, with the route drawn in blue on the map.
- **Walking (est.)** - estimated walk time and distance.
- **Transit** - opens the live transit route and time in Google Maps.
- **Nearest station** - the closest CTA/Metra stop and how far it is.
- **Crime within ½ mi** - the count of incidents in a half-mile radius (also drawn as a dashed circle on the map).

> **Why driving can look longer than walking:** Chicago's one-way streets force cars onto a longer legal path, so a 0.3-mile walk can be a 1+-mile drive. That's exactly why the "Best" recommendation suggests walking for nearby addresses.

### The charts (Sections 02–04)

- **Headline tiles** - the three big takeaways (safest district, 2× downtown, the Austin trap).
- **Violent crime per month, by district** - the safety ranking.
- **Crime volume vs. violent-crime rate (quadrant scatter)** - every district plotted by total volume (x) and violent share (y), split at the median into four color-coded zones: **Safest**, **Deceptive – Austin trap**, **Busy but less violent**, and **Highest risk**. This is the chart that proves total crime alone is misleading.
- **Crime by hour of day** - with the safe window (2–7 AM) and busy daytime peak highlighted. *(The midnight bar is flagged as a data artifact - unknown-time reports default to 00:00.)*
- **Crime by day of week** - nearly flat; Friday slightly highest.
- **Crime types** - Theft (23%), Battery (18%), Criminal Damage (11%), Assault (9%), Motor Vehicle Theft (8%) lead.

---

## The 10 questions answered

1. Which district has the lowest **total** crime per month? → Edgewater (D20)
2. Which has the lowest **violent** crime per month? → Edgewater (D20)
3. Is total crime a reliable safety measure? → No - see Austin (D15)
4. What time of day is risk lowest? → Overnight, ~2–7 AM
5. What are the most common crime types? → Theft, battery, criminal damage
6. Which safe neighborhoods also have a reasonable commute? → Edgewater, Beverly, Logan Square, Jefferson Park, Albany Park
7. Is there a transit-vs-commute tradeoff? → No - safe areas have one-seat rides
8. Does living closer to the office raise violent exposure? → Yes - the Loop is ~2× Edgewater
9. Is day-of-week a strong enough signal to act on? → No - ~1-point spread
10. Which areas are low on **both** volume and violence? → The "Safest" quadrant (D20, D14, D16, D17, D22)

---

## Data & method

- **Source:** City of Chicago "Crimes – 2001 to Present" (Chicago Data Portal, dataset `ijzp-q8t2`), from the Chicago Police CLEAR system.
- **Scope:** 475,651 incidents, June 2024–June 2026, across 22 police districts, 50 wards, and ~250 beats.
- **Monthly averages** are computed over the 24-month period.
- **District 31 is excluded** - it's a small non-residential administrative district (~21 incidents) that would distort comparisons.
- **Violent crime** is defined as these 13 categories: *arson, assault, battery, criminal sexual assault, homicide, human trafficking, intimidation, kidnapping, offenses involving children, robbery, sex offense, stalking, weapons violation.*
- **Day of week** is computed directly from each incident's date (see the data-quality note below).

**Pipeline:** extract → clean & standardize columns → flag violent crime → aggregate by district / ward / beat / hour / day / type → join metrics to official police-district and beat boundaries (ward outlines derived from the incident points) → render as a single self-contained HTML file.

## A data-quality issue I caught (and fixed)

The raw export shipped with a `DayOfWeek` column that was **misaligned with the actual dates** - only ~24% of rows matched the weekday of their own timestamp (the column was shifted by one row). Building the day-of-week chart from that column would have been wrong.

**Fix:** day-of-week is recomputed directly from each incident's date, so the dashboard's per-day counts match a correctly-built pivot exactly. The full list of affected rows is documented in `incorrect_dayofweek_IDs.csv`. *(In the source spreadsheet, the column can be repaired with `=TEXT([@Date],"ddd")`.)*

## Tech

- [Leaflet](https://leafletjs.com/) + CARTO basemap tiles for the map
- [Chart.js](https://www.chartjs.org/) for the charts
- [OpenStreetMap Nominatim](https://nominatim.org/) for address geocoding
- [Overpass API](https://overpass-api.de/) for live CTA/Metra stations (with automatic fallback servers)
- [OSRM](http://project-osrm.org/) for live driving routes; transit time opens in Google Maps
- Everything is baked into one `index.html` - no build step, no server, no database.

## Running it locally

It's a single file. Either:

- Open `index.html` directly in a browser, **or**
- Serve it (recommended, so the map services behave):
  ```bash
  python -m http.server 8000
  # then visit http://localhost:8000
  ```

An internet connection is required, since the basemap, geocoding, transit, and routing load from public services.

### Publishing to GitHub Pages

1. Create a public repo and **rename the file to `index.html`**.
2. **Add file → Upload files → drag in `index.html`** (don't use GitHub's in-browser pencil editor - it refuses files over 1 MB and will truncate this ~3.7 MB file, producing a blank page).
3. **Settings → Pages → Deploy from a branch → `main` / `(root)`**.
4. Wait ~1 minute; the site goes live at `https://USERNAME.github.io/REPO-NAME/`.

## Limitations

- **Driving routes** use the free OSRM engine, which can pick a less-optimal path than Google Maps - distances may run a bit long, though both respect one-way streets and tell the same story.
- **Transit travel time** opens in Google Maps (live in-app transit routing requires a paid API key).
- **District rankings use raw incident counts**, not per-capita rates (district population isn't in the dataset), so very small or large districts could shift slightly under a per-resident measure.
- This is an analytical tool, not a navigation app.

## License & attribution

Crime data © City of Chicago, provided under the [Chicago Data Portal Terms of Use](https://www.chicago.gov/city/en/narr/foia/data_disclaimer.html). Built for an educational data-analysis project.
