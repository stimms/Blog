---
layout: post
title: Open Data -  Countdown to Open Data Day 2
authorId: simon_timms
date: 2013-02-20
---

Open Data day draws ever closer, like Alpha-Centauri would if we lived in a closed universe during it contraction phase. Today we will be looking at some of the data the City of Calgary produces. In particular the geographic data about the city.

I should pause here and say that I really don't know what I'm doing with geographic data. I am not a GIS developer so there are very likely better ways to process this data and awesome refinements that I don't know about. I can say that the process I followed here does work so that's a good chunk of the battle.

A common topic of conversation in Calgary is "Where do you live?". The answer is typically the name of acommunityto which I nod knowingly even though I have no idea which community is which. One of the data sets from the city is a map of the city divided into [communityboundaries](https://cityonline.calgary.ca/Pages/Product.aspx?category=PDCAdministrativeBoundaries&cat=CITYonlineDefault&id=PDC0-99999-99999-00005-P). I wanted a quick way to look up where communities are. To start I downloaded the shape files which came as a zip. Unzipping these got me

- CALGIS.ADM_COMMUNITY_DISTRICT.dbf
- CALGIS.ADM_COMMUNITY_DISTRICT.prj
- CALGIS.ADM_COMMUNITY_DISTRICT.shp
- CALGIS.ADM_COMMUNITY_DISTRICT.shx

It is my understanding that these are ESRI files. I was most interested in the shp file because I read that it could be transformed into a format known a [GeoJSON](http://www.geojson.org)which can be read by D3.js. To do this I followed the instruction on [Jim Vallandingham's site](http://vallandingham.me/shapefile_to_geojson.html). I used a tool called ogr2ogr

`ogr2ogr -f geoJSON output.json CALGIS.ADM_COMMUNITY_DISTRICT.shp `

However this didn't work properly and when put into the web page produced a giant mess which looked a lot like

<div class="wp-caption aligncenter" style="width: 610px">![](http://i.stack.imgur.com/xYfuN.png)Random Mess

</div>I know a lot of people don't like the layout of roads in Calgary but this seemed ridiculous.

I eventually found out that the shp file I had was in a different coordinate system from what D3.js was expecting. I should really go into more detail about that but not being a GIS guy I don't understand it very well.Fortunatelysome nice people on [StackOverflow ](http://stackoverflow.com/questions/14963005/draw-map-from-geojson-in-d3-js)came to my rescue and suggested that I instead use

`ogr2ogr -f geoJSON output.json <strong>-t_srs "WGS84"</strong> CALGIS.ADM_COMMUNITY_DISTRICT.shp `

This instructs ogr2ogr that the input is in World Geodetic System 1984.

Again leaning on work by Jim Vallandingham I used d3.js to build the map in an SVG.

<script src='https://gist.github.com/stimms/4999264.js'></script>

The most confusing line in there is the section with scaling, rotating and translating the map. If these values seem random it is because they are. I spent at least an hour twiddling wit them to get them more or less correct. If you look at the final product you'll notice it isn't quite straight. I don't care. Everything else is fairly easy to understand and should look a lot like the d3.js we've done before.

Coupled with a little bit of jquery for selecting matching elements we can build this [very simple map](http://bl.ocks.org/stimms/raw/4998617/). It will take some time to load as the GeoJSON is 3 meg in size. This can probably be reduced through simplifying the shape files and reducing the number of properties in the JSON. I also think this JSON is probably very compressible so delivering it over a bzip stream will be more efficient.

The full code is available on github at[https://github.com/stimms/VectorMapOfCalgary](https://github.com/stimms/VectorMapOfCalgary)





