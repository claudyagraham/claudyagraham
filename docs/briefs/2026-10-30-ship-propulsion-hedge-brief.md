---
type: brief
engagement: fx-hedging
capability: fx-hedging
date: 2026-10-30
status: committed
hypothesis: "I expect the forward to beat the put in most scenarios because its 1.1807 rate sits about 5.8% above spot, and the euro would need to finish above about 1.2052 for the put to win."
---

# Hedging a EUR 5.6 Million Receivable: Exposure, Options, and Next Steps

## The exposure

We are owed EUR 5,600,000 by a Greek shipyard, due in 365 days. At today's EURUSD of about 1.1158 (TradingView, quoted Oct 31, 2026, 07:57 UTC), that is about $6.25 million. The euro amount is fixed, but the dollar amount is not. If the euro weakens 5% by settlement, we receive about $312,000 less. At 10%, we receive about $625,000 less.

## Why it can hurt us

A weaker euro shrinks the dollars we bank for work we have already done. A $312,000 shortfall would be a real hole in next year's budget, and nothing in our operations would have caused it.

## Three ways to hedge

**Forward:** We agree today to sell EUR 5.6 million at 1.1807 in a year, locking in $6,611,920, about $363,000 above spot dollars. The pro is a certain budget. The con is that we give up any gain if the euro strengthens.

**Money-market hedge:** We borrow euros today, convert them to dollars at spot, and invest the dollars. The receivable repays the euro loan. The pro is certainty without a forward contract. The con is that it uses borrowing capacity, has more moving parts, and depends on interest rates I have not yet sourced.

**Put option:** We pay $0.0245 per euro, $137,200 in total, for the right to sell euros at spot. After the premium, our floor is $6,111,504. The pro is protection with upside if the euro rises. The con is that the premium is gone either way, and the floor sits about $500,000 below the forward's locked amount.

## What I am assuming

I assume the shipyard pays in full and on time, and that the indicative forward rate and option premiums are prices we could actually trade at. Spot comes from one public source. With more time, I would test the quotes against bank pricing and consider payment risk.

## Hypothesis

I expect the forward to beat the put in most scenarios. The forward rate is about 5.8% above spot, and the put only comes out ahead if the euro finishes above roughly 1.2052, about 8% higher than today. That is a large move to count on, and we would pay $137,200 for the chance.

## How I would know I was wrong

If sourced market data in Stage 4 shows a forward rate well below 1.1807, or if the model shows the euro finishing above 1.2052 in most reasonable scenarios, the put would be the better choice and I would revise.

## Next steps

This brief names the exposure before any hedge is chosen. Next I will design the workbook on paper (Stage 2), have an AI build it while I audit its work (Stage 3), replace placeholders with sourced, timestamped market data (Stage 4), and validate the result independently before writing the decision memo (Stage 5). A process is coming, not a gut call.

Reviewed with help from Claude (Anthropic, 2026); edited by me, Claudya Graham. 
