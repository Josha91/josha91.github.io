---
layout: latex_post
title: "Simulating the Mind"
date: 2025-10-04
---
This title might seem grande and you might be expecting at least a discussion on deep learning... but alas. It is about the boardgame The Mind.

A short recap of the game: we have cards that are numbered from 1 to 99 (no duplicates); players get a progressive amount of cards, i.e. 1 each in round 1, 2 in round 2, etc.
The players can only see their own cards, and it is the objective to (cooperatively) put down all cards in the center of the table in ascending order. The catch: you cannot talk!
If you survive for 12 rounds, you win the game.

Beyond some suggestive eyebrow movement, this usually comes down to using some version of the simple heuristic: if the $\Delta$ between the  highest card in the center and the lowest card in my hand is small, I will put down a card fast; if the $\Delta$ is big, I will wait longer (and see if someone else wants to put a card down first).

After playing a game or two with my friends today, I thought it'd be nice to simulate this. There are two baseline scenarios: (1) players put cards down at random and (2) players have a perfect internal clock and use that for precision counting.

It's in between where the interesting stuff happens. How precise does your clock need to be to stand a chance at success?

For the simulations, we make some assumptions:
(1) No throwing stars are gained (a rule I didn't explain and isn't worth getting into here, either)
(2) No lives were gained after passing certain rounds; we just start with 2 lives at the beginning of the game, and that's it.
(3) We model the inner counting as the $\Delta$ between highest card on the table and lowest card in each player's hand and added relative Gaussian noise; that is, $10\%$ noise translates to a Gaussian $\sigma=0.1$ seconds if you have a $\Delta=1$, and $\sigma=1$ second if the $\Delta=10$.

The result is shown in the figure below:

![img](/img/the_mind.png)

As required (for the simulations to be valid), the "perfect internal clock" players win all games, the random players perform terribly. With $10\%$ relative error, players perform well (considering the assumptions are above are not in favor of the players). For a $20\%$ error performance drops, though I'd suspect that if you give extra lives during the game as per the rules, you'd still have a very decent chance of winning.

One more quick thing to look at: how do 3 players fare, under the same assumptions? Below, I compare 2 players to 3 players, all with a $10\%$ relative error - the only difference is that the 3 players start with 3 lives rather than 2.

![img](/img/the_mind_cumulative_distribution.png)

What this shows that in the early game 3 players fare just as well as 2 players, but 3 players struggle more and more as the game progresses. This makes sense, since there are proprotionally more and more cards in the game and the only rule to offset this is at the beginning of the game, where they are given an extra life.

So, my recommendation would be: special rules to give players extra bonuses during the game, not just at the beginning. 


