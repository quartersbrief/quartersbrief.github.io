---
layout: post
title: A new way of providing data
subtitle: How core parts of quartersbrief's internals are being reworked
---
A [previous post]({% post_url 2023-11-06-status-report %}) hinted at changes to "some fairly fundamental parts of quartersbrief". Let's have a look at what that means exactly.

## How quartersbrief manages its data

At an abstract level, quartersbrief works by [extracting data from the game files and using this as its informational base]({% post_url 2023-08-07-what-sorcery-is-this %}/#how-does-it-know-so-much-about-the-ships). Player information is obtained from the official Wargaming API. Internally, all this works through an intermediate layer that is responsible for providing the right piece of data for a given identifier. 

Say you want to know everything about the American tier IX destroyer Fletcher. Fletcher goes by the name `PASD021_Fletcher_1943` internally (catchy, right?) Within quartersbrief, there is an intermediary that knows this, and is able to provide the associated data set when asked for `PASD021_Fletcher_1943`. A similar process happens for information about players, armor, or anything else quartersbrief uses as an information source.

So far so good. Crucially, though, each of those intermediaries is currently developed separately, and with slightly different interfaces. However, clearly all of them share common characteristics: At their core, they each take a "name" (a designator). They each must have a way of translating that designator into a specific place to look within the data source, and then they must each process and return that data. To speed things up a bit, there is usually also a bit caching involved - i.e., data that has already been requested once is held in memory instead of being acquired anew when requested again.

The "fundamental changes" currently taking place acknowledge that and move data provision onto a new, common underlying foundation. This promises synergies - first and foremost improvements in maintainability, stability and extensability. 

## Reaching retirement age

For ship parameters - e.g. how fast a ship is, what guns it has, what torpedoes it carries, that sort of thing - the code that currently does this is actually some of the oldest code in quartersbrief: It literally contains some of the first lines ever written in the project. 

Now, there is nothing wrong per se with old age. But experience has shown some of the design decisions made back then to be a little bit limiting now. For one thing, quartersbrief right now does a lot of work "up front", which leads to a rather large memory footprint, slow application start, and slow briefing generation. In fact, keeping briefing generation performance within tolerable levels currently requires some fairly intricate tricks during data processing.

This code is being rewritten entirely. In the future, data will only be loaded when required, which should reduce memory footprint and application start times. Data processing will be done in a more targeted way, which should speed up briefing generation (although some of that performance gain will be offset by the on-demand loading). 

In addition, these changes allow for much for flexibility in how data is processed before it is delivered to quartersbrief. Wargaming's format for defining game objects has grown over the years, with bits and pieces being added more or less arbitrarily whenever needed. This makes it, shall we say, somewhat messy in places. Previously, quartersbrief's data provision layer just passed all that weirdness on, and then the engine had to work around it. The new way allows for some processing to take place before game objects are handed off to the engine. While this does not benefit you as the end user in any way directly, it should make game objects much smoother to work with internally, and allow a quicker implementation of whatever Wargaming may be bringing next to the game. 

These changes will be tested for their practicality in the next alpha release. 