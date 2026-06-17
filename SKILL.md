---
name: spot-upset-value
description: Use when analyzing sports matchups, betting odds, handicaps, underdogs, upset risk, 爆冷, 冷门, 竞彩, fair odds, value gaps, or market mispricing across football, basketball, tennis, UFC/MMA, and other sports.
---

# Spot Upset Value

## Overview

Evaluate sports matchups for upset potential by comparing live market odds against a data-informed fair probability. Treat the output as probabilistic analysis, not betting advice or a guaranteed pick.

## Required Research

Browse the web unless the user explicitly supplies all current data and asks not to browse. For each matchup, gather and cite recent sources for:

- Market odds: opening odds, current odds, line movement, handicap/spread, total, moneyline or 1X2.
- Prediction-market cross-check: Polymarket, Predict.fun, and 42.space when equivalent markets exist. Record market question, outcome mapping, price, bid/ask spread, liquidity/volume, last trade or midpoint basis, timestamp, and resolution rules.
- Team/player state: injuries, suspensions, expected lineups, rest, travel, schedule density, rotation risk.
- Motivation: table position, qualification, relegation, playoff seeding, cup priority, rematch context.
- Performance quality: opponent-adjusted recent form, home/away splits, style matchup, efficiency metrics.
- Public heat: one-sided market narratives, heavy favorite popularity, stale reputation, media bias.
- Sport randomness: scoring environment, variance drivers, ruleset, judging/referee risk, surface/court/style fit.

Explicitly state the timestamp of key data: odds snapshot time, injury report date, lineup confirmation time, and weigh-in or starting XI time when relevant. For live odds, include the snapshot time to the minute when available. Flag data older than 48 hours as potentially stale. State clearly when data is missing, stale, or inferred.

## Core Workflow

1. Identify the market and underdog outcome.
   - Football/soccer: analyze 1X2, draw, handicap, and double chance separately when relevant. In football 1X2 markets, always evaluate the draw as a separate upset angle alongside the underdog win, especially when the favorite is strong but not dominant enough to win outright.
   - Basketball: analyze moneyline and spread upset separately. For basketball spreads, an "upset" may mean the underdog covers the spread, not necessarily winning outright; label these separately.
   - Tennis/UFC: analyze match winner, method/sets/rounds only when the user asks.
   - Multi-match requests: score every matchup and rank the strongest upset value.
   - Combined outcomes: when the user asks about "win or draw" or similar unions, use the quoted double chance odds if that is the target market, and cross-check it against the sum of the constituent 1X2 no-vig probabilities. If no double chance odds are available, estimate the union from the constituent no-vig probabilities and mark it as derived.
   - Separate outright underdog wins from draw, double chance, and handicap-cover angles. A good draw or +handicap angle does not automatically make the underdog win a good angle.

2. Convert market odds into probabilities.
   - Decimal odds implied probability: `p = 1 / odds`.
   - Remove overround by normalizing all listed outcomes: `no_vig_p_i = implied_p_i / sum(implied_p_all)`.
   - If only one side is available, calculate implied probability but mark no-vig probability as incomplete.
   - Asian handicap requests: use the target handicap market's two-sided prices when available. Use no-vig normalization as a rough estimate, explicitly label the result as approximate, and cap confidence at Medium unless full Asian-handicap-adjusted formulas are available. Half-goal lines have no push, integer lines can refund stake on a push, and quarter-goal lines split the stake; for PK/integer lines, prefer actual AH/DNB/PK prices over substituting 1X2 prices.

3. Run prediction-market cross-check.
   - Check Polymarket, Predict.fun, and 42.space when equivalent markets exist.
   - Do not treat prediction-market prices as sportsbook no-vig odds. Treat them as a separate market-implied probability signal.
   - Compare only equivalent outcomes and settlement rules. If resolution wording differs, mark the signal as weak or unusable.
   - Prefer bid/ask midpoint for liquid markets; note when using last traded price because the spread is wide or the orderbook is thin.
   - If prediction markets disagree sharply with sportsbook no-vig probability, treat it as a prompt to investigate the reason, not automatic value.
   - If a source is unavailable, API-gated, thinly traded, or only manually checked, label it explicitly instead of filling the gap.

4. Estimate model probability.
   - Start from the no-vig market probability.
   - Adjust only for researched factors with evidence.
   - Penalize confidence when the evidence is thin or conflicting.
   - Never invent precision; prefer ranges when inputs are uncertain.
   - Do not adjust the no-vig probability by more than 15 percentage points in either direction unless there is overwhelming, multi-source evidence. Most plausible edges are 2-8 percentage points; be especially cautious when a long-shot probability would more than double.
   - If no-vig probability is incomplete, widen the model probability range by at least +/-5 percentage points, cap score at 59 per the rubric, cap confidence at Low-Medium, and avoid presenting odds value edge as precise.
   - In football, do not convert a draw-heavy tournament trend into a higher outright underdog-win probability. Apply that trend first to draw, double chance, or handicap-cover angles.
   - When the no-vig favorite win probability is 60% or higher, confirm with Asian handicap or sharper spread markets before calling the favorite fragile. If Asian handicap prices do not show favorite weakness, reduce confidence in any underdog-win edge.
   - When the no-vig favorite win probability is 60% or higher and there is no hard negative evidence against the favorite, cap the underdog outright-win score at 60 and normally cap the underdog outright-win probability adjustment at +4 percentage points. Hard negative evidence means confirmed major absences, meaningful rotation, clear motivational downgrade, adverse rest/travel spot, or a respected-market handicap move against the favorite.

5. Calculate fair odds and value gap.
   - Fair odds: `fair_odds = 1 / model_probability`.
   - Odds value edge: `odds_edge_pct = market_odds / fair_odds - 1`.
   - Probability edge: `prob_edge = model_probability - no_vig_market_probability`.
   - Example: market weak-side odds `4.00`, no-vig probability `25.0%`, model probability `28.6%`, fair odds `3.50`, probability edge `+3.6 percentage points`, odds value edge `+14.3%`.

6. Score upset value from 0 to 100.
   - Always use the detailed rubric in `references/scoring-rubric.md` to derive the upset score, including single-match requests.
   - Show a compact component breakdown or name the strongest score drivers so the score is auditable.
   - Do not assign a high score just because the underdog odds are large. High score requires both plausible upset path and market mispricing.
   - Report three scores when possible:
     - `Likelihood score`: how plausible the upset outcome is in football terms.
     - `Value score`: how much the market price appears above fair odds.
     - `Overall upset score`: the decision score after caps, evidence quality, and market confirmation.
   - Rank by `Overall upset score`, not by raw odds value alone. If the value score is high but likelihood is weak or capped, say it is a price-only lean rather than the best upset.

7. Produce a concise ranked report.
   - Show the strongest upset value first.
   - Include market odds, implied/no-vig probability, prediction-market check, estimated probability, fair odds, probability edge, odds value edge, upset score, confidence, and key evidence.
   - Separate "most likely upset" from "best value" when they differ.
   - If no angle clears the value threshold, say "No meaningful upset value found" and explain why passing is the better read.

## Output Format

Use this format for each matchup:

```text
Rank: #1
Match: Team A vs Team B
Underdog angle: Team B win / draw / +spread / handicap
Market odds: 4.00
Data freshness: odds as of [time], injury/lineup as of [time], [flag if stale]
Market no-vig probability: 25.0% (or "incomplete")
Prediction-market check: Polymarket/Predict.fun/42.space prices, spread/liquidity, timestamp, and Cross-market read
Estimated fair probability: 28-30%
Fair odds: 3.33-3.57
Probability edge: +3 to +5 percentage points
Odds value edge: about +12% to +20%
Upset score: 76/100
Likelihood score: 68/100
Value score: 80/100
Overall upset score: 74/100
Confidence: Medium
Why it can upset: 3-5 evidence bullets
Why it can fail: 2-3 risk bullets
Sources checked: concise linked source list
```

Use this shape when no value is found:

```text
Rank: N/A
Match: Team A vs Team B
Verdict: No meaningful upset value
Market odds: 6.50
Data freshness: odds as of [time], injury/lineup as of [time], [flag if stale]
Market no-vig probability: 15.4%
Prediction-market check: unavailable / weak / already priced in
Estimated fair probability: 14-17%
Fair odds: 5.88-7.14
Probability edge: none / marginal
Odds value edge: none / marginal
Reason: Underdog has a plausible path, but the market price already reflects it.
```

End with a ranked table for multi-match cards:

```text
Rank | Match | Underdog angle | Market odds | Prediction-market read | Fair odds | Probability edge | Odds value edge | Upset score | Confidence
```

## Guardrails

- Do not claim certainty or imply guaranteed profit.
- Do not recommend bet sizing unless the user explicitly asks; if asked, keep it educational and risk-first.
- Avoid using unsourced rumors as hard adjustments.
- Do not compare odds from different markets as if they are equivalent.
- Do not average sportsbook odds, prediction-market prices, and 竞彩 into one blended probability. Keep each market source separate and explain agreement or disagreement.
- Do not let name recognition dominate the estimate; focus on current conditions and price.
- When live odds are unavailable, label the analysis as pre-market or incomplete.
- For 竞彩 (official lottery-style odds in China), note that the odds include fixed margins and may differ from sharper exchange, European, or Asian handicap markets. Do not treat 竞彩 alone as a complete market view; if no alternative market odds are available, mark no-vig and market-efficiency judgments as incomplete.
