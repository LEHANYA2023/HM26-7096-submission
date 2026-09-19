# Defense Against the Five Hard Constraints

The handbook names five realities every submission has to survive. Here's how CivicRoute Mysuru handles each one.

## 1. People will lie, and you can't always tell

`<Describe your spam/fake-report defense — e.g. rate-limiting per device, duplicate-detection by location+category+time window, photo metadata sanity checks. Be specific about what you actually built, not what would be ideal.>`

## 2. Responsibility is genuinely unclear right now

This is the core of our chosen sub-problem. Our answer: a **versioned boundary registry** (see [architecture.md](./architecture.md)) instead of a fixed map. Every polygon has an effective-from date, so:
- A complaint filed today routes using today's boundaries.
- When MCC formally absorbs a panchayat, an admin adds the new boundary — the system doesn't break or need a redeploy.
- Complaints near a boundary edge, or in an area with genuinely disputed ownership, get a **confidence score**; low-confidence cases are queued for a human (an admin or the two candidate offices) to confirm rather than silently auto-assigned.

## 3. Not every complaint is equally urgent

`<Describe your prioritization logic — e.g. issue-type severity weighting, report-count clustering for the same location, or explicit escalation rules (school routes, main roads). Note: this is more central to the "Follow-through" sub-problem, but state briefly how routing hands off priority signal to whatever queue receives the complaint.>`

## 4. Handling bad or tricky input (show this in your submission)

In the video (`01:50–02:30`), we demonstrate the system's actual behavior — not a theoretical description — for:
- A duplicate report of the same issue at the same location
- A photo that doesn't match the claimed issue category (or is clearly not from the claimed location)
- A location pinned outside any known boundary polygon (what does routing do — refuse, flag, or fall back to nearest authority with a low confidence score?)

`<Fill in the actual observed behavior for each case once built — this section should describe what really happens, matching the video.>`

## 5. Offline resilience

`<Describe how the citizen app queues a report when offline and syncs when reconnected — this is demonstrated live in the video at 02:30–03:00. Full technical detail belongs in setup.md.>`
