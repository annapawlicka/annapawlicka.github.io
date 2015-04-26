---
title: Pretty charts with dimple.js
author: Anna
layout: post
permalink: /pretty-charts-with-dimple-js/
views:
  - 6809
categories:
  - JavaScript
tags:
  - chart
  - dimple.js
  - JavaScript
---
I have spent half a day trying to create a bar chart with d3 library. It&#8217;s been quite painful so I could not have been happier when I learned about [dimple.js][1]. Dimple is powered by d3, but it does not overwhelm the user with the typical complexity of d3. A simple bar chart can be created in just a few minutes! Have a look below for the step by step example of how to create and customise a chart.

**Load data and create simple bar chart**

****First we need to create a new svg container, specifying its dimensions and div in which it will be placed. Then we load our data. The dataset used in this example is a csv file, but d3 library that works underneath allows for all sorts of files (tsv, xls, etc.). We need to set bounds of the chart, and specify what data is to be added to x- and y-axis.

```JavaScript
<script src="http://d3js.org/d3.v3.min.js"></script>
    <script src="http://dimplejs.org/dist/dimple.v1.min.js"></script>
    <script type="text/javascript">
        var svg = dimple.newSvg("#chartContainer", 490, 430);
        d3.csv("./data/Birds.csv", function (data) {
            var myChart = new dimple.chart(svg, data);
            myChart.setBounds(60, 10, 400, 330)
            var x = myChart.addCategoryAxis("x", ["SPECIES"]);
            myChart.addMeasureAxis("y", "D/S");
            var s = myChart.addSeries("SPECIES", dimple.plot.bar);
            s.barGap = 0.05;
            myChart.draw();
        });
    </script>
```

Dimple takes care of all the rest and creates a nice bar chart with a tiny tooltip:

<div id="chart">
  <br />
</div>

**Customised tooltip**

<span style="line-height: 1.6;">If we want to add more information into the tooltip, or we want additional formatting (e.g. rounding of numbers), we can create our own tooltip.</span>  
For that we need our own event handlers, coordinates of where the popup should be displayed, and a group for the popup rectangle and the text. We also need to specify the attributes and style. Here&#8217;s the code snippet:

<pre class="brush:js">&lt;script src="http://d3js.org/d3.v3.min.js"&gt;&lt;/script&gt;
    &lt;script src="http://dimplejs.org/dist/dimple.v1.min.js"&gt;&lt;/script&gt;
    &lt;script type="text/javascript"&gt;
        var svg = dimple.newSvg("#chartContainer2", 490, 430);
        d3.csv("./data/Birds.csv", function (data) {
            var myChart = new dimple.chart(svg, data);
            myChart.setBounds(60, 20, 400, 330)
            var x = myChart.addCategoryAxis("x", ["SPECIES"]);
            myChart.addMeasureAxis("y", "D/S");
            var s = myChart.addSeries(["SPECIES"], dimple.plot.bar);
            s.barGap = 0.05;

            // Handle the hover event - overriding the default behaviour
            s.addEventHandler("mouseover", onHover);
            // Handle the leave event - overriding the default behaviour
            s.addEventHandler("mouseleave", onLeave);

            myChart.draw();

            // Event to handle mouse enter
            function onHover(e) {

                // Get the properties of the selected shape
                var cx = parseFloat(e.selectedShape.attr("x")),
                    cy = parseFloat(e.selectedShape.attr("y"));

                // Set the size and position of the popup
                var width = 150,
                    height = 70,
                    x = (cx + width + 10 &lt; svg.attr("width") ?
                         cx + 10 :
                         cx - width - 20);
                    y = (cy - height / 2 &lt; 0 ?
                        15 :
                        cy - height / 2);

                // Create a group for the popup objects
                popup = svg.append("g");

                // Add a rectangle surrounding the text
                popup
                        .append("rect")
                        .attr("x", x + 5)
                        .attr("y", y - 5)
                        .attr("width", 150)
                        .attr("height", height)
                        .attr("rx", 5)
                        .attr("ry", 5)
                        .style("fill", 'white')
                        .style("stroke", 'black')
                        .style("stroke-width", 2);

                // Add multiple lines of text
                popup
                        .append('text')
                        .attr('x', x + 10)
                        .attr('y', y + 10)
                        .append('tspan')
                        .attr('x', x + 10)
                        .attr('y', y + 20)
                        .text('Species: ' + e.seriesValue[0])
                        .style("font-family", "sans-serif")
                        .style("font-size", 10)
                        .append('tspan')
                        .attr('x', x + 10)
                        .attr('y', y + 40)
                        .text('Dive to Surface Ratio: ' + Math.round(e.yValue * 10) / 10)
                        .style("font-family", "sans-serif")
                        .style("font-size", 10)
            }

            // Event to handle mouse exit
            function onLeave(e) {
                // Remove the popup
                if (popup !== null) {
                    popup.remove();
                }
            };
        });
    &lt;/script&gt;</pre>

The final bar chart with a customised tooltip is shown below.

<div id="chartContainer2">
  <br />
</div>

**Customise axis label and position**

<pre class="brush:js">// Override x-axis title
x.titleShape.text("My own title");
// Title is placed below the tick labels by default. This overrides this setting and places it immediately below the axis.
x.titleShape.attr("y", myChart.height + 55);</pre>

**Change default colours**

Colours of bars can be changed via CSS:

<pre class="brush:css">rect {
    stroke: #41B6C4;
    fill: #41B6C4;
}</pre>

CSS overrides default colours of bars or circles. We can also change colours by changing the default values:

<pre class="brush:js">// Override tooltip colour
myChart.defaultColors = [
 new dimple.color("#E0FFFF")
];</pre>

There are many more ways in which the charts can be customised, and if we feel the need to dig deeper, d3 objects are accessible and ready for us to mess with them.

For dimple API visit its [Wiki pages][2].

[CSV dataset source: [Simon&#8217;s Discoveries][3]]

 [1]: http://dimplejs.org/
 [2]: https://github.com/PMSI-AlignAlytics/dimple/wiki/_pages
 [3]: http://simonsdiscoveries.com/