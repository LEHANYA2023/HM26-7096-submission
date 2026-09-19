# Defense Against the Five Hard Constraints

The handbook names five realities every submission has to survive. Here's how CivicRoute Mysuru handles each one.

## 1. People will lie, and you can't always tell
What's actually built, in routing_engine.py:

Duplicate detection: a new complaint is compared against the last 25 complaints in the same area + issue-type category. If 4+ shared words of length ≥4 overlap between descriptions, it's flagged duplicate: true. The complaint is still created — it's flagged for a human to see, not auto-rejected.
Reused-evidence detection: if the same image_name string appears on an earlier complaint, the new one is flagged reused_evidence_flag: true, its verification_score is dropped by 40 points, and status becomes "Flagged: Possible Fake Evidence".
Verification scoring: every complaint gets a 0–100 score from description length (≥80 chars: +20, ≥30 chars: +10) and whether a photo was attached (+30), labeled Needs Review / Moderate Evidence / Verified Candidate.

What's honestly not built: no rate-limiting per device or IP, no photo metadata/EXIF checks, and no real image analysis at all — the backend only ever receives a filename string, never actual image bytes. A renamed reused photo defeats the fake-evidence check entirely; this is stated as a known limitation in the decision log.

## 2. Responsibility is genuinely unclear right now

The draft above describes a polygon-based system with a numeric confidence score. That's not what exists, and I don't want this doc to overclaim. Here's what's actually built:

A versioned jurisdiction table (jurisdiction_engine.py) — not polygons, an in-memory list of {area name, authority, valid_from, valid_to} records:

A complaint filed today looks up whichever record's date range covers today, for that exact area name.
POST /api/admin/jurisdiction-change adds a new dated record with zero redeploy — this is the live demo of the "twist."
There's no numeric confidence score. Instead, match_quality is one of three categorical values: "matched", "ambiguous" (two overlapping date-ranged records for the same area — a genuine unresolved conflict), or "unknown_area" (no record exists at all). Both "ambiguous" and "unknown_area" set routing_mode: "Priority human review" and an honest authority string like "UNASSIGNED — needs manual jurisdiction review" instead of guessing.
One real gap: "queued for a human... to confirm" isn't a separate queue — the flagged complaint just sits in the same shared dashboard as every other complaint, distinguishable only by its authority string. There's no dedicated review inbox yet.
Another real gap: this table is in-memory, not a database table. A server restart reverts it to seed data — nothing persists.

## 3. Not every complaint is equally urgent

`calculate_priority() in routing_engine.py — keyword-driven, explainable scoring, starting at 20:

+30 per matched critical term: accident, danger, hazard, exposed wire, fire, collapse, blocked emergency
+18 per matched high term: overflow, flood, sewage, major pothole, open manhole, broken pole, no streetlight
+10 if the description mentions scale (multiple, entire road, whole area, many people)
+5 if the description exceeds 250 characters
Capped at 100, bucketed into Critical (≥70) / High (≥50) / Medium (≥30) / Low

Handoff to the queue — honestly weaker than it sounds: priority and priority_score are stored on the complaint and the dashboard can filter by priority, but the dashboard does not auto-sort by priority — it orders by newest-first regardless of urgency. A Critical complaint from yesterday doesn't visually surface above a Low complaint from five minutes ago unless staff manually applies the filter. There's also no explicit "school route" or "main road" escalation rule — only generic scale words like "school" appearing in free text would need to literally match a keyword, which it currently doesn't (no school keyword exists in either term set).
## 4. Handling bad or tricky input (show this in your submission)

Case	Actual observed behavior
Duplicate report, same issue, same area	Second complaint is created with duplicate: true; nothing is blocked or merged
Photo doesn't match the claimed category	Not handled at all. No image content is ever analyzed — only a filename string is compared. A mismatched photo with a plausible filename passes through undetected. This should be stated as a limitation in the video, not glossed over
Location/area with no matching jurisdiction record	Not refused, not silently defaulted to a "nearest authority" — routed to "UNASSIGNED — needs manual jurisdiction review" with routing_mode: "Priority human review", and it still appears in the shared dashboard like any other complaint


## 5. Offline resilience

`Not built. There is no service worker, no offline complaint queue, and no sync-on-reconnect anywhere in the codebase — the citizen pages are static HTML/JS that require a live connection to the FastAPI backend for every action (routing preview, classification, submission). If this is required for your submission, it's a real gap, not something to describe as already working at 02:30–03:00 in a video — that timestamp shouldn't exist yet.
