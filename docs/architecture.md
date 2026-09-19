# Architecture — CivicRoute Mysuru (Routing)

## 1. One-line summary

`<e.g. "Offline-first PWA → REST API → routing engine backed by a versioned PostGIS boundary registry.">`

## 2. Diagram

```
[Citizen App (PWA)]        [Staff Console]        [Admin: Boundary Editor]
        |                        |                          |
        v                        v                          v
                    [ API Gateway / Backend ]
                        |            |
                        v            v
              [ Routing Engine ]  [ Complaint Store ]
                        |
                        v
        [ Versioned Boundary Registry (polygons + effective_from) ]
                        |
                        v
              [ Issue-Type Rules Table ]
```

Replace this ASCII sketch with an actual diagram image under `docs/images/` before submission, and link it here.

## 3. Components

| Component | Responsibility |
|---|---|
| Citizen App | Capture complaint: category, photo, GPS/pinned location; works offline and syncs on reconnect |
| Staff Console | Per-authority queue (MCC / Town Panchayat / Gram Panchayat), status updates |
| Admin Boundary Editor | Add/update jurisdiction polygons with an effective-from date — no redeploy needed |
| Routing Engine | Point-in-polygon lookup against the boundary version active *at report time*, plus issue-type rule overrides (e.g. C&D dumping routed differently from streetlights), returns authority + confidence score |
| Complaint Store | Source of truth for reports, routing decisions, and status |

## 4. Data model (key entities)

- **Complaint**: id, category, description, photo_ref, lat/lng, reported_at, status, routed_authority, routing_confidence, boundary_version_used
- **BoundaryPolygon**: id, authority_name, authority_type (MCC/Town Panchayat/Gram Panchayat), geometry, effective_from, effective_to (nullable = current)
- **IssueTypeRule**: issue_category, override_authority_type (nullable), notes
- **RoutingDecisionLog**: complaint_id, boundary_version_used, confidence, was_manually_confirmed (for auditing and for measuring how often low-confidence routing needed a human)

## 5. Key APIs

- `POST /complaints` — submit a report (works from an offline queue too)
- `GET /complaints/{id}/routing` — routing decision + confidence + which boundary version was used
- `POST /admin/boundaries` — add a new boundary version (the twist: this is how the system adapts when jurisdictions shift, without a redeploy)
- `GET /authorities/{id}/queue` — staff queue for a given office

## 6. How this handles the "boundaries are changing" twist

Every boundary polygon carries an `effective_from` date. The routing engine always looks up the polygon set that was active on the report's timestamp, not a single hard-coded map. When MCC absorbs a new panchayat, an admin adds new polygon records instead of anyone touching code or redeploying. Historical complaints keep pointing at the boundary version that was true when they were filed, so routing decisions stay auditable even after a boundary changes.

## 7. Tech stack

`<Fill in — frontend, backend, database, hosting, and why each was chosen given the 72-hour build window>`
