# AI Usage Disclosure — Team ZENITH (HM26-7096)

AI tools are permitted under HackMysuru 1.0 rules. This file discloses exactly where and how we used them, both while building the product and inside the running product itself.

## 1. AI used during development

| Tool | What we used it for | What we wrote/checked ourselves |
|---|---|---|
| `<e.g. Claude>` | `<e.g. scaffolding the routing service, drafting docs>` | `<e.g. reviewed every generated function, rewrote the boundary-lookup logic to handle overlapping polygons>` |
| `<e.g. GitHub Copilot>` | `<e.g. boilerplate, test stubs>` | `<e.g. all assertions/edge cases added manually>` |

For every AI-assisted file we can point to during the video/jury Q&A, list it here:

- `<path/to/file>` — `<what AI generated>` — `<what we changed and why>`

## 2. AI/ML inside the product itself

`<State clearly: either "None — the routing engine is deterministic, rules + boundary lookup, no model in the request path" OR name the model/approach (e.g. a confidence-scoring classifier) and what data it runs on.>`

If a model is used at runtime:
- **Model/approach:** `<...>`
- **Input/output:** `<...>`
- **Why this isn't a black box we can't defend:** `<e.g. confidence score is a weighted combination of explainable features — distance to boundary edge, historical correction rate for this polygon, issue-type match — not an opaque model output>`

## 3. What we will NOT claim

We will not present AI-generated boilerplate as original architecture thinking during the code walkthrough. Every design decision defended in the video and the Decision Log is ours, even where AI tools helped write the code that implements it.
