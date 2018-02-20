This is the first of a five part series summarsing the core concepts of
Hadley Wickham's R for Data Science: <http://r4ds.had.co.nz/>.

I'd recommend Hadley's book as an excellent and most comprehensive
introduction to R programming. Hadley immediately introduces data
visualisation concepts in R4DS, giving students a fun and highly
functional introduction to R. While data visualisation is continously
referenced throughout the text, it is heavily emphasized in chapters 3,
7, and 28. This write up is an attempt to synthesis this information in
a valueable and concise manner. Of course, this isn't a conprehensive
guide to ggplot2, rather it's a small collection of code snippets and
ideas that I find solve 80% of my ggplot2 problems.

For more useful resources, including advanced ggplot2 techniques, check
out these links:

R Graphics ggplot2 Cookbook - <http://www.cookbook-r.com/Graphs/>  
Top 50 Visualizations -
<http://r-statistics.co/Top50-Ggplot2-Visualizations-MasterList-R-Code.html>  
R Studio's ggplot2 Cheat Sheet -
<https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf>  
Hadley's ggplot2 book (amazon) -
<https://www.amazon.com/dp/331924275X/ref=cm_sw_su_dp>

<br  />

### Associated Libraries

    # install.packages('tidyverse', 'RColorBrewer', 'maps')

    library('tidyverse')     # install ggplot2 and associated tidyverse packages
    library('ggplot2')       # install only ggplot2
    library('RColorBrewer')  # install for excellent color palettes
    library('maps')          # install for geospatial visualisations

I recommend installing the entire tidyverse package collection whenever
you're working with and graphing data. If you don't want access to the
other useful tidyverse tools, it's fine to just call the ggplot2 package
by itself. RColorBrewer is my favorite way to quickly create visualy
pleasing color schemes, but there are many other packages out there to
choose from. The maps package integrates with ggplot2 really well, but
there are probably better geospatial packages out there too.

<br  />

### Graphing Template

    ggplot( < enter data here > ) +            
    geom_< enter geometry here >(              
      mapping = aes( < enter variables here > ),    
      stat = < enter statistical transformations here >,          
      position = < enter position adjustments here >        
    ) + 
    coord_< enter coordinate system here >() +          
    facet_< enter facet system here >()              

With these seven parameters you can create and describe almost any
graph. This is the underlying tidyverse philosophy, giving a 'grammer to
graphics' as it were. Now let's go through these parameters
individually, and toss in a few additional aestetic parameters for good
measure.

<br  />

### Base Graph

    ggplot(mpg)

![](visualizing_data_with_ggplot2_files/figure-markdown_strict/unnamed-chunk-3-1.png)

Just calling ggplot( ) with an assoicated dataset will create a canvas
for your graphic, but it won't do much else. We really need to add a
geometry to the ggplot recipe.

<br  />

### Geometry

    ggplot(mpg) +
      geom_point(mapping = aes(x = displ, y = hwy))

![](visualizing_data_with_ggplot2_files/figure-markdown_strict/unnamed-chunk-4-1.png)

Declaring a plot geometry is perhaps the most important part of a ggplot
recipe; when you're doing exploratory data analysis I find you typically
don't have to go any further than calling the geom function and
declaring it's mappable variables. I've listed the handful of
geometries, mappings, and aestetics that I use most often. There are
hundreds of geometries if you include the extensions that the ggplot
community has created. If you can imagine it, there's probably a ggplot
geometry that can create it.

##### Geometries

-   **geom\_density( )** - 1 variable gaussian distribution
-   **geom\_point( )** - 2, 3, or 4 variable scatterplot
-   **geom\_smooth( )** - 2 or 3 variable line plot
-   **geom\_bar(stat="identity")** - 2 or 3 variable bar plot
-   **geom\_hex( )** - 2 variable distribution
-   **geom\_tile( )** - 3 variable tile plot
-   **geom\_map( )** - 2, 3, or 4 variable geospatial

##### Mappings

-   **aes( x = )** - x-variable
-   **aes( ..., y = )** - y-variable
-   **aes( ..., color / fill = )** typically a third or fourth variable
-   **aes( ..., size = )** typically a third or fourth variable

##### Aestetics

-   **size =** size of a point, declared by a double
-   **linetype =** 0 (blank), 1 (solid), 1 (dashed), 3 (dotted), 4
    (dot-dash), 5 (long-dash), 6 (two-dash)
-   **weight =** (line thickness) double
-   **color =** (outline) string
-   **fill =** (inside) string
-   **alpha =** (transparency) 0-1

<br  />

### Statistical Transformations

Sometimes you want to visualize a transformed version of the original
data. The statistical transformation I use most often is the identity
trasformation with geom\_bar. There are, however, several other useful
transformations.

-   **geom\_(aes(...), stat = "count")** - visualise the number of
    entires in a variable, n()  
-   **geom\_(aes(...), stat = "identity")** - visuaise the variable, not
    the count  
-   **geom\_(aes(...), stat = "unique")** - visualise only unique
    components of the variable

<br  />

### Position Adjustments

When geometric data occupies the same space, it is useful to use a
position adjustment to better understand the visualisation. I really
only find adjusting the positioning of data useful when I'm working with
bar charts and scatterplots.

-   **geom\_point(aes(...), position = "jitter")** - adds a small amount
    of noise to better view overlapping points  
-   **geom\_bar(aes(...), position = "dodge")** - arranges bar elements
    side-by-side  
-   **geom\_bar(aes(...), position = "fill")** - arranges bar elements
    on top of each other, normalizing height

<br  />

### Coordinates

    ggplot(mpg) +
      geom_point(mapping = aes(x = displ, y = hwy)) + 
      coord_flip()  

![](visualizing_data_with_ggplot2_files/figure-markdown_strict/unnamed-chunk-5-1.png)
<br  />  
Cartesian coordinates are the go-to for 95% of my visualisations, but
the coord\_ function does more than call different coordinate systems.
Often it's useful to fix the x-y ratio, or flip the axis of bar charts.
When your working with exponential data, you may want to transform an
axis to a logarithmic system. These useful functions and many more can
be done by the coord\_ function.

-   **coord\_fixed(ratio, xlim, ylim)** - fixing the aspect ratio
    between x and y
-   **coord\_flip(xlim, wlim)** - flipping the axes
-   **coord\_polar(theta, start, direction)** - coverting cartesian to
    polar coordinates
-   **coord\_trans(xtrans, ytrans, limx, limy)** - transform cartesian
    coordiantes, use with log functions  
-   **coord\_map(projection, orientation, xlim, ylim)** - mapproj
    package projections

<br  />

### Facets

    ggplot(data = mpg) + 
      geom_point(mapping = aes(x = displ, y = hwy)) + 
      facet_wrap(~ class, nrow = 2)

![](visualizing_data_with_ggplot2_files/figure-markdown_strict/unnamed-chunk-6-1.png)

I don't use facets that much; to be honest I find them pretty
distracting. If you're a fan of contrasting minerature plots, you might
find these faceting options useful.

-   **facet\_grid(x ~ y, labeller, scales)** - facets the display into
    rows and columns based on the two given variables (".""
    placeholder)  
-   **facet\_wrap(~ x, labeller, scales, nrow, ncol)** - wraps facets
    into a rectangular layout

<br  />

### Labels

    ggplot(mpg) + 
      labs(
        title = "title", 
        subtitle = "subtitle",
        caption = "caption",
        xlab = "x-label", 
        ylab = "y-label"
      )

![](visualizing_data_with_ggplot2_files/figure-markdown_strict/unnamed-chunk-7-1.png)

Labels are helpful for properly communicating your visualisation.
Including these five labels with your plots can help clarify your topic,
variables, and sources.

<br  />

### Annotations

Annotations are another feature I don't use too often, but I'll include
them anyways. I think annotating individual observations is a mistake,
it clutters the graphic and takes away from the effectiveness of the
visualisation. Packages like plotly and shiny do a better job of
creating interactive graphics if you want individual labels. If you,
however, insist on creating a ggplot with annotations, these might be
useful.

-   **geom\_text(aes(label = "text here"), vjust = , hjust = )** -
    places a summary annotation within the graphic, according to vjust
    and hjust  
-   **geom\_hline(yintercept = , size = , color = , linetype)** -
    creates a horizontal reference line through the plot according to
    yintercept  
-   **geom\_vline(xintercept = , size =, color =, linetype =)** -
    creates a verticle reference line through the plot according to
    xintercept  
-   **geom\_rect(aes(xmin = , xmax = , ymin = , ymax = ))** - creates a
    rectangle to box in and highlight interesting data  
-   **geom\_segment(aes(x = , y = , xend = , yend = ,) arrow = )** -
    creates a line segment within the plot, it can be an arrow

<br  />

### Scales

Scales are automatically set when you create a ggplot, but sometimes
it's useful to alter the color scheme, legend, or axes. The naming sceme
for these functions is scale\_ followed by the name of the aesthetic
then another \_, and then the name of the scale.

##### Aestetic Names

-   **\_x\_**  
-   **\_y\_**  
-   **\_alpha\_**  
-   **\_color\_**  
-   **\_fill\_**  
-   **\_linetype\_**  
-   **\_shape\_**  
-   **\_size\_**

##### Scale Names

-   **\_discrete** - maps discrete variables to visual values  
-   **\_continous** - maps continuous variables to visual values
-   **\_identity** - uses data values as visual values
-   **\_manual** - values = c( ), maps discrete values to manually chose
    visual values
-   **\_gradient** - creates a two color gradient, low - high
-   **\_gradient2** - creates a diverging two color gradient, positive -
    negative
-   **\_gray** -creates a gradient in gray
-   **\_brewer** - brewer( palette = " "), calls RColorBrewer palettes

<br  />

### Axes

    scale_x_continuous(breaks = seq(0, 10, by = 1), labels = c(1:9, "ten")) 
    # define breaks by sequence, define labels of breaks

    scale_x_discrete(breaks=c("ctrl", "trt1", "trt2"),            # defined tick marks
                     labels=c("Control", "Treat 1", "Treat 2"))   # new names
                     
    expand_limits(x = c(0,8), y = c(0,8))                         # expand the limits of the graph to visualise specific values

    scale_y_reverse()     # Reverse y-axis direction (zero on top)

    element_blank()       # Hiding axis labels 

    theme(axis.title.x = element_text(face="bold", color="red", size=20),  # change axis labels, font and style
          axis.text.x = element_text(angle=90, vjust=0.5, size=16))        # change tick mark label, font, style

    theme(panel.grid.major=element_blank(),  # hiding major gridlines
          panel.grid.minor=element_blank())  # hiding minor gridlines

    library(scales)   # changing the y-axis to log 10
    scale_y_log10(breaks = trans_breaks("log10", function(x) 10^x),
                  labels = trans_format("log10", math_format(10^.x)))

Scales can be used to alter axes in ggplot2, it's a fairly straight
forward process. These are some commented examples for fixing common
behavioral issues you may have with your axes.

<br  />

### Legends

    theme(legend.position = "left")    # legend to the left of plot
    theme(legend.position = "top")     # legend above plot
    theme(legend.position = "bottom")  # legend below plot
    theme(legend.position = "right")   # the default
    theme(legend.position = "none")    # no legend will be generated

    geom_point(aes(...), show.legend=FALSE) # don't include this geom in the legend    

    theme(legend.title = element_blank()) # hides the legends title

    # changing the order of items in your legend, scale_ function is dependent upon the type of plot
    scale_fill_discrete(breaks=(c("item1", "item2", "item3")))

    # simply reverse the current item display in thelegend
    scale_fill_discrete(guide = guide_legend(reverse=TRUE))

    # manually alter color, break, label, and name of legend items.
    scale_fill_manual(values=c("#999999", "#E69F00", "#56B4E9"), 
                      name="Experimental\nCondition",
                      breaks=c("ctrl", "trt1", "trt2"),
                      labels=c("Control", "Treatment 1", "Treatment 2"))

Legends can be a bit harder to wrangle than axes. Heere are examples to
several common problems I have with legends.

<br  />

### Colors

Some of my favorite preset colors include

-   thistle
-   slateblue2
-   orchid
-   midnightblue
-   deepskyblue
-   lightslateblue

The RColorBrewer package also has pleasing color palettes.

    RColorBrewer::display.brewer.all()

<br  />

### Themes

Sometimes you don't want to go through the effort of creating your own
theme, so ggplot2 has some built in. Here are four basic themes that are
excellent for exploratory analysis or creating a solid foundation to
build off.

-   **theme\_bw()** - A while background with gridlines
-   **theme\_grey()** - the default theme
-   **theme\_classic()** - A white background with no gridlines
-   **theme\_minimal()** - mystery theme, check it out!

<br  />

### Summary

This is just a fraction of what ggplot2 can do, but with this
infromation I think you should be able to solve 80% of your graphing
needs. Again, the references listed at the beginning of this write up
are excellent and worth checking out. If you want to learn more about R
for Data Science written by Hadley Wickham, it's free to read at
<http://r4ds.had.co.nz/>. If you're too lazy to read the entire book,
check out the other posts in my R for Data Science Summary Series.

Thanks for reading,  
- Fisher