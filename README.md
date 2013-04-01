GoogleCharts

Julia interface to Google Chart Tools.



A Google chart involves basically four steps:

* a specification of a Google "DataTable"
* a specification of chart options
* a call to make the type of chart desired.
* a call to draw the chart

This package allows this to be done within `julia` by

* mapping a `DataFrame` object into a Google DataTable 
* mapping a Dict of options into a JSON object of chart options
* providing various constructors to make the type of chart
* providing a `render` method to draw the chart (or charts) to an IOStream, a file or to a page in a local web browser.

A basic usage (see the test/ directory for more)

```
using GoogleCharts, DataFrames

scatter_data = DataFrame(quote
    Age    = [8,  4,   11, 4, 3,   6.5]
    Weight = [12, 5.5, 14, 5, 3.5, 7  ]
end)

options = {:title => "Age vs. Weight comparison",
           :hAxis =>  {:title => "Age", 
                       :minValue => 0, 
                       :maxValue => 15},	
           :vAxis =>  {:title => "Weight", 
	               :minValue => 0, 
                       :maxValue => 15}
}

chart = scatter_chart(scatter_data, options)

render(chart)   ## displays in browser
```


There are constructors for the following charts (cf. https://developers.google.com/chart/interactive/docs/gallery):

```
       area_chart, bar_chart, bubble_chart, candlestick_chart, column_chart, combo_chart,
       gauge_chart, geo_chart, line_chart, pie_chart, scatter_chart, stepped_area_chart,
       table_chart, tree_chart, annotated_time_line, intensity_map, motion_chart, org_chart,
       image_spark_line
```

The helper function `help_on_chart("chart_name")` will open Google's documentation for the specified chart in a local browser.


The names of the data frame are used by the various charts. The order
of the columns is important to the charting tools. The "Data Format"
section of each web page describes this.

The options are specified through a `Dict` which is translated into
JSON by `JSON.to_json`. There are *numerous* options described in the
"Configuration Options" section of each chart's web page.

In the `tests/` subdirectory is the basic example from Google's web page reproduced with this package.


The `render` method can draw a chart to an IOStream, a specified filename, or (when used as above) to a web page that is displayed locally. One can specify more than one chart at a time using a vector of charts. 

### A plot function

There is a `plot` function for plotting functions:

```
plot(sin, 0, 2pi)
```

A vector of functions:

```
plot([sin, u -> cos(u) > 0 ? 0 : NaN], 0, 2pi, {:lineWidth=>5, 
	                                        :title=>"A function and where its derivative is positive",
						:vAxis=>{:minValue => -1.2, :maxValue => 1.2}
						})
```

The `plot` function uses a `line_chart`. The above example shows that `NaN` values are handled gracefully, unlike `Inf` values, which we replace with `NaN`.

or paired vectors:

```
x = linspace(0, 1., 200)
y = rand(200)
plot(x, y)
```



### TODO

The `googleVis` package for `R` does a similar thing, but has more
customizability. This package should try and provide similar
features. In particular, the following could be worked on:

* Needs a julian like interface, 
* some features for interactive usage,
* some integration with local web server. 
