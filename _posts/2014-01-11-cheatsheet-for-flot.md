---
layout: post
title: "Cheatsheet for Flot"
categories:
- programming
tags:
- appliance
comments: true

---

$.plot() is the main function
 
	var plot = $.plot(placeholder,data,options)

Two ways to specifiy div tag

	var plot = $.plot("#placeholder",data,options)

or

	var plot = $("#placeholder").plot(data,options)

Data Format
How to format the data.
data = rawdata or multiple_rawdata or object or multiple objects
The most basic format is an array of arrays.

	var rawdata = [ [x1,y1], [x2,y2], [x3,y3], [x4,y4] ];
	var rawdata = [ [0,10], [1,11], [2,12], [3,13] ];

Multiple Series are used if you want more than one line on your graph

	multiple_rawdata = [rawdata,rawdata,rawdata];

The object looks like this:

	var object = {
    		color: color or number
    		data: rawdata
    		label: string
    		lines: specific lines options
    		bars: specific bars options
    		points: specific points options
    		xaxis: number
    		yaxis: number
    		clickable: boolean
    		hoverable: boolean
    		shadowSize: number
    		highlightColor: color or number
	}

And multiple objects look like this:

	var object = [object1,object2,object3]

99% of the time you are just using the objects to link to a label

	[ { label: "Foo", data: [ [10, 1], [17, -14], [30, 5] ] },
  	{ label: "Bar", data: [ [11, 13], [19, 11], [30, -7] ] }
	]

 Options

Example of options 

	var options = {
    	series: {
        	lines, points, bars: {
        	show: boolean
        	lineWidth: number
        	fill: boolean or number
        	fillColor: null or color/gradient
       	}
       	lines, bars: {
            	zero: boolean
        	}
        	points: {
            	radius: number
            	symbol: "circle" or function
        	}
        	bars: {
            	barWidth: number
            	align: "left", "right" or "center"
            	horizontal: boolean
        	}
        	lines: {
            	steps: boolean
        	}
        	shadowSize: number
        	highlightColor: color or number
   	}// Series Option
  	colors: [ color1, color2, ... ]
  	legend: {
      		show: boolean
      		labelFormatter: null or (fn: string, series object -> string)
      		labelBoxBorderColor: color
      		noColumns: number
      		position: "ne" or "nw" or "se" or "sw"
      		margin: number of pixels or [x margin, y margin]
      		backgroundColor: null or color
      		backgroundOpacity: number between 0 and 1
      		container: null or jQuery object/DOM element/jQuery expression
      		sorted: null/false, true, "ascending", "descending", "reverse", or a comparator
 		},
	 xaxis, yaxis: {
	      show: null or true/false
	      position: "bottom" or "top" or "left" or "right"
	      mode: null or "time" ("time" requires jquery.flot.time.js plugin)
	      timezone: null, "browser" or timezone (only makes sense for mode: "time")
	
	      color: null or color spec
	      tickColor: null or color spec
	      font: null or font spec object
	
	      min: null or number
	      max: null or number
	      autoscaleMargin: null or number
	
	      transform: null or fn: number -> number
	      inverseTransform: null or fn: number -> number
	
	      ticks: null or number or ticks array or (fn: axis -> ticks array)
	      tickSize: number or array
	      minTickSize: number or array
	      tickFormatter: (fn: number, object -> string) or string
	      tickDecimals: null or number
	
	      labelWidth: null or number
	      labelHeight: null or number
	      reserveSpace: null or true
	      tickLength: null or number
	      alignTicksWithAxis: null or number
	   }
	    grid: {
	        show: boolean
	        aboveData: boolean
	        color: color
	        backgroundColor: color/gradient or null
	        margin: number or margin object
	        labelMargin: number
	        axisMargin: number
	        markings: array of markings or (fn: axes -> array of markings)
	        borderWidth: number or object with "top", "right", "bottom" and "left" properties with different widths
	        borderColor: color or null or object with "top", "right", "bottom" and "left" properties with different colors
	        minBorderMargin: number or null
	        clickable: boolean
	        hoverable: boolean
	        autoHighlight: boolean
	        mouseActiveRadius: number
	    }
	    interaction: {
	        redrawOverlayInterval: number or -1
	    }
	    margin: {
	       top: top margin in pixels
	        left: left margin in pixels
	        bottom: bottom margin in pixels
	        right: right margin in pixels
	    }
	    markings: [ { xaxis: { from: 0, to: 2 }, yaxis: { from: 10, to: 10 }, color: "#bb0000" }, ... ]
	    hooks: {
	        processOptions: function(plot, options)
	        processRawData: function(plot, series, data, datapoints)
	        processOffset: function(plot, offset)
	        drawBackground: function(plot, canvasContext)
	        drawSeries: function(plot, canvascontext, series)
	        draw: function(plot, canvascontext)
	        bindEvents: function(plot, eventHolder)
        	drawOverlay: function(plot, canvascontext)
        	shutdown function(plot, eventHolder)
    	};
	};
  
Methods

	plot.hightlight(series, datapoint_index)
	plot.unhighlight(series, datapoint_index)
	plot.setData(data) - call draw after to update graph
	plot.setupGrid() 
	plot.draw() - redraw the plot canvas
	triggerRedrawOverlay() - force redrawing of overlays
	width()/height()
	offset() - offset is used in calculating mouse position in graphs
	pointOffset({x,y})- dataSpace -> div x,y
	resize() -> force canvas to fit size of div
	shutdown() -> internal function to disable all event handlers

debugging functions

*  getData()
*  getAxes()
*  getPlaceholder()
*  getCanvas()
*  getPlotOffset()
*  getOptions()

