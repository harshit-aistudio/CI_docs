# Source Playbooks & Setup Guides

How to detect signals from each source, plus exact configuration steps. Part 5 = what each source detects. Part 8 = how to set it up. Read only the sub-section for the source you're actually using.

## Contents

- Part 5: Tool-Specific Tracking Playbooks
- 5A: Social Signal Provider (Social Signals + Social Listening)
- 5B: Exa (Web Search for Third-Party Signals)
- 5C: GTM Grid (Signal Combination & Enrichment)
- 5D: Smuggler.dev (LinkedIn Activity Signals)
- 5E: Fireflies (Call-Based Signals)
- 5F: Other Tools Quick Reference
- Part 8: Setup Guides — How to Actually Configure Each Signal Source
- 8A: Public Social & Professional Activity — Provider-Independent Setup
- 8B: Social Signal Provider Keyword Searches — Exact Setup
- 8C: Exa — Recurring Signal Queries
- 8D: Google Alerts — Free Setup (5 Minutes)
- 8E: Smuggler.dev — LinkedIn Activity Monitoring
- 8F: Signal Combination & Scoring
- 8G: Fireflies — Call Signal Setup
- 8H: Apify / Theirstack — Job Board & Event Scraping

---

## Part 5: Tool-Specific Tracking Playbooks

### 5A: Social Signal Provider (Social Signals + Social Listening)

The social signal provider gives you 15 native signal types across person and company monitoring.
These are second-party signals — public social/professional activity.

**The social signal provider Person/Profile Signals**

| Signal Type | What It Detects | Required Config | Best For |
|---|---|---|---|
| `changed_role` | Person changed job title or got promoted | None | Timing outreach to new decision-makers |
| `changed_company` | Person moved to a new company | None | Fresh start = stack evaluation window |
| `became_hiring` | Person posted a job ad | `jobKeywords[]`, `locations[]` | Founder/hiring manager hiring = growth |
| `liked_competitor_content` | Person engaged with competitor's LinkedIn posts | `competitorCompanyUrls[]` | Active shopping behaviour |
| `liked_tracked_company_content` | Person engaged with your company's posts | `trackedCompanyUrls[]` | Warm leads, brand awareness |
| `liked_tracked_person_content` | Person engaged with a tracked individual's posts | `trackedPersonUrls[]` | Influence mapping |
| `commented_on_tracked_content` | Person commented on specific tracked posts | `trackedPostUrls[]` | Deep engagement signal |
| `posted_about_tracked_topic` | Person posted about a topic you're tracking | `topicKeywords[]` | Thought leadership, pain point articulation |
| `competitor_engagement` | Person engaged with competitor content above threshold | `threshold`, `competitorCompanyUrls[]` | Composite: high-intent shopping |
| `buying_window` | Person matched topic + role keywords — explicit intent | `threshold`, `topicKeywords[]`, `jobKeywords[]` | Strongest intent signal the social signal provider offers |
| `influence` | Person has thought leadership on a topic | `threshold`, `topicKeywords[]` | Partnership, content collaboration |

**The social signal provider Company/Account Signals**

| Signal Type | What It Detects | Required Config | Best For |
|---|---|---|---|
| `company_started_hiring` | Company posted jobs matching your keywords | `jobKeywords[]`, `locations[]` | Growth signal = tooling need |
| `company_jobs_count_increased` | Company's job count increased above threshold | `threshold`, `jobKeywords[]`, `locations[]` | Acceleration signal |
| `company_posted_relevant_initiative` | Company posted about a relevant initiative | `initiativeKeywords[]` | Strategic alignment |
| `expansion` | Company expanding to new locations | `threshold`, `locations[]` | Geographic growth |

**The social signal provider Social Listening (Keyword-Based)**

Beyond native signals, the social signal provider's search + workflow engine captures:

- **Brand mentions** — who's talking about you, where, with what sentiment
- **Competitor mentions** — who's talking about competitors
- **Pain point discovery** — people posting about problems you solve
- **Comparison shopping** — "X vs Y" discussions
- **Feature requests** — what people wish existed
- **Question mining** — people asking questions your product answers

Setup pattern:

1. Create a search that casts a broad net (keywords + platform)
2. Build a workflow that uses AI to extract specific signals from those results
3. Route qualified signals to Slack, CRM, or email

**Credit economics (as of 2026):**

- Social Signals: 1–32 credits per target per day depending on signal types enabled
- `buying_window` is the most expensive (17–32 credits/day — AI + enrichment)
- `changed_role` and `changed_company` are the cheapest (1 credit/day each)
- Estimate before provisioning — use the `/v1/social-signals/estimate` endpoint

**The social signal provider as a Second-Party Signal Engine: The Architecture**

```
ICP List (LinkedIn URLs)
    ↓
Social Signals Subscriptions (15 signal types, daily checks)
    ↓
Signal Results (typed, scored, with evidence URLs)
    ↓
Scoring + Routing Logic (tiered: Hot/Warm/Manual)
    ↓
Downstream Action: CRM task | Slack alert | Email | Smuggler/Lemlist campaign
```

Proven pattern from the social signal provider's own ICP pilot (100 B2B SaaS GTM profiles, 8 signals):

- **Tier 1 (pull trigger):** `buying_window`, `liked_competitor_content`, `changed_role`, `company_started_hiring`
- **Tier 2 (nurture):** `posted_about_tracked_topic`, `changed_company`, `company_jobs_count_increased`
- **Tier 3 (accumulate):** `engaged_with_tracked_topic`
- **Cost:** ~3–9k credits/day for 100 profiles
- **Routing:** Smuggler A "Hot" / Smuggler B "Warm" / Manual queue

### 5B: Exa (Web Search for Third-Party Signals)

Exa is your search engine for third-party signals — news, funding, press
releases, job postings, regulatory filings, and everything that lives on the
public web outside social platforms.

| Signal Category | Exa Search Approach | Example Query |
|---|---|---|
| Funding rounds | Search for "[company] funding" + time filter | `"Acme Corp" funding round 2026` |
| Executive hires | Search for "[company] appoints/hires [title]" | `"hires" "chief revenue officer" SaaS` |
| Product launches | Search for "[company] launches/announces [product]" | `"Clay" launches new feature` |
| M&A activity | Search for "[company] acquires/merges" | `"Apollo" acquires` |
| Layoffs | Search for "[company] layoffs/restructuring" | `"tech layoffs" June 2026` |
| Legal/regulatory | Search for "[company] lawsuit/fine/SEC" | `SEC filing data breach SaaS` |
| Industry developments | Search for "[industry] report/trend/analysis" | `"GTM tools market" report 2026` |
| Competitor monitoring | Search for "[competitor] announces" + time filter | `"Common Room" announces` |
| Earnings reports | Search for "[company] Q2 2026 earnings" | `"salesforce" quarterly results` |
| Job postings | Search for "[tool name] experience required" | `"the social signal provider experience" OR "the social signal provider specialist"` |
| Partner/channel news | Search for "[company] partners with" | `"HubSpot" partners with "AI"` |
| Case studies | Search for "case study [competitor] [industry]" | `"case study" "Common Room" SaaS` |

**Exa Search Patterns for Signal Detection**

```
1. Funding radar:
"[company]" "series" OR "seed" OR "raises" OR "funding" -"looking for funding"

2. Hiring surge:
"[company]" "hiring" OR "expanding team" OR "growing our" -"always hiring"

3. Competitor displacement:
"switched from [competitor]" OR "replaced [competitor]" OR "alternative to [competitor]"

4. Pain point mining:
"struggling with [problem]" OR "frustrated with [tool category]" OR "looking for [solution type]"

5. New initiative detection:
"[company]" "launches" OR "announces" OR "introduces" -"coming soon" -"stay tuned"

6. Buying intent (blogs/articles):
"best [tool category]" OR "top [tool category] tools" OR "[tool category] comparison" OR "[tool category] review"
```

**Exa + the social signal provider: The Combined Workflow**

```
Exa builds the universe (firmographic search, company lists, static research)
    ↓
The social signal provider overlays the signal (social monitoring, buying intent, real-time activity)
    ↓
GTM Grid combines both into scored, enriched records
    ↓
Downstream action
```

Example: "Find 500 Heads of Growth at B2B SaaS companies and tell me which ones
are showing buying signals."

1. **Exa:** Search for "Head of Growth" + "B2B SaaS" → build list of names + LinkedIn URLs
2. **the social signal provider:** Feed those LinkedIn URLs into Social Signals subscriptions → detect `buying_window`, `posted_about_tracked_topic`, `changed_role`
3. **GTM Grid:** Combine Exa's firmographic data with the social signal provider's signal data → scored, routed leads
4. **Action:** Hot leads → Smuggler/Lemlist campaigns; Warm leads → nurture sequence

### 5C: GTM Grid (Signal Combination & Enrichment)

The GTM Grid (at gtmgrid.dev) is the combination layer where signals from
multiple sources get merged, scored, and enriched into actionable records.

**What the GTM Grid does:**

- Ingests data from the social signal provider (social signals), Exa (web research), Smuggler (LinkedIn enrichment), CRMs
- Applies scoring and routing logic
- Outputs enriched lead lists for outbound campaigns
- Runs enrichment columns: email finding, company data, ICP scoring

**Key columns for signal combination:**

- public social/professional monitoring → signal type, score, date detected
- Exa research → funding data, news hits, company events
- Smuggler enrichment → verified emails, LinkedIn profile data
- Custom scoring → combined signal strength

### 5D: Smuggler.dev (LinkedIn Activity Signals)

Smuggler.dev detects LinkedIn profile activity that the social signal provider's social signals
don't cover — the private interactions on your own LinkedIn account. These are
second-party signals from your LinkedIn, not public posts.

| Smuggler Signal | What It Detects | How to Use It |
|---|---|---|
| Profile view | Someone viewed your LinkedIn profile | ICP title + profile view = warm opener: "Noticed you checked out my profile — curious what caught your eye." |
| New connection request | Someone sent you a connection request | ICP connection = they already know who you are. Accept and message within 24 hours. |
| Connection accepted | Someone accepted your connection request | Green light to send a follow-up. Don't pitch immediately — reference something specific. |
| Message received | Someone DMed you on LinkedIn | Inbound from an ICP title is the highest-intent signal there is. Respond within 2 hours. |
| Post engagement from non-connection | Someone outside your network engaged with your post | Expand reach — send a connection request referencing the post they engaged with. |
| Endorsement received | Someone endorsed you for a skill | Low-signal but valid opener: "Thanks for the endorsement — what are you working on these days?" |

**Smuggler + the social signal provider combination:**

- Public activity monitoring tells you what prospects are doing publicly (posting, job changes, engaging with competitors)
- Smuggler tells you what prospects are doing *to you specifically* (viewing your profile, connecting, messaging)
- Together they give you the full picture: public intent + private interest

### 5E: Fireflies (Call-Based Signals)

Fireflies (and equivalents: Gong, Chorus, Granola) transcribes and analyzes your
sales and customer calls. These are first-party signals hiding in your own
conversations that most teams never systematically mine.

| Call Signal | What to Listen For | Action |
|---|---|---|
| Competitor mentioned | Prospect/customer named a competitor — especially positively | Log as competitive threat. Attach battlecard. Flag for AE. |
| Budget mentioned | "We have budget for this," "Q3 budget," "just got approval" | Buying signal. Escalate priority. Move deal forward. |
| Timeline mentioned | "We need this by," "evaluating in the next," "going live in" | Real deal. Attach to CRM with specific dates. |
| Decision maker revealed | "My VP needs to sign off," "I'll loop in our CTO" | Map the buying committee. Research the new names. |
| Pain point surfaced | "Our current problem is," "struggling with," "biggest challenge" | Gold for positioning. Use their own words in your follow-up. |
| Feature request | "Do you have," "can your product do," "I wish it could" | Product feedback + expansion signal. Log to Featurebase AND notify CS. |
| Cancellation risk language | "Not sure this is working," "we might pause," "evaluating options" | Churn signal. Escalate immediately. |
| Expansion signal | "Other team might need this," "can we add more seats," "enterprise features?" | Upsell opportunity. Notify CS. |
| Champion enthusiasm | "I love this," "this is exactly what we needed," "game changer" | Ask for referral, case study, or introduction. |
| Objection raised | "Too expensive," "not a priority right now," "need to think about it" | Log objection type. Refine positioning. |

**How to operationalize Fireflies signals:**

1. Connect Fireflies to your calendar so all calls are auto-transcribed
2. Use Fireflies' AI summaries to surface key moments (competitor mentions, objections, action items)
3. Feed call transcripts into the social signal provider via webhook/API as part of the GTM Intelligence evidence layer
4. Set up signal-processing workflows triggered on call-based signals (e.g., competitor mentioned → Slack alert + CRM task)

### 5F: Other Tools Quick Reference

| Tool | Best For | Signal Types Covered |
|---|---|---|
| Google Alerts | Free, set-and-forget company/competitor monitoring | News mentions, press, blog posts |
| Crunchbase / PitchBook | Funding rounds, acquisitions, company data | Investment signals, growth |
| LinkedIn Sales Navigator | Manual prospecting + job changes, company alerts | Role changes, company growth |
| Smuggler.dev | LinkedIn profile views, connections, messages, post engagement | Private LinkedIn activity on YOUR account |
| Apify | Web scraping at scale — job boards, event sites, company pages, reviews | Job postings, event attendees, company data, reviews |
| Theirstack | Job posting intelligence — who's hiring for which roles, with which tools | Hiring signals, tool adoption, growth trajectories |
| BuiltWith / Wappalyzer | Technology stack detection | Tool adoption, stack changes |
| Glassdoor / RepVue | Employee sentiment | Culture/growth signals |
| G2 / Capterra / TrustRadius | Customer reviews, competitor reviews | Satisfaction, competitor displacement |
| Product Hunt | New product launches | Market entry, new competitors |
| SimilarWeb / SEMrush / Ahrefs | Web traffic and SEO data | Growth, marketing investment |
| SEC EDGAR | Public company filings (US) | Financial health, risk factors, MD&A |
| Layoffs.fyi | Tech layoff tracker | Restructuring signals |
| Fireflies / Gong / Chorus | Call transcription and analysis | Competitor mentions, budget/timeline, pain, churn, expansion |
| Mailstore / Google Vault | Email archive search for customer communications | Churn signals (your email history) |

---


## Part 8: Setup Guides — How to Actually Configure Each Signal Source

This section is the "go do this" companion to Part 5. For each tool, here are the
exact steps, configs, and queries to get signals flowing.

### 8A: Public Social & Professional Activity — Provider-Independent Setup

Use whatever public/professional research capability is available. No single social-signal vendor is required.

**Detect person-level changes:**
- New role or promotion
- New company
- Relevant public post
- Explicit buying/evaluation language
- Competitor discussion or dissatisfaction
- Relevant comment/question in a public community
- Hiring activity by a relevant person
- Thought leadership around a tracked topic

**Detect company-level changes:**
- New hiring activity
- Hiring surge in a relevant function
- Expansion to a new geography
- New strategic initiative
- Leadership change
- Public product/market announcement

**Detection workflow:**
1. Start from the known company/person and the relevant ICP context.
2. Search current public sources for recent activity.
3. Prefer dated, original evidence.
4. Compare against the known baseline or prior observations.
5. Extract only genuine changes.
6. Deduplicate the same event across sources.
7. Assign confidence based on evidence quality.
8. Score recency, relevance, and business impact.
9. Store the evidence URL and the exact observation supporting the signal.
10. Route only qualified signals downstream.

**Important:** Some social platforms expose limited activity. Do not infer a like, follow, profile view, connection, or other private/platform-specific event unless the current environment provides reliable evidence for it. If it cannot be verified, mark it `unverified` rather than inventing it.

### 8B: Social Signal Provider Keyword Searches — Exact Setup

For social listening beyond native signals (brand mentions, competitor mentions,
pain point mining).

**Buying intent search (LinkedIn posts):**

```bash
the selected signal source search create linkedin-posts \
  --name "Buying Intent — CRM Evaluation" \
  --keywords "looking for a CRM" "evaluating CRM" "CRM recommendations" "best CRM for" \
  --keywordsAnd "B2B" \
  --keywordsNot "job" "hiring" \
  --frequency DAILY
```

**Competitor displacement search (Reddit):**

```bash
the selected signal source search create reddit-posts \
  --name "Competitor Dissatisfaction" \
  --keywords "switched from [competitor]" "replaced [competitor]" "[competitor] alternative" "frustrated with [competitor]" \
  --frequency DAILY
```

**Pain point search (X/Twitter):**

```bash
the selected signal source search create twitter-posts \
  --name "Outbound Tool Pain" \
  --keywords "\"outbound is broken\"" "\"cold email not working\"" "\"SDR team struggling\"" "\"pipeline dried up\"" \
  --frequency DAILY
```

**Question mining (LinkedIn):**

```bash
the selected signal source search create linkedin-posts \
  --name "GTM Questions" \
  --keywords "how do you handle" "what tool for" "any recommendations for" "what's everyone using for" \
  --keywordsAnd "outbound" "sales" "pipeline" "GTM" \
  --frequency DAILY
```

Then attach a workflow to extract and route signals from the search:

1. Look up workflow examples: `signal-processing workflow examples`
2. Create a workflow: New Post trigger → AI agent (sentiment/qualify) → IF (passes filter) → Slack notification or CRM task
3. Test with a real post: `signal-processing workflow test --id <workflow_id> --post-json '<real_post_json>'`

### 8C: Exa — Recurring Signal Queries

Set these up as recurring searches (weekly or daily). Run them manually or via cron.

**Weekly funding radar (run every Monday):**

```
"[target company 1]" OR "[target company 2]" OR "[target company 3]" "series" OR "seed" OR "raises" OR "funding" OR "investment" after:2026-01-01
```

Combine with Exa's recency filter to only get last-7-days results.

**Weekly exec hire detection:**

```
"[target company]" "appoints" OR "hires" OR "joins" OR "named" "chief" OR "VP" OR "head of" after:2026-01-01
```

**Weekly competitor launch monitoring:**

```
"[competitor 1]" OR "[competitor 2]" OR "[competitor 3]" "launches" OR "announces" OR "introduces" OR "unveils" after:2026-01-01
```

**Daily layoff tracking (for displacement plays):**

```
"layoffs" OR "reduction in force" OR "restructuring" OR "job cuts" SaaS OR software OR tech after:2026-01-01
```

**Weekly case study detection (who's endorsing competitors):**

```
"case study" "[competitor name]" OR "customer story" "[competitor name]" OR "how we use [competitor name]"
```

Automation pattern: Write a cron job that runs each Exa query, extracts company
names from the results, cross-references against your ICP list, and surfaces
matches to Slack.

### 8D: Google Alerts — Free Setup (5 Minutes)

Create these alerts at google.com/alerts. Use exact-match quotes for precision.

| Alert | Query | Frequency |
|---|---|---|
| Competitor launches | `"[competitor] launches" OR "[competitor] announces"` | As-it-happens |
| Competitor funding | `"[competitor] funding" OR "[competitor] raises"` | As-it-happens |
| Target account news | `"[account name]"` (one alert per top-10 account) | Once/day |
| Industry regulation | `"[your industry] regulation" OR "[your industry] legislation"` | Once/week |
| Executive moves | `"appoints [title]" "[your space]"` | Once/day |
| Displacement keywords | `"switched from [competitor]" OR "replaced [competitor]" OR "[competitor] alternative"` | As-it-happens |

### 8E: Smuggler.dev — LinkedIn Activity Monitoring

Smuggler detects LinkedIn activity on YOUR account. Setup via the smuggler-tools skill.

**Profile view alerts:**

- Smuggler tracks who views your profile (with title/company data)
- Filter for ICP titles (e.g., "Head of Sales", "RevOps", "GTM Engineer")
- Route ICP profile views to Slack: "ICP profile view: [Name], [Title] at [Company] — viewed your profile. Reach out referencing specific content on your page."

**Connection request monitoring:**

- Smuggler detects incoming connection requests
- Filter: ICP title + relevant company size = accept and message within 24 hours
- Non-ICP: accept silently or ignore

**Message inbox monitoring:**

- Highest priority. Inbound from ICP title = respond within 2 hours
- Smuggler can auto-tag and notify on specific sender criteria

### 8F: Signal Combination & Scoring

Combine signals from all available sources into a normalized record.

**Recommended structure:**
1. Accounts/People — the monitored universe and baseline context.
2. Signal Events — append-only evidence log with signal type, source, date, confidence, and evidence URL.
3. Action Queue — qualified signals with combined score, routing tier, recommended action, and outreach context.

**Scoring principles:**
- Weight direct buying evidence highest.
- Weight person-level change higher than generic company activity when both are available.
- Apply recency decay.
- Reward corroboration from independent sources.
- Penalize weak, stale, duplicate, or ambiguous evidence.
- Never let an opaque AI confidence score override evidence quality.

### 8G: Fireflies — Call Signal Setup

1. **Connect Fireflies to your calendar.** Sign up at fireflies.ai, grant calendar access. All calls are auto-transcribed.
2. **Set up keyword alerts.** In Fireflies settings, add keyword trackers:
   - Competitor names (triggers on mention in any call)
   - "budget", "timeline", "decision", "approval" (buying signals)
   - "cancel", "not working", "too expensive" (churn risk)
   - "more seats", "other team", "enterprise" (expansion signals)
3. **Feed transcripts into the social signal provider.** Set up a webhook from Fireflies to the social signal provider:
   - Fireflies → webhook/event → signal-processing layer
   - signal-processing workflow: New webhook event → AI agent (extract signals from transcript) → Slack alert + CRM task
4. **Review weekly.** Pull the last 7 days of calls. Scan for the 10 call signal types in Part 5E. Any match → action.

### 8H: Apify / Theirstack — Job Board & Event Scraping

**Apify for job board monitoring:**

- Use Apify's LinkedIn Job Scraper or Indeed Scraper actors
- Set up a recurring run (weekly) scraping job postings for your target companies
- Filter for roles containing your keywords (SDR, AE, RevOps, etc.)
- Export results to Google Sheets or pipe into GTM Grid

**Apify for event attendee scraping:**

- Scrape event websites (Luma, Eventbrite, conference pages) for attendee lists
- Cross-reference attendees against your ICP (title + company)
- Route matches to your campaign queue

**Theirstack for hiring intelligence:**

- Search for companies actively hiring for specific roles
- Filter by: job title keywords + company size + industry + location + funding stage
- Export the company list → feed LinkedIn URLs into public social/professional monitoring
- Theirstack also reveals which tools companies mention in job postings ("experience with [tool name] required") — direct tool adoption signal

---
