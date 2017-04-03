---
layout: post
title: Shrink geoJSON
authorId: simon_timms
date: 2013-04-05
---

A while back I presented [a d3.js based solution for showing a map of Calgary](http://blog.simontimms.com/2013/02/20/open-data-countdown-to-open-data-day-2/ "Open Data: Countdown to Open Data Day2") based on the open data provided by the city. One of the issues with the map was that the data was huge, over 3 meg. Even with goodcompressionthis was a lot of data for a simple map. Today I went hunting to find a way to shrink that data.

I started in QGIS which is a great mapping tool. In there I loaded the same shape file I downloaded from the City of Calgary's open data website some months back. In that tool I went to the Vector menu then Geometry Tools and selected Simplify geometries. That presents you with a tolerance to use to remove some of the lines. See the map is a collection of polygons, but the ones the city gives us are super in detail. For a zoomed out map such as the one we're creating there is no need to have that much detail. The lines can be smoothed

<div class="wp-caption aligncenter" id="attachment_2554" style="width: 682px">[![Smoothing a line](http://stimms.files.wordpress.com/2013/04/smooth.jpg)](http://stimms.files.wordpress.com/2013/04/smooth.jpg)Smoothing a line

</div>if we repeat thisprocessmany thousands of times the geometry becomes simpler. I played around with various values for this number and finally settled on 20 as a reasonabletolerance.

QGIS then shows map with the verticies removed highlighted with a red X. I was alarmed by this at first but don't worry, the export won't contain these.

I then right clicked on the layer and saved it as a shape file. Next I dropped back to my old friend ogr2ogr to transform the shape file into geojson.

ogr2ogr -f geoJSON data.geojson -t_srs "WGS84" simplified.shp

The result was a JSON file which clocked in at 379K but looked indistinguishable from the original. Not too bad, about an 80% reduction.

I opened up the file in my favorite text editor and found that there was a lot of extra data in the file which wasn't needed. For instance the record for the community of Forest Lawn contained

"type": "Feature", "properties": { "GEODB_OID": 1069.0, "NAME0": "Industrial", "OBJECTID": 1069.0, "CLASS": "Industrial", "CLASS_CODE": 2.0, "COMM_CODE": "FLI", "NAME": "FOREST LAWN INDUSTRIAL", "SECTOR": "EAST", "SRG": "N/A", "STRUCTURE": "EMPLOYMENT", "GLOBALID": "{0869B38C-E600-11DE-8601-0014C258E143}", "GUID": null, "SHAPE_AREA": 1538900.276872559916228, "SHAPE_LEN": 7472.282043282280029 }, "geometry": { "type": "Polygon", "coordinates": [ [ [ -113.947910445225986, 51.03988003011284, 0.0 ], [ -113.947754144125611, 51.03256157080034, 0.0 ], [ -113.956919846879231, 51.032555219649311, 0.0 ], [ -113.956927379966558, 51.018183034486498, 0.0 ], [ -113.957357020247059, 51.018182170100317, 0.0 ], [ -113.959372692834563, 51.018816832914304, 0.0 ], [ -113.959355756999315, 51.026144523792645, 0.0 ], [ -113.961457605689915, 51.026244276425345, 0.0 ], [ -113.964358232775155, 51.027492581858972, 0.0 ], [ -113.964626177178133, 51.029176808875143, 0.0 ], [ -113.968142377996443, 51.029177287922145, 0.0 ], [ -113.964018694707519, 51.031779244814821, 0.0 ], [ -113.965325119316319, 51.032649604496967, 0.0 ], [ -113.965329328092139, 51.039853734199696, 0.0 ], [ -113.947910445225986, 51.03988003011284, 0.0 ] ] ] } }

Most of the properties are surplus to our requirements. I ran a series of regex replaces on the file

s/, "SEC.*"ge/}, "ge/g s/"GEODB.*"NA/"NA/g s/,s//g s/]s//g s/[s//g s/{s//g s/}s//g

The first two strip the extra properties and the rest strip extra spaces.  
 This striped a record down to looking like(I added back some new lines for formatting sake)

{"type":"Feature","properties":{"NAME":"FOREST LAWN INDUSTRIAL"},"geometry": {"type":"Polygon","coordinates":[[[-113.947910445225986,51.03988003011284,0.0], [-113.947754144125611,51.03256157080034,0.0],[-113.956919846879231, 51.032555219649311,0.0],[-113.956927379966558,51.018183034486498,0.0], [-113.957357020247059,51.018182170100317,0.0],[-113.959372692834563, 51.018816832914304,0.0],[-113.959355756999315,51.026144523792645,0.0], [-113.961457605689915,51.026244276425345,0.0],[-113.964358232775155, 51.027492581858972,0.0],[-113.964626177178133,51.029176808875143,0.0], [-113.968142377996443,51.029177287922145,0.0],[-113.964018694707519, 51.031779244814821,0.0],[-113.965325119316319,51.032649604496967,0.0], [-113.965329328092139,51.039853734199696,0.0],[-113.947910445225986, 51.03988003011284,0.0]]]}}

The whole file now clocks in at 253K. With gzip compression this file is now only 75K which is very reasonable, and something like 2% of the size of the file we had originally. Result! You can see the new, faster loading, map [here](http://bl.ocks.org/stimms/raw/5321404/7f57097854203cf16cb29b856b981057d39ebebe/)



