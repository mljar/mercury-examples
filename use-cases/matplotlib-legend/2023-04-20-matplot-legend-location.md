---
title: 3 ways to set matplotlib legend location
categories:
  - Mercury Tutorial
date: 2023-04-21
author: Adrian Błazeusz
comments: true
---

<img src="/blog/.png" class="blog_img_100 blog_img_border" alt="3 ways to set matplotlib legend location."/> 

The Matplotlib is a good choice for charting under many issues. It can be used on **Windows, Linux and Mac**.This makes it easy to share charts with other users, no matter what platform they use. It is a very versatile library and can **draw many different types of charts**, including, point graph, histograms, line, bar and pie charts, contour charts and more. It gives a lot of control over the appearance of the plot. The user can customize almost every aspect of the graph, including colors, line styles, fonts, axis labels and legends. And today we are going to focus on the last one mentioned, **i.e. the legend**.

# How to create legends according to your own preferences.
With the ability to customize the legend to our needs, we are able to create more interesting charts, and today we have prepared for you 3 examples of how they can be used.

## 1. Legend inside the plot.
Sometimes the simplest solutions are the best, so if you only have one simple chart to create this method may be appropriate for you.

```python
x1 = [-2, 1, 2, 3]
y1 = [-2, 1, 2, 3]
x2 = [-2, 1, 2, 3]
y2 = [1 ,4, 5, 6]

# Create the plot
fig, ax = plt.subplots()

# Plot the data
ax.plot(x1, y1, label='Line 1')
ax.plot(x2, y2, label='Line 2')

# Add x and y axes at zero
ax.axhline(0, color='black')
ax.axvline(0, color='black')

# Set x and y limits
xlim = ax.get_xlim()
ylim = ax.get_ylim()
ax.set_xlim(xlim[0], xlim[1])
ax.set_ylim(ylim[0], ylim[1])

# Remove the top and right spines of the plot
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

# Create the legend inside the plot
plt.legend(loc='upper left', borderaxespad=1)


# Show the plot
plt.show()
```
Let's divide this code into parts. **The first part** of the code **creates the data** we use for our **subplots() graph**. It then **adds the x and y axis** at the **zero point** using the **axhline()** and **axvline()** functions, respectively. 

The x and y axis boundaries are set to the current plot boundaries using the **get_xlim()** and **get_ylim()** functions, and then the boundaries are **explicitly set** using the **set_xlim()** and **set_ylim()** functions. You can choose what part of the graph you want to show using **ax.spines**, in this scenario we **hide the top and right part**. 

<img src="/blog/.png" class="blog_img_100 blog_img_border" alt="3 ways to set matplotlib legend location."/> 

And finally we **create a legend** and set it's position using **loc**, additionally we can set how far away from our graph the legend will be using **borderaxespad**. With this method, we can easily **create legends inside** our chart.

```
==================   =============
Location String      Location Code
==================   =============
'best' (Axes only)   0
'upper right'        1
'upper left'         2
'lower left'         3
'lower right'        4
'right'              5
'center left'        6
'center right'       7
'lower center'       8
'upper center'       9
'center'             10
==================   =============
```
In case as if you wanted to **change the location** using this method use one of the above **Location String**.

## 2. Legend outside plot
If the legend inside the chart doesn't suit you, you can use the **bbox_to_anchor** function.

```python
...

# Remove the top and right spines of the plot
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

# Create the legend outside the plot
plt.legend(borderaxespad=3, bbox_to_anchor=(1.3, 0.2))

# Show the plot
plt.show()
```

The **bbox_to_anchor** function sets the position of the legend relative to a point (in upper right corner) of the chart area. In this case, bbox_to_anchor=(1.3, 0.2) means that the legend will be shifted 1.3 units to the right and 0.2 units down relative to the upper right corner of the chart.

<img src="/blog/.png" class="blog_img_100 blog_img_border" alt="3 ways to set matplotlib legend location."/> 

## 2.1 Example  
We will demonstrate to you what can be created using this function. **To create the chart below, add this code in one cell.**

```python
# Creating legend
legend_elements = [Line2D([0], [0], color='steelblue', ls='--',lw=2, label='blue plot'),
                   Line2D([0], [0], color='orange', ls='--',lw=2, label='orange plot'),
                   Line2D([0], [0], color='yellowgreen', ls='--',lw=2, label='green plot'),
                   Line2D([0], [0], color='red', ls='--',lw=2, label='red plot'),
                   Line2D([0], [0], color='yellow',ls='--', lw=2, label='yellow plot')]

# Create the subplots and set their size
fig, ax = plt.subplots(nrows=3, ncols=2, figsize=(8, 6))
```

The **legend_elements** variable creates a list of Line2D objects that represent the lines in the chart. Each **Line2D** object is customized with the color, line style, line width and label to be used in the legend. 

The **subplots** will have **3 rows and 2 columns** in which there will be one chart each. To determine how many charts there should be we used the **nrows** and **ncols** functions.

```python
# Remove the top and right spines of the plots
for axi in ax.flat:
    axi.spines['top'].set_visible(False)
    axi.spines['right'].set_visible(False)
    
    axi.set_xticklabels([])
    axi.set_yticklabels([])
```

This part of the code is responsible for **hiding the top and right parts of the chart** and hiding the labels from the numbers.

```python
# First plot
x = np.linspace(0, 2 * np.pi, 100)
y = np.sin(x)
ax[0, 0].plot(x, y, color='steelblue', lw=2, ls='--')

# Second plot
x = np.linspace(0, 2 * np.pi, 100)
y = np.cos(x)
ax[0, 1].plot(x, y, color='orange', lw=2, ls='--')

# Third plot
x = np.linspace(0, 10, 100)
y = x ** 2
ax[1, 0].plot(x, y, color='yellowgreen', lw=2, ls='--')

# Fourth plot
x = np.linspace(0, 10, 100)
y = np.exp(x)
ax[1, 1].plot(x, y, color='red', lw=2, ls='--' )

# Fifth plot
x = np.linspace(0, 10, 100)
y = np.sin(x) + np.cos(x)
ax[2, 0].plot(x, y, color='yellow', lw=2, ls='--')
```

This code adds plots to each of the subplots..

```python
#Hiding last plot
axi.spines['bottom'].set_visible(False)
axi.spines['left'].set_visible(False
            )
axi.get_xaxis().set_visible(False)
axi.get_yaxis().set_visible(False)

#Legend
ax[2, 1].legend(handles=legend_elements,  bbox_to_anchor=(0.7, 0.9))

# Show the plots
plt.tight_layout()
plt.show()
```

In the last column we want to **hide the remains of the chart**, then we **create our legend** and set it according to our preferences using **bbox_to_anchor**.To optimize the spacing between subplots we call the **plt.tight_layout()** function.

<img src="/blog/.png" class="blog_img_100 blog_img_border" alt="3 ways to set matplotlib legend location."/> 

# 3. Legend on line
The last option we have prepared for you today is to describe the lines of the chart at their ends with text.

```python
x = np.linspace(0, 20, 1000)
y1 = np.sin(x)
y2 = np.cos(x)

fig, ax = plt.subplots()  # create the `ax` object

ax.plot(x, y1, "-b", label="sine")
ax.plot(x, y2, "-r", label="cosine")

# Find index of last point for each function
last_index_y1 = np.argmin(np.abs(y1 - y1[-1]))
last_index_y2 = np.argmin(np.abs(y2 - y2[-1]))

# Add labels at last points with custom color and size
ax.text(x[last_index_y1], y1[last_index_y1], "sine", fontsize=16,color="blue")
ax.text(x[last_index_y2], y2[last_index_y2], "cosine",  fontsize=16, color="red")

# Remove legend
ax.legend().remove()

# Remove top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
```
The **linspace** function takes three arguments: the **start point, the end point** and the **number of points to generate between** them. In this case, the start point is 0, the end point is 20, and linspace generates 1000 points in between.

Code creates a figure with two plots of the **sine** and **cosine** functions using the plot function. The label parameter is used to set the legend labels for each function.

Functions **np.argmin and np.abs** finding the last point of sine and cosine. This is done to position the labels at the right end of the plots.

The text function of the ax object is used to add the labels at the last points of each function. in **ax.txt** we can add fontsize parameter and finaly we choose the color of text.

<img src="/blog/.png" class="blog_img_100 blog_img_border" alt="3 ways to set matplotlib legend location."/> 

# 4. Mercury
If we want to add a little more user interaction to our charts we can add widgets from the **mercury library**.For example, in 
chart #1, let's add an option to turn the legend on and off.

```python
app = mr.App(title="Matplotlib legend location", description="plt legend", show_code=False, continuous_update=False)
```

## 4.1 Checkbox

To be able to create an interactive site we must first **define the app**. Then we add the **Checkbox** function to our code.

```python
# add checkbox
lege = mr.Checkbox(value=True, label="ON/OFF Legend")
```
By default, the **value is set to True**, which means that as long as we do not uncheck it, the legend will appear on our chart.

```python
...

if lege.value == True:
    # Create the legend inside the plot
    plt.legend(loc='upper left', borderaxespad=1)
else:
    # Remove legend
    ax.legend().remove()
```
<img src="/blog/.gif" class="blog_img_100 blog_img_border" alt="3 ways to set matplotlib legend location."/> 

If you want to add a **Checkbox** option in the place where you only added the legend change to the above code.

## 4.2 Select

In addition to a simple on/off switch, we will add to the second chart a choice of where we want the legend using the **Switch** option.

```python
place = mr.Select(value='Right Top', choices=['Right Top','Right Button', 'Left Top'], label='Where to set legend:')
```

In the **Select** in the **value** we choose one of the options, this will be our basic version, then through **choice** we create as many variances as we need.
When we done this lets move to **plot #2** to change code.

```python
if place.value == 'Right Top':
    # Create the legend outside the plot
    plt.legend(borderaxespad=3, bbox_to_anchor=(1.3, 1.1))
elif place.value == 'Left Top':
    # Create the legend outside the plot
    plt.legend(borderaxespad=3, bbox_to_anchor=(0.3, 1.1))
elif place.value == 'Right Button':
        # Create the legend outside the plot
    plt.legend(borderaxespad=3, bbox_to_anchor=(1.3, 0.2))
```
Here we use a simple **elif** to select the appropriate arrangement. If the place is equal to 'Right Top' then set the legend position to (1.3, 1.1).

<img src="/blog/.gif" class="blog_img_100 blog_img_border" alt="3 ways to set matplotlib legend location."/> 

# 5. Summary