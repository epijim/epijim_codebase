---
layout: article
title: "Using Google API"
excerpt: "Getting distance data using Google API"
categories: code
tags: [r, api, google]
toc: true
comments: true
ads: true
share: true
image:
  feature:
  teaser: api_thumb.jpg
---

The following is the function I used to make my blog post on the distance to the nearest pub.
 This function was used to get the walking distance and time from a point I specified, to the
 nearest pub.

## Getting an API key

You don't always need an API key, as google will often allow requests based on your IP,
 but getting a key often means you are allowed to make more calls. Plus my current IP at
 Cambridge University seems to be responsible for a lot of keyless API requests, so it's
 often maxed out.

The [Google API console is here](https://console.developers.google.com). Simply set up a project,
 then create a browser key. You can limit requests to your own IP address to prevent someone
 else using it (say if you accidentally share the key), or leave that field blank and let
 anyone with the key use it.

## The available APIs at google

There are heaps of APIs out there, but I'm focusing on Google's as they are really well
 documented. The list of APIs available from Google is [here](https://developers.google.com/apis-explorer/).
 The one I want is the `Google Distance Matrix API`[^1], which is part of the maps API.

## The function

Function to take the latitude and longitude of two points, actually make the call,
 and spit out an object with the distance and time. The function has four inputs.

`origin` and `destination` is the latitude and longitude in the format `"latitude,longitude"`. It can also take
 text like `"Jesus College, Cambridge, UK"`.

`mode` is what mode of travel you want. The options are `"driving"`, `"walking"` and `"bicycling"`.

`key` is your API key.

{% highlight r linenos=table %}
  latlon2ft <- function(origin,destination,mode,key){
    xml.url <- paste0(
      'https://maps.googleapis.com/maps/api/distancematrix/xml?origins=',
      origin,'&destinations=',
      destination,
      '&mode=',
      mode,'
      &key=',
      key,
      '&sensor=false')
    xmlfile <- xmlParse(getURL(xml.url))
    time <- xmlValue(xmlChildren(xpathApply(xmlfile,"//duration")[[1]])$value)
    time <- round(as.numeric(time)/60,1)
    dist <- xmlValue(xmlChildren(xpathApply(xmlfile,"//distance")[[1]])$value)
    distance <- as.numeric(sub(" km","",dist))
    output <- list(time,distance)
    return(output)
  }
{% endhighlight %}

[^1]: [Documentation](https://developers.google.com/maps/documentation/distancematrix/)
