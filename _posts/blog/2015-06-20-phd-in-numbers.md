---
layout: media
title: "My PhD in numbers"
categories: blog
tags: [r, python, phd]
excerpt: "A number based breakdown of my PhD."
ads: true
toc: true
comments: true
share: true
image:
  feature: mappython_feature.png
  teaser: mappython_thumb.png
  creditlink: Credit to Tina for taking the photo
---

## A PhD in numbers

The plot.

<figure>
	<a href="/images/mappython_1_england.png"><img src="/images/mappython_1_england.png"></a>
	<figcaption>My movements around the UK in the last two years (when I’ve had my phone on…).</figcaption>
</figure>

## How I got the data

I used [GitStats](https://github.com/hoxu/gitstats), which requires Python and Gnuplot to be installed.

Once downloaded, `cd` to the folder that has Gitstats, and then tell it where your repo is[^1], and where you want the output. Then run the file, for example:

{% highlight python %}
./gitstats /Users/Jimmy/PhD/Thesis /Users/Jimmy/PhD/ThesisQuantified
{% endhighlight %}


[^1]: This is all done locally, although services like Github have great API's to get statistics.
