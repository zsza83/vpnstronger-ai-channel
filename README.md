# vpnstronger-ai-channel
Consumer guide for the VPNStronger AI channel — blueprint, light/RAG tiers, policy. Canonical: vpnstronger.com (not the CMS)

> **Not canonical.** Live truth is on the site. Refresh hashes and URLs from the blueprint before shipping agents.
>
> | Role | URL |
> |------|-----|
> | Machine entry | https://vpnstronger.com/ai-blueprint.json |
> | HTML guide | https://vpnstronger.com/ai#channel |
> | Policy file | https://vpnstronger.com/ai.txt |

This file is a **secondary** copy for GitHub README / agent repos / paste into issues.  
Do not treat it as the policy pack, corpus version, or auth source.

---

## One-paragraph blurb

VPNStronger publishes a machine-readable AI channel: start at **ai-blueprint.json** (capabilities + tiers), HTML mirror at **/ai#channel**. Public `llms.*` feeds for citation; middle **demo_channel** light-RAG at `/llms-chunks-rag` (free subscribe + `policy_pack_hash`, no stream token, train=no); private/heavy/SSE for operator agents only. Training on the corpus is not allowed.

---

## Start here

1. Fetch https://vpnstronger.com/ai-blueprint.json  
2. If HTML-only tools → https://vpnstronger.com/ai#channel  
3. Policy / crawl rules → https://vpnstronger.com/ai.txt  

Homepage crawl alone is **not** the AI contract.

---

## Channel map

| Tier | Feed | Auth | Use |
|------|------|------|-----|
| Discovery | `/ai`, `ai.txt`, `robots.txt` | none | find the stack |
| Light (cite) | `llms.txt`, `llms.json`, `llms-chunks.json` | none | public citation |
| Light-RAG (retrieve) | `llms-chunks-rag` (`demo_channel`) | `policy_pack_hash` | retrieve + cite; `train=no`; free subscribe list |
| Private | `llms-chunks-private` | Bearer + policy | own agents |
| Heavy | `llms-chunks-heavy` | Bearer + policy | embeddings / heavy twin |
| Realtime | `llms-stream` (SSE); WSS when advertised | Bearer + policy | corpus bump / live sync |

Crawl map: https://vpnstronger.com/sitemap.xml  

---

## Hashes (not interchangeable)

Copy **live** values from the blueprint / `/ai#channel` — do not hardcode from this README:

| Hash | Meaning |
|------|---------|
| `corpus_version` | published content revision |
| `policy_pack_hash` | operator Core/DLP/Egress pack bind |
| `prompt_core_hash` | orientation prompt |
| `channel_pack_hash` | sealed `demo_channel` definition (metadata, not a second secret) |

---

## Public light corpus

- https://vpnstronger.com/llms.txt  
- https://vpnstronger.com/llms.json  
- https://vpnstronger.com/llms-chunks.json  

No token.

---

## demo_channel light-RAG

- Feed: https://vpnstronger.com/llms-chunks-rag  
- Subscribe (list only — **no token issued**):

```http
POST https://vpnstronger.com/api/subscribe
Content-Type: application/json

{"email":"you@example.com","channel":"demo_channel"}
```

- Fetch gate: matching `policy_pack_hash` (query or header). **No stream token** on this tier.
- Subscribe does **not** unlock the feed by itself.
- Isolation: `demo_channel` — **not bank-grade**. Truncated retrieve text; not the heavy embeddings twin.

```http
GET https://vpnstronger.com/llms-chunks-rag?policy_pack_hash=<64-hex-from-live-blueprint>
Accept: application/json
```

Or header: `X-AI-Policy-Pack: <64-hex>`

Without hash → `403` `policy_denied`.

### Local agent sync (this repo)

```bash
php tools/agent/sync.php --channel=demo_channel
# or: php tools/agent/sync.php --rag
# optional list: --subscribe=you@example.com
```

Requires `AI_POLICY_PACK_HASH` in `tools/agent/.env` (or `storage/ai_policy_pack.php`). Inbox: `tools/agent-inbox/chunks-rag-latest.json`.

---

## Private / heavy / stream

Operator **AI Control** stream token (`Authorization: Bearer …` or `?token=`) plus `policy_pack_hash` when bind is on.  
`ws_url: null` in the blueprint means gated / not advertised yet — not “missing from the stack.”

---

## Actions (see live blueprint)

- Newsletter: `POST /api/subscribe` `{"email":"…"}`  
- demo_channel list: `POST /api/subscribe` `{"email":"…","channel":"demo_channel"}`  
- Admin message: `POST /api/message` `{"name","message","email?"}`  

---

## Policy summary

- **Allowed:** read, cite, answer from live feeds; daily crawl if needed for answers.  
- **Denied:** train / fine-tune / retain-for-training on this corpus.  
- **Do not** invent tokens, private URLs, or extra `robots.txt` Allow lines. Report HTTP outcomes (200 / 401/403 / challenge / timeout).

Full policy: https://vpnstronger.com/ai.txt  

---

## Maintenance

When the live channel changes, update this export from:

1. https://vpnstronger.com/ai#channel  
2. https://vpnstronger.com/ai-blueprint.json → `channel_guide.summary`  
3. https://vpnstronger.com/ai.txt → `Channel-Guide`  

CMS path in-repo: `docs/AI-CHANNEL.md`
