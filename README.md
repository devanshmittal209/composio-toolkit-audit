# Composio Toolkit Readiness Audit — 100 Apps

Live case-study page: `site/index.html` (open directly, or deploy the `site/` folder to GitHub Pages / Vercel — it's fully static, no backend).

## What this is

A research pass over the 100 apps in the brief, scoring each for agent-toolkit readiness: auth method, self-serve vs. gated access, API surface, and a buildability verdict, backed by a cited evidence URL. Pattern analysis and an honest accuracy check are on the page.

## How the research was actually run

No paid API calls were used. Instead of running `research_agent.py` against the Anthropic API + Composio's search tool (the scaffolded, reproducible path — see below), the research for this submission was done **live**, turn-by-turn, using Claude's built-in web search:

1. For each app, search for its official developer/auth docs (queries like `"<app> API OAuth2 developer free trial"`, not generic marketing searches).
2. Prefer primary sources: `developer.*`, `docs.*`, official GitHub repos over aggregators or SEO content.
3. Record auth method, self-serve status, API surface, buildability verdict, and the exact evidence URL.
4. Apps flagged as unfamiliar or ambiguous (Waterfall.io, fanbasis, iPayX, Paygent Connect, MrScraper, Sherlock, etc.) got a dedicated search each — well-known apps (Salesforce, Stripe, GitHub, Notion, etc.) were scored from strong existing knowledge with a lighter spot-check.
5. A second pass hand-cross-checked a 20-app sample against the cited docs to produce the accuracy numbers shown on the page.

## Reproducing with the scripted/API path

`scripts/research_agent.py` mirrors the same process programmatically, for anyone who wants to re-run this against the full 100 with a paid key instead of live chat:

```bash
pip install -r requirements.txt
export ANTHROPIC_API_KEY=...      # console.anthropic.com
export COMPOSIO_API_KEY=...       # composio.dev — used for the hosted search/fetch tool
python scripts/research_agent.py --apps data/apps_seed.json --out data/apps_full.json
python scripts/build_site.py      # recomputes pattern stats, writes site/data.js
```

`research_agent.py` calls Claude with Composio's hosted search/fetch tool wired in as a callable tool (rather than a per-app toolkit) — using Composio's own tool-calling infra to do the researching, per the brief's spirit.

## Where a human was needed

- Triage: deciding which apps needed a full search vs. which were safe to score from existing knowledge.
- Judgment calls on borderline cases (e.g. Magento OSS vs. Adobe Commerce being different in gating).
- The final accuracy verification pass — a genuine hand check against docs, not a second AI grading the first.

## Files

- `data/apps_full.json` — all 100 apps, structured
- `site/index.html` + `site/data.js` — the case-study page
- `scripts/` — the reproducible agent path (not used for this submission's actual data, see above)
