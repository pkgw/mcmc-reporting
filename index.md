---
layout: page
---

Introduction
============

**To be fleshed out**. The gist: More and more people are running Markov Chain
Monte Carlo (MCMC) analyses and reporting their results in academic papers. If
you’re very experienced with MCMC, it can seem obvious how to describe your
methods and their results, but for less-experienced folks it can be a bit
daunting to figure out how to write up what you’ve done and what you’ve found.
This document aims to help by suggesting some “best practices” for *reporting*
MCMC analyses and their results. If you’re refereeing a paper with MCMC
analysis, we hope that it will be helpful for deciding whether the analysis is
adequately reported and recommending improvement to the authors.

This document is emphatically *not* a guide about how to perform MCMC analyses
in general. That topic can, and does, fill entire textbooks. This document
will sometimes recommend particular techniques, but we generally trust you,
the practicitioner, to make the choices that are appropriate to your
particular problem. We make recommendations on how to accurately and
thoroughly describe what you’ve done, not how to choose what to do in the
first place.


Goals
=====

These recommendations are motivated by two goals that should be considered
equally important:

The first goal is **reproducibility**. People should be able to rerun your
analysis, or run something that should be indistinguishable from it in
practice. The general issue of scientific reproducibility is of course much
larger than this one topic, and we won’t get into it here, but we do believe
that this is a basic cornerstone of scientific practice.

The second goal is **usability**. People should be able to understand the
results of your analysis and use them correctly in their own work. It’s sad to
admit it, but here in the real world, people do not always read papers and
figures in as much detail as they should, so it is important that the
superficial reader come away with the correct “big picture”.


Describing Your Methods
=======================

When describing MCMC methods, our general stance is that one should always err
on the side of being explicit and thorough: while it is true that in a certain
sense, it doesn’t matter how many “burn-in” samples you discard so long as you
discard *enough*, it takes very little effort or text to just report the
number. We can think of no compelling reason to deny your future readers that
piece of information should they happen to find it interesting.

We recommend that descriptions of MCMC analyses include the following pieces
of information at a minimum.

An **explicit statement of all priors**. Authors will sometimes write
something vague such as “we used an uninformative prior for nuisance parameter
*x*” but this is, well, not that informative. When there are many parameters,
the prior can effectively be summarized in a table, using a notation such as
`U(0,1)` to indicate a uniform distribution on the exclusive unit interval.

An **explicit statement of the sampling algorithm**. This should include a
reference to the algorithm itself (e.g.,
[the Goodman &amp; Weare affine invariant sampler](http://dx.doi.org/10.2140/camcos.2010.5.65))
and the software package implementing it (e.g.
[emcee](http://dan.iel.fm/emcee/current/)). If the implementation of the
sampler is bespoke, it should be described to whatever level of detail is
appropriate for software in your field. (However, to editorialize, in most
cases you should probably be using a proven and established package rather
than your own code.) If the sampling algorithm involves tunable parameters,
any non-default settings should be reported.

An **explicit statement of the likelihood function** or a sufficiently
detailed description such a reader could reliably be expected to reproduce it
exactly.

It is fairly common for MCMC likelihood computations to involve forward
modeling of a data-generating process, then comparison of the simulated data
to actual observations. Ideally, readers should be able to exactly reproduce
your forward modeling process, but sometimes these models can be immensely
complex pieces of software. Once again, in these cases the model should be
described to whatever level of detail is appropriate for software in your
field.

An **explicit statement of the initialization conditions** for your MCMC
chains.

Specification of the **number of chains run**, **number of steps per chain**,
**number of discarded (“burn-in”) samples**, and **thinning factor** used when
gathering samples of the posterior.


Describing Your Results
=======================

Your MCMC analysis results in some number of samples drawn from a posterior
distribution. When describing the results, the overarching goals are to
demonstrate that the samples capture the true shape of the posterior, and to
provide a fair characterization of the posterior’s shape as inferred from the
samples.

We recommend that, at a minimum, reported MCMC results include the following
pieces of information regarding the properties of the chains:

A summary of the **autocorrelation lengths** of the MCMC chains for each
parameter, and a derived **number of independent samples of the key
parameters** (computed simply as the total number of samples divided by the
autocorrelation length). If there are many parameters, some summary numbers
(e.g., the longest and shortest autocorrelation lengths out of all of the
parameters, and the corresponding smallest and largest numbers of independent
samples) are fine. **TODO**: a good reference addressing the question of “how
many independent samples are enough?”.

Likewise, the jump **acceptance fractions** computed for each chain, or a
summary of them if there are many chains and/or parameters. Acceptance
fractions outside the range of 10–90% suggest that the sampler is not
well-matched to your problem and are cause for concern, since your samples may
not be fully exploring the posterior distribution.

A quantitative **convergence criterion** indicating that your chains have
likely converged to fully and adequately sample the posterior distribution.
The [Gelman-Rubin criterion](http://dx.doi.org/10.1080/10618600.1998.10474787)
is popular, but it is important to note that it is not valid to apply it
naively when using the [emcee](http://dan.iel.fm/emcee/current/) sampler,
since Gelman-Rubin examines correlations between chains and
[emcee](http://dan.iel.fm/emcee/current/) imposes correlations on its chains.
**TODO**: check out Geweke (1992) criterion cited in Montet’s paper.

In most cases, more emphasis is placed on the shape of the *n*-dimensional
probability distribution inferred from the MCMC posterior samples, rather than
the samples themselves. However, if you think that readers of your work may
wish to perform studies based on the detailed characteristics of your
posterior, the best way to enable this is to provide a machine-readable table
of all of your samples, including parameter values, the computed likelihood,
and prior probabilities. We normally consider this an “extra credit” level of
detail, but it bears emphasizing that these samples are the fundamental data
product of an MCMC analysis.

All further reporting is fundamentally a question of **fairly representing the
probability distribution implied by your samples**. No hard and fast rules
will apply across all possible results, and in some cases there will be no
simple numerical summary that fairly conveys the shape of the resulting
probability distribution.

However, in many cases a popular representation is a “triangle” or “corner”
plot of two-dimensional marginalizations of your parameters. (**TODO**:
example). It is fair to ignore nuisance parameters if their correlations with
the parameters of interest are not significant, especially since corner plots
become difficult to parse as the number of parameters plotted exceeds four or
five.

In most cases, the reader will be interested in summary statistics describing
the marginalized posterior probability distribution inferred for each
non-nuisance parameter. We recommend reporting the **median value** and a
**68% credible region** when such a distribution is unimodal, approximately
Gaussian in shape, and not highly correlated with another parameter. There are
multiple ways to determine credible intervals, and a large literature
describing their strengths and weaknesses. (**TODO**: examples.) Subsequent
analysis should not be sensitive to the precise bounds of the reported
credible interval, so the particular method used should not be particularly
important.

Readers will assume that distributions are Gaussian in shape unless you
indicate otherwise. Therefore it is often particularly important to
investigate whether your posterior distributions are *leptokurtic*
(fat-tailed) compared to a Gaussian distribution, i.e. whether they contain
more 3σ outliers than would be expected in a Gaussian approximation.

If the posterior for a parameter is not unimodally distributed, or it is
highly correlated with one ore more other parameters, summarizations of its
posterior should highlight these properties **and typical “X ± Y” summaries
should be avoided** since casual readers will assume that parameters are
approximately Gaussian-distributed and not correlated with one another. If a
set of parameters are correlated but have unimodal, approximately normal
distributions, an appropriate summary might be a covariance matrix.

Finally, **the shrinkage of the posterior distribution relative to the prior
distributions** should be investigated. In some cases, it will be obvious that
the posterior distributions are significantly narrower than the priors and
quantitative analysis of this matter is overkill. If it is not obvious, the
appropriate course of action will depend on the situation. **If the shape of a
posterior distribution is largely set by the shape of the prior, this should
be stated clearly.** Note that this is not necessarily a bad thing, if the
prior distribution’s shape is non-arbitrary. **TODO**: I think there’s some
way Single Best Way to quantify the shrinkage, using a K-L divergence or
something?


Contributors
============

The following individuals (listed in alphabetical order) contributed text to this document:

- Peter K. G. Williams

The following individuals (listed in alphabetical order) have discussed its
contents (but their endorsement of some or all of its arguments should not be assumed!):

- Fabienne Bastien
- Trent Dupuy


Colophon
========

**To fill in**: Contributors. License of the document. Version information.
And so on. Here’s the
[README on GitHub](https://github.com/pkgw/mcmc-reporting#readme) with
information about how to contribute to this document.


Ideas for Improving this Document
=================================

These should also be collected on the
[GitHub issue page](https://github.com/pkgw/mcmc-reporting/issues) for this
document.

- A bibliography with recommended citations for relevant algorithms,
  implementations, convergence criteria, visualization methods, etc.

- Write this document with Bibtex somehow so we can use citations more
  liberally and easily.

- Sprinkle in (more) references to inspiring, positive examples from the
  literature.

- Recommendations of software packages or code snippets that can implement
  these best practices in various software environments.
