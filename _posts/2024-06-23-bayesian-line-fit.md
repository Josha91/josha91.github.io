---
layout: latex_post
title: "Line fitting: not as easy as it seems."
date: 2024-06-23
---
Many statistics courses teach students how to do statistical tests. Students are given a step-by-step recipe on which calculations to perform, and the caveats and conditions under which that recipe is valid are discussed. However, more often than not the assumptions made in the modelling are forgotten and only the black-box algorithm is remembered. I have seen this time and time again in all sorts of different statistical applications. Today, I want to talk about a typical example: Fitting a (straight) line to data.

This post is based on [this excellent paper](https://arxiv.org/pdf/1008.4686).

## Ordinary least squares
If we have data that we suspect to be modelled well by a (straight) line, we take the sum of the squared difference between data and model, weighed by the individual variance, and minimize this sum as a function of the model parameters. Simple recipe, easy to remember. It can be shown that this procedure is an objective best-fit, but _only_ if the data truly come from your perfectly-narrow, straight line with known, Gaussian errors in the y-direction only. 

But what if that's not true? For instance, how...
- ... do you account for outliers?
- ... do you account for intrinsic scatter?
- ... do you account for uncertainty not just in $y$ but also in $x$?
- ... do you account for non-Gaussian errors?

Sometimes these things are forgotten entirely, sometimes they are simply ignored, sometimes some procedure (e.g. _sigma clipping_ in the case of outliers) is proposed to still obtain a fit that "looks good". But the authors of this paper argue that that is not objective and (thus) not a scientific thing to do.

## What to do instead
The authors propose creating a _generative model_ that parametrizes the assumptions above _as part of the model_, rather than implicitly making the assumptions and dealing with them in arbitrary ways.

Take the case of outliers. We may assume the uncertainties we measured on the outliers are wrong. If we knew the true uncertainties, they would not be outliers. This is something we can model.
Alternatively, maybe they come from a different distribution altogether. Our generative model could then be the superposition of two Gaussians: one that generates the bulk of the points that follow the relationship we're after, and one that generate outliers.

The point for both of these that we _explicitly parametrize_ the outliers, rather than simply rejecting them. This means that, through posterior marginalization, it is also more accurately accounted for in our confidence in the sought-after parameters (in this case, the line's slope and intercept).

The paper has a lot more to say about this, and is very much recommended reading.