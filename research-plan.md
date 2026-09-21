# User Research Plan: ConvoAds
**Status:** Planned — not yet executed
**Author:** Akshat Sharma

---

## Why This Matters

The entire ConvoAds hypothesis rests on one assumption I haven't proven yet: that users will tolerate ads in conversational AI if the format matches their intent. Everything else — the AIDA classification, the text vs display split, the trust guardrails — is product logic built on top of that assumption.

Until real users validate it, it's a guess. A well-reasoned guess, but still a guess.

This document outlines what the research would look like if I had the resources to run it. I'm documenting it because the research design itself reflects product thinking — knowing what you'd need to learn, how you'd learn it, and what decisions hinge on the results.

## Research Questions

1. Do users find contextual ads acceptable in a conversational AI interface?
2. Does ad format matter — are text ads more tolerable than display ads in low-intent contexts?
3. At what point do ads start degrading perceived response quality (even if the response hasn't changed)?
4. Does clear "Sponsored" labeling meaningfully affect trust, or do users not notice it?
5. Are there user segments with fundamentally different ad tolerance (e.g., power users vs casual)?

## Study 1: Qualitative — User Interviews (n=15-20)

**Goal:** Understand how users think about ads in AI chat. What makes it feel helpful vs intrusive? Where's the line?

**Participants:** 15-20 regular Gemini/ChatGPT users. Mix of age, tech comfort level, and usage frequency. Recruit through a screener that asks about AI assistant usage (at least weekly) without mentioning ads upfront.

**Method:** 30-minute semi-structured interview. Show them the prototype. Let them try 4-5 queries across different intent stages. Then talk through what they noticed and how they felt about it.

**Discussion guide (key questions):**
- Walk me through what you just saw. What stood out?
- Did you notice the sponsored content? What did you think of it?
- How did the ads affect your perception of the AI's answer?
- Were there moments where an ad actually felt useful? Where it felt annoying?
- What would make you turn ads off?
- Would you trust the AI response less knowing it appears alongside ads?
- If this were your actual AI assistant, would you keep ads on or turn them off?

**What I'd look for:** Not just what people say — but the moments where they hesitate, where their language shifts, where they say "it's fine" but their behavior says otherwise. The gap between stated and revealed preference is usually where the real insight is.

## Study 2: Quantitative — Survey (n=500)

**Goal:** Measure ad tolerance across user segments and validate the AIDA format hypothesis at scale.

**Method:** Online survey with embedded prototype screenshots showing the same query with different ad treatments.

**Design:** Between-subjects. Each participant sees one of four conditions:
- Control: AI response, no ads
- Text ads only: AI response + text ads (regardless of intent)
- Display ads only: AI response + display ads (regardless of intent)
- AIDA-matched: AI response + format matched to intent (the ConvoAds approach)

**Measures:**
- Response quality rating (1-7): "How helpful was this response?"
- Trust rating (1-7): "How much do you trust this response?"
- Ad relevance rating (1-7): "How relevant were the sponsored suggestions?"
- Ad intrusiveness rating (1-7): "How intrusive did the sponsored content feel?"
- Willingness to continue using (binary): "Would you use this AI assistant again with this experience?"
- Open text: "What, if anything, would you change?"

**Sample size rationale:** 500 gives enough power to detect a 0.3-point difference on a 7-point scale between conditions at p<0.05 with 80% power. Split 125 per condition.

**Segmentation:** Analyze by age group, AI usage frequency, self-reported tech comfort, and purchase behavior (frequent online shopper vs not).

## Study 3: Usability Test — Intent Classification Accuracy (n=30)

**Goal:** Validate whether the AIDA classifier puts queries in the stage users actually feel they're in.

**Method:** Show participants 20 queries. For each, ask them: "If you typed this query, what would you be trying to do?" with options mapped to the four AIDA stages (in plain language, not the AIDA labels).

Then compare their self-reported intent stage against the engine's classification. Measure agreement rate.

**Target:** >80% agreement between user self-report and engine classification. Below that, the classifier needs retraining.

## What Decisions Depend on This Research

| Research finding | Product decision |
|---|---|
| Users reject all ads in AI chat | Kill the project or pivot to non-ad monetization |
| Text ads tolerated but display ads rejected | Remove display ads entirely, text-only model |
| AIDA-matched outperforms blanket approach | Validates the core product logic, proceed to A/B test |
| Trust drops when ads appear near AI responses | Increase visual separation between response and ads |
| Intent classifier accuracy is below 80% | Retrain classifier before launch |
| High variance between user segments | Consider per-user ad preference settings |

## Constraints

I don't have access to a user research panel, recruiting budget, or a survey platform right now. If I were doing this at Google, I'd use Google's internal UXR team and their existing Gemini user panel. As a prototype, the research plan exists as documentation of what I'd do — not what I've done.

The prototype itself is the fastest form of validation I could ship: a working thing people can react to, rather than a deck describing a hypothetical.
