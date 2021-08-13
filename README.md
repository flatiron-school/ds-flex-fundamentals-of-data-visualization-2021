# Fundamentals of Visualizations

- DS-Flex
- Hosted by James Irving

## Learning Objectives

- Discuss an overview of the types of visualizations available.
- Discuss which types of visuals are best for showing what type of information/comparisons.


- Discuss the Anatomy of a Matplotlib Figure

- Discuss the many ways to create/start a figure
    - Matplotlib plt functions
    - Matploltib OOP interface
    - Pandas
    - Seaborn
- Overhauling the Aesthetics with Matplotlib styles and seaborn contexts.

- Activity: Answering Questions via EDA

## Prerequisites

- To get the most out of this study group, you should have already learned how to work with Panda's DataFrames (Topic 04)

# Types of Data Visualizations

## Data Visualization Overview

- We use visualizations to tell a story about our data. 
    - Knowing which visualization is best to tell the story you are trying to tell is an important skill. 


- Let's Explore Different Types of Visualizations and when they are most appropriate to use. 

    - [The Data Viz Project](https://datavizproject.com/)

    - [Blog Post: How to Choose a Chart Type](https://towardsdatascience.com/data-visualization-101-how-to-choose-a-chart-type-9b8830e558d6)




### Example Galleries [Python Packages]
Let's explore the Python-specific options available to us. 


- https://www.python-graph-gallery.com/
- [Matplotlib Example Gallery](https://matplotlib.org/gallery/index.html#examples-index) 
- [Seaborn Example Gallery](https://seaborn.pydata.org/examples/index.html)
- [Pandas Visualization docs](https://pandas.pydata.org/pandas-docs/stable/user_guide/visualization.html)
- [Plotly Express](https://plotly.com/python/plotly-express/)

# Using Visualizations to Answer Stakeholder Questions

## Business Case

- A home owners association from Ames, Iowa has hired us to provide some insights on the prices of homes in the area.  They have provided us with some data on house sales in the region, as well as a list of questions they'd like answered.

    - They want an answer within a few hours, so we don't have time to perform any modeling to get machine-learning-based insights. 

- We will therefore use the appropriate visualizations to answer their questions in visual-form.


<img src="https://www.brickunderground.com/sites/default/files/styles/blog_primary_image/public/blog/images/080818_desmoinesmain.jpg" width=50%>

### The Questions to Answer

1. What is the distribution of house prices in Ames, Iowa?
    - What is the median home price?
    - What is the average home price?
    

2. What is the relationship between square footage of the living area (`GrLivArea`)  and sale price (`SalePrice`)?
    
2. What is the average sale price for each of the different types of homes (BldgType)?

### The Provided Data


```python
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import matplotlib as mpl
%matplotlib inline
import seaborn as sns

print('- Package Versions:')
print(f'\tMatplotlib = {mpl.__version__}')
print(f'\tPandas = {pd.__version__}')
print(f'\tSeaborn = {sns.__version__}')
```

    - Package Versions:
    	Matplotlib = 3.3.1
    	Pandas = 1.1.3
    	Seaborn = 0.11.0



```python
## From https://github.com/learn-co-curriculum/dsc-regression-boston-lab
cols_to_use = ['YrSold', 'MoSold', 'Fireplaces', 'TotRmsAbvGrd', 'GrLivArea',
          'FullBath', 'YearRemodAdd', 'YearBuilt', 'OverallCond', 
          'OverallQual', 'LotArea', 'SalePrice','BldgType']

df = pd.read_csv('https://raw.githubusercontent.com/learn-co-curriculum/dsc-regression-boston-lab/master/ames.csv',
                usecols=cols_to_use
                )
display(df.head())
df.info()
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LotArea</th>
      <th>BldgType</th>
      <th>OverallQual</th>
      <th>OverallCond</th>
      <th>YearBuilt</th>
      <th>YearRemodAdd</th>
      <th>GrLivArea</th>
      <th>FullBath</th>
      <th>TotRmsAbvGrd</th>
      <th>Fireplaces</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8450</td>
      <td>1Fam</td>
      <td>7</td>
      <td>5</td>
      <td>2003</td>
      <td>2003</td>
      <td>1710</td>
      <td>2</td>
      <td>8</td>
      <td>0</td>
      <td>2</td>
      <td>2008</td>
      <td>208500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9600</td>
      <td>1Fam</td>
      <td>6</td>
      <td>8</td>
      <td>1976</td>
      <td>1976</td>
      <td>1262</td>
      <td>2</td>
      <td>6</td>
      <td>1</td>
      <td>5</td>
      <td>2007</td>
      <td>181500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11250</td>
      <td>1Fam</td>
      <td>7</td>
      <td>5</td>
      <td>2001</td>
      <td>2002</td>
      <td>1786</td>
      <td>2</td>
      <td>6</td>
      <td>1</td>
      <td>9</td>
      <td>2008</td>
      <td>223500</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9550</td>
      <td>1Fam</td>
      <td>7</td>
      <td>5</td>
      <td>1915</td>
      <td>1970</td>
      <td>1717</td>
      <td>1</td>
      <td>7</td>
      <td>1</td>
      <td>2</td>
      <td>2006</td>
      <td>140000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>14260</td>
      <td>1Fam</td>
      <td>8</td>
      <td>5</td>
      <td>2000</td>
      <td>2000</td>
      <td>2198</td>
      <td>2</td>
      <td>9</td>
      <td>1</td>
      <td>12</td>
      <td>2008</td>
      <td>250000</td>
    </tr>
  </tbody>
</table>
</div>


    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1460 entries, 0 to 1459
    Data columns (total 13 columns):
     #   Column        Non-Null Count  Dtype 
    ---  ------        --------------  ----- 
     0   LotArea       1460 non-null   int64 
     1   BldgType      1460 non-null   object
     2   OverallQual   1460 non-null   int64 
     3   OverallCond   1460 non-null   int64 
     4   YearBuilt     1460 non-null   int64 
     5   YearRemodAdd  1460 non-null   int64 
     6   GrLivArea     1460 non-null   int64 
     7   FullBath      1460 non-null   int64 
     8   TotRmsAbvGrd  1460 non-null   int64 
     9   Fireplaces    1460 non-null   int64 
     10  MoSold        1460 non-null   int64 
     11  YrSold        1460 non-null   int64 
     12  SalePrice     1460 non-null   int64 
    dtypes: int64(12), object(1)
    memory usage: 148.4+ KB


# The Many Ways of Making a Figure in Python

- There are MANY ways to make data visualizations with Python. 
- Today, we will focus on non-interactive visualizations.( Sorry, Plotly Express :*-( )

- The ~4 Different Ways to Plot with Python:

    1. Matplotlib plt functions:
        - `plt.plot`/`.bar`,`.scatter`, etc.      
    2. Matploltib OOP interface:
       ```python 
       fig, ax =plt.subplots()
       ax.plot#/ax.scatter, ax.bar, etc
        ```
        
    3. Pandas:
        ```python
         ax = df.plot()
         ax.set(#...
        ```
        
    4. Seaborn:
        ```python
         sns.histplot
         sns.regplot
         sns.scatter
        ```  

- **All 4 of these approaches to making figures with Python ultimately use matplotlib behind the scenes.**

> For this section of the notebook, we will focus on answering one question in each of the 4+ different ways. 
> #### Q1: What is the distribution of house prices in Ames, Iowa?

## Method 01: Using Matplotlib Plt Functions

- Select the correct plt function and plot the data.
- Make sure that:
    - We have an xlabel,ylabel, and title.
    - the figure is large enough
- Add a vertical line for the mean, including a label with the mean. 
    - Make sure its: 
        - Different color than the bars
        - a dotted line
- Add a legend 

#### Plot 1: Sale Price Distribution


```python
#Plot 1: Sale Price Distribution

```

## Method 02: Using Matplotlib OOP Interface

#### A Tale of Two Syntaxes
-  *Matplotlib is powerful but can be a bit confusing at times because of its 2 sets of commands:*
    - the matplotlib.pyplot functions (`plt.bar()`,`plt.title()`)
    - the object_oriented methods (`ax.bar()`,`ax.set_title()`)
    
- The 2 syntaxes can be confusing at first and cause problems & odd results when mixed together.
    - Learn about some of the problems when mixing types.
    - Example: see how plt.title()/plt.xlabel(),etc. can behave strangely in subplots.
    
    - **Bookmark this article, its the best explanation of how matploblib'S 2 interfaces work:**
> ["Artist" in Matplotlib - something I wanted to know before spending tremendous hours on googling how-tos.](https://dev.to/skotaro/artist-in-matplotlib---something-i-wanted-to-know-before-spending-tremendous-hours-on-googling-how-tos--31oo)<br>

<!-- - [My Blog Post on Making Customized Figures in seaborn](https://jirvingphd.github.io/harnessing_seaborn_subplots_for_eda)
    - This covers some concepts we didn't have time to cover, like ticklabel formatters. -->

>- ***So... what are "object-oriented-methods" anyway?***

### Anatomy of a Matplotlib Figure



<center><img src="https://raw.githubusercontent.com/jirvingphd/fsds_100719_cohort_notes/master/images/matplotlib_anatomy.png" width=400></center>

- Matplotlib Figures are composed of 3 different types of objects:
    - `Figure` is the largest bucket and contains everything else. It is like a picture frame without any actual images in it.
  - `Axes` are the actual plot / image inside of the Figure / frame. 
        - this is the same `ax` as in `fig, ax = plt.subplots()` and that is returned when you create a Pandas or Seaborn figure.
        - There is an 'Axes` for each subplot in the Figure

        


```python
## Make an empty figure and ax with plt.subplots

```

### OOP Crash Course

- **Object-Oriented Programming (OOP):**
    - OOP is all about working with Classes, which are the blueprints for a type of variable. 
    - Objects/classes can have functions that are attached to the object. 
        - When a function is attached to a class its called a **method**
    - Object can also store variables inside themselves. 
        - When a variable is attached to an object it is called an **attribute**
        
        
- OOP Examples You already Know:
    - `pd.DataFrame` is a class
    - `df = pd.DataFrame(...)` creates a dataframe object.
    - DataFrames have **atrributes**:
        - `df.columns`
        - `df.index`
        - `df.dtypes`
    - DataFrames have **methods**
        - `df.head()`
        - `df.sort_values()`
        - `df.info()`
        - `df.dropna()`
        - `df.plot()`
        
        
- You will learn about classes in Phase 3.
        


```python
## Run help on fig 

```


```python
# Run help on ax

```

#### Anatomy of an Axes
- `Axes` contain information about the titles, labels, grid,background, they also contain an. See the figure below for the contents of `Axes`
- Inside Axes there is an `Axis` which is further divided into an `Axis.xaxis` and an `Axis.yaxis` that contain the ticks and the tick lables.

    <center><img src="https://raw.githubusercontent.com/jirvingphd/fsds_100719_cohort_notes/master/images/matplotlib_Axes_layout2.png" width=500></center>
    
- However, axes do NOT actually contain the VISUAL for the figure, just the information.
  

#### Now, let's make our figure with Method 02


```python
#Plot 1: Sale Price Distribution - OOP

```

> Note: if you don't need to customize fonts, you can **combine all `ax.set_xxxx(` commands (`ax.set_title`,`ax.set_xlabel`, etc) into 1 `ax.set()`


```python
#Plot 1: Sale Price Distribution - OOP with ax.set()

```

## Method 03: Plot with Pandas

- Pandas's dataframes and series have a `.plot()` method
    - [Documentation](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.plot.html)
    - The `kind` argument lets us specify if we want: 
        - "scatter"
        - "hist"
        - "bar"
        - "barh"
        - etc.
    - There are also some additional plotting methods:
        - e.g.: `df.hist()`


```python
# Plot 1: Sale Price Distribution - pandas .plot kind=hist

```

>- Unfortunately .plot isn't perfect, which is why we also have `df.hist`


```python
#Plot 1: Sale Price Distribution - pandas .hist

```

## Method 04: Seaborn

- Seaborn has 2 different kinds of plotting functions. Basic ones that return an ordinary matplotlib axis and fancy/advanced ones that return an object called a "FacetGrid"


- Basic Functions (return an `ax`)
    ```python
    sns.histplot
    sns.regplot
    sns.scatter
    ```
- Advanced Functions (return a `FacetGrid`)
    ```python
    sns.lmplot
    sns.catplot
    sns.jointplot
    ```
    
- We will make our histogram 2 different ways with Seaborn. 
    - The simpler `sns.histplot` function.
    - The complex `sns.displot` function

### Seaborn - Simple Plot (returns an ax)


```python
## PLot 1 - seaborn histplot

```

### Seaborn - Advanced Plot (returns a FacetGrid)


```python
## PLot 1 - seaborn displot

```

# 🕹Activity: Answering Stakeholder Questions with EDA

## The Questions to Answer

1. What is the distribution of house prices in Ames, Iowa?
    - What is the median home price?
    - What is the average home price?
    

2. What is the relationship between square footage of the living area (`GrLivArea`)  and sale price (`SalePrice`)?
    
2. What is the average sale price for each of the different types of homes (BldgType)?

## Q1: What is the distribution of house prices in Ames, Iowa? 


```python
## Make a larger fig/ax before plotting

```

### Customizing Xtick Label Formatting
- https://matplotlib.org/stable/gallery/pyplots/dollar_ticks.html?highlight=tick
- Let's make our price ticks look more professional
    - Add $'s 
    - Add , separator for thousands
    - Show 2 decimal places


```python
## Make a larger fig/ax before plotting

```

### A1

-   

## Q2: What is the relationship between square footage of the living area (GrLivArea) and sale price (SalePrice)?

- Figure Requirements:
    - Markers are small enough to see distinct dots
    - Price-Formatted y axis
    - Regression line added 
    - Make sure the regression line is easily distinguishable vs the markers


```python
## Plot 2 with seaborn - scatterplot

```


```python
## Plot 2 with seaborn + regression line

```

### A2

-   

## Q3: What is the average sale price for each of the different types of homes (BldgType)?

- Figure Requirements:
    - Bars are sorted in order from largest to smallest mean.
    - Bars have error bars added
    - Price-Formatted y axis


```python
# Use sns.barplot to plot blgdtype vs saleprice (dont worry about order of bars yet)

```

#### Plot 3: Average Sale Price by BldgType


```python
# Use sns.barplot to plot blgdtype vs saleprice - ordered correctly
## First, calculate the correct order for the bars

```


```python
## Now use the info from abvoe to order the bars

```

### A3

-   

### EDA Wrap Up

- We did it! We used our awesome visualization techniques to provide visual answers to real questions. 
- We chose the ideal figures to best answer the questions.

# BRANCH/DECISION POINT:

- IF we are low on time, which would we rather cover?
    - A) Multiple subplots
    - B) Customizing aesthetics

## More Advanced Plots - Subplots

- Combine our plots for Q2 and Q3 into 1


```python
# Combined subplots


### Plot 0


### Plot 1


## Fix overlap

## Add suptitle big and bold

```


```python
## past the full thing into thsi cell and make a function
## make it takes the df and lets move the suptitle to the function definiton

```

# Customizing Figure Aesthetics

## Quick & Easy Visual Overhaul

### Matplotlib Styles
- https://matplotlib.org/stable/gallery/style_sheets/style_sheets_reference.html


```python
## List of available styles
plt.style.available
```


```python
## loop through all styles and use our plotting function.
# changen the suptitle to be the style name
    
    ## Use with plt.style.context to TEMPORARILY change the style

```

>- Once you've found a style and want to apply it to the entire notebook, add the following command at the top of your notebook right after importing matplotlib
```python 
plt.style.use('seaborn-talk')
```

### Seaborn Themes/Contexts/Colorpalette

- https://seaborn.pydata.org/api.html#themeing
- https://seaborn.pydata.org/tutorial/color_palettes.html


```python
sns.color_palette("rocket")
```


```python
palette = sns.color_palette('rocket')
sns.barplot(data=df,x='BldgType',y='SalePrice',palette=palette)
```


```python
sns.set_palette('rocket')
final_eda_figure(df)
```

# All Resources:


### **Matplotlib Documentation**
- [Markers](https://matplotlib.org/3.1.1/api/markers_api.html)
- [Colors](https://matplotlib.org/3.1.0/gallery/color/named_colors.html )
- [Text](https://matplotlib.org/3.1.0/tutorials/text/text_intro.html )
- [Text Properties](https://matplotlib.org/3.1.1/tutorials/text/text_props.html)

- [Tick Formatters](https://matplotlib.org/3.1.1/gallery/ticks_and_spines/tick-formatters.html)


### Cheat Sheets

- [Matplotlib and Seaborn Pages in our Master Cheat sheets pdf](https://drive.google.com/open?id=1PxRAhlaK7ucf0S2F732eJ94ovaPtUSE_)

- **Bookmark this article, its the best explanation of how matploblib'S 2 interfaces work:**
> ["Artist" in Matplotlib - something I wanted to know before spending tremendous hours on googling how-tos.](https://dev.to/skotaro/artist-in-matplotlib---something-i-wanted-to-know-before-spending-tremendous-hours-on-googling-how-tos--31oo)<br>

# APPENDIX

## Alternative Plots For Each Q

### Q2 Alternatives

#### Plot 2: Sqft Living Area vs Sale Price - plt functions


```python
# Plot 2: sqft vs sale price
plt.figure(figsize=(8,4))
plt.scatter(df['GrLivArea'], df['SalePrice'],s=2)
plt.ylabel('Sale Price ($)')
plt.xlabel('Sqft Living Area')
plt.title('Sqft vs Sale Price');
```

#### Plot 2: Sqft Living Area vs Sale Price - oop


```python
## Plot 2 with OOP
fig,ax = plt.subplots()
ax.scatter(df['GrLivArea'], df['SalePrice'],s=2)
ax.set_ylabel('Sale Price ($)')
ax.set_xlabel('Sqft Living Area')
ax.set_title('Sqft vs Sale Price');
```


```python
# Plot 2 with OOP and .set()
fig,ax = plt.subplots()
ax.scatter(df['GrLivArea'], df['SalePrice'],s=2)
ax.set(ylabel='Sale Price ($)', xlabel='Sqft Living Area', title='Sqft vs Sale Price');
```

#### Plot 2: Sqft Living Area vs Sale Price


```python
## Plot 2 with pandas
df.plot(kind='scatter',x='GrLivArea', y='SalePrice',s=2,title='Sqft vs Sale Price');
```

## Q3 Alternatives

#### Plot 3: Average Sale Price by BldgType


```python
# Prepare Data - Plot 3: avg sale price by blgtype
bldgtype_means = df.groupby('BldgType').mean()['SalePrice']
bldgtype_means
```


```python
# Plot 3: avg sale price by blgtype
plt.bar(bldgtype_means.index, bldgtype_means.values)
plt.ylabel('Mean Sale Price ($)')
plt.xlabel('Building Type')
plt.title('Average Sale Price by Building Type');
```

#### Plot 3+Level Up: Average Sale Price by BldgType - with Error Bars


```python
# Level Up - Prepare Data - Plot 3: avg sale price by blgtype
bldgtype_means = df.groupby('BldgType').agg(['mean','std','count'])['SalePrice']
bldgtype_means['Std Error'] = bldgtype_means['std']/ np.sqrt(bldgtype_means['count'])
bldgtype_means
```


```python
# Plot 3-LevelUp: avg sale price by blgtype
plt.bar(bldgtype_means.index, bldgtype_means['mean'],yerr = bldgtype_means['Std Error'])
plt.ylabel('Mean Sale Price ($)')
plt.xlabel('Building Type')
plt.title('Average Sale Price by Building Type');
```

### Method 02: Using Matplotlib OOP Interface


```python
# Prepare Data - Plot 3: avg sale price by blgtype
bldgtype_means = df.groupby('BldgType').mean()['SalePrice']
bldgtype_means
```


```python
# Plot 3: avg sale price by blgtype
fig,ax = plt.subplots()
ax.bar(bldgtype_means.index, bldgtype_means.values)
ax.set(ylabel='Mean Sale Price ($)', xlabel='Building Type', 
       title='Average Sale Price by Building Type');
```

### Method 03: Pandas

#### Plot 3: Average Sale Price by BldgType


```python
df.groupby('BldgType').mean()['SalePrice'].plot(kind='bar');
```


```python

```
