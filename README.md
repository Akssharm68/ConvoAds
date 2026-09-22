# ConvoAds: Contextual Ad Monetization for Conversational AI

**The problem is simple:** Google Search makes $175B+ a year from ads. Gemini makes $0. Every query that moves from Search to Gemini is revenue Google loses and that migration is accelerating.

This prototype explores one possible answer: what if conversational AI could serve ads that users actually find helpful?

[→ Try the Prototype](https://akssharm68.github.io/ConvoAds/)

---

## Why I Built This

I kept noticing something while using Gemini for shopping and travel queries. When I ask "best laptop under $1200," Gemini gives me a solid answer but there's no way for a brand to say "hey, check out our new model that fits exactly what you're looking for." On Google Search, that's a sponsored result. On Gemini, that monetization layer doesn't exist yet.

That's a massive gap. Not just for Google's revenue, but also for users sometimes the sponsored result IS the most relevant one. The question isn't whether ads belong in AI chat. It's how to do it without making the experience worse.

## The Core Idea: Match Ad Format to User Intent

Not all queries deserve the same ad treatment. Someone asking "what are running shoes made of?" is in a completely different headspace than someone asking "buy Nike Pegasus 41 best price."

ConvoAds classifies every query across the AIDA purchase funnel and serves the ad format that matches where the user is in their decision:

| Funnel Stage | What the user is doing | Ad format | Why this format |
|---|---|---|---|
| **Awareness** | Learning, exploring | Text ads only | User isn't ready to buy. Product cards with prices feel pushy. Serve educational content links instead. |
| **Interest** | Comparing, researching | Text ads only | User is evaluating options. Serve reviews, comparisons, expert guides. |
| **Desire** | Has specific needs + budget | Display ads + text | User wants to see options. Product cards with prices and ratings add value here. |
| **Action** | Ready to buy/book/sign up | Display ads | User wants to transact. Strong CTAs, pricing, availability. |

The engine picks up on intent signals budget mentions ("under $150"), comparison language ("vs", "which is better"), action words ("buy", "order", "book") and routes to the right format.

**The product principle:** showing a $129 product card to someone who just asked "what is a running shoe?" is wasted ad spend and damaged trust. Showing it to someone who asked "best stability shoe under $150 for overpronation" is genuinely useful.

## About This Prototype

This is a sandboxed prototype with 7 pre-built demo scenarios spanning all 4 AIDA stages. Free-text input is accepted but routes through a controlled query matcher unrecognized queries surface a guardrail message with suggested demo queries. This keeps the demo self-contained with zero external dependencies.

**Try the same topic at different funnel stages to see the ad format shift:**
1. "What should I look for in running shoes?" → Awareness → text ads only
2. "Nike vs Brooks vs ASICS for flat feet" → Interest → text ads only
3. "Best running shoes for flat feet under $150" → Desire → display ads appear
4. "Buy Nike Pegasus 41 best price free shipping" → Action → display ads with Buy Now CTAs

Toggle **PM View** in the top bar to see the internal engine data, AIDA classification, reasoning, and performance metrics. Users don't see any of this by default; it's the internal PM perspective on how the engine decides what to serve.

## Ad Formats

### Text Ads (Responsive Search Ads)
Headline + display URL + description. Clear "Ad" badge. Low-disruption format that preserves conversational flow. Served on Awareness and Interest queries where the user is learning or researching, not buying.

### Display Ads (Visual Product Cards)
Visual brand/product banner + product name + description + price + rating + CTA. Served only on Desire and Action queries where the user has expressed purchase intent. These ads add value because the user is actively looking for products to compare or buy.

## What I Didn't Do (And What's Next)

I want to be upfront about what this prototype doesn't include:

**No user research yet.** I haven't run interviews or surveys to validate whether users actually find this acceptable. That's the single biggest open question. The research plan is in `docs/research-plan.md` — it outlines the study design, sample sizes, and what I'd measure. The hypothesis is that contextual ads matched to intent stage will be tolerated at significantly higher rates than blanket display ads, but that's unvalidated.

**No real ad inventory.** The ads are pre-built demo content. In production, these would come from Google Ads inventory with real-time bidding, quality scoring, and advertiser controls.

**No A/B test data.** The experiment plan is in `docs/experiment-plan.md`. I've defined the variants, metrics, and sample size calculations, but haven't run the tests.

**No production-grade intent classifier.** The current sandbox uses keyword matching. A production version would need a lightweight ML model trained on labeled query data, running at sub-100ms latency.

These aren't excuses they're the roadmap. Each one is documented with the approach I'd take.

## Metrics That Matter

If this shipped, here's what I'd track:

| Metric | Target | Why |
|---|---|---|
| User satisfaction (CSAT) delta | < 5% drop vs no-ads | Ads shouldn't make the experience worse |
| Ad relevance score | > 75% | Users should find ads contextually appropriate |
| Trust score delta | < 3% drop | Users shouldn't trust AI responses less because ads are present |
| eCPM | $12-45 range | Competitive with Search ad economics |
| Opt-out rate | < 15% | If more than 15% turn ads off, the format is failing |
| Session length delta | No decrease | Users shouldn't leave faster because of ads |

## Project Structure

```
convads/
├── README.md              ← You're here
├── index.html             ← The prototype (7 demo scenarios, no dependencies)
├── docs/
│   ├── PRD.md             ← Product requirements document
│   ├── research-plan.md   ← User research plan (not yet executed)
│   └── experiment-plan.md ← A/B test design
```

## How It Works (Technical)

Single HTML file. Zero external dependencies. All 7 demo scenarios are pre-built with hardcoded responses and ads. The query matcher uses keyword matching to route typed input to the closest demo scenario unrecognized input triggers a guardrail message pointing back to the curated queries.

In a production version, the intent classifier would be a dedicated ML model. The ads would come from real ad inventory with real-time bidding. This prototype demonstrates the UX and product logic, not the production architecture.

```
User query
    ↓
Intent classification (AIDA stage)
    ↓
Ad format selection (text vs display)
    ↓
Contextual ad matching
    ↓
Response + matched ads rendered
```

---

Built by [Akshat Sharma](https://www.linkedin.com/in/aakshatsharma/) · MS Information Systems, Stevens Institute of Technology · Product Management @ JPMorgan Chase
