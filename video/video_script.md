# 2-Minute Video Script — CSCE 676 Project Pitch

---

## The Script

**[0:00] Hook**

Howdy sharks. My name is Ahad Hussain, and I just spent the last semester on a question every grocery retailer is trying to answer. When a customer adds something to their cart, how do you predict what they'll add next?

**[0:15] The problem**

Today, every major grocery app answers that question the same way. They mine millions of baskets for patterns — what people buy together, what you've bought before, when you tend to shop. Then they turn those patterns into recommendations for everyone. The assumption baked into this whole approach is that the patterns are universal. That a grad student grabbing dinner and a family of five doing a weekly shop are driven by the same underlying buying patterns. If that assumption is wrong, every recommender built on top of it is leaving signal on the table.

**[0:45] What I built**

So I dug into 3.4 million real Instacart orders and tested that assumption directly. I mined the patterns four different ways. Three were the industry's standard lenses — splitting by time of day, weekend versus weekday, and sequence of orders. The fourth was different. Instead of mining one universal pattern set, I let the data sort customers into types first, then mined patterns separately for each type.

**[1:10] The finding**

Three clear shopper archetypes emerged. Fresh-produce shoppers. Light-pantry shoppers. And household staples buyers. And here's what's wild. When I compared how much each lens actually changed the discovered patterns — splitting by time of day changed 15% of them. Splitting by weekend versus weekday, 44%. But splitting by archetype? 90% of the patterns changed. The universal-pattern assumption breaks. Produce shoppers and staples shoppers don't just buy different things — the rules that predict what they'll buy next are almost entirely different rules.

**[1:45] The ask**

This is the insight every grocery retailer's recommendation team is missing, and the data to prove it is already sitting in their systems. Two million dollars turns this into a proof of concept on a major platform. Not smarter timing. Not better bundles. Knowing that personalization has to start with who your customer is, before anyone else figures that out.

---