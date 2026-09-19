# HackMysuru 1.0 — Phase 1 Submission Index

> **This is the landing file for your submission.** Reviewers open this file first.
> Every evaluation artifact is uploaded to **Google Drive** and linked below. No files in the repo, no other platforms.
> Freeze: **20 September 2026, 23:59 IST.** Anything not linked here before the freeze does not exist for judging.

---

## 1. Team Details

| Field | Value |
|---|---|
| Team ID (from dashboard) | `HM26-7096` |
| Team Name | `ZENITH` |
| College(s) | `<college name(s)>` |
| Team Leader | `<name>` · `<email>` · `<phone>` |
| Repository | `https://github.com/LEHANYA2023/civicmysuru-hackathon-2026` |

| # | Member | Program & Year | GitHub Handle | Primary Role |
|---|---|---|---|---|
| 1 | `<name>` (Lead) | `<e.g. B.E. CSE, 3rd yr>` | `@<handle>` | `<backend / ML / frontend / ...>` |
| 2 | `<name>` | `<...>` | `@<handle>` | `<...>` |
| 3 | `<name>` | `<...>` | `@<handle>` | `<...>` |
| 4 | `<name>` | `<...>` | `@<handle>` | `<...>` |

---

## 2. What We Built (one-liner)

**Sub-problem:** Routing

**In one sentence:** An offline-friendly reporting flow that auto-routes complaints to MCC, Town Panchayat, or Gram Panchayat using a versioned ward-boundary registry and issue-type rules, with a confidence score and admin fallback for boundary-edge cases.

---

## 3. Repository Documents

| Document | What it covers |
|---|---|
| [README.md](./README.md) | Problem, users, solution overview, links to everything below |
| [ai.md](./ai.md) | AI tools used in development and AI/ML inside the product |
| [docs/architecture.md](./docs/architecture.md) | Diagram, components, data model, APIs, tech stack |
| [docs/constraints.md](./docs/constraints.md) | How we handle the five hard constraints |
| [docs/setup.md](./docs/setup.md) | Local setup, seed data, offline testing |
| [docs/limitations.md](./docs/limitations.md) | Known gaps, edge cases, scaling roadmap |
| [resource-templates/](./resource-templates) | Templates & guides for the video, decision log, and presentation |

---

## 4. Submission Artifacts (Google Drive)

| # | Artifact | Google Drive Link | File Name | SHA-256 (first 16 chars) |
|---|---|---|---|---|
| 1 | Pitch + Code Walkthrough Video (≤ 10 min, MP4) | `<https://drive.google.com/file/d/.../view>` | `HM26-7096_video.mp4` | `<...>` |
| 2 | Decision Log (1 page, PDF) | `<https://drive.google.com/file/d/.../view>` | `HM26-7096_decision-log.pdf` | `<...>` |
| 3 | Presentation (≤ 10 slides, PDF) | `<https://drive.google.com/file/d/.../view>` | `HM26-7096_presentation.pdf` | `<...>` |

### Video Chapters

| Timestamp | Section |
|---|---|
| `00:00` | Hook & problem — Team ID, Routing, real Mysuru scenario |
| `00:20` | Who it's for — personas & constraints |
| `00:40` | Live core flow — report → routing decision → staff queue |
| `01:50` | Bad-input test — duplicate / fake photo / impossible location |
| `02:30` | Offline mode test — airplane mode, queue, reconnect, sync |
| `03:00` | Architecture walkthrough |
| `04:30` | Data model & APIs |
| `05:30` | Core logic walkthrough (routing / boundary lookup / scoring) |
| `07:30` | Decisions & trade-offs |
| `08:30` | Scale & city limits |
| `09:15` | AI usage disclosure (see [ai.md](./ai.md)) |

---

## 5. Live MVP

| Field | Value |
|---|---|
| Live URL | `<https://...>` |
| Platform | `<Web / PWA / Android APK link on Drive / ...>` |
| Test login (if any) | Citizen: `<user / pass>` · Staff: `<user / pass>` · Admin: `<user / pass>` |
| Sample data loaded? | `<e.g. Yes — N synthetic complaints across N wards, including 2 boundary-edge cases>` |
| How to test offline mode | `<one line>`. Full steps in [docs/setup.md](./docs/setup.md#testing-offline-mode) |
| If the live link is down | Follow [docs/setup.md](./docs/setup.md) |

---

## 6. Quick Reviewer Path (≤ 3 minutes)

1. Open the live URL and log in as Citizen.
2. Report an issue at a pre-filled boundary-edge location (e.g. near a newly absorbed panchayat).
3. Observe the routing decision and its confidence score.
4. Log in as Staff → see it appear in the correct office's queue → mark it resolved.
5. Open the public/admin view → confirm the boundary and status are reflected correctly.

---

## 7. Declaration

- [ ] All Drive links open in an incognito window with **Viewer** access (no "Request access").
- [ ] The video is one continuous recording, ≤ 10 minutes.
- [ ] The decision log is one page and written by us in our own words.
- [ ] All AI tools used (development and in-product) are disclosed in [`ai.md`](./ai.md).
- [ ] No code specific to this challenge was written before 18 Sept 2026, 00:00 IST.
- [ ] We will not modify or replace any linked file after 20 Sept 2026, 23:59 IST.

**Submitted by:** `<Team Leader name>` · **Date/Time (IST):** `<20-09-2026 HH:MM>`
