# Experiment Plan: ConvoAds A/B Test
**Status:** Designed — not yet executed
**Author:** Akshat Sharma

---

## What We're Testing

The core question: does matching ad format to user intent (AIDA-aware serving) produce better outcomes than showing the same format to everyone?

"Better" means: comparable or higher revenue without measurably hurting user satisfaction.

## Variants

| Variant | Description | What it tests |
|---|---|---|
| **Control** | No ads. Standard AI response only. | Baseline for satisfaction and trust |
| **A: Text only** | Text ads on every query regardless of intent | Whether text-only is "safe enough" across all queries |
| **B: Display only** | Display ads on every query regardless of intent | Whether display ads hurt satisfaction on low-intent queries |
| **C: AIDA-matched** | Text ads for Awareness/Interest, Display for Desire/Action | The ConvoAds hypothesis — format-intent matching |

Variant C is the one I believe will win. But the whole point of testing is to find out if I'm wrong.

## Assignment

- Random user-level assignment (not query-level — a single user sees one variant across all their queries for the test duration)
- 25% of traffic per variant
- Stratify by: usage frequency (daily/weekly/monthly), device (mobile/desktop), region
- Holdout: keep 10% of the Control group permanently ad-free for long-term trust measurement over 6+ months

## Primary Metric

**CSAT delta vs Control**

Measured through an in-session prompt after every 5th query: "How helpful was your experience today?" (1-5 stars). Compare the mean rating of each variant against Control.

**Success criteria:** Variant C's CSAT must be within 5% of Control. If it drops more than 5%, the feature needs redesign before scaling.

## Secondary Metrics

| Metric | How measured | What it tells us |
|---|---|---|
| **CTR** | Clicks on ads / ad impressions | Are users engaging with the ads? |
| **eCPM** | Estimated revenue per 1,000 impressions | Revenue potential |
| **Ad relevance** | Post-click survey sample: "Was this ad relevant?" | Quality of matching |
| **Session length** | Time from first query to session end | Are ads driving users away? |
| **Queries per session** | Count of queries in a single session | Are users asking fewer questions because of ads? |
| **Opt-out rate** | % of users who toggle ads off (Variants A, B, C only) | Direct signal of ad rejection |
| **Trust score** | Weekly survey sample: "I trust the AI's responses" (1-7) | The most important guardrail |

## Guardrail Metrics and Kill Switches

These trigger an automatic pause of the experiment:

- CSAT drops more than 8% vs Control in any variant → pause that variant
- Trust score drops more than 5% vs Control → pause entire experiment
- Opt-out rate exceeds 20% in any variant → pause that variant
- Session length drops more than 15% vs Control → pause that variant

These are non-negotiable. Revenue is worthless if users leave.

## Duration and Sample Size

**Duration:** 4 weeks minimum

**Sample size calculation:**
- Baseline CSAT: assume 4.1/5.0 (from existing Gemini satisfaction data)
- Minimum detectable effect: 0.15 points (a meaningful drop)
- Significance level: p < 0.05
- Power: 80%
- Required sample per variant: ~1,700 users
- Total required: ~6,800 users across 4 variants

At Gemini's scale this would fill within hours. For a smaller test (internal dogfooding), 4 weeks would be needed.

## Analysis Plan

**Week 1-2:** Monitor guardrail metrics daily. Look for early signals of trust erosion or high opt-out.

**Week 3-4:** If guardrails hold, analyze primary and secondary metrics.

**Key comparisons:**
1. Variant C (AIDA-matched) vs Control — does format-intent matching preserve satisfaction?
2. Variant C vs Variant A (text only) — does adding display ads on high-intent queries improve revenue without hurting CSAT?
3. Variant C vs Variant B (display only) — does removing display ads from low-intent queries improve CSAT?
4. Variant B vs Control — how badly do blanket display ads hurt satisfaction? (This quantifies the cost of the naive approach)

**Segment analysis:**
- Heavy vs light users (do power users tolerate ads differently?)
- Mobile vs desktop (does screen size change ad tolerance?)
- Query type distribution (users who ask mostly informational vs mostly shopping)

## Expected Outcomes (My Predictions)

Putting these on record so the results can prove me right or wrong:

1. Variant B (display everywhere) will have the worst CSAT — probably 8-12% below Control. Display ads on "what is a running shoe?" will feel intrusive.

2. Variant A (text everywhere) will be close to Control on CSAT — maybe 2-3% drop. Text ads are low disruption enough that most users won't care much.

3. Variant C (AIDA-matched) will sit between A and Control on CSAT — maybe 1-3% drop — but will significantly outperform A on revenue because display ads on high-intent queries have much higher CTR and eCPM.

4. Variant C will have the lowest opt-out rate of the three ad variants because users rarely see an ad format that feels wrong for their query.

If prediction #3 holds, the business case writes itself: nearly the same user satisfaction as no-ads, with meaningful revenue per query on the subset of queries where users actually want to see products.

## What I Can't Test Without Google's Infrastructure

- Real ad inventory and real eCPM data (prototype uses simulated ad content)
- Actual Gemini user panel at scale
- Long-term trust effects (needs 6+ month holdout measurement)
- Advertiser-side metrics (bid competition, ad quality scores, return on ad spend)

This experiment design assumes access to these systems. The test structure and analysis plan are valid regardless of platform — only the data source changes.
