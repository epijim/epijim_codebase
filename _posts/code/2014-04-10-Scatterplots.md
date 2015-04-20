---
layout: article
title: "Scatter plots with R"
excerpt: "Making useful scatter plots with R."
categories: code
modified: 2014-09-27T11:57:41-04:00
tags: [r,scatter plot]
toc: true
ads: true
comments: true
image:
  feature:
  teaser: scatter_thumb.png
---

After the histogram, scatter plots are pretty much always the first plot I make when
 exploring a dataset. The code for the plots below mainly come from two sites. Which
 are both brilliant sources for info on using R.

 1. R for public health[^1]
 2. Quick R[^2]

I've only made small changes to fit my data, and to include themes, custom titles, etc.

## Data I'm using

To make it easily to replicate I put the data I'm using in a gist.

{% highlight r linenos=table %}
temporaryFile <- tempfile()
download.file("https://gist.githubusercontent.com/epijim/8819934/raw/6c76df80eb095065a9ce0fa4b8f94410ad528fed/college_data.csv"
              ,destfile=temporaryFile, method="curl")
mydata <- read.csv(temporaryFile)

# convert wine budget to thousands
mydata$Wine_budget <- mydata$Wine_budget/1000

options(scipen=999) # no scientific notation

# Just in case, I'll duplicate college as a string variable.
mydata$College_char <- as.character(mydata$College)
{% endhighlight %}

## Scatter plot with rug

A scatter plot with a rug plot on each axis.

{% highlight r linenos=table %}
png("scatterplot_1_rug.png",width=800)
  ggplot(mydata,aes(Founded,Wine_budget))+
    geom_point()+
    geom_rug(col="darkred",alpha=0.4)+
  theme_bw()+
  xlab("Year college was founded")+
  ylab("Wine budget in thousands")+
  ggtitle("Scatter of year Cambridge college founded and wine budget")
dev.off()
{% endhighlight %}


<figure>
	<a href="/images/scatterplot_1_rug.png"><img src="/images/scatterplot_1_rug.png"></a>
</figure>

## Scatter plot with histograms

Scatter plot with histograms on each axis.

{% highlight r linenos=table %}
library(ggplot2)
library(gridExtra)
#placeholder plot - prints nothing at all
empty <- ggplot()+geom_point(aes(1,1), colour="white") +
  theme(
    plot.background = element_blank(),
    panel.grid.major = element_blank(),
    panel.grid.minor = element_blank(),
    panel.border = element_blank(),
    panel.background = element_blank(),
    axis.title.x = element_blank(),
    axis.title.y = element_blank(),
    axis.text.x = element_blank(),
    axis.text.y = element_blank(),
    axis.ticks = element_blank()
  )

#scatterplot of x and y variables
scatter <- ggplot(mydata,aes(Founded, Wine_budget))+
  geom_point(aes(color=Rating))+
  scale_color_manual(values = c("orange", "purple","blue"))+
  theme_bw()+
  theme(legend.position=c(1,1),legend.justification=c(1,1))+
  xlab("Year college was founded")+
  ylab("Wine budget in thousands of £")

#marginal density of x - plot on top
plot_top <- ggplot(mydata, aes(Founded, fill=Rating)) +
  geom_density(alpha=.5) +
  scale_fill_manual(values = c("orange", "purple","blue")) +
  theme_bw()+
  theme(legend.position = "none",axis.title.x = element_blank()) # removes density x

#marginal density of y - plot on the right
plot_right <- ggplot(mydata, aes(Wine_budget, fill=Rating)) +
  geom_density(alpha=.5) +
  coord_flip() +
  scale_fill_manual(values = c("orange", "purple","blue")) +
  theme_bw()+
  theme(legend.position = "none",axis.title.y = element_blank()) # removes density y

#arrange the plots together, with appropriate height and width for each row and column
png("scatterplot_2_hist.png",width=800)
grid.arrange(plot_top, empty, scatter, plot_right,
             ncol=2, nrow=2, widths=c(4, 1), heights=c(1, 4),
             main="Scatter of year Cambridge college founded and wine budget \nby A/B/C Board of Graduate Studies college grading")
dev.off()
{% endhighlight %}


<figure>
	<a href="/images/scatterplot_2_hist.png"><img src="/images/scatterplot_2_hist.png"></a>
</figure>

## Dealing with large datasets

When you have lots of points, it becomes hard to see the outliers in a scatter plot. A
 super simple solution is just to make the points a bit transparent. Although that can hide
 the outliers.

In the plot below there are more than 16,000,000 data points. As it's financial data, on
 the price paid for bitcoins, it's all very closely clustered in one spot. This makes it
 hard to see the outliers, where trades occurred that were well below or above the market
 value of a bitcoin. The hexbin plot allows you to see this data.


{% highlight r linenos=table %}
library(hexbin)
png("scatter_2_hexbin.png", width=800)
  bin<-hexbin(bitcoin$When, bitcoin$priceperbitcoin_log, xbins=50)
  plot(bin,
       main="Price per bitcoin over time in 16,748,682 Mt.Gox transactions",
       xlab="Days since Jan 1st 1970",
       ylab="Log10 of price per bitcoin"
       )
dev.off()
{% endhighlight %}


<figure>
	<a href="/images/scatter_2_hexbin.png"><img src="/images/scatter_2_hexbin.png"></a>
	<figcaption>Hexbin plot.</figcaption>
</figure>

## Scatter heat maps

The next plot colours points based on how close other points are to it. This code came out of a
 Stack Overflow question.[^3]

{% highlight r linenos=table %}
## Use densCols() output to get density at each point
density <- densCols(mydata_naomit$Undergrad_applications,
                    mydata_naomit$Undergrad_acceptances,
                    colramp=colorRampPalette(c("black", "white")))
mydata_naomit$density <- col2rgb(density)[1,] + 1L

## Map densities to colors
colours <-  colorRampPalette(c("#000099", "#00FEFF", "#45FE4F",
                            "#FCFF00", "#FF9400", "#FF3100"))(256)
mydata_naomit$colours <- colours[mydata_naomit$density]

## Plot it, reordering rows so that densest points are plotted on top
png("scatter_3_heatmap.png")
plot(Undergrad_acceptances~Undergrad_applications,
     data=mydata_naomit[order(mydata_naomit$density),],
     pch=20, col=colours, cex=2,
     xlab="Undergraduate applications",
     ylab="Undergraduate acceptances")
dev.off()
{% endhighlight %}


<figure>
	<a href="/images/scatter_3_heatmap.png"><img src="/images/scatter_3_heatmap.png"></a>
	<figcaption>Scatter heat maps.</figcaption>
</figure>

## Scatter contour plots

And a scatter plot with a coloured contour line around the points.

{% highlight r linenos=table %}
png("scatter_3_contour.png", width=800)
plot(mydata_naomit,
     xlab="Undergraduate applications",
     ylab="Undergraduate acceptances",
     pch=1, cex=.4)
contour(z, drawlabels=FALSE, nlevels=levels, col=colours, add=TRUE)
dev.off()
{% endhighlight %}


<figure>
	<a href="/images/scatter_3_contour.png"><img src="/images/scatter_3_contour.png"></a>
	<figcaption>Contour scatter plot.</figcaption>
</figure>

## Interactive scatter plots

The rCharts package allows you to make awesome, interactive charts. There are are a number
 of libraries available, and each does things slightly differently. None of it seems well
 documented, and it's often confusing if you try and relate the documentation from the
 original libraries back to the rCharts plots. I found it easier to either edit the charts
 once they are in html, or to use *str()* on a plot environment to get a rough idea of
 what's editable.

{% highlight r linenos=table %}
#require(devtools)
#install_github('rCharts', 'ramnathv')

require(rCharts)

selectedfactor = mydata$College

rchart <- rPlot(Firsts_2013 ~ Wine_budget ,
            data = mydata,
            type = "point",
            width = 500,
            color = "College_char")
rchart$guides(y = list(min = 0, max = 40))
rchart$guides(x = list(min = 0, max = 400000))
rchart$guides(
  color = list(
    numticks = length( levels( selectedfactor ) ),
    labels = as.character( levels( selectedfactor ) )
  ),
  y=list(title="% with firsts"),
  x=list(title="Yearly wine budget in thousands (£)"))
rchart$set(width = 550)
rchart$set(height = 460)

rchart$save('index.html', cdn=F)
{% endhighlight %}

Most of above is pretty self explanatory - except maybe *cdn*. If you know you'll have
 internet access, set it as true. If you have no internet, or don't want to rely on the rCharts site,
 set it as false and put the javascript that runs the plot on your computer/server.

<html>
  <head>

    <script src='/jamesscripts/polychart2.standalone.js' type='text/javascript'></script>

    <style>
    .rChart {
      display: block;
      margin-left: auto;
      margin-right: auto;
      width: 550px;
      height: 460px;
    }  
    </style>

  </head>
  <body>
    <div id='charteba1c84cbc4' class='rChart polycharts'></div>  

    <script type='text/javascript'>
    var chartParams = {
 "dom": "charteba1c84cbc4",
"width":    550,
"height":    460,
"layers": [
 {
 "x": "Wine_budget",
"y": "Firsts_2013",
"data": {
 "College": [ "Christ's", "Churchill", "Clare", "Clare Hall", "Corpus Christi", "Darwin", "Downing", "Emma", "Fitzwilliam", "Girton", "Caius", "Homerton", "Hughes Hall", "Jesus", "King's", "Lucy Cavendish", "Magdalene", "Murray Edwards", "Newnham", "Pembroke", "Peterhouse", "Queens", "Robinson", "Selwyn", "Sidney", "St Catharine's", "St Edmund's", "St John's", "Trinity", "Trinity Hall", "Wolfson" ],
"Grad_coll": [ false, false, false, true, false, true, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false ],
"Wine_budget": [ 71055, 87685, 79989, 17400, 79254, 17510, 77798, 131127, 23028, 30051, 96994, 27975, 14034, 212256, 338559, null, 68192, 32917, 27263, 141692, 82133, 111113, 44722, 49498, 97507, 62432, 19304, 260064, 223292, 127186, 39647 ],
"Fellows": [ 84, 165, 120, 67, 54, 68, 65, 87, 56, 118, 114, 90, 53, 85, 132, 40, 83, 58, 70, 102, 41, 84, 82, 58, 67, 60, 98, 149, 185, 75, 271 ],
"Students": [ 617, 770, 816, 181, 483, 633, 696, 650, 780, 722, 775, 1324, 600, 800, 685, 370, 526, 548, 644, 704, 441, 1007, 558, 577, 597, 650, 565, 897, 1060, 638, 711 ],
"Grads": [ 208, 290, 312, 181, 223, 633, 271, 220, 288, 220, 253, 695, 500, 335, 282, 226, 206, 188, 264, 260, 162, 496, 172, 200, 249, 220, 443, 354, 364, 258, 576 ],
"PhD": [ 127, 190, 190, 96, 145, 421, 151, 121, 151, 102, 161, 144, 200, 200, 202, 77, 105, 109, 127, 168, 117, 272, 51, 124, 135, 123, 65, 265, 288, 185, 342 ],
"Other": [ 81, 100, 122, 85, 78, 212, 120, 99, 137, 118, 92, 551, 300, 132, 80, 149, 101, 79, 137, 92, 45, 224, 121, 76, 104, 97, 378, 89, 76, 78, 234 ],
"Rating": [ "B", "B", "A", "B", "B", "C", "B", "A", "C", "B", "B", "A", "C", "A", "A", "C", "B", "C", "C", "B", "B", "B", "C", "B", "B", "B", "C", "A", "A", "A", "C" ],
"Part_time": [ "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "No", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "No", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "No", "Yes", "Yes", "Yes" ],
"Undergrad_applications": [ 451, 350, 573, null, 200, null, 548, 503, 260, 261, 393, 196, 32, 554, 417, 39, 237, 120, 167, 505, 155, 412, 222, 333, 306, 378, 25, 383, 383, 288, 29 ],
"Undergrad_acceptances": [ 99, 89, 134, null, 68, null, 97, 126, 95, 119, 134, 119, 7, 127, 88, 9, 77, 77, 79, 98, 56, 106, 97, 100, 86, 103, 4, 117, 128, 90, 13 ],
"Undergrad_acceptRate": [ 22, 25, 23, null, 34, null, 18, 25, 37, 46, 34, 61, 22, 23, 21, 23, 32, 64, 47, 19, 36, 26, 44, 30, 28, 27, 16, 31, 33, 31, 45 ],
"Founded": [ 1505, 1960, 1326, 1965, 1352, 1964, 1800, 1584, 1966, 1869, 1348, 1976, 1885, 1496, 1441, 1965, 1428, 1954, 1871, 1347, 1284, 1448, 1977, 1882, 1596, 1473, 1896, 1511, 1546, 1350, 1965 ],
"Percent_male": [ 58, 71, 52, 47, 60, 54, 66, 51, 63, 53, 60, 37, 61, 57, 57, 0, 54, 0, 0, 53, 57, 57, 60, 70, 63, 52, 69, 59, 63, 54, 64 ],
"Percent_female": [ 42, 29, 48, 53, 40, 46, 34, 49, 37, 47, 40, 63, 39, 43, 43, 100, 46, 100, 100, 47, 43, 43, 40, 30, 37, 48, 31, 41, 37, 46, 36 ],
"Fixed_assets": [ 66602000, 105978346, 70707000, 10579203, 191233087, 33160032, 86798000, 152640692, 43509000, 64000000, 159332000, 123453808, 18483546, 242995403, 155618000, 24323000, 73763845, 52852893, 90287969, 103991180, 171887000, 57310511, 24863000, 69992285, 64952747, 68797000, 8381224, 653359000, 892529000, 208176916, 47307000 ],
"Tompkin_1997": [ 3, 15, 11, null, 23, null, 12, 7, 13, 22, 8, null, null, 20, 14, null, 17, 24, 18, 5, 19, 3, 21, 16, 4, 9, null, 10, 1, 6, null ],
"Tompkin_1998": [ 2, 13, 6, null, 18, null, 11, 5, 12, 21, 4, null, null, 16, 10, null, 22, 24, 20, 9, 23, 2, 19, 15, 17, 14, null, 8, 1, 7, null ],
"Tompkin_1999": [ 1, 20, 15, null, 8, null, 16, 5, 19, 21, 6, null, null, 11, 13, null, 23, 22, 24, 14, 17, 3, 9, 7, 4, 10, null, 12, 2, 18, null ],
"Tompkin_2000": [ 1, 15, 9, null, 10, null, 8, 3, 21, 18, 12, null, null, 13, 20, null, 22, 16, 24, 6, 14, 5, 19, 7, 23, 11, null, 4, 2, 17, null ],
"Tompkin_2001": [ 1, 9, 6, null, 20, null, 10, 2, 13, 17, 8, null, null, 11, 21, null, 22, 23, 24, 7, 19, 5, 14, 12, 16, 18, null, 4, 3, 15, null ],
"Tompkin_2002": [ 4, 10, 3, null, 18, null, 8, 2, 20, 16, 7, null, null, 9, 14, null, 15, 24, 22, 1, 23, 5, 21, 13, 19, 12, null, 11, 6, 17, null ],
"Tompkin_2003": [ 2, 9, 6, null, 7, null, 12, 1, 20, 17, 4, 25, 27, 10, 16, 26, 18, 24, 21, 3, 22, 5, 23, 14, 15, 11, 29, 13, 8, 19, 28 ],
"Tompkin_2004": [ 2, 19, 4, null, 10, null, 17, 1, 15, 25, 5, 24, 27, 9, 20, 26, 22, 23, 13, 6, 21, 8, 16, 11, 18, 7, 29, 14, 3, 12, 28 ],
"Tompkin_2005": [ 4, 18, 9, null, 16, null, 15, 5, 13, 24, 2, 26, 29, 7, 10, 27, 20, 25, 21, 6, 22, 8, 11, 19, 14, 1, 28, 12, 3, 17, 23 ],
"Tompkin_2006": [ 6, 13, 12, null, 8, null, 11, 1, 19, 22, 2, 25, 29, 10, 17, 26, 20, 24, 23, 4, 21, 14, 18, 7, 9, 3, 28, 15, 5, 16, 27 ],
"Tompkin_2007": [ 2, 15, 17, null, 8, null, 3, 1, 14, 21, 10, 26, 29, 9, 18, 24, 13, 23, 22, 7, 25, 11, 20, 4, 12, 5, 28, 19, 6, 16, 27 ],
"Tompkin_2008": [ 8, 6, 13, null, 9, null, 12, 2, 21, 22, 4, 25, 26, 7, 19, 28, 5, 23, 24, 10, 18, 16, 17, 1, 14, 11, 29, 20, 3, 15, 27 ],
"Tompkin_2009": [ 13, 7, 18, null, 10, null, 15, 2, 21, 20, 4, 25, 26, 11, 17, 29, 8, 23, 24, 6, 16, 12, 19, 3, 22, 5, 28, 14, 1, 9, 27 ],
"Tompkin_2010": [ 12, 3, 8, null, 13, null, 15, 1, 22, 21, 11, 26, 27, 16, 14, 29, 5, 23, 25, 10, 7, 17, 19, 6, 18, 9, 28, 20, 2, 4, 24 ],
"Tompkin_2011": [ 6, 10, 4, null, 12, null, 17, 2, 21, 23, 13, 26, 27, 8, 20, 29, 9, 22, 24, 5, 18, 14, 19, 7, 16, 11, 28, 15, 1, 3, 25 ],
"Tompkin_2012": [ 9, 5, 11, null, 3, null, 20, 2, 19, 22, 16, 27, 26, 7, 13, 29, 15, 24, 23, 4, 18, 12, 21, 6, 17, 10, 28, 14, 1, 8, 25 ],
"Tompkin_2013": [ 8, 5, 11, null, 16, null, 12, 4, 20, 21, 17, 26, 27, 6, 14, 28, 15, 24, 23, 2, 10, 7, 22, 18, 19, 9, 29, 13, 1, 3, 25 ],
"Tompkin_Mean09to13": [    4.9,   11.3,    9.6, null,   12.3, null,   12.6,    2.7,   17.8,   20.8,    7.8,   25.5,   27.3,   10.6,   15.9,   27.4,   15.9,     23,   22.1,    6.2,   18.4,    8.6,   18.1,    9.8,   15.1,    9.2,   28.4,   12.8,    2.9,   11.9,     26 ],
"TompkinsScore_2013": [  66.76,  68.17,  66.08, null,  64.94, null,  66.05,  68.72,  64.16,  62.92,  64.85,  60.33,  57.92,  67.47,  65.49,  57.43,     65,  60.74,  60.92,  70.85,  66.41,  66.89,  61.95,  64.59,  64.37,  66.51,  56.35,  65.84,  73.66,  68.94,  60.51 ],
"Firsts_2013": [   24.7,   28.3,   25.3, null,   23.9, null,     23,   30.5,   21.4,   18.5,   21.1,     16,   13.2,   27.1,   25.9,   13.4,   22.8,   13.9,   15.8,   33.7,   27.8,   26.3,   16.2,   21.6,   19.8,   26.9,   13.4,   25.1,   41.7,   28.1,   18.9 ],
"College_char": [ "Christ's", "Churchill", "Clare", "Clare Hall", "Corpus Christi", "Darwin", "Downing", "Emma", "Fitzwilliam", "Girton", "Caius", "Homerton", "Hughes Hall", "Jesus", "King's", "Lucy Cavendish", "Magdalene", "Murray Edwards", "Newnham", "Pembroke", "Peterhouse", "Queens", "Robinson", "Selwyn", "Sidney", "St Catharine's", "St Edmund's", "St John's", "Trinity", "Trinity Hall", "Wolfson" ]
},
"facet": null,
"type": "point",
"width":    500,
"color": "College_char"
}
],
"facet": [],
"guides": {
 "y": {
 "min":      0,
"max":     40,
"title": "% with firsts"
},
"x": {
 "min":      0,
"max":  4e+05,
"title": "Yearly wine budget in thousands (£)"
},
"color": {
 "numticks": 31,
"labels": [ "Caius", "Christ's", "Churchill", "Clare", "Clare Hall", "Corpus Christi", "Darwin", "Downing", "Emma", "Fitzwilliam", "Girton", "Homerton", "Hughes Hall", "Jesus", "King's", "Lucy Cavendish", "Magdalene", "Murray Edwards", "Newnham", "Pembroke", "Peterhouse", "Queens", "Robinson", "Selwyn", "Sidney", "St Catharine's", "St Edmund's", "St John's", "Trinity", "Trinity Hall", "Wolfson" ]
}
},
"coord": [],
"id": "charteba1c84cbc4"
}
    _.each(chartParams.layers, function(el){
        el.data = polyjs.data(el.data)
    })
    var graph_charteba1c84cbc4 = polyjs.chart(chartParams);
</script>

  </body>
</html>

## 3D scatter plots

A 3D scatter plot from [my post about mapping ski data](/articles/ski-2014/). The 3D scatter plot is super simple to make:
code [is here](/articles/3Dscatter/).

<figure>
	<a href="/images/ski_scattergif.gif"><img src="/images/ski_scattergif.gif"></a>
	<figcaption>3D scatter plot of a day skiing.</figcaption>
</figure>

## Scatter plot matrix's

Code for these are on my page on the scatter plot matrix, [here](/articles/Scatterplotmatrix/).

<figure class="half">
	<img src="/images/wine_1_scattermatrix.jpeg">
	<img src="/images/wine_2_scattermatrix_corrgram.jpeg">
	<figcaption>Two types of scatter plot matrix.</figcaption>
</figure>





[^1]: Part of a the ggplot series on: <http://rforpublichealth.blogspot.co.uk>
[^2]: Quick R has a dedicated page on pretty much every simple type of plot: <http://www.statmethods.net>
[^3]: The original question is here: <http://stackoverflow.com/questions/7073315/how-do-i-create-a-continuous-density-heatmap-of-2d-scatter-data-in-r>
