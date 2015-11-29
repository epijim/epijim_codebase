---
layout: media
title: "Jesus College MCR webpage"
categories: blog
tags: [jesus, mcr]
excerpt: "Looking closely at the MCR webpage usage."
ads: true
toc: false
comments: true
share: true
image:
  feature: mcrpage_feature.png
  teaser: mcrpage_thumb.png
 # creditlink: http://herneman.blogspot.co.uk
---

>Update: In January 2015 we retired the main MCR site, and decided to split our web presence across an internal wiki and booking site (running Wordpress and Mediawiki) and a public facing Jekyll derived static HTML site. In it's two years of life the site had 12,468 visitors (or rather impressions by people without valid cookies form a previous visit) who visited 26,504 times and viewed 59,978 pages. The most commonly visited page (after the front page) was the full page calendar (9.6% of visits), followed by the accommodation page (4.6%) and freshers fortnight page (2.4%). The welfare section of the site was the least visited (<0.01%, a handful of visits). Two of our most important resources, the punt/swap/TV booker and the calendar are not represented here as I kept the tracking code off the booking pages as it uses Raven logins to create the booking accounts (I don't want any possibility of linking when accounts logged in to their use) and most people have probably subscribed to the calendar so get it directly feed into their calendar apps.

Throughout 2012 the Jesus MCR webpage was infected by a series of malicious scripts,
 which led us to can the website entirely (it also had Joomla as a CMS, which I deteste
 compared to Wordpress). So over Christmas 2012/13 Poul and I rebuilt it completely.

Below is all visitor data collected from Jan 2013 to Jan 2014, spanning the first 12 months of the new site
running. During that time 5,819 'individuals[^1]' visited 13,804 times and they viewed 39,942 pages.

The first plot below shows the number of people that visited per country, as well as a
 few other statistics that are visible when you click on a point.

<iframe width="600" height="400" scrolling="no" frameborder="no" src="https://www.google.com/fusiontables/embedviz?q=select+col0+from+1bkoCjVPzxzXuexgh8H1qe5c6t9pnMvXXTdjioQQ&amp;viz=MAP&amp;h=false&amp;lat=31.1673566580505&amp;lng=102.7194905&amp;t=1&amp;z=2&amp;l=col0&amp;y=2&amp;tmplt=2&amp;hml=ONE_COL_LAT_LNG"></iframe>

This plot has the same data, but grouped by city rather than country. Both of these plots
 are google fusion tables.

<iframe width="600" height="400" scrolling="no" frameborder="no" src="https://www.google.com/fusiontables/embedviz?q=select+col0+from+1usjrYMHtxlujxICuJ-8Ik48RD_u62YEreQVb19A&amp;viz=MAP&amp;h=false&amp;lat=48.413979685096336&amp;lng=6.796510445312376&amp;t=1&amp;z=5&amp;l=col0&amp;y=2&amp;tmplt=2&amp;hml=ONE_COL_LAT_LNG"></iframe>

And now a plot with the numbers that visit per week, broken down by new or returning users.
 I haven't found that much info on how it defines new users (*I also haven't looked that hard*),
 but it seems that it's based on whether they have a cookie from a previous visit. As someone
 that uses plugins that block tracking cookies, this means every time I visited the site I would
 count as a new visitor.[^2]

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js"> {"dataSourceUrl":"//docs.google.com/spreadsheet/tq?key=0AvBt7ZO0aI7GdGlPRFBxWXBIcTFiam40czJlUXVfSUE&transpose=0&headers=1&range=A1%3AC54&gid=0","options":{"displayAnnotations":true,"titleTextStyle":{"fontSize":16},"vAxes":[{"useFormatFromData":true,"title":"Left vertical axis title","minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null},{"useFormatFromData":true,"minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null}],"booleanRole":"certainty","title":"Chart title","height":371,"scaleType":"fixed","width":600,"wmode":"opaque","hAxis":{"useFormatFromData":true,"title":"Horizontal axis title","minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null},"animation":{"duration":500}},"state":{},"view":{},"isDefaultVisualization":true,"chartType":"AnnotatedTimeLine","chartName":"Chart 1"} </script>

The data in the last plot is probably a bit shabby, and a clearer picture comes out if you
 use location as indicator of new vs. returning. In the next plot it looks like you can see
 the new students (or people outside Cambridge), who start in October, dominating visits for
 a short period in August to September.

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js"> {"dataSourceUrl":"//docs.google.com/spreadsheet/tq?key=0AvBt7ZO0aI7GdGlPRFBxWXBIcTFiam40czJlUXVfSUE&transpose=0&headers=1&range=F1%3AH54&gid=0","options":{"displayAnnotations":true,"titleTextStyle":{"fontSize":16},"vAxes":[{"useFormatFromData":true,"title":"Left vertical axis title","minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null},{"useFormatFromData":true,"minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null}],"booleanRole":"certainty","title":"Chart title","height":371,"width":600,"wmode":"opaque","hAxis":{"useFormatFromData":true,"title":"Horizontal axis title","minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null},"animation":{"duration":500}},"state":{},"view":{},"isDefaultVisualization":true,"chartType":"AnnotatedTimeLine","chartName":"Chart 2"} </script>

The plot below shows the joys of being a university student orientated website - we get
 a lot of users on OSX.

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js"> {"dataSourceUrl":"//docs.google.com/spreadsheet/tq?key=0AvBt7ZO0aI7GdGlPRFBxWXBIcTFiam40czJlUXVfSUE&transpose=0&headers=1&range=A1%3AB11&gid=2","options":{"vAxes":[{"useFormatFromData":true,"minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null},{"useFormatFromData":true,"minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null}],"titleTextStyle":{"bold":false,"color":"#000","italic":false,"fontSize":16},"pieHole":"0.5","booleanRole":"certainty","title":"Users by operating system","legend":"right","colors":["#3366CC","#DC3912","#FF9900","#109618","#990099","#0099C6","#DD4477","#66AA00","#B82E2E","#316395","#994499","#22AA99","#AAAA11","#6633CC","#E67300","#8B0707","#651067","#329262","#5574A6","#3B3EAC","#B77322","#16D620","#B91383","#F4359E","#9C5935","#A9C413","#2A778D","#668D1C","#BEA413","#0C5922","#743411"],"is3D":false,"hAxis":{"useFormatFromData":true,"title":"Horizontal axis title","minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null},"width":600,"height":371},"state":{},"view":{},"isDefaultVisualization":false,"chartType":"PieChart","chartName":"Chart 2"} </script>

The next plot shows some of the holes in the data - while 26% came via google, and 14% from
 facebook, for over a third of the visitors there was no information on what site brought them
 to our webpage.

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js"> {"dataSourceUrl":"//docs.google.com/spreadsheet/tq?key=0AvBt7ZO0aI7GdGlPRFBxWXBIcTFiam40czJlUXVfSUE&transpose=0&headers=1&range=A13%3AB18&gid=2","options":{"vAxes":[{"useFormatFromData":true,"minValue":null,"viewWindow":{"max":null,"min":null},"maxValue":null},{"useFormatFromData":true,"minValue":null,"viewWindow":{"max":null,"min":null},"maxValue":null}],"titleTextStyle":{"bold":false,"color":"#000","fontSize":16},"pieHole":0.5,"booleanRole":"certainty","title":"Source","height":371,"animation":{"duration":500},"colors":["#3366CC","#DC3912","#FF9900","#109618","#990099","#0099C6","#DD4477","#66AA00","#B82E2E","#316395","#994499","#22AA99","#AAAA11","#6633CC","#E67300","#8B0707","#651067","#329262","#5574A6","#3B3EAC","#B77322","#16D620","#B91383","#F4359E","#9C5935","#A9C413","#2A778D","#668D1C","#BEA413","#0C5922","#743411"],"width":600,"is3D":false,"hAxis":{"useFormatFromData":true,"minValue":null,"viewWindow":{"max":null,"min":null},"maxValue":null},"tooltip":{"trigger":"none"},"focusTarget":"series"},"state":{},"view":{},"isDefaultVisualization":false,"chartType":"PieChart","chartName":"Chart 3"} </script>

For the 5.7% of the individuals that used our site and were logged into their google accounts,
a much richer set of data is recorded.

The last three plots are based only on the fraction of users that were logged into google
 when they came to the site. It's probably not representative at all, but I'll plot it anyway.

 The majority of visitors are young. Then again, the majority of heavy google product users
  are also probably young.

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js"> {"dataSourceUrl":"//docs.google.com/spreadsheet/tq?key=0AvBt7ZO0aI7GdGlPRFBxWXBIcTFiam40czJlUXVfSUE&transpose=0&headers=1&range=A22%3AB28&gid=2","options":{"vAxes":[{"useFormatFromData":true,"title":null,"minValue":null,"logScale":false,"viewWindow":{"max":null,"min":null},"maxValue":null},{"useFormatFromData":true,"minValue":null,"logScale":false,"viewWindow":{"max":null,"min":null},"maxValue":null}],"titleTextStyle":{"bold":true,"color":"#000","fontSize":16},"booleanRole":"certainty","title":"Age distribution (~4.7% of visitors)","height":371,"animation":{"duration":500},"legend":"none","width":600,"hAxis":{"useFormatFromData":true,"minValue":null,"viewWindowMode":null,"viewWindow":null,"maxValue":null},"isStacked":false,"focusTarget":"series","tooltip":{"trigger":"none"}},"state":{},"view":{},"isDefaultVisualization":false,"chartType":"ColumnChart","chartName":"Chart 4"} </script>

Based on the kind of sites you visit while logged into google, google summarises what kind of online personality
 you have. While this is not a robust statement, I am glad the largest group visiting our site
 is News Junkies & Avid Readers.

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js"> {"dataSourceUrl":"//docs.google.com/spreadsheet/tq?key=0AvBt7ZO0aI7GdGlPRFBxWXBIcTFiam40czJlUXVfSUE&transpose=0&headers=0&range=A37%3AB46&gid=2","options":{"vAxes":[{"useFormatFromData":true,"minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null},{"useFormatFromData":true,"minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null}],"titleTextStyle":{"bold":false,"color":"#000","fontSize":16},"pieHole":0.5,"booleanRole":"certainty","title":"'Affinity' of the ~4.7% using google","colors":["#3366CC","#DC3912","#FF9900","#109618","#990099","#0099C6","#DD4477","#66AA00","#B82E2E","#316395","#994499","#22AA99","#AAAA11","#6633CC","#E67300","#8B0707","#651067","#329262","#5574A6","#3B3EAC","#B77322","#16D620","#B91383","#F4359E","#9C5935","#A9C413","#2A778D","#668D1C","#BEA413","#0C5922","#743411"],"is3D":false,"hAxis":{"useFormatFromData":true,"title":"Horizontal axis title","minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null},"width":600,"height":371},"state":{},"view":{},"isDefaultVisualization":false,"chartType":"PieChart","chartName":"Chart 5"} </script>

Less impressive is the market segments google claims our site covers. I have to admit I always
 assumed there was a very strong correlation between being more educated, and not being into
 cars, but as most of the other segments refer to real estate, and the automotive segment
 includes bicycles, it could be that people visiting the MCR site are also doing a lot of
 googling about accommodation and buying a bike.[^3]

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js"> {"dataSourceUrl":"//docs.google.com/spreadsheet/tq?key=0AvBt7ZO0aI7GdGlPRFBxWXBIcTFiam40czJlUXVfSUE&transpose=0&headers=0&range=A50%3AB59&gid=2","options":{"vAxes":[{"useFormatFromData":true,"minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null},{"useFormatFromData":true,"minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null}],"titleTextStyle":{"bold":false,"color":"#000","fontSize":16},"pieHole":0.5,"booleanRole":"certainty","title":"Market segments covered by mcr.jesus.cam.ac.uk users (from the ~4.7%)","height":371,"animation":{"duration":500},"colors":["#3366CC","#DC3912","#FF9900","#109618","#990099","#0099C6","#DD4477","#66AA00","#B82E2E","#316395","#994499","#22AA99","#AAAA11","#6633CC","#E67300","#8B0707","#651067","#329262","#5574A6","#3B3EAC","#B77322","#16D620","#B91383","#F4359E","#9C5935","#A9C413","#2A778D","#668D1C","#BEA413","#0C5922","#743411"],"width":600,"is3D":false,"hAxis":{"useFormatFromData":true,"title":"Horizontal axis title","minValue":null,"viewWindow":{"min":null,"max":null},"maxValue":null}},"state":{},"view":{},"isDefaultVisualization":false,"chartType":"PieChart","chartName":"Chart 6"} </script>



[^1]: 'Individuals' in the sense there isn't a cookie present to say they've visited before.
[^2]: In reality I actually allow cookies from cam.ac.uk and my own domain, so I can exclude my own visits.
[^3]: This doesn't really hold up though, as Jesus provides all it's grads with cheap accomodation, and I doubt google is that good at assessing market segments it ties your site to the kind of sites they visited immediately before yours.
