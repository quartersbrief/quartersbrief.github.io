---
layout: post
title: Dead center
subtitle: Quartersbrief is learning gun accuracy
---
As previously announced, the overarching goal for v0.3 is to bring more information about ships' guns to quartersbrief. A defining characteristic of guns is accuracy. Here's a glimpse of how that works.

## Recapitulation

A lot of [under-the-hood work]({% post_url 2023-12-10-a-new-way-of-providing-data %}) was necessary before quartersbrief was even capable of properly handling guns internally. This, at its heart, was due to the fact that the way World of Warships defines guns in the game files is different from how most other data is presented. Quartersbrief had to adapt its data provision engine to compensate for this. At this point, rather than slapping on a bandaid for a narrow-scoped fix, I decided to rework data provision completely into a more modular, robust and powerful form that just happens to also be faster. Completing and testing these changes was the main theme of the first three alpha pre-releases of this version. 

With these changes now mostly complete to satisfaction, the groundwork is laid, and work on gunnery has begun. In a first step, quartersbrief is going to learn how to model gun accuracy.

## How World of Warships shoots your guns

Shooting your guns is as simple as a click of your mouse in the game, but a lot actually goes on when you do. For each shell you fire, the game essentially rolls the dice to determine where you will hit. For this, a random miss distance is applied to your aim point. This miss distance follows a Normal (Gaussian) distribution, with the famed Sigma parameter controlling the probability of hitting close to the center (i.e., a low miss distance). 

Just one problem, though: It is in the nature of the Normal distribution that every value has a non-zero probability of occurring. Applied to our shots, this would mean that your shot might go somewhere ludicrously far away from where you aimed. This may be unlikely for each individual shot, but over many, many rounds fired, probabilities would accumulate. To account for this, the miss distance is capped at a maximum value: the dispersion value. If the roll of the dice has determined that your shot would fall outside this maximum value, the game will re-roll the shot. For the re-roll it uses a uniform distribution - presumably to ensure a speedy calculation that is guaranteed to not exceed the dispersion. 

To complicate matters further, the dispersion value itself is not constant: It changes with firing range. When you shoot at a target that's close to you, it will be much less than when you're shooting at a target that's far away. (This is in keeping with how you would naturally expect a gun to perform: You'd expect it to hit closer to where you aimed at 1000m than at 20000m.) Dispersion doesn't even _change_ at a constant rate - it drops off dramatically at melee range (ship-dependent around 4000-5000m).

And finally, to top things off, what we have been talking about so far has been only _horizontal_ dispersion - how far shots will digress to the left or right. Vertical dispersion - how far off they are up and down - is calculated separately, as a function of horizontal dispersion, by applying factors that themselves change with range albeit at different points than horizontal dispersion.

## What is accuracy?

With such a bewildering multitude of variables, it becomes difficult to define what accuracy actually is. None of the values by themselves give a complete picture: Dispersion is measure of how far off your shots may go worst case, but it says nothing about how often that would happen. Sigma shows how much shots cluster toward the center, but doesn't give any indication of how large "toward the center" is. Low horizontal dispersion means your shot will fall well where you aim along the length of a broadside target, while vertical dispersion can make the difference between scoring a citadel or overpenning the upper belt or superstructure.

To amalgamate dispersion and sigma, quartersbrief uses _expected horizontal miss distance_ as a measure of accuracy. For this, it models the probability distribution of horizontal miss distance for a given range: The probability for any horizontal miss distance is the sum of the probabilities of getting that distance from the initial roll (normally distributed), and of getting it from a potential re-roll (uniformly distributed). To distill this into a single value for a given range, it then uses the _mean_ or _expected value_ of this distribution. 

Because vertical dispersion is a function of horizontal dispersion, expected vertical miss distance can then easily be calculated from the horizontal mean miss distance. Together, these two values give an indication of accuracy at a given distance.

There are ways of reducing this information even more:

To reduce expected horizontal and expected vertical miss distance into a single value, the area of the ellipse formed by the two may be calculated. This gives a good measure of how "badly" your shot may miss overall at a given range, but hides how much of this is due to vertical dispersion (which, especially when presented with a broadside target, tends to be the more frustrating of the two). 

To go even further, you might also eliminate the range-dependency by averaging the miss distance ellipse's area over the maximum gun range. This results in a single value describing how well your guns hit, but no longer shows gun performance changes with range, thereby hiding whether a ship is better suited to brawling or sniping.

How useful these values actually are remains to be seen. I may run some experiments to gauge this at a later stage.

## Charting our course

The heavy lifting for bringing gun accuracy to quartersbrief isn't so much the programming as it was figuring out the math. This is now mostly complete, with a formula for expected horizontal miss distance fully worked out. The double-reduction of this into a miss distance ellipse averaged over gun range is still missing, but will hopefully not prove quite as challenging. After this, the mathematical work will be done.

On the programming side of things, horizontal and vertical expected miss distance will soon be integrated into quartersbrief. This should not be difficult. Then work will start on designing a new topic to visualize main gun accuracy. At a later time, I also plan on adding a topic on secondary battery capabilities, which is planned to include range, pen, and accuracy.