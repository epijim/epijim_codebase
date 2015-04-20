---
layout: article
title: "Pulling an XML file off the internet"
excerpt: "Getting an online XML file into a dataframe."
categories: code
tags: [r, xml]
comments: true
share: true
ads: true
image:
  feature:
  teaser: xml_thumb.jpg
---

For my pub post, I pulled an XML file from the Food Standards Agency, and put it into
 a data frame.

This function will pull the latest file, loop through the XML, and put the data into a dataframe.
 There must be a better way to work with XML files, so this snippet is a work in progress.

{% highlight r linenos=table %}
library(XML)

# Get the data
  data <- xmlParse(
    "http://ratings.food.gov.uk/OpenDataFiles/FHRS027en-GB.xml")
  xml_data <- xmlToList(data)

# Extraction date
  extractedon <- xml_data$Header$ExtractDate
  extractedon

# Loop..
  number <- length(xml_data$EstablishmentCollection)
  data <- matrix(ncol=5,nrow=number)
for (i in 1:number){
  # Extact columns
  name <- xml_data$EstablishmentCollection[i]$EstablishmentDetail$BusinessName
  type <- xml_data$EstablishmentCollection[i]$EstablishmentDetail$BusinessType
  rating <- xml_data$EstablishmentCollection[i]$EstablishmentDetail$RatingValue
  lat <- xml_data$EstablishmentCollection[i]$EstablishmentDetail$Geocode$Latitude
  lon <- xml_data$EstablishmentCollection[i]$EstablishmentDetail$Geocode$Longitude
  # Make null NA
  lon <- replace(lon,is.null(lon),"Missing")
  lat <- replace(lat,is.null(lat),"Missing")
  data[i,] <- c(name,type,rating,
                lat,lon
                )
}

  data <- as.data.frame(data, stringsAsFactors = F)

#################################################################################
### Name variables and drop the catering companies
#################################################################################

  colnames(data) <- c("name", "type","rating","lat","lon")
  data <- subset(data, !(type=="Other catering premises"))

#################################################################################
### Format
#################################################################################

  data$rating <- as.numeric(data$rating)
  data$lat <- as.numeric(data$lat)
  data$lon <- as.numeric(data$lon)

  # Clear out some random ones - like the unrated hospital kitchen
  data <- na.omit(data)
{% endhighlight %}
