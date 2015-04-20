---
layout: article
title: "LaTeX sparklines from R"
excerpt: "Function to make LaTeX sparkline code from an R variable."
categories: code
tags: [r, latex, sparklines]
toc: true
ads: true
comments: true
image:
  feature:
  teaser: sparklines_thumb.png
---

## Why?

Standard deviations and inter-quartile ranges help give a general impression of the distribution of a variable, but sometimes I want to show what a variable looks like, without devoting half the page to add a figure. In my PhD, I needed a quick way to take a variable in R, and produce a histogram or line graph that could slot directly into the text alongside the mean/median and SD/IQR. This way I could provide much more information on what the variable spread look liked, quickly.

## My solution

Sparklines are tiny charts, usually line or histographs, that can be embedded
 in text into documents. Below are two examples of sparklines from my PhD. The top
 line has a histogram, the second line has
 two linegraphs. The two sentences are
 from different parts of my PhD, which is why it's confusing to read the lines together.
 A latex function exists for plotting these values, but there was no easy
 way to take a variable in R, and quickly get LaTeX ready code. To speed things up I wrote the function below.

<figure>
	<a href="/images/sparklines_examples.png"><img src="/images/sparklines_examples.png"></a>
	<figcaption>Examples of what the code produces in LaTeX.</figcaption>
</figure>

## My function

In the most basic application, all that is required is the `variable` you want (a vector or column of a dataframe),
  as the only input. For instance `jb_CutIntoBins(variable)` is sufficient to get a sparkline histogram of a variable in an R dataframe. My function will output all the LaTeX code required to plot a sparkline
  histogram with 10 bins (bars) and a width of 6. If you provide two pieces of data, then the function will assume you want a
  linegraph (first variable will be on the y-axis, second on the x-axis) The number of bins can be edited via the options `bins=` and `width=`. You can also name the sparkline with `name=`. I put a proper help file into the package, that can
  be called from R with `?jb_CutIntoBins`, if you install it via github (instructions below).

## Example use

#### Load in some test data

This is only needed to recreate the demo sparklines I make below.

{% highlight r %}
url <- 'https://gist.githubusercontent.com/epijim/8819934/raw/6c76df80eb095065a9ce0fa4b8f94410ad528fed/college_data.csv'
 library(RCurl)
 data <- getURL(url, ssl.verifypeer=FALSE)
 data <- read.csv(text = data)
{% endhighlight %}

#### Load in my function.

Assuming you have the `devtools` package installed you can load my function via the line of code below. Installing it this way will load up all my functions, and will also load up the help documentation. Otherwise, at the bottom of the post is the full code for the function.

{% highlight r %}
install_github('epijim/EpiJimFunctions')
{% endhighlight %}

#### Test the function

Firstly - you have to add the package `sparklines` to the front matter in LaTeX to render this code.

Typing `jb_CutIntoBins(data$Wine_budget)` will result in the following output. This code can then be added to the front matter, and then you can call it in text with `\sparkline`:

{% highlight latex %}
\newcommand{\sparkline}{\begin{sparkline}{6}
 \sparkspike 0 1
 \sparkspike 0.111111111111111 0.5
 \sparkspike 0.222222222222222 0.7
 \sparkspike 0.333333333333333 0.3
 \sparkspike 0.444444444444444 0
 \sparkspike 0.555555555555556 0
 \sparkspike 0.666666666666667 0.2
 \sparkspike 0.777777777777778 0.1
 \sparkspike 0.888888888888889 0
 \sparkspike 1 0.1
\end{sparkline}}
{% endhighlight %}

As an side, I prefer to keep the LaTeX code for the sparklines separate from the main text, which is why I use `\newcommand` to define the sparkline in the front matter, then I can just call it via a single word in the text.

A more complicated example using all the options to make a line graph is below.

{% highlight r %}
#quickly make an age variable from my example data
  data$collegeage <- 2015 - data$Founded

# Using my function
jb_CutIntoBins(data$Fixed_assets, # First variable is the thing I'm interested in
  data$collegeage, # Adding a second variable switches from histograph to linegraph
  bins=5, # Number of points I'm collapsing the data into
  width=4, # How wide the in text plot should be
  name="wineandage") # I can name the sparkline, if I make more than one.

{% endhighlight %}

This will produce the following LaTeX code.

{% highlight latex %}
\newcommand{\sparkoldmoney}{\begin{sparkline}{4}
 \spark
   0   0.285653902704857
   0.25   0.524112445240158
   0.5   0.656947333939176
   0.75   0.692538305471606
   1   1
/ \end{sparkline}}
{% endhighlight %}

Which can be added to the front matter of the LaTeX document, then in text you can call
 it like below.

{% highlight latex %}
The following line is slope-y \sparkoldmoney.
{% endhighlight %}

And it LaTeX will render the following.

<figure>
	<a href="/images/sparklines_slopey.png"><img src="/images/sparklines_slopey.png"></a>
	<figcaption>An example linegraph, AKA the original sparkline.</figcaption>
</figure>

Although the variables I used are a little confusing, it's much more instinctive to
 read if you have linear time as the x-axis and are tracking something. I should also note
 that the linegraph function takes the median of each bin.

## The function code

I wrote this function in under an hour, in the hopes of it saving me
 from a job that would only take a couple of hours. So this is not polished code
 and I'm keen to hear how it can be improved. If I do revisit this code I'll add an
 option to use means or maybe a smoother rather than medians, and will incorporate all the other sparkline
 options available in the LaTeX package, like boxes and coloured points on the linegraphs.

{% highlight r linenos=table %}
jb_CutIntoBins <- function(variable,time="potato",name="line",bins=10,width=6){
  # Make histograph ###################### if clause ###########################
  if(length(time)==1){
    temp_ranges <- range(variable, na.rm=T) # Get range
    temp_breaks <- seq(temp_ranges[1],temp_ranges[2], # From first to last
                       length=(bins+1)) # Number of bins
    temp_cut <- as.data.frame(cut(variable,
                                  breaks=temp_breaks,labels=F))
    # Set up dataset
    temp_data <- data.frame(bin=1:bins,
                            value=c(NA)
    )
    temp_cut <- as.data.frame(table(temp_cut),stringsAsFactors=F) #
    temp_cut$temp_cut <- as.numeric(temp_cut$temp_cut)
    # Fill dataset
    for(i in 1:bins){
      tryCatch(
        if(i==temp_data[i,1]){
          temp_data[i,2] <- temp_cut[temp_cut$temp_cut==i,2]
        },error=function(e){})
    }
    # Make sparkline
    binlocations <- seq(0,1,length=bins)
    middle<-NA
    for(i in 1:nrow(temp_data)){
      ifelse(is.na(temp_data[i,2]),
             middle[i] <- paste0(" \\sparkspike ",binlocations[i]," ",0),
             middle[i] <- paste0(" \\sparkspike ",binlocations[i]," ",
                                 temp_data[i,2]/max(temp_data$value,na.rm=T))
      )
    }
  }
  # Make line ###################### if clause ###########################
  if(length(time)>1){
    temp_ranges <- range(time, na.rm=T) # Get range
    temp_breaks <- seq(temp_ranges[1],temp_ranges[2], # From first to last
                       length=(bins+1)) # Number of bins
    temp_cut <- as.data.frame(cut(time,
                                  breaks=temp_breaks,labels=F))
    temp_data <- data.frame(temp_cut,time=time,value=variable)
    names(temp_data)[1] <- "group" # messy as temp_cut is a df
    temp_data <- aggregate(temp_data$value,by=list(temp_data$group),FUN=median,na.rm=T)
    names(temp_data)[1] <- "group"
    temp_data$x <- temp_data$x/max(temp_data$x)
    temp_output <- data.frame(group=1:bins,value=c(0))
    for(i in 1:bins){
      tryCatch(
        if(i==temp_data[temp_data$group==i,1]){
          temp_output[i,2] <- temp_data[temp_data$group==i,2]
        },error=function(e){})
    }
    temp_output$datalocations <- seq(0,1,length=bins)
    middle <- c(" \\spark",
                paste0("   ",temp_output$datalocations,"   ",temp_output$value)
    )
  }
################### OUTPUT
  opening <- paste0("\\newcommand{\\spark",
                    name,"}{\\begin{sparkline}{",
                    width,"}")
  close <- "/ \\end{sparkline}}"
  output <- c(opening,middle,close)
  cat(output,sep="\n")
}
{% endhighlight %}
