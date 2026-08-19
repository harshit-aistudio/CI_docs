Based on the confirmed pricing data, here is the fully corrected table with precise cost estimates.

---

## Corrected CI Capability Cost Table

| CI Capability / Task | API / Provider | Recommended LLM | Estimated Usage | Estimated Cost |
|---|---|---|---|---|
| **Competitor battlecard generation** | OpenAI Responses API + Web Search | **GPT-5.4 mini** | ~1 run / competitor | **~$0.01–$0.05 per battlecard*** |
| **Competitor battlecard refresh** | OpenAI Web Search | **GPT-5.4 mini** | Monthly / competitor | **~$0.01–$0.05 per refresh*** |
| **Competitor event discovery** | **Clay** + existing Signals plumbing | Clay AI / existing Clay function | 1 run / competitor / cadence | **TBD — Clay credits** |
| **Event classification** | Existing Clay pipeline | Clay AI | Per discovered event | Included in Clay run cost |
| **Event summarization / key facts** | Existing Clay pipeline | Clay AI | Per discovered event | Included in Clay run cost |
| **Competitor → company mapping** | OpenAI Web Search + HubSpot data | **GPT-5.4 mini** | 1 research run / company | **~$0.005–$0.05 per company*** |
| **Mapping evidence extraction** | OpenAI Responses API | **GPT-5.6 Luna** | High-volume | **~$0.0005–$0.005 / company*** |
| **Mapping confidence / relationship reasoning** | Existing application logic + LLM | **GPT-5.6 Luna** | Per mapping candidate | **~$0.0005–$0.005** |
| **Competitive impact reasoning** | Existing Signal scoring + LLM | **GPT-5.6 Luna** | Per event × affected account | **~$0.0005–$0.01** |
| **Lead priority scoring** | PostgreSQL `scoring_configs` | **No LLM** | Every affected lead | **$0 API cost** |
| **Competitive recommendations / angle** | OpenAI Responses API | **GPT-5.4 mini** | Per affected account | **~$0.002–$0.02** |
| **AI outreach draft with competitive context** | Existing Signal prompt pipeline | **GPT-5 mini / GPT-5.4 mini** | Per draft | **~$0.002–$0.02** |
| **Call-prep / competitive talking points** | OpenAI Responses API | **GPT-5.6 Luna** | On demand | **~$0.0005–$0.01** |
| **Competitive event deduplication** | Application hash first; LLM only if needed | **GPT-5.6 Luna** | Only ambiguous duplicates | **~$0.0005–$0.005** |
| **Slack competitive digest** | Existing Slack pipeline | **No LLM / Luna for summarization** | Weekly | **~$0–$0.02** |
| **Cross-competitor feed** | PostgreSQL query | **No LLM** | Every page load | **$0 API cost** |
| **Affected-account fan-out** | PostgreSQL join | **No LLM** | Every event | **$0 API cost** |
| **CRM write-back** | HubSpot API | **No LLM** | Per write | HubSpot/API dependent; currently blocked |
| **Competitor CRUD / admin UI** | Supabase/Postgres | **No LLM** | Normal application usage | **$0 LLM cost** |
| **Evidence/source storage** | Supabase/Postgres | **No LLM** | Per source | **$0 LLM cost** |
| **Scheduled monitoring** | pg_cron + pg_net + Clay/OpenAI | Clay + Web Search | Weekly | Mainly Clay credits + ~$10 / 1,000 OpenAI searches |

---

## Confirmed Pricing (August 2026)

| Model | Input (per 1M tokens) | Output (per 1M tokens) | Source |
|---|---|---|---|
| **GPT-5 mini** | $0.25 | $2.00 | OpenAI official |
| **GPT-5.4 mini** | $0.75 | $4.50 | OpenAI official |
| **GPT-5.6 Luna** | $0.20 | $1.20 | OpenAI (July 30, 2026 price drop) |
| **Web Search (GPT-5 mini / GPT-5.4 mini)** | — | $10 / 1,000 calls | OpenAI |

---

## Rough Monthly Scenario

| Scenario | Estimated API Cost |
|---|---|
| 20 competitors × 1 battlecard/month | **~$0.20–$1.00** |
| 20 competitors × weekly monitoring | Mostly Clay cost + OpenAI search costs |
| 500 company mappings/month | **~$2.50–$25** |
| 1,000 competitive-aware outreach drafts/month | **~$2–$20** |
| **Likely total OpenAI spend for MVP** | **~$5–$50/month** |
| **Biggest unknown** | **Clay credit cost** |

---

## Key Takeaways

1. **GPT-5.6 Luna is now the cheapest option** — $0.20/$1.20 per 1M tokens after the 80% price drop. Use it for high-volume extraction, evidence processing, and lightweight reasoning.

2. **GPT-5.4 mini is the workhorse** — $0.75/$4.50 per 1M tokens. Use it for battlecards, company mapping, and outreach where you need stronger reasoning than Luna but don't need flagship GPT-5.

3. **Web Search is $10/1,000 calls** for reasoning models like GPT-5 mini and GPT-5.4 mini. This is the second-largest cost driver after Clay.

4. **The architecture already uses the right tiering** — mini for research/battlecards, Luna for high-volume extraction. No expensive frontier models needed for MVP.

5. **The biggest unknown is Clay credit cost** — get the actual per-run credit cost before Phase 3 as the research recommends.

---

*Note: Per-run costs are estimates based on typical token usage. Actual costs depend on prompt length, response length, and number of Web Search calls per run.*