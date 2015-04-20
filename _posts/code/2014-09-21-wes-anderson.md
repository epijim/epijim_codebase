---
layout: article
title: "Wes Anderson palettes for R"
excerpt: "Using the Wes Anderson themed palettes from the wesanderson package in R."
categories: code
tags: [r]
toc: true
ads: true
comments: true
image:
  feature:
  teaser: wes_thumb.jpg
---

## Wes Anderson palettes

A tumblr user set up a [blog](http://wesandersonpalettes.tumblr.com/) where he breaks
 down the colour palettes used in Wes Anderson movies. This by itself is ok, but then
 someone called Karthik Ram turned these palettes R palettes, and released it
 as a package called *wesanderson* on CRAN.

## The package

First load the package, and get the available palettes. *namelist* tells us which movies
 have been added, and how many colours are in the palette.

{% highlight r linenos=table %}
  library(wesanderson)
# Current list of available palettes
  namelist
{% endhighlight %}

## The palettes

Code below loops through the list of palettes and plots them.

{% highlight r linenos=table %}
  par(mfrow=c(3,3))
for (i in 1:nrow(namelist)){
  name_movie <- namelist[i,1]
  number_colours <- namelist[i,2]
  display.wes.palette(number_colours,
                      name_movie)
}
{% endhighlight %}

<figure>
	<a href="/images/wes_1_palettes.jpg"><img src="/images/wes_1_palettes.jpg"></a>
	<figcaption>The palettes in the package.</figcaption>
</figure>

## Example of use

{% highlight r linenos=table %}
qplot(Sepal.Length,
      Petal.Length,
      data = iris,
      color = Species,
      size = Petal.Width)+
  theme_bw()+
  scale_color_manual(values = wes.palette(3, "GrandBudapest"))
{% endhighlight %}

<figure>
	<a href="/images/wes_2_example.jpeg"><img src="/images/wes_2_example.jpeg"></a>
	<figcaption>Example plot using the package.</figcaption>
</figure>
