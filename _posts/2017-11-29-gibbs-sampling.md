---
layout: post
title: Gibbs Sampling
date: 2017-11-29 20:55:00
categories: study
tags: gibbs-sampling
---

## 정의
In statistics, Gibbs sampling or a Gibbs sampler is a Markov chain Monte Carlo (MCMC) algorithm for obtaining a sequence of observations which are approximated from a specified multivariate probability distribution, when direct sampling is difficult. This sequence can be used to approximate the joint distribution (e.g., to generate a histogram of the distribution); to approximate the marginal distribution of one of the variables, or some subset of the variables (for example, the unknown parameters or latent variables); or to compute an integral (such as the expected value of one of the variables). Typically, some of the variables correspond to observations whose values are known, and hence do not need to be sampled.

Gibbs sampling is commonly used as a means of statistical inference, especially Bayesian inference. It is a randomized algorithm (i.e. an algorithm that makes use of random numbers), and is an alternative to deterministic algorithms for statistical inference such as the expectation-maximization algorithm (EM).

As with other MCMC algorithms, Gibbs sampling generates a Markov chain of samples, each of which is correlated with nearby samples. As a result, care must be taken if independent samples are desired. Generally, samples from the beginning of the chain (the burn-in period) may not accurately represent the desired distribution and are usually discarded. If necessary, one possible remedy is thinning the resulting chain of samples (i.e. only taking every nth value, e.g. every 10th value). It has been shown, however, that using a longer chain instead (e.g. a chain that is n times as long as the initially considered chain using a thinning factor of n) leads to better estimates of the true posterior. Thus, thinning should only be applied when time or computer memory are restricted.[1]

hello
$$
 y = \frac{x ^ 2}{2} K\left(1-\sqrt{x^3 - 8}\right)
$$
world



출처 : [Gibbs Sampling](https://en.wikipedia.org/wiki/Gibbs_sampling#Introduction){:target='_blank'}
