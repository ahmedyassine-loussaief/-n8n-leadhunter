# 🎯 LeadHunter — Reddit Lead Finder for Automation Builders
<img width="1483" height="989" alt="Screenshot 2026-07-17 183439" src="https://github.com/user-attachments/assets/cfd72f22-2709-4cd8-a63e-be32a5d800eb" />

**Stop scrolling Reddit for clients. LeadHunter reads six business communities for you, scores every fresh post for automation pain, and delivers qualified leads to your Discord — each one with a personalized outreach DM already written.**

If you sell automation, workflows, or "I'll fix your repetitive work" services, your best clients are complaining about their problems on Reddit right now. LeadHunter finds them while you do billable work.

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

> 🎯 **New Lead — service-based startups**
> **Post:** reddit.com/r/Entrepreneur/…
> **Pain point:** manually carrying and posting client documents
> **Score:** 7/10 (inferred)
> **Suggested message:**
> "Hey! Saw your post about juggling client intake by hand — I actually build automations for this exact kind of thing…"


Example:
<img width="1452" height="478" alt="Screenshot 2026-07-17 183439" src="https://github.com/user-attachments/assets/5c36501b-3cf6-4ac7-a48c-9bb11e755edb" />



Copy, tweak one line, send. That's the whole workflow.

## Setup — under 5 minutes

You bring exactly one thing: **a Discord bot token** for the server where you want leads delivered (create one free at discord.com/developers, invite it to your server with send-message permission).

Then set two values on your instance:
- `DISCORD_GUILD_ID` — your server ID
- `DISCORD_CHANNEL_ID` — the channel that should receive leads

AI scoring and DM writing are included — **no AI provider account or API key needed.**

## Run it your way

- **On demand**: hit the webhook with `{"niche": "saas"}` whenever you want fresh leads
- **On a schedule**: point any scheduler (cron, calendar automation, another agent) at the webhook and wake up to leads
- Each run rates up to 9 fresh posts and finishes in well under a minute

## Why this beats manual prospecting

- **Rule-safe**: posts flaired "I will not promote" or from no-solicitation threads are auto-disqualified — protects your accounts and reputation
- **Zero duplicate outreach**: the seen-post memory guarantees each prospect is surfaced once, ever
- **Conservative by design**: the rubric prefers missing a weak lead over handing you a bad one — when it says 6+, the pain is real and the poster can pay
- **Personalized at scale**: every DM is grounded in what the poster actually wrote; if a message could be sent to anyone, it gets rewritten

One good client covers years of runs. Point it at your niche and let Reddit come to you. 🚀
