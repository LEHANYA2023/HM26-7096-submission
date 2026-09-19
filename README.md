# CivicRoute Mysuru — auto-routing civic complaints as jurisdictions shift

> HackMysuru 1.0 · Phase 1 · Civic Governance & Clean Mysuru
> Team **ZENITH** (`HM26-7096`)

| 📎 Submission links | 📋 Templates | 🏗️ Architecture | 🛡️ Hard constraints | ⚙️ Setup | 🤖 AI usage | ⚠️ Limitations |
|---|---|---|---|---|---|---|
| [resource.md](./resource.md) | [resource-templates/](./resource-templates) | [docs/architecture.md](./docs/architecture.md) | [docs/constraints.md](./docs/constraints.md) | [docs/setup.md](./docs/setup.md) | [ai.md](./ai.md) | [docs/limitations.md](./docs/limitations.md) |

---

## 1. Problem Understanding & Sub-problem

**Chosen sub-problem:** Routing

- **The gap we saw:** A citizen who reports a pothole, an overflowing bin, or a broken streetlight has no way of knowing whether it belongs to Mysuru City Corporation (MCC), a Town Panchayat, or a Gram Panchayat — and right now, even officials disagree, because MCC is mid-absorption of one City Municipal Council, four Town Panchayats, and eight Gram Panchayats. A complaint filed at the wrong office bounces, stalls, or is silently dropped.
- **Why it matters:** Every bounced complaint is a pothole that stays unfilled and a resident who stops trusting the system enough to report the next one. The boundary confusion isn't hypothetical — local leaders have already publicly disagreed on which villages belong where.
- **Why we chose this over the others:** Routing is the first point of failure in the whole civic pipeline — if a complaint starts at the wrong desk, follow-through, visibility, and verification never get the chance to matter. It's also the sub-problem most directly created by the *current, real* boundary redraw, which is the twist we wanted to build for rather than around.
- **What "solved" looks like for us:** A citizen files one report with a location; the system tells them (and the right office) who owns it and why, with a confidence score — and when a ward's authority changes, an administrator updates one boundary record instead of us shipping a new build.

## 2. Target Users & Mysuru Context

| User | Their situation | What they need from us |
|---|---|---|
| Resident near an MCC–panchayat boundary | Doesn't know which office to call; patchy 4G in outer wards | Report once, in Kannada or English, and see who owns it |
| MCC / Town Panchayat / Gram Panchayat officer | Inherits new wards mid-year with no updated systems of record | A queue that's already filtered to what's actually theirs, and a way to flag a misroute |
| Ward-boundary administrator (Corporation staff) | Has to reflect boundary changes as they're notified, not on a fixed schedule | A simple way to redraw or reassign a boundary without redeploying the app |

**Local context we designed for:** overlapping/contested jurisdiction during the MCC expansion, inconsistent connectivity in outer wards, bilingual (Kannada/English) input, and a mix of smartphone and low-end Android users.

## 3. Solution Overview

CivicRoute Mysuru takes a citizen's complaint (photo, category, GPS or pinned location) and routes it to the correct civic authority using a versioned boundary registry plus issue-type rules, returning a confidence score so low-confidence or boundary-edge cases get a human review step instead of a silent wrong answer.

**Core flow:**
1. Citizen reports an issue with a category and a location (GPS pin or manual pin on map).
2. The routing engine looks up which jurisdiction polygon the point falls in *as of today's boundary version*, applies issue-type rules (e.g. C&D dumping vs. streetlight), and returns an authority + confidence score.
3. Low-confidence or edge-of-boundary cases are queued for a quick admin confirmation instead of auto-routing blind.
4. The receiving office (MCC / Town Panchayat / Gram Panchayat) sees the complaint in their queue; the citizen sees who it went to and why.

**Screenshots:** see `docs/images/` (add 2–4 screenshots, each under 1 MB, before submission).

## 4. Architecture

Citizen-facing app → routing API → versioned boundary + rules store, with an admin console for redrawing boundaries without a redeploy.

➡️ Diagram, components, data model and APIs: **[docs/architecture.md](./docs/architecture.md)**

## 5. Tech Stack & AI Usage

**Stack:** *fill in your actual stack here (e.g. React PWA · FastAPI · PostgreSQL + PostGIS · Render)* — full rationale in [docs/architecture.md](./docs/architecture.md#tech-stack)

**AI tools used in development:** *e.g. Claude, GitHub Copilot* · **AI inside the product:** *e.g. none — rules-based routing engine* (or name the model if you use one for confidence scoring)

➡️ Full disclosure: **[ai.md](./ai.md)**

## 6. Decision Log Summary

- **Chose:** a versioned polygon boundary registry (each boundary has an effective-from date) with a rules engine on top, **over:** hard-coding a single static ward-to-office map.
- **Because:** boundaries are changing *this year* — a static map breaks the day one ward moves; a versioned registry lets an admin add a new boundary version without touching code.
- **First thing to break at city scale:** point-in-polygon lookups against un-indexed geometry across 65+ wards and adjacent panchayats during a Dasara-scale spike in reports.

➡️ Full decision log: uploaded to Google Drive and linked in [resource.md](./resource.md#4-submission-artifacts-google-drive) · Template: [resource-templates/decision-log-template.md](./resource-templates/decision-log-template.md)

## 7. Setup & Run

```bash
git clone https://github.com/LEHANYA2023/HM26-7096-submission.git
# fill in: one-line install && one-line run
```

➡️ Prerequisites, environment variables, seed data and offline testing: **[docs/setup.md](./docs/setup.md)**

## 8. Known Limitations & Roadmap

- Boundary data for the newly absorbed panchayats is only as accurate as the source shapefiles we could get in 72 hours — some edges are approximated.
- No live sync yet with an official MCC GIS system, if one exists — boundary updates are manual/admin-entered.
- Confidence scoring is currently rules-based, not learned from historical misroute corrections.

➡️ Full list, edge cases and scaling roadmap: **[docs/limitations.md](./docs/limitations.md)**

---

## Team

| Name | Role | GitHub |
|---|---|---|
| ADITYA JEEVAN NAIK (Lead) | DEPLOYEMENT  | NaikAdityaJeevan|
| R V LEHANYA | BACKEND  | LEHANYA2023 |
| RANA BISWAS | FRONTEND  | ranabiswas30042005@gmail.com |
| H S AMRUTHA | DATABASE ,RESEARCH | amruthahs19@gmail.com |

## License

`<MIT / Apache-2.0 / None>`. You retain full ownership of your code.
