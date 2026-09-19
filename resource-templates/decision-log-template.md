# Decision Log Template — HM26-7096 · ZENITH

**Format reminder:** 1 page, A4, ≥10pt font, 400–550 words total. Export as `HM26-7096_decision-log.pdf`, upload to Google Drive with "Anyone with the link can view", and link it in `resource.md`.

---

## Q1. What approach did we take, and what did we reject? (~150 words)

- One-line summary (inputs → logic → output): `<...>`
- Alternative approach seriously considered but not built: `<e.g. a static, hard-coded ward-to-office lookup table>`
- Why it looked attractive at first: `<e.g. much faster to build in 72 hours, no polygon math needed>`

## Q2. Why did we reject it? What was the trade-off? (~150 words)

Compare both approaches across 3–4 dimensions relevant to this problem (e.g. correctness under boundary change, spam/edge-case resistance, offline resilience, build complexity in 72h):

| Dimension | Chosen approach | Rejected approach |
|---|---|---|
| Correctness when a boundary changes | `<...>` | `<...>` |
| Build complexity in 72h | `<...>` | `<...>` |
| `<...>` | `<...>` | `<...>` |

**Specific cost we knowingly accepted:** `<e.g. "point-in-polygon lookup adds ~150ms versus an O(1) hash lookup, and boundary data entry requires an admin step we didn't have time to make self-service.">`

## Q3. What breaks at the scale of all of Mysuru? (~150 words)

Assume 65 MCC wards, adjacent panchayats, thousands of daily reports, Dasara traffic spikes.

- **First thing to break, with estimated numbers:** `<...>`
- **Exact architectural change we'd make:** `<...>`
