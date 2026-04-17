# ClaimHit MCP Server

AI-powered patent infringement intelligence. Find products and technical 
standards that potentially infringe your patent using 9 AI models in parallel.

## MCP Server URL
https://claimhit.com/api/mcp
## Authentication

API key via query parameter: `?api_key=ch_live_xxx`

Get a free API key at [claimhit.com/claimhit/account](https://claimhit.com/claimhit/account)

## Available Tools

- `claimhit_search` — search for infringing products or standards
- `claimhit_generate_chart` — generate an AI claim chart for a target
- `claimhit_get_history` — retrieve past searches
- `claimhit_get_result` — fetch full results by search ID
- `claimhit_get_credits` — check remaining credits
- `claimhit_rerun_search` — re-run a previous search

## Scoring Methodology

Results are scored across four independent factors:
- **Model Consensus** — how many of the 9 models independently flagged the target
- **Claim Coverage** — which claim elements are matched, weighted toward inventive steps
- **Evidence Strength** — specific datasheets and FCC filings score higher than inferences
- **Functional Equivalence** — doctrine of equivalents proxy

Full methodology at [claimhit.com/methodology](https://claimhit.com/methodology)

## SEP Analysis

Standards mode searches published specifications from 3GPP, IEEE, ETSI, ITU-T and IETF.

## About

Built by [Scintillation Research and Analytics Services Pvt. Ltd.](https://claimhit.com)  
Homepage: [claimhit.com](https://claimhit.com)
