# Known Limitations, Edge Cases & Scaling Roadmap

## Known limitations (top 3+)

1. Boundary polygons for the newly absorbed panchayats are built from whatever shapefile/OSM data we could source in 72 hours — some edges are approximate, not survey-grade.
2. No live integration with an official MCC GIS/records system (if one exists) — boundary updates are entered manually by an admin.
3. Confidence scoring is currently a rules-based heuristic (distance to boundary edge, issue-type match, etc.), not learned from a history of confirmed/corrected routings — because we don't have that history yet.


## Edge cases we've thought about (and what happens)

| Edge case | Current behavior |
|---|---|
| Complaint pinned exactly on a boundary line | Doesn't apply yet, and that's worth saying honestly. Your system has no polygon/coordinate geometry at all — an area is a self-reported string (e.g. "Hootagalli") the citizen types or picks, not a GPS point checked against a boundary shape. The 📍 GPS button captures real lat/lng, but it only gets written into the free-text location field for staff to read — it's never passed into determine_jurisdiction(). So there's no "boundary line" for a complaint to sit on yet; that only becomes a real question once routing is coordinate-based instead of name-based. |
| Complaint in an area with two overlapping/disputed claims |Handled, but by a different mechanism than geography. If two jurisdiction records for the same area name have overlapping valid-date ranges (e.g. a data-entry conflict, or two authorities both claiming an area during a transition), _lookup_authority() returns match_quality: "ambiguous", and the complaint is routed to "DISPUTED — multiple overlapping jurisdiction records, needs manual review" with routing_mode: "Priority human review" — never split the difference or guess one. This is a temporal overlap check today, not a spatial one, since there's no polygon geometry to overlap. |  
| GPS unavailable / citizen manually pins a location | detectLocation() catches the permission-denied/unsupported case and shows a toast ("Location permission was not granted") without blocking the form — the citizen just types the location manually instead. Since (as above) this field never feeds routing anyway, GPS availability has zero effect on which authority a complaint reaches — only on what staff read as a landmark description.|
| A boundary changes *after* a complaint was filed but *before* it's resolved | Half true as written — needs a correction. determine_jurisdiction() runs once, at creation time, and the result is stored permanently on the row — so yes, an existing complaint's routing is never silently rewritten by a later boundary change. But "a manual re-route is possible via the admin console" is not actually built yet — there's no endpoint or button that lets staff reassign an existing complaint's authority. /api/admin/jurisdiction-change only affects future complaints for that area, not ones already filed.|

## Scaling roadmap (what breaks first at full Mysuru scale, and the fix)

At 65+ MCC wards plus adjacent panchayats and a Dasara-scale spike in daily reports:

At 65+ MCC wards plus adjacent panchayats and a Dasara-scale spike in daily reports:

First thing to break: not polygon lookups — there aren't any yet. It's SQLite's single-writer model. duplicate_check() and reused_evidence_check() also do unindexed ilike/exact-match scans on area, issue_type, and image_name (plain TEXT columns, no index beyond the primary key). At low volume this is invisible; once the complaints table reaches the tens of thousands of rows a real Dasara spike would generate, and many citizens submit within the same minute, SQLite's single-writer lock starts serializing those writes, and each new submission's duplicate-check scan gets slower as the table grows — the two problems compound at exactly the moment (a festival surge) when speed matters most.
Fix: migrate storage to Postgres (already flagged as the upgrade path in your decision log) for real concurrent multi-writer support, and add indexes on area, issue_type, and image_name (or a composite index on (area, issue_type)) so duplicate/reused-evidence checks become index lookups instead of table scans. If routing later moves from named areas to real coordinates (closing the boundary-line gap above), that's when a PostGIS GiST spatial index over ward/panchayat polygons becomes the relevant fix — not before, since there's no geometry to index yet.
