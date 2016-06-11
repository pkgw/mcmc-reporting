---
layout: page
title: Recommendations for Reporting the Results of MCMC Analyses in Scientific Literature
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


Colophon
========

Contributors. License of the document. Version information. And so on. Here’s
the [README on Github](https://github.com/pkgw/mcmc-reporting#readme) with
information about how to contribute to this document.
