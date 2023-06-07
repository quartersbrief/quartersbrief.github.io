---
layout: post
title: Controlling the agenda
subtitle: How to make sure you get the information you need
---
Not all information is equal, and if quartersbrief just showed you everything it could for every battle, you would quickly become overwhelmed with information, unable to find what you really need, and generally no better off than before. Quartersbrief recognizes this and offers a powerful mechanism to adjust briefing content based on the ship you are sailing: agendas.

Agendas are little text files that consist of two basic building blocks: a matches section and a topics section. The matches section describes what battles the briefing is eligible for. The topics section lists and configures the topics that will be part of the briefing.

### Matching matches

Whether or not an agenda is considered a match for a given battle depends on the ship you are sailing in that battle. For this, every agenda contains at least one matcher: a set of conditions that must be fulfilled by your ship for the agenda to be considered a match. For example, a matcher might state that the agenda is eligible for Japanese destroyers of tiers VIII and above. 

Matchers can be based on a ship's class, nationality, tier or name. The individual terms of a matcher are called clauses, and all clauses of a matcher must be satisfied for the matcher to be fulfilled. However, not all possible clauses need to be present - missing ones are simply considered to match everything. 

### Too many choices

More than one agenda might match a given battle. For a bit of an extreme example, you might have an agenda for destroyers in general, then one for Japanese destroyers, another one for tier VIII+ Japanese destroyers, and yet another one for the tier X Japanese destroyer Shimakaze specifically. All of these would match a battle in which you are sailing in a Shimakaze. How does quartersbrief decide which one to base the briefing on?

In simple terms, it looks at how specific your matcher is. In the above example, "destroyers" as a general term is less precise than "Japanese destroyers", which in turn is less precise than "Japanese destroyers of tiers VIII and above". The most specific one is "Shimakaze", and so that is the one that quartersbrief would choose. 

### Setting topics

The second major component of an agenda is its list of topics. This defines what content will be shown in the briefing, and in what order. Most topics offer some options for customization, for example restricting them to certain ships or modifying details of their behavior. 

