# CivicMysuru — Resource Links

**HackMysuru 1.0, Phase 1 — Civic Governance & Clean Mysuru**
**Chosen sub-problem: Routing**

| Deliverable | Link |
|---|---|
| Live working MVP demo | `<PASTE your deployed Netlify frontend URL here>` |
| Backend API (docs) | `<PASTE your deployed Render backend URL here>/docs` |
| 10-minute video walkthrough | `<PASTE Google Drive link — permission: "Anyone with the link can view">` |
| Decision log | [`decision-log.pdf`](./decision-log.pdf) |
| Presentation deck | [`presentation.pdf`](./presentation.pdf) |

## What this project does

CivicMysuru is a citizen complaint reporting and routing platform. A citizen
reports a civic issue (pothole, garbage, streetlight, drainage, water
supply, or public safety); the system classifies it, resolves the correct
governing authority using a **time-aware jurisdiction engine** (so it
survives real boundary changes like Mysuru City Corporation absorbing
nearby panchayats), scores its priority and evidence credibility, and flags
duplicates and reused/fake evidence automatically — all documented in
[`decision-log.pdf`](./decision-log.pdf).

## Repo layout

See [`README.md`](./README.md) for the full folder structure and local run
instructions (`python main.py` for the backend, a static file server for
the frontend).
