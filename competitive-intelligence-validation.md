# Competitive Intelligence for Signal — Validated Research & Implementation Plan

**Status:** Validated against the actual `signal-L-S` codebase (branch `ui/company-detail-layout`, Aug 2026).
**Source under review:** `Signal-March_Competitive_Opportunity_Intelligence_Final.docx` ("the research doc").
**Audience:** engineers implementing the CI feature. Every claim below was checked against migrations, edge functions, and frontend code; file paths are verbatim.

---

## 1. Verdict (TL;DR)

The research doc's **core thesis is right and fits this codebase unusually well**: "one competitive event → intel, battlecard, affected accounts, recommendation, action" is exactly the loop Signal already runs for buying signals — just without a competitor entity. Its **product framing is wrong for this product**: it describes a standalone multi-tenant CI platform ("Signal-March") competing with Crayon/Klue, with five new top-level surfaces and technographic account mapping. Signal is a single-tenant SDR tool for L&S (a lighting manufacturer selling physical components), where:

- **Technographic competitor detection does not exist for this domain.** You cannot detect "this furniture maker uses Domus-style LED strips" from a website tag the way you detect "this SaaS company uses Salesforce". This invalidates the doc's cheapest mapping source and makes its 85%-precision target unrealistic for automated detection.
- **Most of the proposed architecture already exists** and must be reused, not rebuilt: signal ingestion (Clay → `signals-webhook` → `company_signals`), an AI web-search research agent with cost ledger (`company-intelligence` + `agent_runs`), a provider-ladder abstraction (`_shared/enrichment/providers/`), versioned config with admin UIs (`scoring_configs`, `prompt_configs`), a deterministic priority queue, and a full outreach/action/task/CRM layer.
- **The real gaps are small and specific:** no competitor entity anywhere in the schema; signals are snapshot-replaced with no history; signals feed nothing (not scoring, not drafts, not CRM); `signals_config` has no editor and is empty; there is no scheduled monitoring for signals; HubSpot write-back is a stub.

**Recommendation:** build CI as a thin extension of the existing signals + intelligence subsystem — one new `competitors` entity, one mapping table, append-only competitive events, an AI battlecard reusing the dossier pattern, and two integration hooks (draft context + priority scoring). No new top-level app, no new services, no new providers in the MVP.

---

## 2. What the research doc gets right, wrong, and misses

### 2.1 Right (keep these)

| Research claim | Codebase confirmation |
| --- | --- |
| "One event → multiple outputs" from a shared normalized event (§4) | Matches the existing pattern: one Clay signal row already renders as scored card + source link. Extending the same row to battlecard/impact/draft is natural. |
| "LLMs reason and generate; application code owns scoring, state, permissions, actions" (§10, §15) | This is literally the repo's architecture: `gpt-5-mini` generates, while `compute_lead_score` / `priority_queue` / `thread_action_state` (Postgres) own scoring, and edge functions own state. |
| "Explainable deterministic scoring" (§15) | Already implemented: `scoring_configs` (versioned JSON rules) → `compute_lead_score` / `compute_contact_score`, surfaced via `ScoreBreakdownTooltip`. A CI factor slots into this engine; do not invent a second scorer. |
| "Evidence before confidence; weak evidence must not become a sales claim" (§6) | Matches existing conventions: `company-intelligence` prompt hard rules ("Source every fact", "Never invent"), per-field `field_sources` provenance in enrichment, `needs_review` on contacts. Reuse `needs_review` semantics for suspected competitor mappings. |
| Lightweight AI-generated, evidence-backed battlecards, not a battlecard CMS (§8) | The dossier pattern (`companies.intelligence` jsonb + status + polling UI + source-tagged tabs) is a proven, cheap implementation vehicle. |
| The "what not to build" list (§14) | All correct, and mostly already true here (no crawling infra exists; the only web search is OpenAI's built-in tool). Keep every item. |
| Validate competitor-account mapping before building the opportunity layer (§13) | Correct discipline; adapt the experiment (see §7 below) — the 85% precision target needs revision for this domain. |

### 2.2 Wrong or inapplicable (reject or reframe)

1. **"Signal-March" as a CI platform / market positioning (§1–3, §11, §16).** Signal is an internal SDR tool for one client. "Feature parity with CI platforms" is not a goal; nobody is evaluating Signal against Klue. Strip all positioning; keep only the capability loop.
2. **Five new top-level product surfaces (§7: Intelligence, Alerts, Competitors, Opportunities, Actions).** Signal already has Actions (suggested queue + task inbox), Leads (prioritized list), and per-company detail. Adding four new sidebar sections would fragment the SDR workflow the app is built around (Leads → company detail → people → threads). CI must appear *inside* the existing surfaces plus at most **one** new admin-facing page (Competitors). No separate "Opportunities" page: the priority queue *is* the opportunities surface.
3. **`AccountCompetitor` via technographics/job postings (§6).** The doc's `source_type` list (technographic, job posting) is SaaS-shaped. L&S sells LED lighting systems to furniture makers, retailers, hotels, shipyards. Realistic evidence sources: CRM history (HubSpot closed-lost deals, engagement notes), sales-team knowledge (`companies.relationship_notes` exists today), news/press about supplier partnerships, product catalogs and fair exhibitions, and the AI web-search agent reading a prospect's product pages ("integrated lighting by X"). Evidence density is far lower; treat mappings as **research aids with confidence levels**, not facts.
4. **"3–5 high-value data sources, provider abstraction" as new build (§12 Foundation phase).** The provider abstraction already exists (`_shared/enrichment/providers/types.ts` — capability-declared providers, registry, ladder tables, quality gates). The realistic source list for MVP is exactly **two**: Clay (a new signals-style table) and the OpenAI `web_search` agent. Adding more sources is a config/registry entry later, not architecture.
5. **AI architecture table (§10) as new components.** Signal classifier, fact extractor, summarizer and scorer already run inside Clay for buying signals (`event_type`, `key_facts`, `summary`, `buying_intent_score`, `confidence` arrive precomputed). The outreach generator exists (`composePrompt` + `prompt_configs`). Only two functions are genuinely new: **impact reasoning** (event × mapped account) and **recommendation** — and both are prompt additions to existing pipelines, not new services.
6. **Alerts as a product surface (§7).** There is no notification infrastructure beyond the polling inbox bell and the Slack digest (`sdr-completion-digest` / `slack-sdr-completion`). Building an alerts center is unjustified; a CI section in the existing Slack digest + a badge on affected leads covers the MVP need.
7. **Weeks 1–12 timeline with "sequences" (§12).** Sequences do not exist in Signal and are out of scope (threads + tasks are the execution model). The Foundation phase is also overweight because most of it exists.

### 2.3 Missing from the research doc (the actual hard parts here)

1. **Signals have no history.** `signals-webhook` **deletes and reinserts** the `(cid, brand)` snapshot on every run (`20260805120000_company_signals.sql`: "Each run REPLACES the prior snapshot"). CI is fundamentally about *change over time*; an event log is a precondition. The doc assumes event persistence without noticing the existing table can't provide it.
2. **Signals feed nothing.** `company_signals` and `companies.intelligence` are read by exactly zero action-generation code paths — `fetchCompanyContext()` in `_shared/prompts.ts` (the sole company-context builder for all draft generators) selects neither. The doc's whole differentiator ("connect intelligence to action") requires building this bridge; it exists nowhere today.
3. **`signals_config` is an empty stub.** The per-brand ICP/"High Priority Signals"/"Scoring Preferences" sheet that steers Clay is three empty `{}` rows with no editor UI (the migration comment promises "Config → Signals"; no frontend code references the table). CI quality depends on this config; it must be built regardless.
4. **No scheduling for signals.** Only two pg_cron jobs exist (`sync-mailbox-replies`, `run-enrichment-pump`). "Competitor monitoring" (continuous detection) requires a scheduled runner — new, but the pg_cron + pg_net pattern is established.
5. **The brand dimension.** Signals are scoped `(cid, brand)` with `brand ∈ {L&S, Visplay, FLUX}` derived from the SDR's email domain. Competitors differ per brand/segment (HOME vs COMMERCIAL vs INDUSTRIAL). The doc's flat competitor model needs a brand/segment axis.
6. **HubSpot write-back is a stub.** `_shared/hubspot-writeback.ts` logs what it *would* send, gated on `HUBSPOT_WRITEBACK_ENABLED`; real CRM write-back is still the legacy Dynamics path. Any "push competitive intel to CRM" item inherits this blocker.
7. **Detection logic lives in Clay, outside the repo.** The signals classifier/scorer is a Clay workspace function — unversioned, not code-reviewed, not reproducible from the repo. The doc never addresses ownership of detection logic. Decision needed (see §9 unknowns).
8. **Deployment/config hygiene issues to fix in passing:** `signals-webhook`, `run-company-signals`, `company-intelligence` are **not** in `config.toml`'s `verify_jwt = false` list (unlike `enrichment-webhook`/`apollo-phone-webhook`) — verify how the Clay callback currently authenticates in production and make config.toml the source of truth. `supabase/types.ts` is stale (misses `company_signals`, `signals_config`, all `hubspot_*` tables), forcing `sb` casts everywhere; regenerate it.

---

## 3. Current-state inventory (what exists and matters for CI)

### 3.1 The buying-signals subsystem (the template to extend)

- **Flow:** UI button → `run-company-signals` (POST `{cid, brand}`; reads `leads_page_view` + `signals_config`; POSTs to `CLAY_SIGNALS_WEBHOOK_URL` with `ref = cid`) → Clay computes → `signals-webhook` (`?token=CLAY_WEBHOOK_TOKEN&ref=cid`; unwraps payload; resolves company by cid → domain → name-ilike; **snapshot-replaces** `company_signals` for `(cid, brand)`) → `companies.signals_status = done` → UI polls every 4 s.
- **Table:** `company_signals(id, cid, brand, event_type /*free text*/, headline, summary, reasoning, is_buying_signal, buying_intent_score, confidence, published_date, source_name, source_url, city, country, key_facts jsonb, raw jsonb, ...)`. No FK, no taxonomy, no history, RLS read for authenticated.
- **UI:** `components/lead-company-detail/company-tabs/section.tsx` — segmented tabs `research | signals`; `company-signals/section.tsx` renders scored cards (≥80 emerald / ≥60 amber), headline → `source_url` link, meta row. Brand picker + Run button (admin only).

### 3.2 The AI research agent (the battlecard vehicle)

- `company-intelligence` edge function: OpenAI Responses API, `gpt-5-mini`, `reasoning.effort medium`, `tools:[{type:"web_search"}]`, `max_output_tokens 16000`, runs under `EdgeRuntime.waitUntil`, UI polls `intelligence_status`. Web search can't combine with JSON mode → `extractJson()` fence-stripping.
- Output: source-tagged dossier in `companies.intelligence` jsonb (`summary, confidence, headquarters, offices[], corporate{}, firmographics, products[], customers[], recent_signals[], ls_angle{}, placeholders{recent_project, specific_detail}, sources[]`). **No competitors key today.**
- `placeholders.*` already flows into outreach templates (`{{recent_project}}`, `{{specific_detail}}` in `use-thread-templates.ts`) — the proven route for getting research facts into messages.
- Every run writes an `agent_runs` cost row (`input/output/reasoning tokens, web_search_calls, cost_usd`; rates inline: $0.25/1M in, $2/1M out, $10/1k searches). **Any CI agent must write here.**

### 3.3 Scoring, prioritization, actions, CRM

- Deterministic scoring engine: `scoring_configs` (versioned JSON, admin editor at `/signal/config`) → SQL `compute_lead_score` / `compute_contact_score` → `company_scores`, `contact_scores`, `person_priority_queue(_mat)` → `suggested_persons` (the Actions "Suggested" tab). Signals do not participate.
- Draft pipeline: `fetchCompanyContext()` (`_shared/prompts.ts`) + `composePrompt()` (`_shared/prompt-compose.ts`) + versioned `prompt_configs` → `gpt-5-mini` JSON drafts → `threads`/`messages` → Outlook push → `record-outreach` → tasks + CRM.
- CRM: HubSpot is a **read mirror** (`hubspot_companies/_contacts/_deals/_engagements/_owners` via `hubspot-incremental-sync`) + promotion source (`promote-hubspot-lead`); write-back is the stub above; live write-back is legacy Dynamics (`crm-sync`, being retired).
- Config-in-table precedents for taxonomies: `persona_bucket`/`persona_bucket_rule` (INSERT to extend, no migration) — the right pattern for a CI event-type taxonomy.
- Provider abstraction: `_shared/enrichment/providers/` registry + `enrichment_ladders` + quality gates — reuse if CI ever needs multi-provider collection; do not rebuild.

---

## 4. Capability-by-capability feasibility

| Research capability | Feasible without breaking anything? | How, concretely |
| --- | --- | --- |
| Competitor monitoring | **Yes (scheduled, not continuous)** | New `run-competitor-signals` mirroring `run-company-signals` (Clay table keyed by competitor domain) and/or a `competitor-intelligence` web_search agent; pg_cron weekly. No crawling infra — correct per the doc's own "don't build" list. |
| Intel feed | **Yes, later** | A filtered view over an append-only `competitor_signals` table. Requires history first. MVP: feed lives on the competitor page; a cross-competitor feed page is Phase 3. |
| Alerts | **Lite only** | Reuse the Slack digest pattern (`sdr-completion-digest`) with a CI section; in-app = badge on affected leads + inbox-bell-style polling if ever needed. No alerts center. |
| Battlecards | **Yes — cheapest win** | `competitors.battlecard jsonb` via a dossier-style web_search agent; UI clones `company-intelligence` tabs; "live" sections join `competitor_signals` + mapped accounts at render time. |
| Account impact (AccountCompetitor) | **Partially — the data is the risk, not the code** | `company_competitors(cid, competitor_id, relationship, confidence, evidence, needs_review)`. Populated from: HubSpot mirror (closed-lost deals/engagement text — verify fields exist), `relationship_notes`, dossier prompt extension (ask the agent for incumbent lighting suppliers with sources), manual entry by sales. Automated-only mapping will not hit 85% precision in this domain. |
| Opportunity scoring | **Yes** | A new optional factor in `scoring_configs` (e.g. `competitive_signal` bands on recent event score for mapped competitors), flowing through the existing `compute_lead_score` → priority queue → `ScoreBreakdownTooltip`. Deterministic and explainable, as the doc demands. |
| Recommendations | **Yes** | Prompt addition: inject competitive context into `fetchCompanyContext()`/`composePrompt()` (a `<competitive_context>` block) so cold/follow-up drafts and call prep carry the angle. Optionally a `competitive_angle` field on the mapping row, AI-suggested, human-editable. |
| Outreach / tasks | **Already exists** | Threads, drafts, templates (add `{{competitor_name}}`, `{{competitive_angle}}` placeholders), thread_tasks. Build nothing new. Sequences: out of scope. |
| CRM write-back of CI | **Blocked** | Depends on un-stubbing `hubspot-writeback.ts`. Do not couple CI's MVP to it; write a note via the existing seam once write-back ships. |

---

## 5. Recommended architecture

### 5.1 Data model (new tables — SQL sketch, follow migration + rollback conventions)

```sql
-- competitors: the entity that does not exist today. Small, manually seeded set.
create table public.competitors (
  id bigint generated always as identity primary key,
  name text not null,
  domain text,
  linkedin_url text,
  brands text[] default '{}',            -- which L&S brands compete: L&S / Visplay / FLUX
  segments text[] default '{}',          -- HOME / COMMERCIAL / INDUSTRIAL
  status text not null default 'active', -- active | archived
  battlecard jsonb,                      -- dossier-pattern AI battlecard, source-tagged
  battlecard_status text,                -- researching | done | error  (mirror intelligence_status)
  battlecard_updated_at timestamptz,
  signals_status text,                   -- running | done | error (mirror companies.signals_status)
  signals_updated_at timestamptz,
  notes text,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);

-- company_competitors: the AccountCompetitor of the research doc, adapted.
create table public.company_competitors (
  id bigint generated always as identity primary key,
  cid bigint not null,
  competitor_id bigint not null references public.competitors(id),
  relationship text not null default 'suspected',
    -- incumbent_supplier | evaluating | former_supplier | suspected
  confidence integer,                    -- 0-100
  evidence jsonb default '[]',           -- [{source_type, source_url, text, detected_at}]
  source text,                           -- ai_research | hubspot | manual | clay
  needs_review boolean not null default true,
  verified_by uuid, verified_at timestamptz,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),
  unique (cid, competitor_id)
);

-- competitor_signals: append-only competitive events (NOT snapshot-replaced).
create table public.competitor_signals (
  id bigint generated always as identity primary key,
  competitor_id bigint not null references public.competitors(id),
  event_type text,                       -- validated against ci_event_types (config-in-table)
  headline text, summary text, reasoning text,
  importance integer, confidence integer,
  published_date timestamptz,
  source_name text, source_url text,
  key_facts jsonb, raw jsonb,
  dedupe_key text generated always as (md5(coalesce(source_url,'') || coalesce(headline,''))) stored,
  run_id uuid,                           -- which collection run produced it
  status text not null default 'new',    -- new | reviewed | dismissed
  created_at timestamptz not null default now(),
  unique (competitor_id, dedupe_key)
);

-- ci_event_types: taxonomy via the persona_bucket config-in-table pattern (INSERT to extend).
create table public.ci_event_types (
  key text primary key,                  -- pricing_change, product_launch, leadership_change, ...
  label_en text, label_it text,
  default_importance integer,
  enabled boolean not null default true
);
```

Notes:
- **Affected accounts are a join, not a table:** `competitor_signals × company_competitors` gives "this event touches these leads". No opportunity table in MVP; the priority queue is the opportunity surface.
- Match repo conventions: RLS read policies for `authenticated`, service-role writes, `NOTIFY pgrst, 'reload schema'`, rollback files in `supabase/rollbacks/`, migrations user-applied.
- Keep `company_signals` untouched for buying signals; separately, add history there in Phase 0 (below) since CI thinking exposed that flaw.

### 5.2 Backend (edge functions — all follow the existing handler shape)

| Function | Modeled on | Purpose |
| --- | --- | --- |
| `competitor-intelligence` | `company-intelligence` (near-clone) | Web-search agent → `competitors.battlecard` jsonb; writes `agent_runs`. Prompt asks for: positioning, product lines vs L&S/Visplay/FLUX, pricing posture, strengths/weaknesses, recent moves, recommended counters — every fact sourced. |
| `run-competitor-signals` + reuse `signals-webhook` (new `?kind=competitor` branch) or a sibling `competitor-signals-webhook` | `run-company-signals` / `signals-webhook` | Clay-driven event collection per competitor; **insert with `on conflict (competitor_id, dedupe_key) do nothing`** — append-only. |
| `ci-monitor` (pg_cron, weekly) | `run-enrichment-pump` pattern | Iterates active competitors, kicks `run-competitor-signals` (and battlecard refresh monthly). Respect edge time limits: one competitor per invocation chunk, self-chain like `run-enrichment`. |
| Extension of `company-intelligence` prompt | — | Add `competitors: [{name, relationship_hint, evidence, source}]` to the dossier schema; a small post-step upserts `company_competitors` rows (`source: 'ai_research'`, `needs_review: true`, matching against the seeded competitor set by name/domain). |
| Extension of `fetchCompanyContext()` | — | Select verified `company_competitors` (+ top recent `competitor_signals`) and emit a `<competitive_context>` block in `formatCompanyXml`; add prompt-config copy so writers use it ("if an incumbent supplier is known, position against it factually, never disparage"). |

Config: add all new functions **and the existing signals trio** to `config.toml` `verify_jwt` as appropriate; webhook auth via the established shared-token query param.

### 5.3 Frontend (component-structure convention applies)

| Surface | Where it goes | What it shows |
| --- | --- | --- |
| **Competitors admin page** (the one new page) | `src/components/competitors/` + route `/signal/competitors` (admin-gated like `/signal/hubspot`) | List of seeded competitors; detail view cloning the `company-intelligence` tab pattern: Battlecard tabs (source-tagged, `<Src url>` from `dossier-parts.tsx`), live signals feed (card list cloning `company-signals/section.tsx`), affected accounts table (join view, rows linking to `/signal/leads/company/:cid`). Run/refresh buttons + status polling identical to existing patterns. |
| **Lead company detail** | Extend `company-tabs/section.tsx` with a third tab `competitive` (or fold into `signals`) | Known/suspected competitor relationships with confidence + evidence + review/confirm/reject buttons (admins), recent events for mapped competitors, and the competitive angle used in drafts. Add a `data-tour` anchor. |
| **Leads list** | Optional small badge column when a lead has a verified incumbent competitor with a fresh event | Do not redesign the table; one pill like the existing SAL/CRM badges. |
| **Config → Signals** | New admin section (finally building the promised editor) | Edits `signals_config` per brand **and** the CI knobs (`ci_event_types`, monitoring cadence). Follows the `scoring_configs` list/detail editor pattern. |
| **Slack digest** | Extend `sdr-completion-digest` | "Competitive this week" section: new high-importance events + counts of affected assigned leads per SDR. |

i18n: every label through `tu()`/`tut()` in `app-ui-i18n.ts` (extend the existing `signals*`/`ci*` key families); 11-locale files get fallbacks automatically.

### 5.4 How CI fits the existing workflows (the fit statement)

- **Leads:** unchanged flow; competitive presence is one more explainable score factor and one badge. SDRs keep working the same queue — accounts affected by a fresh competitive event simply rank higher, with the reason visible in the score tooltip.
- **Company detail:** the existing research/signals tab row gains the competitive lens; evidence-first, admin-reviewable.
- **Signals:** buying signals (demand-side, per-lead) and competitive signals (supply-side, per-competitor, fanned out to leads) stay separate tables with the same visual language and the same Clay/webhook plumbing.
- **SDR execution:** nothing new to learn — drafts and call prep silently improve because `composePrompt` now knows the incumbent and the angle; templates gain two placeholders; tasks/threads/CRM flow untouched.

---

## 6. MVP definition

### Build now (MVP)

1. **Phase 0 hygiene (prereqs, ~2–3 days):**
   - Make `company_signals` history-safe (add `run_id`/`superseded_at` and stop hard-deleting, or archive to `company_signals_history` on replace).
   - Build the missing **Config → Signals** editor for `signals_config` (CI quality depends on it; buying-signal quality already does).
   - Fix `config.toml` `verify_jwt` entries for the signals trio; regenerate `supabase/types.ts` (project ref `bhbmjxmfeaxnaizenuaf`).
2. **Competitor foundation:** `competitors` + `company_competitors` + `competitor_signals` + `ci_event_types` tables; manual seeding of the competitor set (sales/EMK provide it — it is small); Competitors admin page; `competitor-intelligence` battlecard agent.
3. **Mapping with review:** dossier prompt extension writing suspected `company_competitors` (needs_review), HubSpot-mirror mining script for closed-lost/engagement evidence (read-only, uses existing `hubspot_deals`/`hubspot_engagements`), review UI on lead detail.
4. **Action hook:** `<competitive_context>` in `fetchCompanyContext` + prompt-config copy + `{{competitor_name}}`/`{{competitive_angle}}` template placeholders.
5. **Validation gate (§7)** before Phase-3 investment.

### Build later

- Scheduled monitoring (`ci-monitor` cron) + Slack digest section — after the manual-run loop proves signal quality.
- `competitive_signal` scoring factor in `scoring_configs` + leads-list badge — after mappings are reviewed and trustworthy.
- Cross-competitor intel feed page; event → affected-accounts fan-out view.
- HubSpot write-back of competitive notes — after `hubspot-writeback.ts` is un-stubbed (separate workstream).
- Additional data sources via the enrichment provider registry, if validation shows Clay + web_search insufficient.

### Do not build (agreeing with and extending the research doc)

- A standalone CI app / five new surfaces; a separate Opportunities page; an alerts center.
- Sequences; technographic providers; proprietary crawling; MCP/assistant infra; battlecard governance/CMS; win/loss suites; predictive forecasting; 100+ sources; custom taxonomy *systems* (the `ci_event_types` table is enough).
- A second scoring engine or any scoring outside `scoring_configs`.

---

## 7. Validation experiment (adapted from research §13)

The research doc's gate is right; its target is not. Adapted:

1. Sales/EMK provide **30–50 accounts with known incumbent lighting suppliers** (from HubSpot history and rep knowledge) plus the competitor list.
2. Run the extended dossier agent + HubSpot-mirror mining over those accounts *blind*.
3. Measure precision/recall of `relationship = incumbent_supplier` suggestions.
4. **Realistic targets:** ≥85% precision is plausible for *"a competitor relationship exists"* when evidence is CRM-history-based; for pure web-research detection expect materially lower recall — that is acceptable because mappings are review-gated research aids, never auto-asserted sales claims (the doc's own "evidence before confidence" principle, already enforced culturally by the `company-intelligence` hard rules).
5. If web-research precision is poor, MVP still stands on CRM-derived + manual mappings; only the automated-detection ambition shrinks.

---

## 8. Phased implementation plan (engineering changes)

| Phase | Scope | Key changes (files/areas) |
| --- | --- | --- |
| **0 — Hygiene** (days) | Signal history, signals config editor, deploy config, types | Migration + rollback for `company_signals` history; `signals-webhook/index.ts` (stop delete); new `components/signals-config/` admin section + route; `config.toml`; `bunx supabase gen types ... --project-id bhbmjxmfeaxnaizenuaf` |
| **1 — Competitor foundation** (~2 wks) | Tables, seed, battlecard, admin page | 4 migrations + rollbacks (§5.1); `functions/competitor-intelligence/` (clone of `company-intelligence`, writes `agent_runs`); `components/competitors/` per component-structure spec; sidebar `navigation.json` + role gating in `app-auth.ts`; i18n keys |
| **2 — Mapping + lead surface** (~2 wks) | company_competitors population + review + draft context | `company-intelligence/index.ts` prompt/schema extension + upsert step; one-off `functions/scripts/mine-hubspot-competitors.ts` (pattern: `scripts/hubspot-backfill.ts`); `company-tabs` third tab + review components; `_shared/prompts.ts` (`fetchCompanyContext`, `formatCompanyXml`) + `prompt-defaults.ts` copy; template placeholders in `config-templates/types.ts` + `use-thread-templates.ts` |
| **⛔ Validation gate** | §7 experiment; go/no-go on automation depth | — |
| **3 — Monitoring + opportunity** (~2–3 wks) | Events pipeline, cron, digest, scoring | `functions/run-competitor-signals/` + webhook branch (append-only, dedupe); Clay competitor-signals table (workspace work); `ci-monitor` + pg_cron migration (self-chaining like `run-enrichment`); `sdr-completion-digest` CI section; `scoring_configs` new factor + `compute_lead_score` SQL + `scoring-default-rules.ts` + `ScoreBreakdownTooltip` row; leads badge |
| **4 — Later** | Feed page, fan-out view, HubSpot write-back, extra providers | Only after 0–3 prove value |

Non-breaking by construction: every phase is additive (new tables, new functions, new tabs); the only touched shared code paths are `fetchCompanyContext` (additive XML block) and the scoring rules (a disabled-by-default factor, togglable in the admin editor). `bun tsc --noEmit` green per commit; `deno check` on all edge functions before deploy (known RCA).

---

## 9. Risks, dependencies, costs, unknowns

**Risks**
1. **Mapping precision in a physical-goods domain** — the top product risk; mitigated by review-gating, CRM-first evidence, and the validation gate. If mapping stays sparse, battlecards + draft context still deliver value without it.
2. **Detection logic ownership in Clay** — unversioned, opaque, per-run credit costs. Mitigation: keep the web_search agent as the in-repo alternative; decide per capability (battlecards in-house; volume event monitoring wherever cheaper).
3. **Append-only event growth + dedupe quality** — `dedupe_key` on (source_url, headline) is crude; near-duplicate stories will leak through. Acceptable for MVP volumes (≤ ~20 competitors); revisit with an embedding/LLM dedupe pass only if it hurts.
4. **Edge-function limits for fan-out** — batch + self-chain like `run-enrichment`; never loop all competitors in one invocation.
5. **Snapshot semantics regression** — Phase 0 change to `signals-webhook` must keep the UI's "checked, none vs never ran" behavior intact.

**Dependencies:** competitor list + known-relationship ground truth from L&S sales/EMK; Clay workspace access to build the competitor-signals table; HubSpot property audit (does a closed-lost-reason/competitor field exist?); the HubSpot write-back workstream (for CRM push only).

**Costs (bounded, measurable via `agent_runs`):** battlecard generation ≈ company-intelligence dossier cost per run (gpt-5-mini + web_search at $10/1k calls; observed dossiers cap at 16k output tokens) — with ~10–20 competitors refreshed monthly this is trivial. The variable cost is Clay credits for event monitoring: **get the per-run credit cost from the Clay workspace before Phase 3** and set cadence accordingly. Company-mapping research piggybacks on dossier runs already being paid for.

**Unknowns to resolve with the client:** the competitor set per brand/segment; how reps identify incumbent suppliers today; HubSpot fields carrying competitor/loss data; Italian/German-language source coverage in Clay and OpenAI web_search; desired monitoring cadence; who owns mapping review (admins vs SDRs).

---

## 10. Appendix — research-concept → codebase reuse map

| Research doc concept | Existing asset to reuse | Net-new |
| --- | --- | --- |
| Competitive event ingestion | `run-company-signals` / `signals-webhook` / Clay plumbing | competitor-keyed variant, append-only |
| Signal classifier/extractor/summarizer | Clay signals function output columns | taxonomy table + config sheet |
| Battlecards | `company-intelligence` dossier pattern + `dossier-parts.tsx` UI + `agent_runs` | competitor prompt + page |
| AccountCompetitor + CompetitiveEvidence | `needs_review`, `field_sources` provenance culture, `relationship_notes`, HubSpot mirror tables | `company_competitors` table + review UI |
| Opportunity scoring | `scoring_configs` → `compute_lead_score` → priority queue → `ScoreBreakdownTooltip` | one factor |
| Recommendations | `composePrompt` + `prompt_configs` + lenses | `<competitive_context>` block |
| Outreach generation/execution | threads, drafts, templates (+placeholders), Outlook push, `record-outreach`, `thread_tasks` | two template tokens |
| Alerts | `sdr-completion-digest` Slack pipeline, inbox bell pattern | one digest section |
| Provider abstraction | `_shared/enrichment/providers/` registry + ladders | nothing (until new sources) |
| Monitoring/scheduling | pg_cron + pg_net pattern (`run-enrichment-pump`) | one cron + runner |
| Cost control | `agent_runs` ledger | nothing |
| Config/taxonomy administration | `scoring_configs`/`prompt_configs` editors, `persona_bucket` config-in-table | Signals/CI config section |
