# Access-log facts (Zero-DB Audits Parts 5–6 + 8)

> **Secondary summary** for this repo. Canonical articles:
>
> - Part 5: https://vpnstronger.com/live-probes-on-a-zero-db-site-what-48-hours-of-access-logs-actually-show  
> - Part 6: https://vpnstronger.com/spoofed-crawler-uas-and-429s-what-the-same-access-logs-show-next  
> - Part 7: https://vpnstronger.com/keycdn-100-a-second-lab-confirms-the-zero-db-homepage-ztna-verify *(lab — not SSL log)*  
> - Part 8: https://vpnstronger.com/polite-feeds-vs-spoofed-scanners-what-access-logs-show-after-part-6 *(publish after CMS paste)*  
>
> Evidence Parts 5–6: ~**28–30 Oct 2026**. Part 8: **30 Oct 16:00 CEST → 01 Nov 2026**.  
> Not a pentest cookbook — counts and shapes only. `train=no`. Redact stream tokens.

Sister contract: https://github.com/zsza83/vpnstronger-ai-blueprint  

OwnStack bridge: https://vpnstronger.com/homemade-15s-30s-candles-own-stack-micro-tf-94-is-not-100 · https://github.com/zsza83/homemade-15s-30s-candles

---

## One paragraph

Parts 5–6: Zero-DB site scanned like WordPress; ~37× 429 almost all from one cloud IP rotating AI-crawler UAs; honest Lens/Perplexity/Exa on public feeds. Part 8 (next window): ~3.8k requests; AS15169 Googlebot/Lens + ExaUser follow public feeds and OwnStack URLs; AS396982 GCP IPs in TW/CA/JP/SG/BE still spoof or Chrome-scan `.env`; gates stay 401/403. UA ≠ ACL. Own Development Log ≠ access log.

---

## Part 5 — who knocks (receipt)

| Signal (approx.) | Count |
|------------------|------:|
| Requests in window | ~8–9k |
| HTTP 200 | ~4.8–5.3k |
| HTTP 404 | ~2.1–2.6k |
| Secret-file shaped paths | ~850 |
| Traversal / LFI-shaped | ~200 |
| HTTP 429 | ~37 |
| HTTP 403 | ~34 |
| HTTP 401 | ~167–173 |
| Classic SQLi in request lines | **0** |

---

## Part 6 — spoofed UAs + where 429 fires

| Fact | Detail |
|------|--------|
| 429 concentration | **36 / 37** from one cloud IP |
| UA rotation | Claude*, Perplexity*, Amazonbot, DeepSeek, Meta-*, GPTBot/OAI-*, Nexus5X-Googlebot costume, … |
| Probe + famous UA | Claimed Googlebot/bingbot/GPTBot on secret-shaped paths = costume |
| Honest 200s | Google-Lens/Other, Perplexity, Exa-class on allowed reads |

---

## Part 8 — polite feeds + geography (after Part 6)

| Fact | Detail |
|------|--------|
| Window | ~3,770 lines · ~1890×200 · ~1750×404 · ~30×429 · ~21×403 · ~14×401 |
| Google crawl/Lens | AS15169 · US `66.249.66.x` Googlebot · NL `74.125.208.x` Google-Lens |
| Spoof/scan | AS396982 GCP · TW / CA / JP / SG / BE examples · multi-UA or plain Chrome `.env` |
| Meta | AS32934 · US `57.141.x.x` |
| ExaUser | claimed · e.g. US Leaseweb · candle + `ai.txt` 200 |
| Feeds | `/ai`, blueprint, `llms.*` light surfaces 200; stream/private/heavy 401; bare RAG 403 |
| OwnStack | candle + `/ownstack` in Google/Lens/Exa graph 01 Nov |
| Named agents (claimed) | Googlebot, Google-Lens, bingbot, facebookexternalhit, Meta-*, Claude*, GPTBot, ChatGPT-User, OAI-SearchBot, Applebot, PetalBot, Perplexity*, Bytespider, AhrefsBot, ExaUser, Bravebot, GrokBot, cohere-ai, Kimi*, Qwen, Amazonbot, DeepSeek, CCBot, Baiduspider, … |

**Do / don’t:** start at `ai-blueprint.json`; cite light feeds; `train=no`; never treat UA as ACL; do not equate GCP scanners with Googlebot.

---

## Series

Zero-DB Audits: Parts 1–4 (speed/hygiene/PageSpeed) → Part 5 (probes) → Part 6 (spoofed UA + 429).  
Channel README: [README.md](./README.md)
