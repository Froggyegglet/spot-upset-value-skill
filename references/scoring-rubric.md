# Upset Scoring Rubric

Use this rubric to turn research into a 0-100 upset value score. The goal is to identify underdogs whose actual chance appears better than the market price, not merely long shots.

## Score Components

| Component | Points | What to reward |
|---|---:|---|
| Market mispricing | 0-30 | Model probability exceeds no-vig market probability; fair odds shorter than market odds |
| Favorite vulnerability | 0-20 | Injuries, rotation, schedule congestion, low motivation, poor matchup, inflated reputation |
| Underdog path | 0-20 | Home edge, tactical/style fit, defensive resilience, counterpunching, serve/pace/grappling edge |
| Variance profile | 0-15 | Low-scoring sport, single-elimination, judging volatility, high 3-point variance, red-card/penalty risk |
| Market heat signal | 0-15 | Public favorite steam, odds drop without line confirmation, narrative one-sidedness |

## Evidence Quality Cap

Do not give positive points merely because data is abundant. Use data quality as a confidence cap or penalty:

| Evidence state | Cap / penalty |
|---|---|
| Current odds, injuries, lineups, and context are available from multiple sources | No cap; confidence may be High if the analysis also supports it |
| Key data is older than 48 hours | Cap confidence at Medium and flag as potentially stale |
| Lineups, weigh-ins, or injury status are missing for a lineup-sensitive event | Cap confidence at Low-Medium and normally cap score at 69 |
| Odds are incomplete or from only one side of the market | Mark no-vig probability incomplete and normally cap score at 59 |
| Sources conflict materially | Use a probability range, explain the conflict, and cap confidence at Low-Medium |

## Interpretation

| Score | Meaning |
|---:|---|
| 85-100 | Rare strong upset/value signal; still uncertain, but market may be meaningfully wrong |
| 70-84 | Good upset candidate; price and matchup both support the angle |
| 55-69 | Watchlist; some upset signals, but price or evidence is not strong enough |
| 40-54 | Weak lean; underdog can win but market price is probably close |
| 0-39 | Low value; upset requires too many things to break right |

## Probability And Odds Notes

- Convert decimal odds with `1 / odds`.
- For a two-way market at `1.50 / 2.75`, implied probabilities are `66.7% / 36.4%`; total is `103.1%`; no-vig probabilities are `64.7% / 35.3%`.
- For a three-way football market, normalize home/draw/away together. Do not normalize only favorite and underdog while ignoring draw.
- For combined outcomes such as win-or-draw, use the actual double chance odds when analyzing that market. Also compare against the union of constituent 1X2 no-vig probabilities as a sanity check.
- For Asian handicap markets, simple no-vig normalization is approximate. Half-goal lines have no push, integer lines can refund stake, and quarter-goal lines split stake across adjacent handicaps.
- Fair odds are the inverse of the estimated probability. If estimated underdog probability is `0.286`, fair odds are `3.50`.
- Show both probability edge and odds value edge. Probability edge is measured in percentage points; odds value edge is measured as a percentage return versus fair odds.
- Do not adjust the no-vig probability by more than 15 percentage points in either direction without overwhelming, multi-source evidence. Most real edges are 2-8 percentage points.
- Positive value exists only when market odds are meaningfully higher than fair odds after uncertainty. If the edge disappears inside the probability range, call it marginal or no value.

## Prediction-Market Cross-Check

Use Polymarket, Predict.fun, and 42.space as corroborating or dissenting market signals when equivalent markets exist. Do not create a separate score component; feed this evidence into Market mispricing, Market heat signal, and confidence caps without double-counting.

- Do not treat prediction-market prices as sportsbook no-vig odds. They are market-implied probabilities from a different venue structure.
- Compare only equivalent outcomes and settlement rules. If the market question or resolution rules differ, mark the signal as weak or unusable.
- Record price basis: bid/ask midpoint, last trade, displayed probability, orderbook spread, liquidity/volume, and timestamp.
- Stronger signal: a liquid prediction market with tight spread agrees with an underdog probability above sportsbook no-vig, or multiple venues point in the same direction.
- Weaker signal: wide spread, low liquidity, stale last trade, unresolved wording ambiguity, exact-score fragments that do not map cleanly to 1X2/spread, or an API-gated/manual-only source.
- Treat disagreement as an investigation prompt. Look for lineup news, market segmentation, settlement wording, liquidity distortion, or stale odds before adjusting probability.
- If prediction-market data is unavailable or API-gated, say so and do not penalize the matchup unless the missing data was central to the user's requested comparison.

## Market Heat Signals

Reward market heat only when it supports mispricing rather than simply confirming the favorite is popular:

- Favorite ticket or money share is very one-sided on public platforms, especially around 80% or higher, while price action or respected-market movement does not confirm it.
- Favorite odds shorten without corresponding lineup, injury, rest, matchup, or tactical news.
- Headline previews and social narratives overwhelmingly favor one side while the price has already moved toward that story.
- Avoid double-counting heat and line movement if they describe the same signal.

## Sport-Specific Triggers

Football/soccer:
- Favorite only needs a draw, has congested schedule, rotates for cup/Europe, lacks ball-progressing midfielders, faces a low block or fast counter side.
- Draw can be the best upset angle when the favorite is strong but lacks urgency.
- Always evaluate the draw as its own upset angle in 1X2 markets, especially when the underdog win is too thin but the favorite's win probability looks overstated.

Basketball:
- Back-to-back, travel, rest disadvantage, injury to primary creator or rim protector, three-point volatility, pace mismatch, inflated public favorite.
- Spreads can show upset value even when moneyline is thin.
- For spreads, "upset value" can mean the underdog covers the spread without winning outright. Keep spread-cover analysis separate from moneyline upset analysis.

Tennis:
- Surface fit, serve hold rate, return weakness, recent workload, injury rumors, head-to-head style, pressure in short formats.

UFC/MMA:
- Style clash, takedown defense, cardio over five rounds, short-notice changes, weight cut, judging volatility, durability, finishing upside.

## Common Mistakes

- Treating "underdog odds are high" as value without proving market mispricing.
- Ignoring the draw in football 1X2 markets.
- Treating double chance, 1X2, handicap, spread, and moneyline as interchangeable markets.
- Using stale injury news after lineups or weigh-ins changed the market.
- Double-counting the same signal, such as rotation and low motivation from the same cup context.
- Giving high confidence when the market data is incomplete.
- Reporting an odds value edge without also showing the probability edge.
- Forcing a pick when the honest answer is pass / no meaningful upset value.
