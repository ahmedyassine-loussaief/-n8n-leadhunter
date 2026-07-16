# LeadHunter — n8n Reddit Lead Generation Workflow
<img width="1517" height="536" alt="Screenshot 2026-07-16 162149" src="https://github.com/user-attachments/assets/60f299dc-5ee1-41fb-b023-6b1b0b5cc03e" />

An automated lead generation tool that scrapes Reddit for potential clients, qualifies them with AI, generates personalized cold DMs, and sends everything to a Discord review queue for manual sending.

Built to find freelance automation clients without cold blasting — only reaches out to people actively describing a problem you can solve.


**Live on indie.money:** 
---

## What It Does

1. User submits a niche via webhook (`GET /leadhunter?niche=saas`)
2. Workflow scrapes 2 Reddit RSS feeds per niche (6 niches total)
3. Deduplicates against previously seen posts (Google Sheets)
4. AI (DeepSeek) qualifies each post and scores it 1-10
5. Filters only high-score posts (≥7)
6. Generates a personalized cold DM for each qualified lead
7. Sends lead + DM to Discord for manual review before sending
8. Error notifications on failures (Discord alerts)

---

## Supported Niches

| Niche | Subreddits |
|---|---|
| Real Estate | r/realestate + r/realtors |
| E-Commerce | r/dropship + r/FulfillmentByAmazon |
| Coaching | r/Coaching + r/entrepreneur |
| Agency | r/agency + r/digitalmarketing |
| SaaS | r/SaaS + r/startups |
| Small Business | r/smallbusiness + r/Entrepreneur |

---

## Workflow Architecture
Webhook (/leadhunter?niche=saas)
→ Respond to Webhook (200 OK)
→ nicheextractor (Set node)
→ NicheRouter (Switch node)
→ 6 HTTP Request nodes (Reddit RSS feeds)
→ XML → Split Out → ExtractFields (Code node)
→ Merge (combines all 6 branches)
→ GetSeenPosts (Google Sheets)
→ CheckForSeenPosts (Code node — dedup by URL)
→ IF node (isSeen true/false)
→ true → AllPostsAreSeen (Discord notification) → skip
→ false → AddNewPosts (append to SeenPosts sheet)
→ LoopOverPosts (splitInBatches, batch size 3)
→ RatingPosts (DeepSeek — scores 1-10)
→ Parser (Code node — extracts JSON)
→ NicheAndUrlAdder (Set node)
→ Wait node (rate limit protection)
→ FilterHighScores (score ≥6)
→ AreThereHighScores? (IF node)
→ NoHighScoreNotification (Discord)
→ AddedTitlerAndContent (Set node)
→ ColdDmWriter (DeepSeek — generates personalized DM)
→ Parser1 (Code node — extracts DM)
→ GatherFinalInfo (Set node)
→ FinalMessageSender (Discord — sends lead + DM for review)

text

---

## AI Models

| Node | Model | Purpose |
|---|---|---|
| RatingPosts | DeepSeek Chat | Post qualification + scoring |
| ColdDmWriter | DeepSeek Chat | Personalized DM generation |

---

## Scoring System

- **1-3:** Disqualified — spam, no recurring task, solution-sharing, "I will not promote" posts, interpersonal problems
- **4-6:** Weak signal — real task exists but low urgency or inferred
- **7-8:** Strong signal — explicit recurring pain, business owner context
- **9-10:** High priority — quantified cost, explicit ask for solution

---

## DM Generation Rules

| Score | Approach |
|---|---|
| 9-10 | Direct, references pain point with confidence |
| 7-8 | Softer, names a pattern without asserting their words |
| 6 | Question only, no automation mention |
| Inferred pain points | Never stated as confirmed fact |

**Tone Rules:**
- Casual and direct, no corporate language
- Under 5 sentences, ends on a question
- No banned words: streamline, leverage, game-changer, optimize, solution, revolutionize
- Never a call-to-action

---

## Deduplication

- **SeenPosts Google Sheet** stores all processed URLs
- **CheckForSeenPosts** Code node compares incoming URL against existing
- Already seen posts skipped automatically on next run
- New posts appended to sheet after processing

---

## Error Handling

| Failure Point | Notification |
|---|---|
| Subreddit fetch error | Discord alert — rate limit or feed unavailable |
| AI scoring failure | Discord alert — service issue |
| DM generation failure | Discord alert — service issue |
| No qualifying leads | Discord — "No posts scored 6+ this run" |
| All posts already seen | Discord — "No new posts found" |

---

## Discord Output Example
🎯 New Lead — ecommerce

Post: https://www.reddit.com/r/dropship/comments/12345/
Pain point: manually calculating and consolidating costs
Score: 6/10 (inferred)
Logged at: 2026-07-16T15:12:38.289Z
niche: ecommerce

Suggested message:
Hey! Saw your post about manually calculating and consolidating various costs across different spreadsheets — I actually build automations for this exact kind of thing...

text

---

## Setup


1. **Reconnect credentials:**
   - Google Sheets OAuth2 (deduplication)
   - OpenRouter API (DeepSeek Chat)
   - Discord Webhook (leads + error notifications)
2. **Create a Google Sheet** with columns: `url`, `seen`, `niche`
3. **Configure webhook:** `GET /leadhunter?niche=saas`
4. **Available niches:** `saas`, `ecommerce`, `realestate`, `coaching`, `agency`, `smallbusiness`

---

## Known Limitations

- Reddit RSS feeds have limited post history (~25 posts per feed)
- Rate limits on AI providers — Wait node added between iterations
- DMs are generated for review only — manual sending required
- Unauthenticated Reddit requests may occasionally hit rate limits (future fix: Reddit OAuth)

---

## What's Changed (v2)

| Change | Description |
|---|---|
| **RSS → HTTP Request** | Replaced unsupported RSS nodes with HTTP + XML parsing |
| **Webhook trigger** | Replaced Form Trigger with webhook (`GET /leadhunter?niche=saas`) |
| **Error handling** | Added Discord notifications for all failure points |
| **User feedback** | Added "no leads found" and "all posts seen" messages |
| **Simplified credentials** | Consolidated to single Discord webhook |

---

## Author

**Ahmed Yassine Loussaief** — n8n Automation Specialist
- [LinkedIn](https://www.linkedin.com/in/ahmedyassine-loussaief-6393a7339/)
