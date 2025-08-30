---
layout: post
title: "KiesKompas: Centricity bias."
date: 2023-05-13
---
In a [previous post](2023-05-12-kieskompas.md) we examined the KiesKompas and the suboptimal choice of axes. Here, we look at another phenomenon of the KiesKompas that is problematic: centricity bias.

There are many ways to end up in the center of the spectrum. In fact, you can vote completely opposite to a central party and still be advised to vote for them. Extreme parties, on the other hand, can only be reached in one way (in the most extreme case): voting in one direction and one direction only. Low-entropy configurations, in essence.

I explore this by taking the PCA from the previous post, and applying an inverse transform on it. The inverse transform is then compared to the input (=the votes of the parties on the various topics). The standard deviation of this residual is a measure of how well we can project the answers (a $30$ dimensional vector) from the principal components (a 2d vector). We can plot this against a measure of the centricity (in this case, the length of the vector in (PC1, PC2) space.

![img](img/centricity_vs_inverse_pca_residuals.png)

What we then see is that indeed the most central parties (lowest centricity) are the least predictable (largest spread in residuals ~ highest entropy).

## Faulty recommendations
To compound to this issue, it seems the people behind the KiesKompas do not compute the party recommendation in the right way. The proper way to make a recommendation is to have a look at the residual between a voter's choices and each party's choices for each of the $30$ questions. The recommended party would be the one with the smallest residual sum.

Instead, the recommendation seems to be based on the proximity in the 2d (left/right - progressive/conservative) space. While in many cases these might coincide, as per the discussion above it is possible to vote contrary to a party and still be recommended to vote for them. To test this, I followed the following algorithm:

(1) Categorize each question as left/right or progressive/conservative. I may mis-categorize some of these but I will assume that in the majority of cases I did it correctly - not a very important assumption.
(2) Pick one party and find its answers to each of the questions. I picked CDA.
(3) Fill out the KiesKompas, _contrary_ to the party above, but in such a way that the left/right and progressive/conservative scores are the same.

Doing this, I found that indeed in 2d space it is closest to CDA, even though the sum of residuals is lower for other parties (e.g. 50PLUS) - confirming my theory.

One caveat: it is possible that the scoring system used behind the scenes by the KiesKompas is different. I used an equidistant scale (-2, -1, 0, 1, 2), it's possible they weighed extreme answers ("completely agree", "completely disagree") more.
