# Thalamic

**Federal intelligence infrastructure for AI agents.**

Thalamic builds PLEXUS — the only pay-per-call API that transforms raw US government data into scored, synthesized intelligence. 41 federal data sources. 12 cognitive reasoning engines. 5 intelligence products. Pay with USDC on Base via x402. No API keys, no subscriptions.

[![nohumans status](https://nohumans.directory/badge/54dc47a1-9ed.svg)](https://nohumans.directory/l/54dc47a1-9ed)

---

## PLEXUS Intelligence API

**Live endpoint:** `https://plexus-public-production.up.railway.app`

### Intelligence Products — $0.20–$0.35/call

Scored, synthesized reports combining 4–6 sources into a 0–100 risk score:

| Product | Sources | Price |
|---|---|---|
| `POST /v1/products/company-risk` | EPA + OSHA + CFPB + CourtListener + OpenSanctions + GLEIF | $0.35 |
| `POST /v1/products/political-intel` | FEC + USASpending + Federal Register + Congress.gov | $0.35 |
| `POST /v1/products/supply-chain-risk` | OSHA + EPA + SAM.gov + FMCSA | $0.30 |
| `POST /v1/products/healthcare-ddil` | NPI + CMS Medicare + FDA + CFPB | $0.30 |
| `POST /v1/products/nonprofit-intel` | IRS 990 + GLEIF + CFPB + CourtListener | $0.20 |

### Data Adapters — $0.01–$0.03/call

Direct normalized access to 29 federal sources:

`fec` · `epa` · `osha` · `sec` · `courtlistener` · `opensanctions` · `gleif` · `cfpb` · `npi` · `cms-payments` · `fda` · `fda-food` · `bls` · `census` · `fema` · `fmcsa` · `usaspending` · `sam` · `propublica-990` · `nhtsa` · `worldbank` · `usgs` · `federal-register` · `clinical-trials` · `fdic` · `noaa` · `congress` · `college-scorecard` · `web-extract`

### AI Reasoning Engines — $0.05–$0.15/call

`assertion-router` · `gscore` · `tribunal` · `postcog` · `whetstone` · `tessryx` · `verify` · `oracle-router` · `dap` · `homeostasis` · `signal-diversity`

---

## Quickstart

PLEXUS uses [x402](https://x402.org) — pay per call with USDC on Base. No signup required.

```bash
# Company risk score
curl -X POST https://plexus-public-production.up.railway.app/v1/products/company-risk \
  -H "Content-Type: application/json" \
  -d '{"entity": "ExxonMobil"}'
# Returns HTTP 402 with payment details → pay → retry → get scored intelligence

# Direct FEC data
curl -X POST https://plexus-public-production.up.railway.app/v1/adapters/fec \
  -H "Content-Type: application/json" \
  -d '{"params": {"type": "candidates", "name": "Sanders", "state": "VT"}}'
```

### Python Client

```python
import requests

BASE = "https://plexus-public-production.up.railway.app"

# Without x402 payment library — inspect the 402 challenge
r = requests.post(f"{BASE}/v1/products/company-risk", json={"entity": "ExxonMobil"})
print(r.status_code)   # 402
print(r.headers["X-Payment-Required"])  # payment details

# With x402 client library
# pip install x402-client
from x402 import Client
client = Client(wallet_private_key="0x...")
result = client.post(f"{BASE}/v1/products/company-risk", json={"entity": "ExxonMobil"})
print(result.json())
```

---

## MCP Server

PLEXUS is available as a Model Context Protocol server. Connect any MCP-compatible AI client directly.

**Endpoint:** `https://plexus-public-production.up.railway.app/mcp`  
**Qualified name:** `thalamic/plexus-intelligence`  
**Transport:** Streamable HTTP (MCP 2025-03-26)  
**Tools:** 46 (all adapters + products + engines)  
**Auth:** None required — free to connect, x402 payment happens per tool call

```json
{
  "mcpServers": {
    "plexus": {
      "url": "https://plexus-public-production.up.railway.app/mcp"
    }
  }
}
```

---

## Discovery

| Endpoint | Description |
|---|---|
| `GET /.well-known/x402` | x402 discovery manifest — 46 endpoints, pricing, wallet |
| `GET /openapi.json` | OpenAPI 3.0 specification |
| `GET /llms.txt` | LLM-friendly service description |
| `GET /mcp` | MCP server manifest |
| `GET /health` | Service health + adapter status |

---

## Payment

- **Protocol:** [x402](https://x402.org) v2
- **Network:** Base mainnet (`eip155:8453`)
- **Asset:** USDC (`0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`)
- **Wallet:** `0xBD7dd8b1309823082F42dC042091EfFCb89Cd474`
- **Facilitator:** Coinbase CDP

No accounts. No API keys. Agents discover and pay autonomously.

---

## Listings

- [agentic.market](https://agentic.market) — Coinbase x402 marketplace
- [nohumans.directory](https://nohumans.directory/l/54dc47a1-9ed) — live status monitoring
- [Smithery](https://smithery.ai/server/dk-dkes/plexus-intelligence) — MCP registry
- [RapidAPI](https://rapidapi.com) — traditional API marketplace
- [awesome-x402](https://github.com/xpaysh/awesome-x402) — community directory (PR pending)

---

## Built by

**Silent Ampersand LLC** — Simi Valley, CA  
[thalamic.systems](https://thalamic.systems) · [silentampersand.com](https://silentampersand.com)

---

## License

MIT — see [LICENSE](LICENSE)
