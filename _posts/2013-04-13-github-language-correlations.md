---
layout: post
title: GitHub Language Correlations, Revised
---

Akshay Shah recently published [a correlation matrix between popular languages
on GitHub](http://datahackermd.com/2013/language-use-on-github/). As commenter
"conjugateprior" observed, however,

> I'd guess your correlation routine only works with complete cases a.k.a
> 'listwise deletion'. That means a bunch of your correlations are based on
> very small numbers of cases and will be super noisy. For example, the R to
> ActionScript correlation is 0.49 (and I can replicate it in R) but it depends
> on exactly 6 Github users. And the reason there are so few complete cases is
> that people _don't_ tend to use these languages together.

> Sorry, to be a bit clearer: In your data preparation (and for all I know in
> the original data) when user does not use a particular language you represent
> it as missing data, which the correlation routine duly throws out. In fact
> these should be zeros, which makes a huge difference. If you put the zeros
> back in the R / ActionScript columns then the correlation is -0.0038. Pretty
> much where we might expect to be.

With [minor revisions to the code](https://gist.github.com/coyotebush/5379476),
I included those zeros and regenerated the plot:

![GitHub Language Correlations](/images/2013/spearman_language_correlation.svg)
