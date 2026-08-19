# Signal Stack Design

Read this when the user is choosing which signals to monitor, or standing up a signal program from scratch.

## Contents

- Part 3: How to Identify the Right Signals for YOUR Business
- Step 1: Define Your Buying Motion
- Step 2: Map Your ICP to Signals
- Step 3: Prioritize by Signal Strength
- Step 4: Build Your Signal Stack
- Part 6: Implementation Playbook — Starting From Scratch
- Phase 1: Define Your Signal Stack (Day 1)
- Phase 2: Set Up Monitoring (Day 2–5)
- Phase 3: Build Scoring & Routing (Week 1–2)
- Phase 4: Optimize (Ongoing)
- Part 7: Pitfalls & Principles

---

## Part 3: How to Identify the Right Signals for YOUR Business

Don't start with a signal catalogue. Start with your business model.

### Step 1: Define Your Buying Motion

| Motion | Signal Focus | Example ICP |
|---|---|---|
| Outbound SDR (high volume) | Second-party social signals showing active shopping or change | B2B SaaS selling to RevOps/GTM buyers |
| ABM / Account-Based (named accounts) | Company-level third-party + second-party signals on a watchlist | Enterprise selling to Fortune 500 |
| PLG / Product-Led (self-serve → expand) | First-party usage signals in your own product | Developer tools, freemium SaaS |
| Expansion / Upsell (existing customers) | First-party usage + second-party hiring/growth signals | Customer Success at B2B SaaS |
| Churn Prevention | First-party usage decline + second-party job changes | Customer Success at any SaaS |
| Channel / Partner | Third-party market expansion signals | Reseller/VAR ecosystems |

### Step 2: Map Your ICP to Signals

Ask these five questions:

1. **What triggers a purchase?** New role? Funding? Competitor dissatisfaction? Compliance deadline? Growth milestone?
2. **Who experiences the pain?** What job titles? What does their day look like? Where do they complain?
3. **What does the buying committee look like?** Champion (user), economic buyer (VP/C-suite), technical evaluator. Signals for each differ.
4. **What's the typical timeline?** Impulse (SDR tools, under $500/mo), considered (2–6 weeks), enterprise (3–12 months). Short timelines need immediate signals; long timelines need early indicators.
5. **What would make someone switch from their current solution?** This is the most powerful signal category.

### Step 3: Prioritize by Signal Strength

Not all signals are equal. Rank yours:

| Tier | Signal Strength | Examples | Action |
|---|---|---|---|
| Tier 1 — Pull the trigger | Direct evidence of active buying or immediate need | "Looking for a new CRM," competitor dissatisfaction, champion just got budget authority, funding round closed | Reach out within 24 hours with specific context |
| Tier 2 — Nurture | Suggests a window may be opening | New role, company hiring for relevant function, conference attendance, relevant blog post | Add to nurture sequence, engage on social |
| Tier 3 — Accumulate | Weak individually, strong in combination | Liked your content, visited pricing page, following competitors, growing team | Monitor and surface when accumulation crosses threshold |

### Step 4: Build Your Signal Stack

Start with 5–8 signals that map to your top 3 buying triggers. Add more as you
validate conversion. The most common mistake is enabling 50 signals and drowning
in noise. **Precision > volume.**

---


## Part 6: Implementation Playbook — Starting From Scratch

### Phase 1: Define Your Signal Stack (Day 1)

- Pick 3–5 buying triggers specific to your business (e.g., "just hired a Sales Ops person," "competitor frustration," "raised Series A")
- Map each trigger to a signal from the catalogue above
- Assign each signal a tier (1/2/3) and a response SLA
- Document: what changes → who we contact → what we say

### Phase 2: Set Up Monitoring (Day 2–5)

- **the social signal provider:** Create a 100-profile ICP pilot with 5–8 Social Signals. Start with `changed_role`, `changed_company`, `buying_window`, `liked_competitor_content`, `posted_about_tracked_topic`, `company_started_hiring`. Estimate credits first, then provision.
- **Exa:** Set up a recurring research workflow for your top 20 target accounts — weekly check for news, funding, and executive changes.
- **Google Alerts:** Set up alerts for your top 10 competitors, top 20 target accounts, and 5 key industry terms.
- **Website:** Add Clearbit Reveal or RB2B for de-anonymizing website visitors.

### Phase 3: Build Scoring & Routing (Week 1–2)

- Create a scoring model: Tier 1 signals = instant route to outreach; Tier 2 = accumulate and surface; Tier 3 = log for pattern detection
- Set up Slack alerts for Hot signals
- Build CRM task creation for qualified signals
- Create the Google Sheet for tracking (see the social-signals-scoring-pipeline reference for the 5-tab sheet structure)

### Phase 4: Optimize (Ongoing)

- Track conversion: which signal types → meetings → pipeline → closed won
- Kill signals that never convert (precision > volume)
- Add signals that correlate with won deals
- Adjust scoring weights based on actual outcomes
- Expand monitored profiles as you validate the model

---

## Part 7: Pitfalls & Principles

1. **Precision over volume.** 5 high-quality signals > 50 noisy ones. Every false positive erodes trust in the system.
2. **Response time matters.** A Tier 1 signal responded to in 2 hours converts. Same signal in 2 days is dead.
3. **Context is everything.** A signal without "why this matters for them" is just noise. Every alert should include the reason and the recommended action.
4. **Don't over-automate early.** Run signals in semi-automated mode (alerts + manual review) for the first month. Automate routing once you've validated which signals actually convert.
5. **Second-party beats third-party for outbound.** Knowing a specific person just changed jobs (the social signal provider) is worth more than knowing a 500-person company is "showing intent" (third-party intent data). Person-level > account-level.
6. **Credit discipline.** Estimate before provisioning. Social Signals billing is per successful check, not per signal produced. Empty days still cost. Budget for max, not min.
7. **Signal decay is real.** A funding round from 6 months ago means nothing. A job change from 2 weeks ago is gold. Weight signals by recency.
8. **Combine, don't silo.** A single signal rarely converts alone. The magic is: funding round (Exa) + hiring SDRs (the social signal provider) + visited pricing page (website) = triple-confirmed buying window.

---
