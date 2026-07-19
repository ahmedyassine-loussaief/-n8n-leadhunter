# 🎯 LeadHunter — Reddit Lead Finder for Automation Builders
<img width="1452" height="478" alt="Screenshot 2026-07-17 183439" src="https://github.com/user-attachments/assets/8a8bc12d-8d25-42b6-85cf-9008bf61a9ce" />


Stop scrolling Reddit for clients. LeadHunter reads six business communities for you, scores every fresh post for automation pain, and delivers qualified leads to your Discord — each one with a personalized outreach DM already written.If you sell automation, workflows, or "I'll fix your repetitive work" services, your best clients are complaining about their problems on Reddit right now. LeadHunter finds them while you do billable work.

▶ Run it instantly on indie.money — no setup required: https://7jtbhw.s.indie.money/

---

## What it does

Every run, the agent:

1. **Pulls the newest posts** from a curated pair of subreddits for the niche you pick
2. **Filters out everything it has already seen** — a built-in memory means you never get the same lead twice
3. **Scores each fresh post 1–10** with a strict AI qualification rubric tuned for automation work:
   - ✅ Real, recurring manual task (daily / weekly / per-order pain)
   - ✅ Poster owns the business or can buy a fix
   - ✅ Explicit asks or quantified cost ("3 hours a day", "$X lost every month") score highest
   - ❌ Rants, tool-shopping threads, "no self-promo" flairs, case studies, and one-time decisions are disqualified on sight — so you don't burn goodwill (or break subreddit rules) messaging the wrong people
4. **Writes a custom cold DM for every post scoring 6+** — it quotes the poster's exact problem, explains why it happens, proposes 2–3 concrete things you'd build, and ends with one specific question. No corporate buzzwords, no "streamline/leverage/unlock" filler
5. **Posts each lead to your Discord channel**: post link, pain point, score, and the ready-to-send message

No qualifying leads this run? You get a short "nothing scored 6+" note instead of silence, so you always know it ran.

## Six niches, one parameter

Call the agent's webhook with a single `niche` value:

| `niche=` | Communities watched |
|---|---|
| `saas` | r/SaaS + r/startups |
| `realestate` | r/realestate + r/realtors |
| `ecommerce` | r/dropship + r/FulfillmentByAmazon |
| `coaching` | r/Coaching + r/entrepreneur |
| `agency` | r/agency + r/digitalmarketing |
| `smallbuisness` | r/smallbusiness + r/Entrepreneur |

Rotate niches across runs to keep a steady lead flow — the seen-post memory is shared, so every run only spends effort on genuinely new posts.

## What lands in your Discord
Example:

<img width="1390" height="716" alt="Screenshot 2026-07-18 170614" src="https://github.com/user-attachments/assets/50d31eb5-6df3-4ece-81f6-1c6c8ce6e479" />



Copy, tweak one line, send. That's the whole workflow.

## Setup — indie.money buyers

What you need before activating:

A Discord bot token (free, takes 5 minutes)

An OpenRouter API key (free account, costs less than $0.01 per run)

Your Discord server and channel IDs

## Steps to create the above if you don't have them:

## Step 1 — Create a Discord bot

Go to discord.com/developers/applications

Click "New Application" → give it any name

Go to "Bot" → click "Add Bot" → copy the bot token

Go to "OAuth2" → "URL Generator" → select "bot" scope → select "Send Messages" permission

Copy the generated URL, open it in your browser, invite the bot to your server

## Step 2 — Get your Discord IDs

Open Discord → Settings → Advanced → enable Developer Mode

Right-click your server name → "Copy Server ID" → this is your DISCORD_GUILD_ID

Right-click your target channel → "Copy Channel ID" → this is your DISCORD_CHANNEL_ID

## Step 3 — Get your OpenRouter API key

Go to openrouter.ai and create a free account

Go to "Keys" → create a new API key

LeadHunter uses DeepSeek Chat — one of the most accurate models for lead scoring at under $0.01 per run. Your key, your costs, full transparency.

## Step 4 — Activate on indie.money
When activating LeadHunter you'll be asked to fill in:

<img width="1070" height="507" alt="image" src="https://github.com/user-attachments/assets/ce513e21-1852-4ec9-9fc5-ad936546c1e2" />

DISCORD_GUILD_ID — your server ID

DISCORD_CHANNEL_ID — your channel ID

<img width="642" height="521" alt="image" src="https://github.com/user-attachments/assets/04c6320f-52f4-4858-8c29-df9290f0fd14" />

Discord bot token — as a credential

OpenRouter API key — as a credential


## Run it your way
On indie.money, use the built-in interface and fill in the parameter as shown:

URL Parameters : niche=saas

Body (JSON) : {"niche": "saas"}

Available niche values: saas ,realestate ,ecommerce ,coaching ,agency ,smallbuisness

<img width="784" height="366" alt="image" src="https://github.com/user-attachments/assets/75e9f8be-0805-4110-a793-96f0d261e928" />

On demand: hit the webhook whenever you want fresh leads

On a schedule: point any scheduler at the webhook and wake up to leads

Each run scores up to 9 fresh posts and finishes in under a minute

## Why this beats manual prospecting

- **Rule-safe**: posts flaired "I will not promote" or from no-solicitation threads are auto-disqualified — protects your accounts and reputation
- **Zero duplicate outreach**: the seen-post memory guarantees each prospect is surfaced once, ever
- **Conservative by design**: the rubric prefers missing a weak lead over handing you a bad one — when it says 6+, the pain is real and the poster can pay
- **Personalized at scale**: every DM is grounded in what the poster actually wrote; if a message could be sent to anyone, it gets rewritten

One good client covers years of runs. Point it at your niche and let Reddit come to you. 🚀
