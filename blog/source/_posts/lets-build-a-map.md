---
layout: post
title: Let's build a map!
authorId: simon_timms
date: 2014-05-09
---

If you spend any time working in oil and gas in this province then you're going to run into a situation where you need to put some stuff on a map. If you're like me that involves complaining about it on twitter

> I don't know how I get myself into working with GIS data. I seriously have no idea.
> 
> "” Simon Timms (@stimms) [April 28, 2014](https://twitter.com/stimms/status/460864099862065152)

<script async="" charset="utf-8" src="//platform.twitter.com/widgets.js"></script>

The problem is that GIS stuff is way harder than it looks on the surface. Maybe not timezone hard but still really hard. Most of it comes from the fact that we live on some sort of roughly spherical thing. If we live in flatland mapping would be trivial. As it stands we need to use crazy projections to map a 3D world onto a 2D piece of paper

https://www.youtube.com/watch?v=n8zBC2dvERM

There are literally hundreds of projections out there which stress different things. Add to that a variety of coordinate systems which can be layered on top of it. There is the latitude/longitude system with which we're all familiar but there are also a bunch of others. In Western Canadathe important one is the Dominion Land Survey(well most of Western Canada, we'll get to that). The Dominion Land Survey was actually a series of surveys starting as far back as the 1870s. Bands of bearded men (there may have been bearded women too, everybody back then had beards) traveled around Canada plunking down lines to divide the land into 1sq mile sections called, well, sections. Why miles? Because of some guys called<span style="color:#252525;">J. S. Dennis and William McDougall figured that a lot of people would be coming up from the states and would better understand miles. Thanks for screwing us over, again, with your stupid outdated measurement system, United States.</span>

Anyway you can read a ton more about the system over at[wikipedia](http://en.wikipedia.org/wiki/Dominion_Land_Survey). The important thing to know is that Western Canada is divided into 6 mile by 6 mile blocks know as townships and that these are divided into 36 sections and each section is divided into 4 quarter sections. Sections can also be divided into 16 legal subdivisions commonly known as LSDs. The LSDsare numbered in the stupidest way possible starting in the bottom right corner and counting up by going left and then up then pretending that we're a snake and just flipping back and forward

<table class="wikitable" style="color:black;"><tbody><tr class="center"><td>13</td><td>14</td><td>15</td><td>16</td></tr><tr class="center"><td>12</td><td>11</td><td>10</td><td>9</td></tr><tr class="center"><td>5</td><td>6</td><td>7</td><td>8</td></tr><tr class="center"><td>4</td><td>3</td><td>2</td><td>1</td></tr></tbody></table>You might has well have numbered this things completely randomly as far as I'm concerned

<table class="wikitable" style="color:black;"><tbody><tr class="center"><td>1</td><td>7</td><td>27</td><td>cranberry</td></tr><tr class="center"><td>12</td><td>6</td><td>also 6</td><td>9</td></tr><tr class="center"><td>5</td><td>6</td><td>7</td><td>[![coke](http://stimms.files.wordpress.com/2014/05/coke.png)](http://stimms.files.wordpress.com/2014/05/coke.png)</td></tr><tr class="center"><td>4</td><td>33</td><td>null</td><td>1</td></tr></tbody></table>I'm told that this all makes sense if you have some background in cartography.

<div class="wp-caption aligncenter" id="attachment_3340" style="width: 760px">[![How I picture the average cartographer ](http://stimms.files.wordpress.com/2014/05/average.jpg)](http://stimms.files.wordpress.com/2014/05/average.jpg)How I picture the average cartographer

</div>The point of all of this is that LSDs are super important in the oil and gas industry because that is how you lease land. The result is that people want to see LSDs on their maps and, as seems to frequently happen, I got the task of building a map. This post is about how to get LSDs onto a map and show it to your clients who aren't asses. Mostly.

The company for which I'm working has most of its interests in Saskatchewan, Alberta and BC. I started with Saskatchewanas I figured that I might as well get into thinking like a cartographer and working right to left straight off the bat(I hear that cartographers from Arabic countries work from left to right to maximize the confusion). The first step was to find some LSD data. Saskatchewan have an open data portal at [http://opendatask.ca/data/](http://opendatask.ca/data/)which contains a link for LSD data. What you want in particular is the[SaskGrid2012](ftp://portaldata:freedata@ftp.isc.ca/PackagedData/SaskGrid2012/SaskGrid2012.zip). This file contains a lot of stuff but the four things you want are the various high level map structures: township, section, quarter section and legal sub division. We probably don't need quarter section as once we get to that level most people are interested in LSDs.[![grids](http://stimms.files.wordpress.com/2014/05/grids.png)](http://stimms.files.wordpress.com/2014/05/grids.png) Inside one of these zip files are a number of files which, as it turns out, are Esri shape files. Esri is a piece of GIS software. It is expensive. However there is a free alternative which has all the functionality we need along with 9000 pieces of functionality we don't:[QGIS](http://www.qgis.org/en/site/). If you download and install this software it will let you take a look at the shape files. You can add it by clicking on "Add Vector Layer" then pointing it at the .shp file.[![add layer](http://stimms.files.wordpress.com/2014/05/add-layer.png)](http://stimms.files.wordpress.com/2014/05/add-layer.png)If you load the township file it will get you something which looks like very much likeSaskatchewan. What's more is that if you click on the little identify featuretool then on the map it will tell you the name of that township. Awesome!

To give you an idea of how many of these features there are here is what the townships look like:[![sask](http://stimms.files.wordpress.com/2014/05/sask.png)](http://stimms.files.wordpress.com/2014/05/sask.png)



For each township there are 36 section (6Ã—6) and for each section there are 16 (4Ã—4) LSDs. So for Saskatchewan there are something like 7000 townships, 250 000 section and a mind blowing 4 million LSDs. Quite a bit of data.

So now we have a map. But it only works inside of QGIS and I'm sure not going to go around supporting that. It would be really nice if this layer was available on a Google maps like thing.


# Leaflet

Leaflet.js is a nifty library for manipulating maps. It can use any number of map backends but I usedOpenStreeMap because it is awesome. Like really awesome. Start a new ASP.net project and then go and grab the latest leaflet from their[site](http://leafletjs.com/download.html). Leaflet is in nuget but it is an older version which hasn't been updated for 6 months or so. Add the files to the site bundles in BundleConfig.cs. I also included my site files in this bundle for convenience

<script src='https://gist.github.com/stimms/8d3a2b7c6fc6062e2f48.js'></script>

I created a typescript file for the home page based on the example on the lealflet home page

<script src='https://gist.github.com/stimms/cb0c6b1375d79e0aa2e8.js'></script>

Once I'd added a div in my Index.cshtml under the home controller and constructed the map I ended up with a map centered on, roughly, the border between Alberta and Saskatchewan.[![map1](http://stimms.files.wordpress.com/2014/05/map1.png)](http://stimms.files.wordpress.com/2014/05/map1.png)

<div style="background:#b3b3b3;">**Note:**We're using the open street map tiles directly from their title server in this example. This is frowned upon as it cost the project money. There are a number of proxy services you can use instead or write your own. Be a good citizen and cache the tiles so that the project can spend money on something else.</div>Now to get our grid lines onto the map. To start I added a simple polygon

<script src='https://gist.github.com/stimms/d428182b8aeb50428048.js'></script>#file-index-ts-L14-L18

This builds a map which includes a nifty redbox.[![map2](http://stimms.files.wordpress.com/2014/05/map2.png)](http://stimms.files.wordpress.com/2014/05/map2.png)This basically proves that we can draw out LSDs as needed. So the next thing is going to be combining the shape file data we had above and the map we have from OpenStreetMap.


# Exporting KML Files

I went down a few blind alley with this one before coming up with, what I think, is the best option. I decided to exploit the power of the geometry types in SQL Server to find any LSDs inside a bounding box. To display all the LSDs or even the townships at a low zoom level is messy. It covers up the map and is too detailed for that level. As such only showing a few at a time is a good idea. To figure out which ones to show requires putting in place a filter to show only LSDs within a bounding box, the bounding box described by the map at a high zoom level. Getting the data into SQL server is a two step process: this first is to create KML files and the second is to load them into SQL server. For each of township, section and LSD I loaded them into QGIS. Then right clicked on the layer and hit Save as. In that dialog I selected KML as the format and a file into which to save the layer.[![save](http://stimms.files.wordpress.com/2014/05/save.png)](http://stimms.files.wordpress.com/2014/05/save.png)This generates a pretty sizable export file, well over 4GB for the LSDs. Don't worry though because these crummy things are XML so most of that will disappear when loaded into SQL server. If you want you can attempt to simplify the geometry in QGIS which will reduce the export size at the cost of fidelity. The file size does, however, pose a bit of a problem for our import tools as they use a DOM Parser for reading KML instead of a SAX parser. If I were going to make a living at manipulating maps this would be a place I expended some effort to correct.


# Importing KML Files into SQL Server

I hunted around and found a tool called[KML2SQL](https://github.com/Pharylon/KML2SQL)to import KML files into SQL server. It had a few issues with it so I[forked it](https://github.com/Pacesetter/KML2SQL)and made some updates. If you're going to make use of the tool then you're going to be better off using my fork at the current time. However if my pull requests are merged then the master repo may be more healthy.

As I mentioned the KML files are too large to be consumed by the import tool. To solve this I wrote a quick KML splitter application, which splits the KML files into 500 feature blocks. I've included the source on[github](https://github.com/stimms/LSDMap/tree/master/KMLSplitter). It isn't pretty but it gets the job done.

[![kml2sql](http://stimms.files.wordpress.com/2014/05/kml2sql.png)](http://stimms.files.wordpress.com/2014/05/kml2sql.png)

The KML2SQL tool will dynamically build tables with the correct columns in them so that's great. All I did was plop in my SQL server credentials and point the tool at the directory containing the output files from the splitter. I left this to churn for a few minutes. A few hours for the LSD divisions. I did see some import errors related to open polygons which I generally ignored. It is something I'll have to come back to in a while but they were perhaps 1% of the imports. SQL server wants the polygons to have the same star coordinate as they have end coordinate, which is only reasonable. The data from the government doesn't quite have that but for free data you can't complain too much.


# Querying the Data

Now that we have the data in the database the next step is to get it out onto the map. To start we need to have the map ask for some data when it is resized or panned. I hooked into the zoomend and dragend events in Leaflet. I found that it made sense to display the townships starting at zoom level 10 and at higher zoom levels show more detailed data such as sections or LSDs.

I threw together a Web API controller to do the work of querying the database. Doing geospatial queries in SQL server is a bit more complicated that I would like but complex types in relational databases always are. I don't know, relational databases, man.

I'm a big fan of the light weight ORM Dapper so I installed that and stole a bit of code for doing spatial queries from a posting by Sam Saffron on StackOverflow. I had to modify it a bit and ended up with:

<script src='https://gist.github.com/stimms/81d050afee1d7737bd28.js'></script>

This can be used to select the intersecting polygons by doing

<script src='https://gist.github.com/stimms/203d48c7d32fd62f7f98.js'></script>

STIntersects will check the number of intersection points between the given polygon and the ones in the database. If there are any intersections then we have a match and we add that to the result set. Actually building the search area can be a pain as you actually build a geometry like so

<script src='https://gist.github.com/stimms/d0ac4cbf71897c7e021e.js'></script>

This provides a set of data to return to the client.


# Plotting the Data

We're almost there, folks, thanks for staying with it. The last step is to get the returned data plotted on the map. This is simply done by adding the polygons to a multipolygon layer

<script src='https://gist.github.com/stimms/2598c6daed5ef4e7b427.js'></script>

That's it folks! You now have a map which looks like

[![map3](http://stimms.files.wordpress.com/2014/05/map3.png)](http://stimms.files.wordpress.com/2014/05/map3.png)The labels are a bit jaggedy right now as I'm just using the envelope center to calculate the position of the label. It doesn't take a whole lot to screw that up. Putting them in the top left corner helps with that a lot

[![map4](http://stimms.files.wordpress.com/2014/05/map4.png)](http://stimms.files.wordpress.com/2014/05/map4.png)



P.S. I promised I would get back to BC's system. They don't use the DLS they use the[National Topographic System](http://en.wikipedia.org/wiki/National_Topographic_System)



