# Product Requirements Document: ConvoAds
**Author:** Akshat Sharma
**Status:** Prototype / Concept
**Last updated:** September 2026

---

## The Problem

Google's ad business depends on Search. In 2025, Google Search generated over $175 billion in advertising revenue — roughly 57% of Alphabet's total revenue. That model works because Search queries have clear commercial intent, and the ad auction system matches that intent to advertisers willing to pay for it.

Gemini doesn't have this. Every conversational AI query that replaces a traditional search query is a query Google can't monetize. At current adoption rates, this cannibalization could represent tens of billions in at-risk revenue over the next 3-5 years.

The competitive picture makes this more urgent. ChatGPT and Claude have no ads. Perplexity has experimented with sponsored follow-up questions. Nobody has cracked it yet. Whoever figures out how to monetize conversational AI without destroying the user experience gets a structural advantage.

## Hypothesis

Contextually relevant sponsored content, served in the right format based on user intent, can generate meaningful ad revenue within conversational AI without measurably degrading user satisfaction or trust.

The key variable is format-intent matching. A blanket approach (same ad format for every query) will fail — either too aggressive for informational queries or too weak for high-intent ones. The ad engine needs to understand where the user is in their decision journey and adapt accordingly.

## Goals

1. Demonstrate that conversational AI CAN serve ads without wrecking the experience
2. Prove that intent-aware ad format selection (text vs display) outperforms one-size-fits-all
3. Define the guardrails that keep user trust intact
4. Estimate the revenue potential at scale

## Non-Goals

- Maximizing ad revenue from day one — trust matters more than CPMs at this stage
- Building a production ad serving infrastructure
- Replacing or modifying the AI response content to favor advertisers
- Personalized targeting using user data (this prototype is query-context only, no user profiling)

## User Stories

**As a user asking an informational question,** I want to get a complete, helpful answer without being distracted by product ads — so I can learn without feeling sold to.

**As a user researching a purchase,** I want to see relevant product options alongside my answer — so I can compare and decide without leaving the chat.

**As a user ready to buy,** I want to see specific products with prices, ratings, and direct links — so I can complete my purchase quickly.

**As an advertiser,** I want my ads shown to users who are actually in the market for what I'm selling — so I don't waste spend on users who aren't ready to buy.

## The AIDA Framework

The engine classifies each query into one of four intent stages and selects the ad format accordingly.

### Awareness
**User behavior:** Exploring, learning basics. No purchase intent.
**Signal words:** "what is", "how does", "explain", "tell me about", "what are"
**Ad format:** Text ads only — educational content, brand awareness, guide links
**Rationale:** User hasn't formed a preference yet. Visual product ads would be irrelevant noise.

### Interest
**User behavior:** Actively researching, comparing options. Forming preferences but not ready to buy.
**Signal words:** "vs", "compare", "pros and cons", "which is better", "review", "difference between"
**Ad format:** Text ads only — comparison guides, expert reviews, sponsored content
**Rationale:** User is evaluating. They want information, not checkout buttons.

### Desire
**User behavior:** Narrowed down needs, has budget parameters, looking at specific options.
**Signal words:** Price ranges ("under $150"), specific features ("for flat feet"), "best", "top", "recommended for"
**Ad format:** Display ads (product cards with pricing and ratings) + supporting text ads
**Rationale:** User WANTS to see options. Product cards with prices and ratings add value here — they help the user make a decision.

### Action
**User behavior:** Ready to transact. Looking for where/how to buy.
**Signal words:** "buy", "order", "book", "sign up", "get", "where to buy", "best price", "free shipping", "discount", "deal"
**Ad format:** Display ads with strong conversion CTAs (Buy Now, Book Now, Get Started)
**Rationale:** User has decided. Reduce friction to conversion.

## Ad Formats

### Text Ads (Responsive Search Ads)
- Headline (clickable, blue)
- Display URL (green, indicates destination)
- Description (1-2 lines explaining the offering)
- Clear "Ad" badge
- Positioned below the AI response

Best for: Awareness and Interest queries where the ad's job is to educate or build familiarity, not to sell.

### Display Ads (Visual Product Cards)
- Brand/product image area (gradient with icon in prototype; real product images in production)
- Brand name badge
- Product name and tagline
- Description
- Price and star rating
- CTA button (Shop Now, Book Now, etc.)
- Clear "Ad" badge

Best for: Desire and Action queries where the user is comparing specific products or ready to purchase.

## Trust Guardrails

These are non-negotiable. If any of these are violated, the feature should be rolled back.

1. **The AI response is never modified to favor an advertiser.** The response is generated independently. Ads are appended below, never inserted into or substituted for the actual answer.

2. **All ads are clearly labeled.** Every sponsored item carries an "Ad" or "Sponsored" label visible without scrolling.

3. **Users can turn ads off.** A single toggle removes all sponsored content. No dark patterns to re-enable.

4. **The AI response must be fully useful on its own.** If you remove every ad, the answer should still completely address the user's question. Ads are additive, never substitutive.

5. **No user data profiling for ad targeting.** This version relies only on the content of the current query — not search history, location, or demographic data.

## Success Metrics

### Primary
- **CSAT delta:** User satisfaction must not drop more than 5% compared to no-ads baseline
- **Ad relevance score:** Users should rate ads as relevant >75% of the time

### Secondary
- **CTR by funnel stage:** Text ads CTR in Awareness/Interest, Display ads CTR in Desire/Action
- **eCPM:** Revenue per 1,000 ad impressions, broken down by format and funnel stage
- **Format accuracy:** Did the engine serve the right format for the query's actual intent stage?

### Guardrail Metrics (kill switches)
- **Trust score drop > 5%** → pause rollout
- **Opt-out rate > 15%** → reassess ad format and frequency
- **Session length decrease > 10%** → ads are driving users away
- **Response quality perception drop** → ads are contaminating trust in the AI itself

## Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Users reject any ads in AI chat | Medium | High | Phased rollout starting at 5%, measure before expanding |
| Ads erode trust in AI response accuracy | Medium | Critical | Strict separation — response generated before ads, never modified |
| Regulatory (FTC disclosure) | Low | High | Clear labeling, no deceptive patterns, legal review |
| Advertisers game the system | Medium | Medium | Quality scoring, manual review at launch, automated filters at scale |
| Intent misclassification (showing product ads on informational queries) | Medium | Medium | Conservative classification — default to text ads when uncertain |

## What's Not Built Yet

This is a prototype. The following would be needed for production:

- **Dedicated intent classifier** — lightweight ML model trained on labeled query data, not an LLM call per query
- **Real ad inventory integration** — Google Ads API, real-time bidding
- **User research validation** — the research plan exists (see `research-plan.md`) but hasn't been executed
- **A/B testing infrastructure** — the experiment design exists (see `experiment-plan.md`) but hasn't been run
- **Advertiser controls** — brand safety, category exclusions, bid management
- **Feedback loops** — user signals (dismiss, click, ignore) feeding back into relevance scoring
