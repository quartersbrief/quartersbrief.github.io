---
layout: post
title: Target reacquired
subtitle: A redesign of how gun accuracy is displayed
---
A [previous post]({% post_url 2024-06-05-gun-accuracy %}) had introduced expected miss distance as a measure of gun accuracy. Learn why quartersbrief is now shifting to a different metric.

## Ideas

Over the course of previous work, expected miss distance had been developed as a measure of gun accuracy. Expected miss distance is, basically, the expected average of how far your guns will be off target over many, many shots. In [the most recent alpha]({ post_url 2024-11-17-v0.3.0-alpha.6-released }), a draft version of the "Main Artillery" topic was introduced, which showed a graph of how expected miss distance changed according to range for displayed ships.

![The "Main Artillery" topic as it was previously](/assets/img/2025-02-08-target-reacquired/main-artillery-old.png) 

This design promised a number of advantages:

1. It is visually different from other topics, making it easy to identify.

2. Showing ships in a common graph makes it easy to see how ships' maximum ranges compare.

3. In addition, it makes it clear how ships' gun accuracy relates to each other, and in what ranges it favors one side or the other. In the image above, for example, Kremlin outperforms Marlborough at ranges up to about 19km.

## Problems

Unfortunately, this approach proved to also have a number of problems that go against core design principles of quartersbrief. 

Firstly, displaying all ships in the same graph produces a cluttered result that makes it hard to pick out individual ships quickly. This goes against the principle that information should be provided in such a way that it can be picked up at a glance. The highlighting feature (in the example image, Marlborough) helps against this, but doesn't completely alleviate it. 

In addition, expected miss distance is a metric where less is better: More accurate ships have lower expected miss distances. Within the graph, this makes more accurate ships appear lower than inaccurate ships. This, however, is a serious problem: People tend to take in information from the top. So, at first glance, the ships that will stand out will be the _inaccurate_ ones, and the more accurate ones - the far bigger threat - will only become obvious on closer inspection. This goes against the principle that information should be displayed in an intuitive fashion that makes it easy to identify who you need to worry about and who you don't.

Ultimately, I therefore decided to abandon this approach altogether and redesign the topic.

## Solutions

To solve the difficulties with expected miss distance, I developed a new metric for accuracy: _accurate shot probability_. This is simply the likelihood of your shot falling within a certain distance from where it was aimed. The size is configurable, but the default is a box of 30 meters horizontally and 10 meters vertically. In this metric, more is better, making it easier to display this information in such a way that the biggest threats will stand out visually.

I also discarded the idea of displaying all ships in the same graph, instead settling on the more traditional paradigm of showing ship characteristics by individual, vertically stacked colored bars. The characteristic shape of these "bars" (if you can even call them that? Maybe let's call them spikes.) helps to make the topic as a whole visually distinguishable.

![The redesigned "Main Artillery" topic](/assets/img/2025-02-08-target-reacquired/main-artillery-new.png)

The width of the spike represents the chance of *at least one shot out of a full broadside* hitting within the box. The box can be configured in the topic options (`horizontalBoxSize` and `verticalBoxSize`), but defaults to 30m&times;10m. The spike's length shows the ship's maximum range.

In the image above, you can clearly see how cruiser accuracy is much better than battleship accuracy. Ibuki and Charles Martel in particular possess good accuracy even out to range. You can also see that Marlborough - a ship infamous for its bad accuracy - is a significant threat when you adjust for the number of its guns. Also, République's superior range jumps out at you even at a glance.

This topic is still in an early stage, and far from finished. What remains to be done? Add hover details. Find a way to show the various range and accuracy increasing consumables, such as spotter planes, the Japanese battle cruisers' high precision optics, and Satsuma's funny button. Ideally, I would also like to make it better stand out where you are at an accuracy advantage and where not - i.e., what a preferred combat range should be.

This topic will be included in the next pre-release as a draft.