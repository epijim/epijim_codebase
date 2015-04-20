---
layout: article
title: "Scatter plot matrix with R"
excerpt: "Making two types of scatter plot matrix with R."
categories: code
modified: 2014-09-27T11:57:41-04:00
tags: [r,scatter plot matrix]
toc: true
ads: true
comments: true
image:
  feature:
  teaser: scattermatrix_thumb.png
---

## Mixed variables

Scatter plot matrices tend to be something I always run, but never present. So I don't bother
 with making them pretty. My two favourite functions are *ggpairs*, and *corrgram*.

{% highlight r linenos=table %}
png("scattermatrix.png")
ggpairs(mydata,
        #axisLabels = "none"
)
dev.off()
{% endhighlight %}

<figure>
	<a href="/images/wine_1_scattermatrix.jpeg"><img src="/images/wine_1_scattermatrix.jpeg"></a>
	<figcaption>Scatterplot matrix of pairwise correlations. Plot type is defined by data in each pair.</figcaption>
</figure>

## All continuous

This second plot type doesn't handle discrete variables, so is good when you just want
 continuous vs. continuous pairs.

{% highlight r linenos=table %}
png("scattermatrix_corrgram.png")
corrgram(mydata,
         order=FALSE, lower.panel=panel.pts,
         upper.panel=panel.ellipse, text.panel=panel.txt,
         diag.panel=panel.minmax)
dev.off()
{% endhighlight %}

<figure>
	<a href="/images/wine_2_scattermatrix_corrgram.jpeg"><img src="/images/wine_2_scattermatrix_corrgram.jpeg"></a>
	<figcaption>Scatterplot using an alternative plotting function (this one can't handle categories as well).</figcaption>
</figure>
