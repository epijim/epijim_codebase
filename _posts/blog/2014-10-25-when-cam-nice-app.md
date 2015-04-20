---
layout: media
title: "When is it nice in Cambridge (interactive app)?"
categories: blog
tags: [r, weather, cambridge, app]
excerpt: "Using R to build an interactive weather app."
ads: true
toc: false
comments: true
share: true
image:
  feature: weather_feature.jpg
  teaser: weatherapp_thumb.png
  credit: Picture by Trinity College
  #creditlink: http://herneman.blogspot.co.uk
---

[Last week](http:epijim.uk/blog/when-cam-nice/) I plotted when the weather is nice in Cambridge.

After getting pointed towards the University Weather Station data, I decided to recreate it
using the this better data source as an interactive app where people can define
what nice means themselves.

<iframe src="http://epijim.shinyapps.io/IsItNice/" name="weatherapp" scrolling="auto" frameborder="no" align="center" height = "1000px" width = "700px">
</iframe>

This app is built using R, and hosted on the free Shiny servers on ShinyApps.io.
