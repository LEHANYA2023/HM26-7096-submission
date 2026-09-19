# 10-Minute Video Guide — HM26-7096 · ZENITH

Continuous MP4, uploaded to Google Drive only (never committed to Git), max 10:00, every registered member speaks at least once. Set sharing to "Anyone with the link can view" and test it in an incognito window before submitting.

| Time | What to show/say live on camera |
|---|---|
| 00:00–00:20 | Team ID `HM26-7096`, team `ZENITH`, sub-problem: **Routing**, the real Mysuru scenario (boundary redraw + bounced complaints) |
| 00:20–00:40 | Who it's for: citizen near a boundary edge, receiving-office staff — and the real constraints (patchy network, bilingual input) |
| 00:40–01:50 | Live demo of the core flow: citizen reports → system routes with a confidence score → staff sees it in their queue |
| 01:50–02:30 | Bad-input test, live: duplicate report, a mismatched/fake photo, or an impossible location |
| 02:30–03:00 | Offline mode test, live: airplane mode / DevTools offline, queue the report, reconnect, show it sync |
| 03:00–04:30 | Architecture walkthrough using `docs/architecture.md`: clients, API, boundary registry, routing engine |
| 04:30–05:30 | Open the schema/models file — explain key entities (Complaint, BoundaryPolygon, RoutingDecisionLog) and 2–3 endpoints |
| 05:30–07:30 | Open the routing/boundary-lookup logic in the IDE and explain it line by line |
| 07:30–08:30 | Decisions & trade-offs — must match the Decision Log |
| 08:30–09:15 | What breaks first at full city scale (65+ wards, Dasara traffic) and the planned fix |
| 09:15–10:00 | Walk through `ai.md` — open one AI-assisted file and explain what you changed by hand |

**Checklist before upload:**
- [ ] Every member speaks
- [ ] One continuous recording, no cuts stitched from separate takes that skip required segments
- [ ] Bad-input and offline tests are shown actually happening, not described
- [ ] Sharing permission set to "Anyone with the link can view" and verified in incognito
