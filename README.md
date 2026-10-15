# ClaimHit — MCP Server

**Find the companies using a patent — to license, partner, or enforce — right from your AI client.**

ClaimHit is a patent licensing and infringement research engine. Give it a patent number
and it returns real, currently-shipping **products** (and, for SEPs, **standards**) that
use the claimed technology — each one verified against live evidence on the manufacturer's
own pages, not guessed. It also generates element-by-element **claim charts** for the
targets you want to pursue.

This is the remote MCP server for [claimhit.com](https://www.claimhit.com), so you can run
a search, work through the results and build charts without leaving Claude (or any MCP
client).

## Connect

- **URL:** `https://app.claimhit.com/api/mcp`
- **Auth:** OAuth 2.1 — you sign in to your ClaimHit account and approve; the connector
  then works with your account's credits. No key to paste, no config file.

In Claude: **Settings → Connectors → Add custom connector →** paste the URL →
**Connect** → sign in → **Authorize**.

You'll need a (free) [ClaimHit account](https://www.claimhit.com). A free account includes
a trial search so you can try it immediately; deeper usage is credit-metered on your plan.

### API keys

For a programmatic client that cannot do OAuth, generate a key (prefix `ch_`) at
[app.claimhit.com/claimhit/account](https://app.claimhit.com/claimhit/account) → Developer
tab, and send it as a **Bearer token**:

```
Authorization: Bearer ch_live_…
```

A `?api_key=` query parameter is also accepted, for clients built against it. Prefer the
header wherever you have the choice: a credential in a URL is a credential in access logs
and in browser history.

A `ch_test_` key is a **sandbox** key — it returns fixture data and spends no credits.

## Tools

| Tool | What it does | Cost |
|------|--------------|------|
| `claimhit_search` | Search a patent number for infringing products / essential standards. Returns verified, evidence-backed targets. | 1 search credit |
| `claimhit_get_result` | Read a past search — and narrow, filter, sort and page through it. Ask for one company, a technology, the low-confidence leads, a different order, the next page, or the full evidence and per-element scores behind a score. | Free, unlimited |
| `claimhit_refine_product` | Look again at one target. Give it a vague result plus what you want instead — a specific model, the latest one, or a capability or market — and it returns up to five real, current products with a reason for each, grounded in a live web search. | Free |
| `claimhit_generate_chart` | Generate an element-by-element claim chart mapping a patent claim to a product or standard. Quotes the cost and the target it matched first; runs nothing until confirmed. | 1 chart credit, confirmed |
| `claimhit_rerun_search` | Re-run a prior search to pick up newly-released products, repeating the inputs that steered it. Lists what it will repeat before running; can widen a scoped search to the whole market. | Free within 6 months, confirmed |
| `claimhit_get_history` | List your recent searches. | Free |
| `claimhit_get_credits` | Check your remaining search and chart credits. | Free |

One search is the paid step; the conversation after it is not. Narrowing, sorting, paging,
reading evidence and refining a target are free and uncapped, and the two tools that spend
a credit quote the cost first and run nothing until you agree — so a long session working
through a result cannot run up a bill because the assistant decided to be thorough.

A search also completes in phases: `claimhit_get_result` returns a `status`, and background
enrichment keeps finding implementers for several minutes after the first results land.
Treat results as final only at `COMPLETE`.

## How it works

ClaimHit doesn't just ask an LLM "who infringes this?" — several models run the discovery
pass independently, and every candidate is then put through claim-alignment scoring and an
evidence check (the product must resolve on the manufacturer's own domain and match the
claim vocabulary), plus a forward-citation expansion that surfaces companies already active
in the technology. The goal is an **honest, comprehensive** target list you can take into a
licensing campaign — not a list of plausible-sounding guesses.

Results are scored across four independent factors:

- **Model consensus** — how many models independently flagged the target
- **Claim coverage** — which claim elements are matched, weighted toward inventive steps
- **Evidence strength** — specific datasheets and regulatory filings score higher than inferences
- **Functional equivalence** — a doctrine-of-equivalents proxy

Full methodology at [claimhit.com/methodology](https://www.claimhit.com/methodology).

## SEP analysis

Standards mode searches published specifications from 3GPP, IEEE, ETSI, ITU-T and IETF,
citing the clause where the relevant requirement appears and marking each requirement
mandatory, optional or conditional.

## What this is not

ClaimHit returns a probable list of products with the evidence behind each one. It is
preliminary research to focus attorney time — **not a legal opinion, and not legal advice.**

## Troubleshooting

**Connection fails with a redirect (307 / 308)**
Use `https://app.claimhit.com/api/mcp`. The marketing hosts `claimhit.com` and
`www.claimhit.com` redirect to it, and MCP clients that do not follow redirects cannot
connect through them.

**Authentication failed**
Re-authorize the connector, or — if you are using a key — check that it starts with `ch_`
and is still active. Generate a new one at
[app.claimhit.com/claimhit/account](https://app.claimhit.com/claimhit/account) → Developer
tab.

**No results returned**
Check your balance with `claimhit_get_credits`, and top up at
[app.claimhit.com/claimhit/credits](https://app.claimhit.com/claimhit/credits). If a search
did run, try `claimhit_get_result` with `risk: ["LOW"]` — the low-confidence leads are
excluded from the default list.

**Fewer results than expected**
The default list is HIGH and MEDIUM confidence only. Pass `risk: ["HIGH","MEDIUM","LOW"]`
to see everything, and page with `offset` — the response's `more` field says when further
matches remain.

**Patent not found**
Include the kind code: `US10123456B2`, not `US10123456`.

**Support**
Email hello@claimhit.com with your request ID.

## About

Built by [Scintillation Research and Analytics Services Pvt. Ltd.](https://www.claimhit.com)

- Product: https://www.claimhit.com
- Security: https://www.claimhit.com/security
- Privacy: https://www.claimhit.com/privacy
