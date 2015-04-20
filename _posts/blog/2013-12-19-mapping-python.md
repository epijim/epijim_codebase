---
layout: media
title: "Mapping my google location data"
categories: blog
tags: [r, python, geocoding]
excerpt: "Mapping my google location data with python."
ads: false
toc: false
comments: true
share: true
image:
  feature: mappython_feature.png
  teaser: mappython_thumb.png
 # creditlink: http://herneman.blogspot.co.uk
---

The plot.

<figure>
	<a href="/images/mappython_1_england.png"><img src="/images/mappython_1_england.png"></a>
	<figcaption>My movements around the UK in the last two years (when I’ve had my phone on…).</figcaption>
</figure>

Two github users[^1] have put together two awesome scripts
 (I’ve merged the two and made some small edits at the bottom of the page). 
 One can convert json location data into kml data, and the other can plot kml over an image. 
 A few days after I posted this the first time, a fellow Jesuan did in R. I'd recommend using his 
 code[^2], as it's a lot easier to do it in R. 
 
I find most of the learning comes from trying to add to the 
 scripts, rather than looking at how they work, which is why I am doing this in R. 
 Because the two scripts almost did 
 exactly what I wanted I didn’t really pick much up from doing this (although that also 
 meant it only took about 15 mins to make!).
 
[Google takeout](http://en.wikipedia.org/wiki/Google_Takeout) lets you take out all the data google holds you.
 That includes the location data it’s pulled on me from my phone 
 (I have the google now app installed). It’s very very patchy data, but in general shows 
 where I’ve been. The script, written by Snowdonjames, lets you also output csv’s, 
 which I’ll probably use in R.

The maps use the *PIL* library – which is a annoying to install. As I didn’t read the
 documentation for *PIL*, I had to re-install it a few times. Thankfully I used PIP, 
 so I was able to quickly uninstall it each time I realised PIL depended on something 
 I didn’t have installed (I also had to put a symlink in so PIL 
 could find the jpeg encoder).

You have to chuck in the image yourself (as R can pull from google maps, it will be even easier there).
 The England map is already in the [original repo](https://github.com/snowdonjames/LatitudeHistoryPlotter). For the other maps I used a site that looked American 
 Govermnent-y – but now I can’t find it. It lets you draw a square and gives the co-ordinates. 
 The actual google data is quite shoddy. Or rather, it only logs a position if I have the google 
 now app (or before 2012, the discontinued latitude app) open. So there are big gaps, often days 
 long, where there is no data at all. The script joins up each location with the last one, 
 which could be minutes or days ago. The script allows you to set a cut off, where it won’t 
 draw a line between two location points if the distance between them is too high. 
 Below I made a script to cycle through 5 to 100, then jumped up to 1000.

<figure>
	<a href="/images/mappython_2_gif.gif"><img src="/images/mappython_2_gif.gif"></a>
	<figcaption>A gif of different distance cut offs.</figcaption>
</figure>

<figure>
	<a href="/images/mappython_3_at70.png"><img src="/images/mappython_3_at70.png"></a>
	<figcaption>My movements round Cam, with the cut off set to 70. It’s easy to spot home/college in the centre, a trip to Grantchester and the hospital/department south of Cam. Not sure why there is so much data from around the train station though, unless it’s because I pass to a new cell receiver (which is one of the prompts iOS uses to know if you’ve moved location without running the battery down by leaving the GPS on 24/7).</figcaption>
</figure>
 
 Below I’ve set the cut off very high (10,000), so those straight lines are flights.
  Outside of the UK I think this only shows airports, friends houses, or conferences 
  where I’ve use free wifi, as I don’t use mobile data abroad. Still pretty impressive 
  that, even if it’s not exact, google has a pretty good lock on where I am without me 
  actively telling them. And combined with having my gmail account, google drive, and my 
  search and browsing history they have a pretty impressive dataset on me.
  
<figure>
	<a href="/images/mappython_4_europe.png"><img src="/images/mappython_4_europe.png"></a>
	<figcaption>Travel around Europe – or rather trips I made that google’s servers have logged. This map also is a little offset, which I should of fixed with the nudge parameters – but I was trying to get this done in <30 mins, as I know I can make it much prettier in R.</figcaption>
</figure>

If you wanna fork/contribute to the original scripts by Snowdonjames and Scarygami,
 the one to format the data is here, and the one to make the plot is here.

[^1]: [Snowdonjames](https://github.com/snowdonjames) and [Scarygami](https://github.com/scarygami)
[^2]: His blog is [here](http://seasci.wordpress.com/2013/12/20/it-knows-where-i-live/).

{% highlight python %}
#I've merged code from two different sources, originally published by github users snowdonjames and Scarygami
 
#Import JSON, spit out KML
#usage.. location_history_json_converter.py input output [-h] [-f {kml,json,csv,js,gpx,gpxtracks}] [-v]
 
from __future__ import division
 
import sys
import json
import math
from argparse import ArgumentParser
from datetime import datetime
 
 
def main(argv):
    arg_parser = ArgumentParser()
    arg_parser.add_argument("input", help="Input File (JSON)")
    arg_parser.add_argument("output", help="Output File (will be overwritten!)")
    arg_parser.add_argument("-f", "--format", choices=["kml", "json", "csv", "js", "gpx", "gpxtracks"], default="kml", help="Format of the output")
    arg_parser.add_argument("-v", "--variable", default="locationJsonData", help="Variable name to be used for js output")
    args = arg_parser.parse_args()
    if args.input == args.output:
        arg_parser.error("Input and output have to be different files")
        return
 
    try:
        json_data = open(args.input).read()
    except:
        print("Error opening input file")
        return
 
    try:
        data = json.loads(json_data)
    except:
        print("Error decoding json")
        return
 
    if "locations" in data and len(data["locations"]) > 0:
        try:
            f_out = open(args.output, "w")
        except:
            print("Error creating output file for writing")
            return
 
        items = data["locations"]
 
        if args.format == "json" or args.format == "js":
            if args.format == "js":
                f_out.write("window.%s = " % args.variable)
 
            f_out.write("{\n")
            f_out.write("  \"data\": {\n")
            f_out.write("    \"items\": [\n")
            first = True
 
            for item in items:
                if first:
                    first = False
                else:
                    f_out.write(",\n")
                f_out.write("      {\n")
                f_out.write("         \"timestampMs\": %s,\n" % item["timestampMs"])
                f_out.write("         \"latitude\": %s,\n" % (item["latitudeE7"] / 10000000))
                f_out.write("         \"longitude\": %s\n" % (item["longitudeE7"] / 10000000))
                f_out.write("      }")
            f_out.write("\n    ]\n")
            f_out.write("  }\n}")
            if args.format == "js":
                f_out.write(";")
 
        if args.format == "csv":
            f_out.write("Time,Location\n")
            for item in items:
                f_out.write(datetime.fromtimestamp(int(item["timestampMs"]) / 1000).strftime("%Y-%m-%d %H:%M:%S"))
                f_out.write(",")
                f_out.write("%s %s\n" % (item["latitudeE7"] / 10000000, item["longitudeE7"] / 10000000))
 
        if args.format == "kml":
            f_out.write("<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n")
            f_out.write("<kml xmlns=\"http://www.opengis.net/kml/2.2\">\n")
            f_out.write("  <Document>\n")
            f_out.write("    <name>Location History</name>\n")
            for item in items:
                f_out.write("    <Placemark>\n")
                # Order of these tags is important to make valid KML: TimeStamp, ExtendedData, then Point
                f_out.write("      <TimeStamp><when>")
                f_out.write(datetime.fromtimestamp(int(item["timestampMs"]) / 1000).strftime("%Y-%m-%dT%H:%M:%SZ"))
                f_out.write("</when></TimeStamp>\n")
                if "accuracy" in item or "speed" in item or "altitude" in item:
                    f_out.write("      <ExtendedData>\n")
                    if "accuracy" in item:
                        f_out.write("        <Data name=\"accuracy\">\n")
                        f_out.write("          <value>%d</value>\n" % item["accuracy"])
                        f_out.write("        </Data>\n")
                    if "speed" in item:
                        f_out.write("        <Data name=\"speed\">\n")
                        f_out.write("          <value>%d</value>\n" % item["speed"])
                        f_out.write("        </Data>\n")
                    if "altitude" in item:
                        f_out.write("        <Data name=\"altitude\">\n")
                        f_out.write("          <value>%d</value>\n" % item["altitude"])
                        f_out.write("        </Data>\n")
                    f_out.write("      </ExtendedData>\n")
                f_out.write("      <Point><coordinates>%s,%s</coordinates></Point>\n" % (item["longitudeE7"] / 10000000, item["latitudeE7"] / 10000000))
                f_out.write("    </Placemark>\n")
            f_out.write("  </Document>\n</kml>\n")
 
        if args.format == "gpx" or args.format == "gpxtracks":
            f_out.write("<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n")
            f_out.write("<gpx xmlns=\"http://www.topografix.com/GPX/1/1\" version=\"1.1\" creator=\"Google Latitude JSON Converter\" xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" xsi:schemaLocation=\"http://www.topografix.com/GPX/1/1 http://www.topografix.com/GPX/1/1/gpx.xsd\">\n")
            f_out.write("  <metadata>\n")
            f_out.write("    <name>Location History</name>\n")
            f_out.write("  </metadata>\n")
            if args.format == "gpx":
                for item in items:
                    f_out.write("  <wpt lat=\"%s\" lon=\"%s\">\n"  % (item["latitudeE7"] / 10000000, item["longitudeE7"] / 10000000))
                    if "altitude" in item:
                        f_out.write("    <ele>%d</ele>\n" % item["altitude"])
                    f_out.write("    <time>%s</time>\n" % str(datetime.fromtimestamp(int(item["timestampMs"]) / 1000).strftime("%Y-%m-%dT%H:%M:%SZ")))
                    f_out.write("    <desc>%s" % datetime.fromtimestamp(int(item["timestampMs"]) / 1000).strftime("%Y-%m-%d %H:%M:%S"))
                    if "accuracy" in item or "speed" in item:
                        f_out.write(" (")
                        if "accuracy" in item:
                            f_out.write("Accuracy: %d" % item["accuracy"])
                        if "accuracy" in item and "speed" in item:
                            f_out.write(", ")
                        if "speed" in item:
                            f_out.write("Speed:%d" % item["speed"])
                        f_out.write(")")
                    f_out.write("</desc>\n")
                    f_out.write("  </wpt>\n")
            if args.format == "gpxtracks":
                f_out.write("  <trk>\n")
                f_out.write("    <trkseg>\n")
                lastloc = None
                # The deltas below assume input is in reverse chronological order.  If it's not, uncomment this:
                # items = sorted(data["data"]["items"], key=lambda x: x['timestampMs'], reverse=True)
                for item in items:
                    if lastloc:
                        timedelta = -((int(item['timestampMs']) - int(lastloc['timestampMs'])) / 1000 / 60)
                        distancedelta = getDistanceFromLatLonInKm(item['latitudeE7'] / 10000000, item['longitudeE7'] / 10000000, lastloc['latitudeE7'] / 10000000, lastloc['longitudeE7'] / 10000000)
                        if timedelta > 10 or distancedelta > 40:
                            # No points for 10 minutes or 40km in under 10m? Start a new track.
                            f_out.write("    </trkseg>\n")
                            f_out.write("  </trk>\n")
                            f_out.write("  <trk>\n")
                            f_out.write("    <trkseg>\n")
                    f_out.write("      <trkpt lat=\"%s\" lon=\"%s\">\n" % (item["latitudeE7"] / 10000000, item["longitudeE7"] / 10000000))
                    if "altitude" in item:
                        f_out.write("        <ele>%d</ele>\n" % item["altitude"])
                    f_out.write("        <time>%s</time>\n" % str(datetime.fromtimestamp(int(item["timestampMs"]) / 1000).strftime("%Y-%m-%dT%H:%M:%SZ")))
                    if "accuracy" in item or "speed" in item:
                        f_out.write("        <desc>\n")
                        if "accuracy" in item:
                            f_out.write("          Accuracy: %d\n" % item["accuracy"])
                        if "speed" in item:
                            f_out.write("          Speed:%d\n" % item["speed"])
                        f_out.write("        </desc>\n")
                    f_out.write("      </trkpt>\n")
                    lastloc = item
                f_out.write("    </trkseg>\n")
                f_out.write("  </trk>\n")
            f_out.write("</gpx>\n")
 
        f_out.close()
 
    else:
        print("No data found in json")
        return
 
 
# Haversine formula
def getDistanceFromLatLonInKm(lat1,lon1,lat2,lon2):
    R = 6371 # Radius of the earth in km
    dlat = deg2rad(lat2-lat1)
    dlon = deg2rad(lon2-lon1)
    a = math.sin(dlat/2) * math.sin(dlat/2) + \
    math.cos(deg2rad(lat1)) * math.cos(deg2rad(lat2)) * \
    math.sin(dlon/2) * math.sin(dlon/2)
    c = 2 * math.atan2(math.sqrt(a), math.sqrt(1-a))
    d = R * c # Distance in km
    return d
 
 
def deg2rad(deg):
    return deg * (math.pi/180)
 
 
if __name__ == "__main__":
    sys.exit(main(sys.argv))
 
# Load KML, picture and ImageData.csv and print picture
# Need a csv with the following. Nudge is for correcting lat and long.
# IMAGE.jpg,TOPOFBOXLAT, BOTTOMBOXLAT, LEFTBOXLONG, RIGHTBOXLONG,NUDGELAT,NUDGELONG
 
 
import os, time, math
from datetime import datetime
from time import mktime
import xml.etree.ElementTree as ET
from PIL import Image, ImageDraw
 
def GetKmlFiles():
    """Locates and reads local .kml files, returns a list of kml dictionary data""" 
    KmlData = []
    for dirname, dirnames, filenames in os.walk('.'):
        for filename in filenames:
            sp = filename.split('.')
            if  sp[len(sp)-1]== "kml": #locate kml files
                print "Reading kml file " + filename
                KmlData.append(ReadKmlFile(dirname, filename))
    return KmlData
 
def ReadKmlFile(dirname, filename):
    """Parses a single kml file, returns a dict of format {time: [lat, long]}"""
    KmlData = {}
    kmltime = datetime.time
    latlist = []
    longlist = []
    timelist = []
    cnt =0
    f = open(filename)
    line = f.readline()
    while line:
        if 'when' in line:
            timelist.append(time.strptime(ET.fromstring(line)[0].text,"%Y-%m-%dT%H:%M:%SZ"))
        if 'coordinates' in line:
            latlist.append(float(ET.fromstring(line)[0].text.split(',')[0]))
            longlist.append(float(ET.fromstring(line)[0].text.split(',')[1]))
            cnt+=1
            if cnt % 5000 ==0:
                print "Parsing " + filename + ": points found: " + str(cnt)
        line = f.readline()
    f.close()
    return [latlist, longlist, timelist]
 
def DrawMapData(KmlData,InputImage, OutputImage, itop, ibottom, ileft, iright,xnudge,ynudge):
    """Draws kml line data on top of the specified image"""
    im = Image.open(InputImage)
    draw = ImageDraw.Draw(im)
    cnt =0
    for KmlD in KmlData:
        for d in range(len(KmlD[0])-1):
            #Get points x and y coordinates and draw line
            x1=(LongToX(KmlD[0][d],ileft,iright,im.size[0]))+xnudge
            y1=(LatToY(KmlD[1][d],itop,ibottom,im.size[1]))+ynudge
            x2=(LongToX(KmlD[0][d+1],ileft,iright,im.size[0]))+xnudge
            y2=(LatToY(KmlD[1][d+1],itop,ibottom,im.size[1]))+ynudge
            if(EuclidDistance(x1,y1,x2,y2) < 10000):
                #setting this around 80 works okay. Attempts to remove some noise  
                draw.line((x1,y1, x2,y2), fill=255)
            cnt+=1
            if cnt % 10000 ==0:
                print "Drawing point number " + str(cnt)
    im.save(OutputImage)
        
def LongToX(InputLong, LeftLong, RightLong, ImWidth):
    """Converts a longitude value in to an x coordinate"""
    return ScalingFunc(InputLong+360, LeftLong+360, RightLong+360, ImWidth);
 
def LatToY(InputLat, TopLat, BottomLat, ImHeight):
    """Converts a latitude value in to a y coordinate"""
    return ScalingFunc(InputLat+360, TopLat+360, BottomLat+360, ImHeight);
 
def EuclidDistance(x1, y1, x2, y2):
    """Calculates the euclidean distance between two points"""
    return math.sqrt((x1 - x2)**2+(y1 - y2)**2)
 
def ScalingFunc(inputv, minv, maxv, size):
    """Helps convert latitudes and longitudes to x and y"""
    if((float(maxv) -float(minv)) ==0):
       return 0
    return ((((float(inputv) - float(minv)) / (float(maxv) -float(minv))) * float(size)));
 
def ParseImageFile():
    """Reads SatelliteImageData.csv containing:
    <File name of image to draw data on>,
    <image top latitude>,
    <image bottom lattitude>,
    <image left longitude>,
    <image right longitude>,
    (optional) <x value nudge>,
    (optional) <y value nudge>"""
    with open('ImageData.csv', 'r') as f:
        read_data = f.read().split(',')
    while 5 <= len(read_data) < 7:
        read_data.append(0)
    ReturnData = [0]*7
    ReturnData[0]=read_data[0]
    for i in range(1,7):
        ReturnData[i] = float(read_data[i])
    return ReturnData
 
if __name__ == "__main__":
    ImageData = ParseImageFile()
    DrawMapData(GetKmlFiles(),ImageData[0], "LatitudeData.png", ImageData[1], ImageData[2], ImageData[3], ImageData[4],ImageData[5],ImageData[6])
{% endhighlight %}