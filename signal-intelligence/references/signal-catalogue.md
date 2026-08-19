# Signal Catalogue

Lookup table of 200+ signals by use case. Read the section matching the user's motion — don't load the whole file if you only need one section.

---

## Part 4: The Master Signal Catalogue

Organized by business use case: New Business, ABM/Targeted Account, Account
Expansion, Customer Success/Upsell, and Churn Prevention. Each entry: signal
name, how to track it, and recommended tools.

### Section A: New Business (Net-New Prospects)

These signal that a company or person you haven't spoken to before may be
entering a buying window.

#### A1: People Signals (person-level)

| Signal | How to Track | Tools |
|---|---|---|
| New Job / Role Change | Monitor LinkedIn profiles of ICP titles for job changes | public profile monitoring, LinkedIn alerts, Sales Navigator |
| New to Company | Person moved to a new company — fresh stack evaluation window | public profile monitoring |
| Promotion | More budget, more authority, new priorities | public profile monitoring (title upgrade subset), LinkedIn monitoring |
| Posted About Relevant Topic | Publicly writing about a problem you solve | public social/topic search, keyword searches |
| Posted About Buying / Evaluating | "Looking for recommendations for X" or "Evaluating X vs Y" | AI classification of public buying-intent posts, social listening with buying-intent keywords |
| Engaged With Competitor Content | Liked, commented on, or shared a competitor's post | public competitor-engagement evidence where observable, `competitor_engagement` |
| Engaged With Your Content | Interacted with your company's LinkedIn posts | public engagement evidence where observable, engagement monitoring |
| Asked a Question in a Community | Posted in Slack/Discord/Reddit about a problem | the social signal provider Reddit/community monitoring, manual Slack scanning |
| Commented on Tracked Content | Left comments on specific posts you're watching | public comment monitoring |
| Became Hiring | Posted a job ad themselves (founder/hiring manager) | public hiring/post monitoring |
| Published Thought Leadership | Wrote an article, newsletter, or thread on a relevant topic | public thought-leadership analysis, `posted_about_tracked_topic`, Substack monitoring |
| Followed Your Company Page | Just followed your LinkedIn page | LinkedIn page analytics, the social signal provider profile monitoring |
| Attended a Webinar / Event | Registered for or attended your event | Zoom/WebinarJam registration lists, event platforms |
| Downloaded a Lead Magnet | Downloaded a whitepaper, template, guide, or tool | Your website analytics, form submissions, CRM |
| Visited Pricing Page | Multiple visits to pricing or demo pages | Website analytics (GA/PostHog), Clearbit Reveal, RB2B |
| Visited Competitor Comparison Page | On your site reading "X vs Y" comparisons | Website analytics, content-specific tracking |
| New LinkedIn Connection | Just connected with you or your sales team | LinkedIn notifications, CRM connection tracking |
| Birthday / Work Anniversary | Low-signal but valid conversation opener | LinkedIn notifications, CRM enrichment |
| Speaker at a Conference | Speaking at an industry event — thought leadership + visibility | Event websites, LinkedIn event posts, Exa news search |
| Changed Profile Photo or Bio | Updating LinkedIn presence — often signals active job searching | the social signal provider profile monitoring |
| Endorsed Someone for Skills | Recent LinkedIn activity — active on platform | LinkedIn monitoring |
| Joined a New LinkedIn Group | Joined a professional group related to your space | LinkedIn group monitoring (limited) |
| Posted a Job Ad Themselves | Founder or exec directly recruiting — growth/urgency | public hiring/post monitoring, LinkedIn job posts |
| Changed Location | Moved to a new city/country | the social signal provider profile monitoring, LinkedIn |
| Completed a Certification | New skills or credentials — often precedes role change | LinkedIn profile updates |
| Viewed Your LinkedIn Profile | ICP title checked out your profile | Smuggler.dev profile view alerts |
| Sent You a Connection Request | ICP title connected with you unprompted | Smuggler.dev connection monitoring, LinkedIn notifications |
| DM'd You on LinkedIn | ICP title messaged you — highest intent inbound signal | Smuggler.dev message monitoring, LinkedIn inbox |
| Engaged With Your Post (Non-Connection) | Someone outside your network liked/commented on your post | Smuggler.dev post engagement tracking |

#### A2: Company Signals (company-level)

| Signal | How to Track | Tools |
|---|---|---|
| Funding Round Announced | Press release, Crunchbase, TechCrunch, company blog | Exa web search, Google Alerts, Crunchbase, PitchBook |
| Company Hiring for Relevant Roles | Job postings indicating they're building the function you serve | careers/job-board monitoring, LinkedIn job search, Indeed, Greenhouse |
| Job Count Increased | Headcount growth acceleration | job-count comparison across observations |
| New Product or Feature Launch | Announced something new — strategic shift or growth | Exa news search, Product Hunt, company blog, TechCrunch |
| Merger or Acquisition | Bought or was bought — tech stack consolidation | Exa news, press releases, SEC filings, Crunchbase |
| Entered New Market or Geo | Expanding to new country or vertical | Exa news, company blog, LinkedIn company page updates |
| Opened New Office | Physical expansion — growth signal | Exa news, local business press, LinkedIn company updates |
| Closed Office or Downsized Location | Retrenchment — may signal restructuring | Exa news, local press |
| Rebranded or New Website | New name, logo, website — often a strategic pivot | Visualping, Exa news, BuiltWith |
| New Executive Hire | C-suite or VP hire — new priorities, new budget, new tool evaluation | Exa news, LinkedIn company page, press releases |
| Executive Departure | C-suite or VP left — instability or strategic shift | Exa news, LinkedIn, press |
| Layoffs / RIF | Reduced workforce — restructuring, budget reallocation | Exa news, Layoffs.fyi, LinkedIn posts, WARN notices |
| Leadership Restructuring | Reorganization — new decision-makers, new priorities | Exa news, LinkedIn, company blog |
| New Management Strategy | CEO letter, strategy change announcement | Exa news, company blog, earnings calls |
| Won an Award | Industry recognition — growth and credibility | Exa news, industry publications, award sites |
| Featured in Analyst Report | Gartner MQ, Forrester Wave — often triggers tool evaluation | Exa news, analyst firm websites, press releases |
| Good Quarter / Earnings Beat | Strong results — expansion budget unlocked | Exa news, earnings calls, financial news |
| Bad Quarter / Earnings Miss | Poor results — cost-cutting OR new tool investment | Exa news, earnings calls, financial news |
| Revenue Milestone | ARR announcement, revenue milestone | Exa news, company blog, press releases, TechCrunch |
| Customer Milestone | "X customers," "Y users" announcement — growth signal | Exa news, company blog |
| New Partnership Announced | Strategic partnership — ecosystem expansion | Exa news, press releases, LinkedIn |
| Changed CRM or Tech Stack | Adopted/dropped a tool (job posts mentioning tools) | Exa search + job postings, BuiltWith, StackShare |
| New Tool Adoption | Publicly mentioned adopting a tool in your ecosystem | Exa news, LinkedIn posts, job postings |
| Change in Marketing Agency | Hired/fired an agency — may re-evaluate martech | Exa news, agency announcements, LinkedIn |
| IPO Filed or Rumored | Going public — process/compliance tooling upgrade | Exa news, SEC EDGAR, financial news |
| Lawsuit Filed or Resolved | Legal action — may trigger compliance/security tooling | Exa news, PACER, legal databases, press |
| Regulatory Action | Fined, investigated, or new regulation applies | Exa news, government agency sites, industry press |
| Cybersecurity Incident / Breach | Data breach — immediate security tooling need | Exa news, HaveIBeenPwned, security press, SEC filings |
| Website Traffic Spike or Drop | Significant traffic change — growth or trouble | SimilarWeb, SEMrush, Ahrefs |
| SEO / SEM Spend Increase | Spending more on search — growth or new market entry | SEMrush, Ahrefs, BuiltWith, ad monitoring |
| New Marketing Channel | Started podcast, YouTube, newsletter — growth activity | Exa news, company blog, platform-specific search |
| Changed Marketing Budget | Publicly discussed marketing investment changes | Exa news, earnings calls, marketing press |
| Event Sponsorship or Exhibition | Sponsoring a conference — growth/visibility play | Exa news, event websites, LinkedIn |
| Change in Inbound vs Outbound Strategy | Shift in sales approach — new tooling needs | Exa news, job postings for SDR/AE roles, company blog |
| Negative Press Coverage | Bad PR — crisis mode, may need new tools/agencies | Exa news, Google News, X |
| Positive Press Coverage | Good PR — growth moment, awareness spike | Exa news, Google News |
| Supply Chain Issues | Public supply chain problems — logistics/procurement tools | Exa news, industry publications |
| Product Recall | Recalled a product — crisis, quality/compliance tools | Exa news, CPSC, FDA, industry press |
| Selling Assets or Divesting | Selling business units — restructuring | Exa news, financial press |
| Facing Piracy or IP Issues | IP theft — legal/security needs | Exa news, legal press, TorrentFreak |
| Offering Free Trials or Freemium | New pricing model — growth push | Website monitoring, Exa news |
| Published Case Study With a Competitor | Publicly endorsing a competitor | Exa search, competitor case studies, LinkedIn |
| Competitor Launched a Feature Targeting Them | Competitor announcement mentioning their industry | Exa news, competitor monitoring |
| Customer Base Increase/Decrease | Public customer count change | Exa news, company blog, earnings calls |
| New Website / Major Website Change | Redesigned site — often accompanies repositioning | Visualping, BuiltWith, homepage monitoring |
| Website Security Issues | SSL cert expiry, defacement, downtime | BuiltWith, security monitoring |
| Change in Web Traffic Source Mix | Traffic shift from one channel to another | SimilarWeb, SEMrush |
| Increase in Software Expenses | Publicly discussed SaaS spend increases | Exa news, earnings calls, tech press |
| Shift in Sales Channel | Moving from direct to channel or vice versa | Exa news, job postings, company blog |
| Company Following Competitors on Social | Following competitors on LinkedIn/X | Social media monitoring |
| Employee Count Increase | Headcount growth above industry average | LinkedIn company page, Crunchbase, Exa news |
| Employee Count Decrease | Headcount reduction | LinkedIn company page, Layoffs.fyi, Exa news |
| Change in Employee Review Trend | Glassdoor/Indeed reviews turning positive or negative | Glassdoor, Indeed, RepVue, Exa news |
| Change in Customer Review Trend | G2/Capterra/Trustpilot reviews shifting | G2, Capterra, Trustpilot, review monitoring |
| New Legislation Affecting Their Industry | Regulatory change creates compliance needs | Exa news, government sites, industry associations |
| Industry Downturn or Upturn | Macro changes to their sector | Exa news, industry reports, economic data |
| Major Industry Development | Sector-defining event (new tech, consolidation, disruption) | Exa news, industry publications, analyst reports |
| New Technology Trend in Their Space | AI, blockchain, etc. affecting their vertical | Exa news, tech press, industry publications |
| Conference or Trade Show Announcement | Industry event where they might be present | Event websites, Exa news, LinkedIn |
| Participating in an Accelerator / Incubator | Startup in YC, Techstars — rapid growth phase | Exa news, accelerator pages, Crunchbase |
| Grant or Government Funding Received | Non-dilutive funding — R&D, innovation grants | Exa news, government grant databases, SBIR/STTR |
| Patent Filed or Granted | IP activity — R&D investment signal | USPTO, Google Patents, Exa news |
| Open Source Release or Contribution | Company released OSS — technical signal | GitHub, Exa news, tech press |
| Hiring Freeze Announced | Stopped hiring — budget caution | Exa news, LinkedIn, press |
| Return-to-Office Mandate | Office policy change — facilities/collaboration tools | Exa news, company blog, press |
| Remote-First Announcement | Going remote — tooling and process changes | Exa news, company blog, LinkedIn |
| DEI or ESG Initiative Launched | New corporate initiative — related tooling | Exa news, company blog, press |
| Sustainability Report Published | Corporate responsibility focus | Exa news, company website |
| Change in Billing Model | Moved from subscription to usage, etc. | Website monitoring, Exa news |
| Compliance Certification Achieved | SOC 2, ISO 27001, HIPAA — signals maturity | Exa news, certification registries, company blog |
| Competitor Launched a New Product | A competitor of YOUR prospect launched something | Exa news, competitor monitoring, industry publications |
| Company Announced AI/ML Initiative | Building AI capabilities — infra, data, tooling need | Exa news, job posts for ML roles, company blog |
| Company Sunset a Product or Feature | Killing a product line — consolidation or pivot | Exa news, company blog, customer communications |
| Filing for Bankruptcy / Restructuring | Chapter 11, administration — major change | Exa news, court filings, financial press |
| Change in Ownership | PE/VC acquired majority stake — new tooling decisions | Exa news, Crunchbase, financial press |
| Spin-off or Divestiture | Business unit becoming independent — new tech stack | Exa news, financial press |
| Joint Venture Formed | Two companies forming a JV — new entity with needs | Exa news, press releases |
| B Corp or Benefit Corporation Status | Corporate structure change | Exa news, B Lab registry |
| New Board Member Appointed | Governance change — new priorities | Exa news, press releases, LinkedIn |
| Annual Report Published | Yearly review — strategy, priorities, challenges | Company investor relations, Exa news |

### Section B: ABM / Targeted Account Signals

Signals mapped to named accounts you've already identified as high-value targets.
You're watching specific companies for buying windows.

| Signal | How to Track | Tools |
|---|---|---|
| Target Account Visited Your Website | IP-based identification of named account traffic | Clearbit Reveal, RB2B, 6sense, Demandbase |
| Target Account Visited Pricing Page | Named account on pricing — high intent | Clearbit Reveal, RB2B + page-level tracking |
| Target Account Downloaded Content | Gated asset download from target account | CRM + website analytics |
| Target Account Started Free Trial | Named account signed up — PLG entry point | Product analytics, CRM |
| Target Account's Champion Changed Jobs | Your contact left — risk or opportunity | public profile monitoring on watched profiles |
| Target Account Engaged With Your Outbound | Opened email, clicked link, replied | Outreach/SalesLoft/Instantly analytics, CRM |
| Target Account Following You on Social | Company or key contacts followed you | LinkedIn, X analytics |
| Target Account Mentioned in Competitor Case Study | Competitor claims them as a customer — displacement opportunity | Exa search, competitor website monitoring |
| Target Account Running Competitor's Ads | They're promoting a competitor's product | Competitor social monitoring |
| Target Account Attended Competitor's Webinar | Engaging with competitor content | Competitor event monitoring, LinkedIn |
| Target Account Posted Competitor Job | Hiring someone to manage competitor's tool | Job board search with competitor name |
| Target Account's Tech Stack Changed | New tools added or removed — stack in flux | BuiltWith, HG Insights, job postings |
| Target Account Won a New Contract | Won business — may be scaling | Exa news, press releases |
| Target Account's Customer Satisfaction Dropped | Public complaints, negative reviews about current provider | G2, Capterra, social listening, Reddit |
| Target Account's Competitor Made a Move | Their competitor launched something — reactive buying | Exa news, competitor monitoring |

### Section C: Account Expansion (Existing Accounts)

Signals within accounts you've already landed — not new logos, but more seats,
more products, higher tier.

| Signal | How to Track | Tools |
|---|---|---|
| Usage Spike | Account usage suddenly increased — ready for more seats/features | Product analytics (PostHog, Mixpanel, Amplitude) |
| New Department or Team Using Product | Different team started using unofficially | Product analytics by user domain/department |
| Key Contact Promoted | Champion got promoted — more budget and influence | public profile monitoring, LinkedIn monitoring |
| Account Hired in Relevant Function | Staffing up the team that uses your product | careers/job-board monitoring, LinkedIn job posts |
| Account Entered New Market | Geographic or vertical expansion — more users | Exa news, company monitoring |
| Account's Company-Wide Rollout Mentioned | Internal champion pushing wider adoption | CRM notes, CS calls, email |
| Account Asked About Enterprise Features | Feature requests for SSO, audit logs, permissions | Support tickets, CS calls, Featurebase |
| Account's Contract Renewal Approaching | 90/60/30 days out — expansion conversation window | CRM, billing system, contract management |
| Account Exceeded Plan Limits | Hit seat cap, usage cap, or rate limit | Product analytics, billing system |
| Account Using API / Webhooks | Technical integration = sticky, expansion-ready | Product analytics, API gateway |
| Account Adopted a New Workflow | Started using a feature they didn't before | Product analytics, onboarding data |
| Multiple Users From Same Account Created | Organic bottom-up growth within account | Product analytics |
| Account Invited External Collaborators | Sharing with partners/clients — platform stickiness | Product analytics |
| Account Sent You a Referral | Referred another company — strong advocate | CRM, referral tracking |
| Account Agreed to Case Study | Willing to be public reference — expansion-ready | CRM, CS notes |
| Account Attended Your Event | Showed up to webinar, dinner, conference — engaged | Event platforms, CRM |
| Account's CSAT/NPS Score Increased | Satisfaction trending up | Survey tools, CS platform |
| Account Joined Your Community | Joined Slack/Discord/forum — deeper engagement | Community platform analytics |
| Account Submitted Feature Requests | Investing time to tell you what they need | Featurebase, support, CS calls |
| Account Integrated New Tool | Connected a new integration — expanding stack around you | Product analytics (integration events) |
| Account's Executive Became Your Champion | C-suite started engaging with your content | LinkedIn monitoring, event attendance |
| Account Asked About Uptime SLA | Evaluating for mission-critical use | Support tickets, CS calls |
| Account Requested Security Review | Procurement process starting — enterprise expansion | CS calls, security team communications |
| Account's Credit Card Expiring and Replaced | Billing change — often prompts plan review | Billing system |
| Account Added Payment Method | Added backup card, moved from monthly to annual | Billing system |
| Account's Team Size Grew Beyond Plan | More people than seats — natural expansion trigger | Product analytics, billing vs actual usage |
| Account's Email Domain Added New Users | New users with same domain signing up | Product analytics |

### Section D: Customer Success & Upsell

Signals from existing customers indicating they're ready to upgrade, cross-sell,
or add products.

| Signal | How to Track | Tools |
|---|---|---|
| Customer Asked About a Feature In Your Higher Tier | Feature gating working as designed | Support tickets, CS calls, Featurebase |
| Customer Compared Your Plans Publicly | Asked on social/community about plan differences | Social listening, community monitoring |
| Customer's Use Case Expanded | Started doing something new with your product | Product analytics, CS calls |
| Customer Asked for Training | Wants to skill up their team on your product | CS calls, support tickets |
| Customer's Industry Peers Upgraded | Competitive pressure within their vertical | Internal data on peer accounts |
| Customer Received Funding | Their company got investment — budget unlocked | Exa news, Crunchbase |
| Customer's Revenue Grew Significantly | Scaling — your product needs to scale with them | Exa news, financial data |
| Customer's Account Manager Changed | New CSM or AE — fresh discovery opportunity | CRM, internal handoff |
| Customer Renewed Early | Signed renewal before deadline — upsell window | Billing system, CRM |
| Customer Paid Annually | Annual commitment shows serious intent | Billing system |
| Customer Asked for Invoice / PO Process | Moving from card to procurement — enterprise trajectory | Billing communications |
| Customer's Executive Sponsor Changed | New exec — re-establish value, new priorities | LinkedIn monitoring, CRM |
| Customer Merged or Was Acquired | Entity change — more seats or new buying center | Exa news, CRM |
| Customer Launched New Product Line | New business unit = new use case | Exa news, company blog |
| Customer Mentioned You in a Case Study or Talk | Publicly endorsing — high expansion likelihood | Social listening, Exa news |
| Customer Attended Your User Conference | Invested time and travel — highly engaged | Event registration |
| Customer Joined Your CAB / Advisory Board | Customer advisory board — strategic relationship | Internal CAB roster |
| Customer's Product Usage Diversified | Using more features than before | Product analytics |
| Customer Asked About Multi-Product Bundle | Interested in cross-buy | CS calls, sales conversations |
| Customer Mentioned a Competitor's Feature | "X does this, do you?" — retention risk AND upsell | CS calls, support tickets |
| Customer's Industry Got New Regulation | Compliance requirement your product helps with | Exa news, industry monitoring |
| Customer's Headcount Grew Significantly | More employees = more seats needed | LinkedIn, Exa news |
| Customer Asked About SSO / Advanced Security | Enterprise readiness — precedes plan upgrade | Support tickets, CS calls |
| Customer Asked About API Access or Webhooks | Technical integration = deeper commitment | Support tickets, CS calls |
| Customer's NPS Detractor Became Promoter | Turned around a negative relationship | NPS surveys |
| Customer Asked to Be Design Partner | Wants to co-develop features — high investment | CS calls, product conversations |
| Customer Recommended You on G2/Capterra | Public review posted — advocacy signal | G2, Capterra monitoring |
| Customer Referenced You in a Job Posting | "Experience with [your product] required" — deep adoption | Exa job search |

### Section E: Churn Prevention

Signals that a customer may be at risk of leaving. Act fast.

| Signal | How to Track | Tools |
|---|---|---|
| Product Usage Dropped Significantly | 50%+ decline in key actions | Product analytics |
| Login Frequency Declined | Users logging in less often | Product analytics |
| Key Champion Left the Company | Your advocate is gone — relationship at risk | public profile monitoring, LinkedIn monitoring |
| Champion's Role Changed Internally | Moved to a different team — lost influence | public profile monitoring, LinkedIn |
| Support Ticket Volume Spiked | Sudden increase in issues | Support system, Featurebase |
| Support Satisfaction Dropped | CSAT scores declining | Support system |
| Customer Asked About Data Export | "How do I get my data out?" — strongest churn signal | Support tickets, CS calls |
| Customer Asked About Cancellation Policy | Checking terms before leaving | Support tickets, CS calls |
| Customer Stopped Responding to CS Outreach | Gone dark — disengaged | CRM, email analytics |
| Customer's Payment Failed | Card declined, invoice unpaid | Billing system |
| Customer Downgraded Plan | Moved to lower tier — value perception dropped | Billing system |
| Customer Removed Seats | Reduced user count | Billing system, product analytics |
| Customer Disconnected Integrations | Removed integrations — reducing stickiness | Product analytics, integration health |
| Customer Started Using a Competitor | Detected via product analytics (competitor URLs) or social | Product analytics, the social signal provider competitor monitoring |
| Customer's Contract Renewal Passed Without Action | Auto-renewed without engagement — silent risk | Billing system, CRM |
| Customer Complained Publicly on Social Media | Negative post about your product | Social listening, the social signal provider keyword monitoring |
| Customer Left a Negative Review | 1–2 star review on G2/Capterra | G2, Capterra monitoring |
| Customer's Account Executive Left Your Company | Their AE churned — relationship gap | Internal CRM, HR system |
| Customer's Industry in Decline | Sector struggling — budget cuts likely | Exa news, economic data |
| Customer Had Layoffs | Headcount reduction = fewer seats, lower budget | Exa news, LinkedIn |
| Customer's Budget Was Cut | Explicit budget reduction conversation | CS calls, CRM notes |
| Customer Asked for Pricing Concession | Pushing for discount — value perception issue | CS calls, billing conversations |
| Customer Comparing to Competitor in Calls | Mentioning competitors more frequently | CS call transcripts (Gong/Chorus/Fireflies) |
| Customer Not Using Key Features | Core features adopted during onboarding now unused | Product analytics |
| Customer's Time-to-Value Exceeded Threshold | Onboarded but never reached activation milestone | Product analytics, CS platform |
| Customer's Billing Contact Changed | New person paying the bill — re-evaluation risk | Billing system |
| Customer Requested a Service Credit or Refund | Unhappy enough to ask for money back | Billing system, support tickets |
| Customer Escalated to Executive Team | Made it to CEO/CTO level complaint | Internal communications, support escalations |
| Customer's NPS Score Dropped Significantly | Promoter → Detractor in one cycle | NPS survey tool |
| Customer's Feature Requests Ignored for >6 Months | They asked and nothing happened — frustration builds | Featurebase, product feedback |
| Customer Experienced Downtime or Major Bug | Service disruption — trust erodes | Incident management, status page |

---
