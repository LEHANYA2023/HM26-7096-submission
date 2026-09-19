# Architecture — CivicRoute Mysuru (Routing)

## 1. One-line summary

Static citizen/staff web pages → FastAPI REST backend → a routing engine backed by a date-versioned jurisdiction table keyed by area name, not geographic polygons.

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

Layer	Choice	Why
Frontend	Static HTML/CSS/JS, no framework, no build step	Zero tooling setup — direct trade-off against offline/PWA capability, which the time budget didn't allow
Backend	FastAPI + Pydantic	Free automatic /docs UI, which doubled as the live demo interface for the jurisdiction-change "twist"
Database	SQLite via SQLAlchemy	Zero setup, no separate DB server to provision in a 72-hour window — explicitly flagged in the decision log as the first thing to swap for Postgres at real scale
Hosting	Backend: Railway/Render (Python web service). Frontend: Vercel/Netlify (static)	Both have config-free or near-config-free deploys for exactly these two project shapes, on free tiers
