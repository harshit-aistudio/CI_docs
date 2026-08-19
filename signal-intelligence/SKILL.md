---
name: signal-intelligence
description: >-
  Detects live buying signals for a company, person, or ICP list and returns them
  scored, tiered, and ready to action. Use this whenever the user asks what's
  changed at an account, why now for a prospect, who to reach out to and when, or
  wants a prospect list researched, an account prioritised, or a trigger found for
  outreach — even if they never say the word "signal." Also use for scoring or
  routing signals they already have, designing a signal stack, or turning a signal
  into a message. Trigger on phrases like "why now", "what's going on at", "warm
  leads", "buying intent", "trigger event", "who's in market", "research these
  accounts", "is this account worth working", "account prioritisation".
---

# Signal Intelligence

Find observable evidence that a person or company is entering a buying window,
turn it into scored and evidenced records, and hand back something actionable.

## Non-Negotiable: Source Independence

This skill defines the **detection methodology, evidence model, scoring, and
action logic**. Tools are interchangeable implementations of it.

Never reduce detection to one vendor's API schema. If a preferred provider is
unavailable, degrade in this order and say which tier you used:

1. Another connected signal provider
2. Public web/search (news, filings, job boards, press)
3. First-party company sources (the target's own site, changelog, careers page)
4. CRM / product / support / call data, if connected
5. Mark the signal `unverified` and say why

A signal with a weaker source is still a signal. A signal with no evidence URL is
not a signal — it's a guess, and you should drop it rather than pad the output.

## What Counts as a Signal

An observable event suggesting someone is entering a buying window, has a problem
you solve, or is receptive right now. It supplies the *why now*.

Four tests — a signal that fails any of them is noise:

- **Observable** — detectable publicly or from data the user owns
- **Timely** — something changed recently, creating urgency
- **Relevant** — connects to a problem the user's product solves
- **Actionable** — implies who to contact and what to say

**Taxonomy.** First-party = the user's own data (CRM, product, billing, support,
calls); zero latency, highest trust. Second-party = the target's public social and
professional activity. Third-party = news, funding, filings, jobs, events. Read
`references/signal-catalogue.md` for the full 200+ signal list by use case.

---

## The Detection Workflow

### Step 1 — Establish the frame

Before detecting anything, you need two inputs. Ask only for what's genuinely
missing; infer the rest from context.

- **Targets:** named accounts, specific people, or an ICP description
- **What the user sells:** without this, relevance can't be judged and every
  output degenerates into generic company news

If the user gave you an ICP but no accounts, say so and either build a candidate
list from available sources or ask them to supply one. Don't silently invent
companies.

### Step 2 — Select signals before scanning

Do not scan for everything. Pick **5–8 signals** that map to the user's top
buying triggers. Precision beats volume; enabling 50 signals produces noise that
buries the real ones.

Match motion to signal focus:

| Motion | Focus on |
|---|---|
| Outbound SDR | Second-party: active shopping, role change |
| ABM / named accounts | Company-level third-party + second-party on a watchlist |
| PLG / expansion | First-party usage, then hiring/growth |
| Churn prevention | First-party usage decline + champion job change |

For deeper selection logic, read `references/stack-design.md`.

### Step 3 — Inventory your tools, then detect

Check what's actually available this session before planning detection. Search
connectors if a relevant one might exist. Then run detection across the sources
you have, and note which planned signals you couldn't check.

Read `references/source-playbooks.md` for what each source detects and how to
configure it — but only the sub-section for the source you're using.

When detecting from public web, search per target and per signal type rather than
one broad query; broad queries return surface-level results across all of them.

### Step 4 — Normalize every hit

Every detected signal becomes one record in this shape, whatever the source:

```
signal_type      canonical name, e.g. role_change, funding_round, competitor_engagement
party            first | second | third
entity           person or company it attaches to
account          the company in play
detected_on      date the event happened (not the date you found it)
source           where it came from
evidence_url     link a human can verify — required
confidence       high | medium | low | unverified
summary          one line: what changed
```

Two records describing the same event from different sources are one signal with
two evidence URLs — not two signals. Deduplicate before scoring, or corroboration
scoring will double-count.

### Step 5 — Score and tier

Scoring principles, in priority order:

- Weight direct buying evidence highest
- Weight person-level change above generic company activity
- Apply recency decay — a 90-day-old signal is not a live signal
- Reward corroboration from **independent** sources
- Penalize weak, stale, duplicate, or ambiguous evidence
- Never let an opaque vendor confidence score override evidence quality

| Tier | Meaning | Action |
|---|---|---|
| **1 — Trigger** | Direct evidence of active buying or immediate need | Reach out within 24h with specific context |
| **2 — Nurture** | A window may be opening | Nurture sequence, engage socially |
| **3 — Accumulate** | Weak alone, strong combined | Monitor; surface when accumulation crosses threshold |

### Step 6 — Return something actionable

Default output is a table ranked by tier then recency:

| Account | Person | Signal | Tier | Detected | Evidence | Why now |
|---|---|---|---|---|---|---|

Then, briefly: coverage gaps (which signals you couldn't check and why), and the
single highest-value next action.

If asked for outreach, read `references/outreach-framework.md`. The core rule is
**Signal → Pain → Bridge → Ask** — never open with the signal itself. Some
signals (layoffs, bad quarters, churn indicators) shape strategy but must never
appear in the message.

---

## Pitfalls

- **Stating the signal back to the target.** "Congrats on the new role!" wastes
  the advantage. The signal is context, not content.
- **Padding.** Six evidenced signals beat thirty speculative ones. Volume here
  reads as low quality, not thoroughness.
- **Inferring intent from presence.** A company having a careers page is not a
  hiring signal. Something must have *changed*.
- **Stale signals presented as live.** Always surface `detected_on`, and say when
  a signal is old.
- **Losing the evidence URL.** Unverifiable claims are worse than no claim, since
  the user will act on them.

## Reference Files

| File | Read when |
|---|---|
| `references/signal-catalogue.md` | Choosing signal types; 200+ by use case |
| `references/source-playbooks.md` | Detecting from a specific source, or setting one up |
| `references/stack-design.md` | Designing a signal stack, or starting from scratch |
| `references/outreach-framework.md` | Converting a signal into a message |
