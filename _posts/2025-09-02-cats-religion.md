---
layout: latex_post
title: "Cats and religion"
date: 2025-09-02
---
This is _not_ a post about reviving the ancient Egyptian religion (even though, weirdly, that _is_ [a thing](https://en.wikipedia.org/wiki/Kemetism)). Instead, it is about the correlation (causation?) between being religious and having a pet / pets.

I heard the claim recently that it has been "scientifically proven" that atheists are more likely to be cat owners, and theists are more likely to be dog owners. An interesting claim, but also a claim that raises numerous alarm bells. "Scientifically proven" is, after all, often code for "there was one paper that claimed something and a newspaper made a sensationalised story out of it, and by the way it has been debunked since but nobody reports on THAT, and what do you MEAN replication crisis, can I eat that?".

## The original paper
The original paper is [How Religion Predicts Pet Ownership in the United States](https://www.researchgate.net/publication/338020013_How_Religion_Predicts_Pet_Ownership_in_the_United_States) by Perry & Burge, 2020. One thing I immediately learned from the paper, is how common pet ownership is in the US. With a cat population of 74 million, the US is not only the number 1 in absolute term, but also per (human) capita, and it's not particularly close.

The authors state they had an a priori expectation of religious attendance to correlate negatively with pet ownership, in short because pet ownership can be a (partial) substitute for human companionship.
Following other research that found dog owners to score higher on agreeableness and be more extroverted, and cat owners, conversely, to be more introverted and comfortable being alone. Therefore, they speculate that higher church attendance, a pro-social activity, would correlate positively with dog-to-cat ownership - which is the basis of the claim that "atheists are cat people".

## The data
The data comes from the [GSS survey](https://gss.norc.org/us/en/gss/get-the-data/spss.html), with an explanation of the many (>1000) variables [here](https://gssdataexplorer.norc.org/variables/vfilter). In the paper, only a table with some summary statistics are given. To understand the distributions better, below I plot the data with the sub-selection as well (all respondents that answered the question of whether they have pets):

![img](img/cats_religion/distributions.png)

There are some small deviations from the numbers reported by Perry & Burge 2020. For instance, the subsample of respondents that were asked about pet ownership is $673$ for us, whereas they report $699$. As a consequence, there are some differences in downstream statistics as well, such as the mean number of pets reported. They also make some minor rounding errors, $699$ is $29.8\%$ of $2348$, rather than $29.7$ as they report. It's unclear where this difference comes from (error from the authors, updated catalog since their paper?), but it's out of the scope of this blog post to investigate further.

Another point of criticism is the categorical encoding of some variables. The believe in the Bible is modeled with two dichotomous variables (one for "literal belief in the Bible", the other for "the Bible is a book of fairy tales", where respondents fall in between ("non-literal belief in the Bible") if they answer negatively to both). On the other hand, religious attendance is modeled as a uniformly spaced numeric variable from 0 to 8, where 0 equals "never", 8 represents "several times a week". This is not equally spaced; in fact, if you convert the literal category to a numeric annual frequency, the mapping would be something like $0, 0.5, 1.5, 5, 12, 30, 45, 52, 100$.

Some further observations: this shows a very loosely bimodal distribution in church attendance (peaks at "never" and "every week") and, predictably, a strong peak for $\lt3$ number of pets with a tail to people owning up to $20$ pets. The income distribution is also strongly peaked, such that not much statistical power should be expected there. Our selected sample (=number of pets is not NaN) seems broadly representative of the large sample (which is representative of the American population by construction).

## Model fit
The data is fit with [negative binomial regression](https://josha91.github.io/2025/09/01/negative-bimodal.html). This is justified, because (1) pet ownership can be assumed to be independent between different households, (2) different family types might be expected to have different "expected" pet counts.

This type of regression deserves some further explanation. First, we link the expectation value $\mu_{\#pets}$ to the predictors, $X_{i}$. The predictors, in this case, are the religious attendance as well as confounding variables such as age, sex, income, etc. as explained in the paper. These are log-linked, i.e.

$\log{\mu} = X_{i}\beta_{i}$

with $\beta$ the proportionality parameter that captures the amount of influence a predictor has.

This linkage ensures that the expectation is strictly non-negative, as per definitial requirement.

With the negative bimodial distribution, in terms of $\mu$ and $r$, we then have

$P(Y_i = y_i \mid \mu_i, r) = \frac{\Gamma(y_i + r)}{\Gamma(r) \, y_i!} \left( \frac{r}{r + \mu_i} \right)^r \left( \frac{\mu_i}{r + \mu_i} \right)^{y_i}$

which leads to the log-likelihood

$\ell(\beta, r) = \sum_{i=1}^n \Big[\log\Gamma(y_i + r) - \log\Gamma(r) - \log(y_i!) + r \log(r) - r \log(r + \mu_i) + y_i \log(\mu_i) - y_i \log(r + \mu_i)\Big]$

This can then be maximized through MLE to find the values for $\beta$ - the amount of influence a given predictor has on the number of pets (and cat and dog ownership, respectively).

I performed this fit as well (ignoring some of the details like religious denomination encoding -- too much effort for too little expected gain). In addition to the paper, I looked to see if social satisfaction and one's self-rating about how well they handle social situations is a significant confounder:

![img](img/cats_religion/pets_attendance.png)

Essentially the same fit they find in the paper, and the social confounder does not make a difference.

The fit for the cat and dog ownership separately is not performed, as I spent too much time on this already :-)

## Conclusions
Reading this paper, I come away with a couple of conclusions.


- Contrary to the popular claim, this paper does not look at \emph{atheism} at all, but religious attendance. The more frequently people visit a religious insititution, the fewer pets they are expected to have.
- The analysis seems fairly robust and the result holds. However, the suggested reason (personality type, social life) is underexplored and unfortunately the dataset doesn't have good measures to address this. I tried with "satsoc", but that had little effect. This makes some sense, as introverts might be simultaneously more likely to own pets, less likely to go to church, and yet satisfied with their social life
- Maybe this is my STEM-bias, but I'm not a fan of the lack of plots in these kind of papers

