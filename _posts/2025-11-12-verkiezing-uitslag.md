---
layout: post
title: "Political polarization: map of the Netherlands"
date: 2025-11-12
---
I have been eagerly awaiting this: the Kiesraad [published](https://www.verkiezingsuitslagen.nl) the full election results per municipality. Let's play around with this.

## Result maps
Various news outlets (such as the [NOS](https://app.nos.nl/nieuws/tk2025/)) have published maps of the biggest party per municipality. Which is interesting, but also obscures a lot, especially in a political landscape that is as fragmented as it is in the Netherlands. After all, these maps would look the same irrespective of the winning party having a 50 or 1 percent point victory.

This is the map from the NOS:

![img](/img/verkiezingen_2025/map_nos.png)

What I thought might be somewhat more informative, is to compute the an average "left-right" score per municipality. I have done this as follows:

(1) As is by now [well known](https://josha91.github.io/2023/05/12/kieskompas.html), the variation in Dutch parties can be fairly well projected on a single axis. Using this, we can compute a centricity score: a value that shows a party's position along this single dimension, normalized by the most extreme value (such that the most extreme left/progressive party, the PvdD, is -1 by definition and the most extreme right/conservative party approaches +1).
(2) Per municipality, take the weighted average of these centricity scores, weighted by the number of votes a given party received
(3) This is only done for the parties that were actually elected; the smaller fringe parties are assumed to be negligble, even though this might not be true in individual municipalities

The map that we then obtain is shown below (the colormap is centered on the national average centricity score, 0.19 - close to CDA):

![img](/img/verkiezingen_2025/weighted_average.png)

This is not too dissimilar from the "winner takes all" map above, although it highlights some important things:

(1) There are some municipalities that are green (i.e. CDA is biggest) in for instance Twente, the east of the country. This suggests it is more moderate / centrist than other municipalities, in which the PVV is the biggest. Our map reveals that there is no meaningful difference between these municipalities.
(2) Contrarily, there are municipalities that appear very right wing because the PVV won there, such as Enschede; looking at our map, though, we find that it is actually somewhat skewed left (D66 is the second biggest party, only trailing the PVV slightly).

From our analysis, the most left wing / progressive municipalities are:
(1) Amsterdam
(2) Utrecht
(3) Wageningen
(4) Nijmegen
(5) Leiden

All big cities and / or student towns, reinforcing the important adage that _land does not vote_.

On the other hand, the most right wing / conservative municipalities are:

(1) Rucphen
(2) Urk
(3) Tubbergen
(4) Edam-Volendam
(5) Renswoude

## Municipalities by coalition
While this is a bit better than the winner-takes-all map, different shades of red are still not very informative as I'd like. 
What I therefore did instead, is to divide the political landscape in three blocks: left/progressive, center, and right/conservative.
This is, of course, somewhat arbitrary, and there is significant discussion about just how right wing D66 is, for instance. My definition is the following: Everything more left/progressive than D66, including D66 itself, is one block; Everything more conservative/right than VVD is classified as another block (_excluding_ the VVD itself this time - again somewhat debatable). The VVD was included in the remaining center block (mostly to preserve a balance in the number of parties).

The intuition for each block is that, generally speaking, they'd be able to form a coalition together quite easily if given a majority. Defining these blocks like this, the actual number of seats are:

* Left/progressive: 56
* Center: 48
* Right/conservative: 46

So the blocks are roughly equal in size.

We then find which block is biggest in each municipality:

![img](/img/verkiezingen_2025/plurality.png)

This shows the following:

* Urban areas tend to be left / progressive
* Border areas tend to be right / conservative
* Non-border rural areas are more likely to be center-dominated.

In a next post, we will plot these metrics against e.g. population, population density, house prices, proximity to a border and proximity to a asylum center. 
