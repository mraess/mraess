---
title: Information hidden in non-linearity
author: Matthias Raess
date: '2018-04-11'
slug: information-hidden-in-non-linearity
draft: yes
categories:
  - Statistics
tags:
  - GAM
  - RStudio
---

<h2>Introduction</h2>
This is a short blog-post about non-linear relationships in your data and how to model them with a generalized additive model (GAM). It assumes familiarity with R and generalized linear models.

Often, when dealing with natural data, we run into non-linearity, because that is just how the natural world works. While many things are normally distributed, such as hight and intelligence, and many of those relationships are of a linear nature, the relationship between two variables does not always follow a straight line.

A good way to assess if your data (or the variables in question) follow a linear or potential non-linear (polynomial) relationship is, of course, to visualize your data with a scatter plot - before start any modeling.
<h2>Generalized additive models</h2>
Now, if you have an inkling that your variables might follow a non-linear relationship, it is crucial you don't model them with a linear model, because it can be limiting in terms of its scope, i.e. its ability to model polynomial or wiggly curves. Thus, with a linear model, the hypothesis test, or the prediction is not going to be an apt representation of what is going on, and the model will fit the data poorly.

Enter the generalized additive model, hailed as the predictive modeling silver bullet (<a href="https://multithreaded.stitchfix.com/blog/2015/07/30/gam/">Larsen, 2015</a>) and still somewhat underutilized. The great benefit of GAMs is that they do fit a straight line if the relationship is linear, but fit a curve if the relationship is polynomial.

The underlying math is fairly straight forward and builds on what we know from generalized linear models, adding a link function, <em>g()</em>, to the linear equation (e.g. linear regression: link = identity; logistic regression: link = logit).

For example, <img class="alignnone size-full wp-image-253" src="https://matthiasraess.files.wordpress.com/2018/02/codecogseqn.gif" alt="CodeCogsEqn.gif" width="197" height="18" />

GAMs now add a smooth non-parametric function to capture any wiggliness:

<img class="alignnone size-full wp-image-254" src="https://matthiasraess.files.wordpress.com/2018/02/codecogseqn-1.gif" alt="CodeCogsEqn (1).gif" width="197" height="52" />

Here, <em>y</em> is the dependent variable, <em>E(y)</em> is the expected value, with <em>g(y)</em> being the link function that relates the outcome to the predictors <em>X<sub>1</sub></em>, …, <em>X<sub>p</sub></em>. Beta zero (<em>β<sub>0</sub></em>) is the intercept (like in a SLiM), and <em>f(X<sub>1</sub>)</em>, …, <em>f(X<sub>p</sub>)</em> are smooth nonparametric functions, which are estimated iteratively and automatically during model estimation (T. J. Hastie & Tibshirani, 1986, 1990; Larsen, 2015, July 30; Wood, 2006).

There is a lot more to be known about the intricacies of generalized additive models, so I suggest you read up on it if you're planning on implementing one yourself.

For example, you can go to <a href="https://multithreaded.stitchfix.com/blog/2015/07/30/gam/">Kim Larsen's</a> and <a href="https://m-clark.github.io/generalized-additive-models/">Michael Clark's</a> awesome quick and dirty introductions. Read also Wood, 2006 and Hastie and Tibshirani (1986, 1990) for more detailed information on GAMs.
<h2>Example from personal experience</h2>
This example comes from my dissertation (Interactions<sup>3</sup>: Language, Demographics, and Personality; an In-Depth Analysis of German Tweets) - find the dashboard <a href="http://bit.ly/german_twitter">here</a>.

The analysis in question investigated the relationship between age and words related to positive feelings, resulting in the following plot:

<img class="alignnone size-full wp-image-252" src="https://matthiasraess.files.wordpress.com/2018/02/gam3_posfeel_plot.png" alt="gam3_posfeel_plot" width="2834" height="1751" />

The GAM revealed a polynomial relationship between age and positive feeling words. It has a discernible U-shape in between 30-45 years, which has also been shown in previous psychological research and relates to a dip in the overall life-happiness of people. However, others have reported linear relationships, so this has to be taken with a grain of salt. In this data set, non-linearity only applied to the female participants, while the relationship was linear for the male participants. You can assess this in the GAM output, the effective degrees of freedom (an edf of 1 indicates linearity, while an edf > 1 indicates non-linearity).

This is puzzling and interesting at the same time as it underscores the complexity of natural data.

Find the GitHub repo with all the code <a href="https://github.com/mraess/dissertation_R">here</a> and the ggplot.gam.R function I used to generate the above plot <a href="https://github.com/mraess/dissertation_R/blob/master/ggplot.gam.R">here</a>.