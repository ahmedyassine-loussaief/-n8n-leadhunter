# LeadHunter — n8n Reddit Lead Generation Workflow

An automated lead generation tool that scrapes Reddit for potential 
clients, qualifies them with AI, generates personalized cold DMs, 
and sends everything to a Discord review queue for manual sending.

Built to find freelance automation clients without cold blasting — 
only reaches out to people actively describing a problem you can solve.
<img width="1487" height="391" alt="Screenshot 2026-07-14 124918" src="https://github.com/user-attachments/assets/06849ec0-b508-40b3-9f51-5db296881421" />


## What it does

1. User submits a niche via a form trigger
2. Workflow scrapes 2 Reddit RSS feeds per niche
3. Deduplicates against previously seen posts (Google Sheets)
4. AI qualifies each post and scores it 1-10
5. Filters only high-score posts (≥7)
6. Generates a personalized cold DM for each qualified lead
7. Sends lead + DM to Discord for manual review before sending

## Supported Niches

| Niche | Subreddits |
|---|---|
| Real Estate | r/realestate + r/realtors |
| E-Commerce | r/ecommerce + r/shopify |
| Coaching | r/Coaching + r/entrepreneur |
| Agency | r/agency + r/digitalmarketing |
| SaaS | r/SaaS + r/startups |
| Small Business | r/smallbusiness + r/Entrepreneur |

## Workflow Architecture
LeadHunter (Form Trigger)
→ NicheRouter (Switch node)
→ 6 RSS Feed nodes (2 per niche)
→ Merge (append)
→ GetSeenPosts (Google Sheets)
→ CheckForSeenPosts (Code node — dedup by URL)
→ IF node (isSeen true/false)
→ true → mark seen, skip
→ false → AddNewPosts (append to SeenPosts sheet)
→ Limit node
→ LoopOverPosts
→ RatingPosts (openrouter deepseek — scores 1-10)
→ Parser (Code node — extracts JSON)
→ NicheAndUrlAdder (Set node)
→ Wait node (rate limit protection)
→ FilterHighScorePosts (score ≥7)
→ ColdDmWriter (OpenRouter — generates personalized DM)
→ Parser1 (Code node — extracts DM)
→ GatherFinalInfo (Set node)
→ LogInfo (n8n Data Table)
→ Discord (sends lead + DM to review channel)
## AI Models

| Node | Model | Purpose |
|---|---|---|
| RatingPosts | openrouter| Post qualification + scoring |
| ColdDmWriter | openrouter | Personalized DM generation |

## Scoring System

- **1-3**: Disqualified — spam, no recurring task, solution-sharing post,
  "I will not promote" posts, interpersonal problems
- **4-6**: Weak signal — real task exists but low urgency or inferred
- **7-8**: Strong signal — explicit recurring pain, business owner context
- **9-10**: High priority — quantified cost, explicit ask for solution

## DM Generation Rules

- Score 9-10: Direct, references pain point with confidence
- Score 7-8: Softer, names a pattern without asserting their words
- Score 6: Question only, no automation mention
- Inferred pain points never stated as confirmed fact
- No banned words: streamline, leverage, game-changer, optimize, 
  solution, revolutionize
- Under 5 sentences, ends on a question, never a call-to-action

## Deduplication

- SeenPosts Google Sheet stores all processed URLs
- CheckForSeenPosts Code node compares incoming URL against existing
- Already seen posts skipped automatically on next run
- New posts appended to sheet after processing

## Discord Output Example
Score 9 — WhatsApp support eating entire day
Niche: E-Commerce
Subreddit: r/shopify
Link: [post URL]
Suggested DM: [personalized message]
## Setup

1. Import the workflow JSON into your n8n instance
2. Reconnect credentials:
   - Claude API (Haiku)
   - OpenRouter API
   - Google Sheets (SeenPosts sheet)
   - Discord webhook
3. Create a Google Sheet with columns: `url`, `seen_at`, `niche`
4. Update the form trigger niche options if needed
5. Activate the workflow and submit a niche via the form

## Known Limitations

- Reddit RSS feeds have limited post history (~25 posts per feed)
- Rate limits on AI providers — Wait node added between iterations
- DMs are generated for review only — manual sending required
- "I will not promote" subreddit posts occasionally slip through 
  qualification (known edge case, being improved)


## Author

Ahmed Yassine Loussaief — n8n Automation Specialist  
[LinkedIn]https://www.linkedin.com/in/ahmedyassine-loussaief-6393a7339/
