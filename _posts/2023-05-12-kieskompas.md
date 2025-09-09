---
layout: latex_post
title: "KiesKompas PCA: a new way to classify political parties"
date: 2023-05-12
---
The “KiesKompas” (voting compass) is a popular voting aid in the Netherlands which has been used since 2006 to help voters make an informed choice in national elections. Based on 30 politically relevant theses, political parties are stratified along two axes: “left/right” and “conservative/progressive”. By giving their opinion on each of these theses, a voter is placed somewhere in this space as well and the proximity to each of the parties is translated into a voting advise. 

In the Netherlands, however, parties are often distributed along the diagonal that runs from left-and-progressive down to right-and-conservative, with very little in the remaining quadrants (right-and-progressive or left-and-conservative). This is, in fact, not a uniquely Dutch phenomenon as evidences by the frequency with which "left" and "progressive" (and "right" and "conservative") are used as synonyms in online discourse. 

As such, it stands to reason that while these axes are politically meaningful, they are not optimised to differentiate between parties, which is essential for the purpose of helping voters choose. In this post, I will explore whether it is possible, based on these 30 theses, to create a better separation between parties. If not, we can argue whether certain parties are in fact distinct enough to warrant being separate entities, and conversely we will explore electoral niches that may be filled in the future.

Finally, we will look at some problematic features of the KiesKompas.

## KiesKompas 2017

![KiesKompas 2017](/img/kieskompas_2017.png)

The Figure above is the KiesKompas from the general elections in 2017. Clearly, all but two parties (D66 and PVV) appear to be on a diagonal running from progressive-and-left to conservative-and-right. As suggested above, this _may_ be due to the particular way of projecting the multidimensional "political beliefs"-space onto this two dimensional space. Here, we will try to improve on this by finding two axes that maximize the variance of the data, i.e. the separability of parties. To do this, we will employ _principal component analysis_ or PCA.

What is principal component analysis? It is a way of reducing the dimensionality of a dataset while retaining as much information as possible. This can help to understand complex data by reducing correlations, removing redundancies, and allowing us to visualise high-dimensional data. The principal components are newly defined dimensions along which the explained variation is as high as possible. Component 1 explains most variance, component 2 the second most, etc. An intuitive way to think about this is that if you have an cloud of points distributed like an ellipsoid, the first principal component (PC1) runs along the major axis of the ellipsoid, PC2 runs along the semi-major axis and PC3 runs along the minor axis. If we drop PC3, we have an ellipse in a two-dimensional space that is easier to interpret while minimising the amount of information lost.

We can apply this to the KiesKompas by considering each question as one dimension. We can then encode the possible answers (“strongly disagree”, “disagree”, “neutral”, “agree”, “strongly agree”) numerically as -2, -1, 0, 1, 2 respectively. Each political party thus has a position in a 30-dimensional space that we reduce, with PCA, to a position in two-dimensional (PC1, PC2) space.

![PCA 2017](/img/kieskompas_2017_pca.png)

The results are shown in Figure 2. Visually that seems to work as expected: the parties are less clustered and all four quadrants are occupied. We can confirm this by computing the variance that is explained by the principal components (versus the default KiesKompas axes). In PCA, the principal components are equivalent to eigenvectors of the covariance matrix of the data, and the explained variance is equivalent to the respective eigenvalues. The _relative_ explained variance is thus the ratio of an eigenvalue to the sum of all eigenvalues. All of this is computed under the hood in _scikit-learn_ in Python.

Doing this, we find PC1 explains $62.6\%$ of the variance and PC2 explains an additional $12.6\%$ for a total of $75.2\%$. In other words, even though we reduced the 30 dimensional space to a two dimensional space, $75\%$ of the differences between parties can be analysed through these two axes.

This, however, is not very meaningful unless we know it's _better_ than the original two axes. The explained variance is not so readily obtained for those, however, since we don't know how the original data is projected upon them. Therefore, to find the explained variance, we:

(1) Fit the (n_parties, 30) original matrix to the observed (n_parties, 2) projected coordinates.
(2) Use this model to predict the observed projected coordinates
(3) Find the variance between the true coordinates and the predicted coordinates.

The _explained_ variance is then $1$ minus the _unexplained_ variance - the scatter in this plot. Formally, that is equivalent to computing the $R^2$.

This gives us an explained variance of the _original_ KiesKompas axes of $55\%$[^1]. In other words, the PCA decomposition differentiates between parties much better than the original decomposition.

## Meaning of the new axes
Assigning meaning to these new axes is complicated, because the KiesKompas is _designed_ with these axes in mind, i.e. every question moves a party in one of the original directions and one direction only. Contrarily, each question can (and will) move a party in both PC1 ánd PC2. Roughly speaking, we may expect PC1 to align with the diagonal in which parties are stratified in the original graph, since that is where the biggest variance ostensibly is. That is, a negative PC1 is equivalent to "left AND progressive", a positive PC1 is "right AND conservative".

This is confirmed when we look at which questions contribute most to PC1. On one side of the spectrum, we have:

- More money to Defense spending
- The climate crisis is exaggerated
- Judges should give more jail sentences rather than community service

with on the other side:

- Black Piet is racist
- The Netherlands should receive more refugees
- To reduce nitrogen emissions, we should reduce the number of cattle

For PC2, we have on the one hand:

- The 'own risk' [in health care] should be abolished, even if that means people are less aware of the costs of healthcare
- Being an EU member has been mostly detrimental to the Netherlands
- The climate crisis is exaggerated

and on the other hand:

- Companies should be allowed to refuse serving customers that were not vaccinated against COVID
- More money for to developmental aid
- Privacy of citizens can be restricted to improve national security

My two cents on this, is that positive PC2 seems to align with "traditional" political parties, that are OK with governmental involvement and have a high level of trust in the status quo. Contrarily, negative PC2 seems to align with populist newcomers, with more anti-establishment ideas, ready to shake things up. This differs from the "big" vs. "small" government that traditionally divides the left and right, insofar that the left/right divide is mostly about government _scope_, whereas PC2 is more about government _trust_.

So, to summarize, I would call PC1 the _Values_ and PC2 the _Institutional trust_.

If we look again at the decomposition above, then this makes sense. The VVD and SGP are establishment (trust/PC2) right-wing/conservative (values/PC1) parties, whereas JA21, PVV and FvD are populist (trust/PC2) right-wing/conservative (values/PC1) parties. On the other side, GroenLinks, D66, PvdA are establishment (trust) left-wing (values) parties, whereas SP, DENK and PvdD are more populist (trust) left-wing (values) parties.

## Are all these parties necessary?

We have *many* parties in the Netherlands, which can be difficult for coalition forming. Therefore, a jusitified question is: are all these parties necessary? If in this idealised decomposition we cannot separate parties, perhaps there is no good reason for both of them to exist.

This decomposition does a good job separating some parties that are very close in the original. DENK and the labor party (PvdA) are not as close in this new decomposition, for instance, nor are 50Plus and CDA. Contrarily, PvdA and GroenLinks were close to each other in the original and still are; the same for JA21 and FvD (not surprising, since the former split off from the latter). For these parties, one may wonder if separate parties are justified since ideologically they are very similar.

That's it for now. In a next post, we will look at the centricity bias of this political aid.

[^1]: As a sanity check, we also compute the explained variance for the PCA components. This gives $72\%$, roughly equivalent to the eigenvalue computation.
