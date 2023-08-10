---
layout: post
title: What sorcery is this?
subtitle: A look inside the engine room of quartersbrief
---
Curious how quartersbrief works under the hood? Take a peek behind the curtain in this post and explore some of the internal workings of quartersbrief.

## How does it know when I'm in battle?

Easy: If shells are raining down around you, you are probably in battle. 

## Haha. But seriously

Whenever you enter a battle, World of Warships creates a temporary file called `tempArenaInfo.json` in your replays directory. Quartersbrief maintains a watch on the replays folder. When the file is created, that means you have just entered battle. When it is deleted, you have finished the battle. This is a technique also used by other tools such as Potato Alert, Matchmaking Monitor and the like. It's also the reason you need to have replays enabled (nowadays the game default) for quartersbrief to work.

## How does it know what ships to show?

`tempArenaInfo.json` also contains some metadata about the battle - first and foremost, the names of all participating players, what team they are on, and numeric codes for the ships they are sailing. Quartersbrief reads this file, translates the code to the corresponding ships, and creates a briefing about all the ships present in the battle.

What ships actually get shown may differ from topic to topic, though. Some topics may only show enemy ships, or may be filtered to specific classes. This is usually done to avoid overwhelming you with information, and much of it is configurable.

## How does it know so much about the ships?

Quartersbrief uses data mining: It extracts data directly from the game files, translates it into a more convenient format, and uses this as the data base. Whenever quartersbrief detects a new game version, it will update its data. This is a technique commonly employed by the more "serious" tools for the game, such as the [World of Warships Fitting Tool](https://wowsft.com/) (an excellent resource about the game in its own right, if you haven't tried it out).

This approach makes quartersbrief largely independent of the official API, and allows it to provide you additional information that is not available through the API, such as ship armor models. It also means that even if Wargaming should allow the API to deteriorate again (it had been hopelessly out of date for a long time before a facelift was announced in early 2023), quartersbrief will still be able to deliver accurate briefings to you. The same is true for new ships, and even for the test ships you encounter from time to time. Basically, as soon as anything is included in the game, quartersbrief has access to it.
