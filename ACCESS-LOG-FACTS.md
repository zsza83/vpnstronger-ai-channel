# Access-log facts (Zero-DB Audits Parts 5–6)

> **Secondary summary** for this repo. Canonical articles:
>
> - Part 5: https://vpnstronger.com/live-probes-on-a-zero-db-site-what-48-hours-of-access-logs-actually-show  
> - Part 6: https://vpnstronger.com/spoofed-crawler-uas-and-429s-what-the-same-access-logs-show-next  
>
> Evidence: Apache SSL access logs for `vpnstronger.com`, ~28–30 Oct 2026.  
> Not a pentest cookbook — counts and shapes only. `train=no`.

Sister contract repo: https://github.com/zsza83/vpnstronger-ai-blueprint (link only — keep the blueprint README as the contract).

---

## One paragraph

A Zero-DB / flat-file public site still gets scanned like WordPress: `.env` walks, traversal, fake `/api/…` file views. In ~48h of production SSL logs we saw thousands of 404s, hundreds of secret-file shaped requests, Bearer 401s, a thin 403 slice, and **37× HTTP 429**. Almost all 429s came from **one cloud IP** rotating “AI crawler” User-Agents (Claude / Perplexity / GPTBot / Amazonbot / CCBot / Meta / Nexus5X-Googlebot costumes, etc.). Claimed `Googlebot` + `/.env` is a costume, not a crawl. Honest traffic (Google-Lens, Perplexity, Exa-class) hit public feeds with 200. Light cite surfaces stay open; dense context stays behind policy + Bearer.

---

## Part 5 — who knocks (receipt)

| Signal (approx.) | Count |
|------------------|------:|
| Requests in window | ~8–9k |
| HTTP 200 | ~4.8–5.3k |
| HTTP 404 | ~2.1–2.6k |
| Secret-file shaped paths (`.env`, `wp-config`, cousins) | ~850 |
| Traversal / LFI-shaped URLs | ~200 |
| HTTP 429 | ~37 |
| HTTP 403 | ~34 |
| HTTP 401 | ~167–173 |
| Classic SQLi signatures in request lines | **0** |

Stack answers: missing secrets → **404**; gated storage/engine → **403**; burst `/api/…` probes → **429**; private/heavy/SSE without Bearer → **401**. No live SQL surface on the public read path — scanners still bring a SaaS/CMS threat model.

---

## Part 6 — spoofed UAs + where 429 fires

| Fact | Detail |
|------|--------|
| 429 concentration | **36 / 37** from a **single** cloud IP; 1 other IP |
| Same IP mix | ~700× 404, ~110× 401, ~36× 429, ~24× 200 |
| UA rotation on that IP | Claude*, Perplexity*, Amazonbot, DeepSeek, Meta-*, DuckAssist, Qwen, GPTBot/OAI-SearchBot, Nexus 5X “Googlebot phone”, Bravebot, Kimi, Hunyuan, YouBot, CCBot, Baiduspider, … + generic Chrome |
| 429 path shape | Almost all **API-shaped** secret/traversal probes (`.env`, `proc/self`-style) — short minute bursts |
| Claimed crawler **on probe paths** | Googlebot ~35 · bingbot ~17 · GPTBot ~14 · other bot-like UAs ~400+ |
| Honest 200s (same window) | Google-Lens/Other hundreds · Perplexity dozens · Exa-class small · some OpenAI/Claude on **non-probe** paths |

**UA is a claim, not an ACL.** One IP + many bot names + probe paths ≫ trusting the brand string in the User-Agent.

---

## For agents (do / don’t)

| Do | Don’t |
|----|--------|
| Start at `ai-blueprint.json` / `/ai` | Probe `/.env`, traversal, fake `/api` file views |
| Cite light feeds (`llms.txt`, `llms.json`, `llms-chunks.json`) | Treat spoofable bot UAs as authorization |
| Bind `policy_pack_hash` for demo_channel RAG | Assume 429 means “site blocks AI” — it rate-limits **abuse bursts** |
| Read for answers (`train=no`) | Weight-ingest the corpus |

Live policy / entry: https://vpnstronger.com/ai-blueprint.json · https://vpnstronger.com/ai.txt · https://vpnstronger.com/ai

---

## Series

Zero-DB Audits: Parts 1–4 (speed/hygiene/PageSpeed) → Part 5 (probes) → Part 6 (spoofed UA + 429).  
Channel README: [README.md](./README.md)
