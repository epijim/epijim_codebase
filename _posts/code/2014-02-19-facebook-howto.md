---
layout: article
title: "How to plot FB network data"
excerpt: "How I plotted my Facebook data using R."
categories: code
modified: 2014-09-27T11:57:41-04:00
tags: [r, facebook, network]
toc: true
ads: true
comments: true
image:
  feature:
  teaser: network_thumb.png
---

## Background

In an [earlier post I showed my facebook friend network](http://epijim.co.uk/articles/facebook-network/), plotted with R. Below is the code.
 I found the original code [to access the Facebook API](http://thinktostart.wordpress.com/2013/11/19/analyzing-facebook-with-r/),
 and [draw the plot](http://blog.revolutionanalytics.com/2013/11/how-to-analyze-you-facebook-friends-network-with-r.html)
 , on other sites.

## Required packages

So, load the needed packages.

{% highlight r linenos=table %}
library(Rfacebook)
library(Rook)
library(igraph)
library(gtools) # Needed for cutting variables into quartiles
{% endhighlight %}

## Setup OAuth

You need to get the necessary *APP ID* and *APP SECRET* from the [facebook developers site](https://developers.facebook.com/apps).
 Then feed that info into an object called *fb_auth*, which you call whenever you want to
 access the API.

{% highlight r linenos=table %}
fb_oauth <- fbOAuth(app_id="APP ID FROM SITE", app_secret="APP SECRET FROM SITE")
#now we have our fb_oauth connection
#	so we will just save them to be able to use them later
save(fb_oauth, file="fb_oauth")
#so if you want to connect to Facebook again you just have to call
load("fb_oauth")

me <- getUsers("me", token=fb_oauth)
me$name # my name # If it works - you'll see your name

my_friends <- getFriends(token=fb_oauth, simplify=TRUE)
head(my_friends, n=10) #first 10 friends - ID is the order they joined FB
nrow(my_friends) # number of friends
{% endhighlight %}

## Download friends

Now to download your friends. This isn't needed for the network plot, but worth
 downloading anyway.

{% highlight r linenos=table %}
#Too many friends in one group gets rejected by FB API.
#Break into groups of 80.
split_my_friends_1 <-  my_friends[1:80,]
split_my_friends_2 <-  my_friends[81:160,]
split_my_friends_3 <-  my_friends[161:240,]
split_my_friends_4 <-  my_friends[241:320,]
split_my_friends_5 <-  my_friends[321:400,]
split_my_friends_6 <-  my_friends[401:480,]
split_my_friends_7 <-  my_friends[481:nrow(my_friends),]

my_friends_info_1 <- getUsers(split_my_friends_1$id, token=fb_oauth, private_info=TRUE)
my_friends_info_2 <- getUsers(split_my_friends_2$id, token=fb_oauth, private_info=TRUE)
my_friends_info_3 <- getUsers(split_my_friends_3$id, token=fb_oauth, private_info=TRUE)
my_friends_info_4 <- getUsers(split_my_friends_4$id, token=fb_oauth, private_info=TRUE)
my_friends_info_5 <- getUsers(split_my_friends_5$id, token=fb_oauth, private_info=TRUE)
my_friends_info_6 <- getUsers(split_my_friends_6$id, token=fb_oauth, private_info=TRUE)
my_friends_info_7 <- getUsers(split_my_friends_7$id, token=fb_oauth, private_info=TRUE)

my_friends_info<- rbind.fill(my_friends_info_1,my_friends_info_2,
                             my_friends_info_3,my_friends_info_4,
                             my_friends_info_5,my_friends_info_6,
                             my_friends_info_7)
# Test it worked
head(my_friends_info, n=10) #first 10 friends - ID is the order they joined FB
nrow(my_friends_info) # number of friends
table(my_friends_info$relationship_status)
table(my_friends_info$gender)
table(my_friends_info$location)
table(my_friends_info$hometown)
{% endhighlight %}

## Get connections

Download who is friends with who (within those friends with me!).

{% highlight r linenos=table %}
my_network <- getNetwork(fb_oauth, format="adj.matrix")
{% endhighlight %}

## Plot it

The following code sets up the data. I define three plots, based on three algorithms. They are:

* layout.drl - force-directed graph layout developed by Shawn Martin and colleagues at Sandia National Laboratories.
* layout.fr - a force-based algorithm proposed by Fruchterman and Reingold
* layout.kk - an algorithm for drawing general undirected graphs by Kamada and Kawai

{% highlight r linenos=table %}
# friends who are friends with me alone
singletons <- rowSums(tina_network)==0

# remove singletons
my_graph <- graph.adjacency(tina_network[!singletons,!singletons])

# make connections one way
my_graph_simple <- simplify(my_graph,
                            remove.multiple = TRUE, remove.loops = TRUE)

# set up plot
#actual model
layout.drl <- layout.drl(my_graph_simple,options=list(simmer.attraction=0))
layout.fr <- layout.fruchterman.reingold(my_graph_simple,
                                         options=list(simmer.attraction=0))
layout.kk <- layout.kamada.kawai(my_graph_simple,options=list(simmer.attraction=0))
{% endhighlight %}

Now to style the plot, and colour the nodes/vertices based on their "betweenness".

{% highlight r linenos=table %}
#styling of plot
E(my_graph_simple)$color <- rgb(.5, .5, 0, 0.15)
E(my_graph_simple)$width <- 0.0001

# Let's colour based on connectedness.
#this is the number of "shortest paths" going
#through a particular individual
hc4 <- heat.colors(10,alpha=0.9)
betweenness <- betweenness(my_graph_simple)
vcolors <- factor(cut(betweenness, quantile(betweenness), include.lowest = TRUE))
vcolors <- quantcut(betweenness, q=seq(0,1,by=0.1))
vcolors2 <- hc4[vcolors]
{% endhighlight %}

Now plot the different layouts.

{% highlight r %}
pdf('network_drl.pdf')
plot(my_graph_simple, vertex.size=2, vertex.color=vcolors2,
     vertex.label=NA,
     vertex.label.cex=2,
     edge.arrow.size=0, edge.curved=TRUE,layout=layout.drl)
dev.off()

pdf('network_fr.pdf')
plot(my_graph_simple, vertex.size=2, vertex.color=vcolors2,
     vertex.label=NA,
     vertex.label.cex=2,
     edge.arrow.size=0, edge.curved=TRUE,layout=layout.fr)
dev.off()

pdf('network_kk.pdf')
plot(my_graph_simple, vertex.size=2, vertex.color=vcolors2,
     vertex.label=NA,
     vertex.label.cex=2,
     edge.arrow.size=0, edge.curved=TRUE,layout=layout.kk)
dev.off()
{% endhighlight %}

And the three plots that code makes (this isn't my FB data).

<figure>
	<a href="/images/networkhowto_drl.png"><img src="/images/networkhowto_drl.png"></a>
	<figcaption>DRL: force-directed graph layout developed by Shawn Martin and colleagues at Sandia National Laboratories.</figcaption>
</figure>

 <figure>
	<a href="/images/networkhowto_fr.png"><img src="/images/networkhowto_fr.png"></a>
	<figcaption>FR: a force-based algorithm proposed by Fruchterman and Reingold.</figcaption>
</figure>

 <figure>
	<a href="/images/networkhowto_kk.png"><img src="/images/networkhowto_kk.png"></a>
	<figcaption>KK: an algorithm for drawing general undirected graphs by Kamada and Kawai.</figcaption>
</figure>
