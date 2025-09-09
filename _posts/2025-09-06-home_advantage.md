---
layout: latex_post
title: "Home advantage in football"
date: 2025-09-06
---
Home advantage is a well-known concept in football (and sports more generally), but just how big is the advantage? In this post, I will have a look at the home advantage across time and space (that is, different competitions).

Data is provided by [football-data.co.uk](https://www.football-data.co.uk). They provide data for many different European competitions. We use data over the last 15 years from England (top 4 leagues), France (top 2 leagues), Spain (top 2 leagues), Germany (top 2 leagues), Italy (top 2 leagues) and the top leagues from Portugal, the Netherlands, Belgium and Greece.

I define the _home advantage_ over a given season in a given competition as the total points that teams obtain in home games divided by the total points they obtain in away games. Thus, for a home advantage to exist we expect this ratio to be greater than 1.

In the figure below, this is plotted over time, separately for each competition:

![img](/img/football/home_advantage_time.png)

Some immediate observations: the home advantage is ~40-60% on average, which is much bigger than I expected; there is a downward trend over time, and there appears to be a dip in 20/21 and 21/22 - the COVID19 years with no audiences.

To quantify this, let's do a quick fit; we take the mean of home advantage ratios by year, and perform a linear fit (the COVID years are considered outliers to the trend and are masked):

![img](/img/football/home_advantage_fit.png)

with equation $y = -0.017465\cdot x + 1.67705$ where $x$ is the number of years since 2010 which is used as the zero point. That is, in 2010 the average home advantage was $68\%$, in 2024 this shrunk to $43\%$, and the home advantage decreased by almost $2\%$ a year.

At this point we may ask ourselves the question why the home advantage exists, and why it is decreasing with time. COVID-19 provides us with a wonderful natural experiment, showing that the support of the home crowd _is_ important; however, not enough to nullify the home advantage entirely.

Even if it explains some of the effect, the home crowd could not explain the observed trend - after all, crowds have not been getting significantly bigger over the last 15 years. More plausible, therefore, is an influx of money. Budgets continue to skyrocket, and with it professionalism. This might have all sorts of downstream effects:

1. Referees are more professional, less likely to favour the home team
2. Video referees are less biased
3. Pitch standardization: previously fields might be slightly bigger, smaller, wonkier, or just poorer quality grass. The home team being used to those conditions was an advantage: now, less so.
4. Accomodations: it's plausible teams spend more money and effort on making the away trips as comfortable as possible now; comfortable, standardized hotels; flights rather than long bus drives, etc.

If this is the case, one would expect lower tier leagues to have a higher home advantage than top tier leagues, perhaps lagging behind by a few years. This is not what I observe:

![img](/img/football/home_advantage_top_vs_lower.png)

One reason for this might be that the money does not just flow to clubs, but also the league organizers. Thus, referee professionalism, video referees or pitch standardizations might disseminate simultaneously to all professional leagues belonging to the same football association. If so, the only reason listed above for which this is not plausibly true (point 4, accomodations) would seem to be a relatively minor contribution.

Another interesting perspective is looking at the difference between the home advantage for the top teams _within a given competition_ versus the lower ranked teams in that competition. For each year, we split the competition in half and compute the home advantage for both groups respectively:

![img](/img/football/home_advantage_performance.png)

This time, we _do_ observe a significant difference: lower ranked teams appear to have a systematically bigger home advantage. What's more, it seems top teams have a more shallow decline in home advantage over the last 15 years compared to lower ranked teams.

This can be understood as follows: top teams tend to win by a larger margin than lower ranked teams. _If_ lower ranked teams win, odds are it will be 1-0, 2-1, but rarely 4-0. This means, that if there is a marginal effect of home advantage (say, 0.2 extra goals for the home team on average) that will have a more dramatic effect on the amount of points gained for the lower ranked teams than for the higher ranked teams. I simulated this by drawing expected goals from a Poisson distribution with on average 3 goals per game (strong team), 2 goals (mid team), or 1 goal (weak team) and adding a 0.2 goal home advantage:

![img](/img/football/home_advantage_sim.png)

## TL;DR
In short, what we find is that:
1. There is a home advantage of about $43\%$ as of 2025
2. This advantage is decreasing by roughly $1.7\%$ per annum
3. Home crowds are a significant part of the home advantage as evidenced by the COVID years, during which some competitions even registered an unprecedented _negative_ home advantage.
4. Top-tier leagues don't have a different home advantage from lower-tier leagues, but within the same league, lower-ranked teams have a bigger home advantage.
