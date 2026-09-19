# Known Limitations, Edge Cases & Scaling Roadmap

## Known limitations (top 3+)

1. Boundary polygons for the newly absorbed panchayats are built from whatever shapefile/OSM data we could source in 72 hours — some edges are approximate, not survey-grade.
2. No live integration with an official MCC GIS/records system (if one exists) — boundary updates are entered manually by an admin.
3. Confidence scoring is currently a rules-based heuristic (distance to boundary edge, issue-type match, etc.), not learned from a history of confirmed/corrected routings — because we don't have that history yet.
4. `<add any others specific to what you actually built>`

## Edge cases we've thought about (and what happens)

| Edge case | Current behavior |
|---|---|
| Complaint pinned exactly on a boundary line | `<...>` |
| Complaint in an area with two overlapping/disputed claims | `<...>` |
| GPS unavailable / citizen manually pins a location | `<...>` |
| A boundary changes *after* a complaint was filed but *before* it's resolved | Routing decision uses the boundary version active at report time and is not silently rewritten; a manual re-route is possible via the admin console |

## Scaling roadmap (what breaks first at full Mysuru scale, and the fix)

At 65+ MCC wards plus adjacent panchayats and a Dasara-scale spike in daily reports:

- **First thing to break:** `<e.g. un-indexed polygon lookups start timing out under concurrent load — estimate the numbers if you can (e.g. "point-in-polygon on N unindexed polygons at M requests/sec")>`
- **Fix:** `<e.g. spatial index (PostGIS GiST index / R-tree), pre-computed ward lookup grid, or caching boundary lookups by geohash cell>`
- **Next thing to break after that:** `<...>`
