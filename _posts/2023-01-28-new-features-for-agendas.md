---
layout: post
title: New features for agendas
subtitle: Agendas are getting smarter. A lot smarter.
---
Quartersbrief has always included mechanisms for custom-tailoring briefings to your needs. Right now, while workable, these are often verbose, needlessly repetitive and require a lot of user maintenance. In other words, they get the job done, but they are not very nice to use. With v0.2, agendas are going to learn a lot of new tricks to address these shortcomings.

## The state of affairs

So what, specifically, is the problem with agendas as they are in v0.1? 

Well, for one thing, beyond deploying a default set of agendas the first time you run quartersbrief, there is no central administration in place for them. And this means that any time a new topic is added to quartersbrief, you need to manually insert that topic into all the agendas where you want it. This is obviously far from user-friendly - if that new quartersbrief version you just installed comes with a new type of topic, you should be able to benefit from that without having to manually edit a bunch of files.

In addition, the matching logic for agendas is somewhat limited at the moment: While it is possible to target any constellation of ships imaginable, it may require you to set up several agendas that are identical except for minute differences in their matchers. Applying an agenda to Japanese cruisers and British battleships, for example, is currently only possible by setting up two otherwise identical agendas.

## Centrally administered agendas

With v0.2, agendas now come from two different sources: a user-administered repository, and a new centrally administered one. 

Having a user-administered repository allows quartersbrief to keep the flexibility of custom-tailoring agendas to be exactly the way you want them. A matching user agenda will always take precedence over a matching default agenda, so if you want it, you can always take full control. In addition to the traditional TOML format, agendas may now be defined in YAML or JSON as well - whichever you prefer.

In addition, though, with v0.2 agendas can also come from a central repository that is administered by the installer. That way, agenda definitions can be updated in the same way that the app itself gets updated. So when quartersbrief learns a new topic, it can automatically be included in the appropriate agendas. 

## Extensible agendas

While reading the above paragraph, you may have thought: But wait a minute - wouldn't I still have to manually insert new topics into my user agendas though? The answer is: Well, yes, but actually, no. Or, somewhat more specifically: v0.2 brings a new mechanism that should make this unnecessary in many cases - extensible agendas.

This feature lets you say that an agenda is based on another one. The new agenda will then inherit all settings from the one it is based on, overwriting those settings that are specified in the new agenda. Any additional topics are prepended. 

Why might this be useful? An example: Imagine you have an agenda for destroyers, and that agenda includes the radar topic. This topic has a setting defining at what threshold a ship is considered to "almost" have a stealth radar capability. Now imagine you want to increase that value for Soviet and French destroyers due to their high speed. Previously, this would have required you to have a separate agenda that is identical in everything except that one setting. With agenda extension, while you will still need a separate agenda, it need only specify that one setting, inheriting everything else from the original agenda.

In addition, this mechanism means that as long as you always base your custom agendas off of a default agenda, any changes to those default agendas will automatically be reflected in your user agendas. Thus, when quartersbrief features a new topic, and that topic is incorporated into the appropriate default agendas, it will also be included in all your user agendas based on those default agendas. 

Of course, this feature is not limited to extending default agendas - your user agendas can also extend from each other. So you might have an agenda controlling what to show for destroyers, then extend that in a new agenda controlling what to show in German destroyers, for example.

This feature should greatly cut down on the verbosity and repetitiveness of agenda files, and in conjunction with centrally administered agendas, make new topics available to you with a minimum of work. 

## More versatile matching

Agendas may now define more than one matcher, which will greatly cut down on the need to repeat yourself. To stay with the above example (Japanese cruisers and British battleships), instead of having to have two largely identical agendas, you can now have one agenda that defines two matchers: one for the Japanese cruisers, and one for the British battleships. 

The number of matchers is not limited, and the way individual matchers are evaluated is still the same. An agenda will be considered to match a battle if at least one of its matchers matches the battle. 

Of course, existing agendas may still continue to use the old format of having a single matcher. 

## New has clause

The matchers themselves are also becoming more powerful with v0.2, which brings a new type of clause: the has-clause. Where previously you could match only against ship class, tier, nationality and name, the has clause allows you to match against just about any property of the ship. This is an extremely powerful tool: Using has, you can, for instance, express that an agenda should match any ship that has the smoke generator consumable, or that has guns of a caliber of at least 150mm. You can even combine the two. Previously, this could only be expressed by listing all ships explicitly.
