---
layout: latex_post
title: "Negative binomial as a Poisson-Gamma mixture"
date: 2025-09-01
---
Recall the binomial distribution: in $n$ Bernouilli trials, each with a chance $p$ of "success" (and 1-p of failure), what is the probability of $k$-times success. This follows this discrete distribution:

$P(X = k) = \binom{n}{k} p^k (1-p)^{n-k}, \quad k = 0,1,2,\dots,n$

A related distribution is the $negative$ binomial distribution. Here, we ask the question: for those same $n$ experiments with the same probability $p$ on every throw, we wait until we have $r$ successes. When this number of reached, the number of _failures_ is distributed according to the negative binomial,

$P(X = k) = \binom{k+r-1}{k} (1-p)^r \, p^k, \quad k = 0,1,2,\dots$

This can also be related to the Poisson distribution. Just like the Poisson distribution relates to the binomial distribution in the limit where $n\rightarrow\infty$ and $p\rightarrow0$, the negative binomial approach the Poisson distribution in the limit where $p\rightarrow1$ and $r\rightarrow\infty$, in such a way that the expected number of failures/trials are kept constant. That is, by definition the mean is equal to the Poisson parameter $\lambda$, but it can be shown that the variance is

$Var(X) \;=\; \lambda \;+\; \frac{\lambda^2}{r},$

For $r\rightarrow\infty$ the variance equals the Poisson variance, but for small(er) $r$ the variance is _larger_ - for this reason, the negative binomial is often used as an _overdispersed_ alternative to the Poisson distribution.

Another very interesting way of deriving the negative binomial is by _starting_ from the Poisson distribution, and considering that the rate $\lambda$ is now itself a random variable, rather than a constant. That is, any random event described by the Poisson distribution might now have a different $\lambda$. It can be [shown](https://gregorygundersen.com/blog/2019/09/16/poisson-gamma-nb/) that the negative binomial distribution is equal to a Poisson-Gamma mixture. The intuition behind this can be seen:

1. Let's say we have a "Failure" Poisson process with rate $1-p$. For a Poisson process, the time $t$ between these counts with rate $1-p$ is exponentially distributed with rate $1-p$, i.e. $\exp^{(1-p)t}$.
2. Therefore, the time $T$ until $r$ failures happen is the sum of $r$ independent exponentials, all with the same rate $(1-p)$. This is equal to a gamma distribution with shape parameter $r$ and rate $1-p$.
3. Now suppose you have a second, independent "Success" Poisson process, with rate $p$. If you ask the question "how many successes happened by the time $T$?", that is simply a Poisson with rate $pT$ - conditional on $T$. 
4. Because $T$ is random and has a gamma distribution, the number of successes until you get $r$ failiures is a Poisson-Gamma mixture, but that is also the definition of the negative binomial distribution.

The interesting thing is that this gives a different meaning to the negative binomial: it can be seen as a generalized Poisson process where the rate is not a constant. Think, for instance, about the number of days in the hospital, but the risk factors for patients being very different. Or car insurance policies, where not every driver has the same expected number of crashes on account of being a better/worse driver. The intuition is that the negative-binomial, on account of its larger variance, captures this convolution better.
