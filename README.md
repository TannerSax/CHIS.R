# CHIS.R

Marimekko/Mosaic Plot
In the previous exercise we looked at different ways of showing the frequency distribution within each BMI category. This is all well and good, but the absolute number of each age group also has an influence on if we will consider something as over-represented or not. Here, we will proceed to change the widths of the bars to show us something about the n in each group.

This will get a bit more involved, because the aim is not to draw bars, but rather rectangles, for which we can control the widths. You may have already realized that bars are simply rectangles, but we don't have easy access to the xmin and xmax aesthetics, but in geom_rect() we do! Likewise, we also have access to ymin and ymax. So we're going to draw a box for every one of our 268 distinct groups of BMI category and age.

The clean adult dataset, as well as BMI_fill, are already available. Instead of running apply() like in the previous exercise, the contingency table has already been transformed to a data frame using as.data.frame.matrix().

To build the rectangle plot, we'll add several variables to DF:
groupSum, containing the sum of each row in the DF. Use rowSums() to calculate this. groupSum represents the total number of individuals in each age group.
xmax: the xmax value for each rectangle, calculated as cumsum(DF$groupSum)
xmin: the xmin value for each rectangle, calculated by subtracting the groupSum column from the xmax column.
The names of the x axis groups are stored in the row names, which is pretty bad style, so make a new variable, X, that stores the values of row.names() for DF.
Now we are ready to melt the dataset. Load reshape2 and use melt() on DF. Specify the id.vars variables as c("X", "xmin", "xmax") and the variable.name argument as "FILL". Store the result as DF_melted.
Have a look at the dplyr call that calculates the ymax and ymin columns of DF_melted. It first groups by X and then calculates cumulative proportions. The result is stored as DF_melted again.
If all goes well you should see the plot in the viewer when you execute the plotting function at the bottom of the script.


Adding statistics
In the previous exercise we generated a plot where each individual bar was plotted separately using rectangles (shown in the viewer). This means we have access to each piece and we can apply different fill parameters.

So let's make some new parameters. To get the Pearson residuals, we'll use the chisq.test() function.

The data frames adult and DF_melted, as well as the object BMI_fill that you created throughout this chapter, are all still available. The reshape2 package is already loaded.

Use the adult$RBMI (corresponding to FILL) and adult$SRAGE_P (corresponding to X) columns inside the table() function that's inside the chisq.test() function. Store the result as results.
The residuals can be accessed through results$residuals. Apply the melt() function on them with no further arguments. Store the resulting data frame as resid.
Change the names of resid to c("FILL", "X", "residual"). This is so that we have a consistent naming convention similar to how we called our variables in the previous exercises.
The data frame from the previous exercise, DF_melted is already available. Use the merge() function to bring the two data frames together. Store the result as DF_all.
Adapt the code in the ggplot command to use DF_all instead of DF_melted. Also, map residual onto fill instead of FILL.


Adding text
Since we're not coloring according to BMI, we have to add group (and x axis) labels manually. Our goal is the plot in the viewer.

For this we'll use the label aesthetic inside geom_text(). The actual labels are found in the FILL (BMI category) and X (age) columns in the DF_all data frame. (Additional attributes have been set inside geom_text() in the exercise for you).

The labels will be added to the right (BMI category) and top (age) inner edges of the plot. (We could have also added margin text, but that is a more advanced topic that we'll encounter in the third course. This will be a suitable solution for the moment.)

The first two commands show how we got the the four positions for the y axis labels. First, we got the position of the maximum xmax values, i.e. at the very right end, stored as index. We want to calculate the half difference between each pair of ymax and ymin (e.g. (ymax - ymin)/2) at these index positions, then add this value to the ymin value. These positions are stored in the variable yposn.

We'll begin with the plot thus far, stored as object p. In the sample code, %+% DF_all refreshes the plot's dataset with the extra columns.

Plot 1: In the geom_text() function, define the x, y and label aesthetics.

Set x to max(xmax), so the labels are on the right side of the plot.

Set the position of y to yposn.

Set the label text to FILL.

Plot 2: The same thing for the x axis label positions. You don't need to find an index here, since you can use the same y position for all these labels: 1.

Calculate the half difference between each pair of xmax and xmin then add this value to xmin.

Complete the plot command by adding the labels in the xposn to our plot, the label this time will be X, which in this case is the age.

This plot isn't perfect, but it does a pretty good job for an exploratory plot.

Generalizations
Now that you've done all the steps necessary to make our mosaic plot, you can wrap all the steps into a single function that we can use to examine any two variables of interest in our data frame (or in any other data frame for that matter). For example, we can use it to examine the Vocab data frame we saw earlier in this course.

You've seen all the code in our function, so there shouldn't be anything surprising there. Notice that the function takes multiple arguments, such as the data frame of interest and the variables that you want to create the mosaic plot for. None of the arguments have default values, so you'll have to specify all three if you want the mosaicGG() function to work.

Start by going through the code and see if you understand the function's implementation.

Print mosaicGG and read its contents.
Calling mosaicGG(adult, "SRAGE_P","RBMI") will result in the plot you've been working on so far. Try this out. This gives you a mosaic plot where BMI is described by age.
Test out another combination of variables in the adult data frame: Poverty (POVLL) described by Age (SRAGE_P).
Try the function on other datasets we've worked with throughout this course:
mtcars dataset: am described by cyl
Vocab dataset: vocabulary described by education.
