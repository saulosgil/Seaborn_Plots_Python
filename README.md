# Commonly plots using Seaborn from Python

This script shows 10 commonly plots using Seaborn library from Python.

1. Bar Plots
2. Count Plots
3. Histograms
4. Cat Plots (Box, Violin, Swarm, Boxen)
5. Multiple Plots using FacetGrid
6. Joint Plots
7. KDE Plots
8. Pairplots
9. Heatmaps
10. Scatter Plots

- Libraries

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib
import matplotlib.pyplot as plt

%matplotlib inline
```

- Import datasets

```python
exercise = sns.load_dataset('exercise')
iris = sns.load_dataset('iris')
penguins = sns.load_dataset('penguins')
mpg = sns.load_dataset('mpg')
titanic = sns.load_dataset('titanic')
tips = sns.load_dataset('tips')
```

# 1) Bar plots

Bar plots offer a means to visually represent diverse data sets, including counts, frequencies, percentages, or averages.

They prove especially valuable for illustrating and contrasting data across various categories.

```python
titanic.head(5)
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>survived</th>
      <th>pclass</th>
      <th>sex</th>
      <th>age</th>
      <th>sibsp</th>
      <th>parch</th>
      <th>fare</th>
      <th>embarked</th>
      <th>class</th>
      <th>who</th>
      <th>adult_male</th>
      <th>deck</th>
      <th>embark_town</th>
      <th>alive</th>
      <th>alone</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>7.2500</td>
      <td>S</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>71.2833</td>
      <td>C</td>
      <td>First</td>
      <td>woman</td>
      <td>False</td>
      <td>C</td>
      <td>Cherbourg</td>
      <td>yes</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>3</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>7.9250</td>
      <td>S</td>
      <td>Third</td>
      <td>woman</td>
      <td>False</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>yes</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>1</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>53.1000</td>
      <td>S</td>
      <td>First</td>
      <td>woman</td>
      <td>False</td>
      <td>C</td>
      <td>Southampton</td>
      <td>yes</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>8.0500</td>
      <td>S</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>

- Categorical comparison

```python
# Simple bar plot
sns.barplot(data=titanic, x='who', y='age', estimator='mean', errorbar=None, palette='viridis')
plt.title('Simple Barplot')
plt.xlabel('Person')
plt.ylabel('Average')
plt.show()
```

![png](images/output_10_1.png)

- Proportional Representation through Stacked Bar Charts

```python
# prepare dataset - groupby
data = titanic.groupby('embark_town').agg({'who':'count','sex': lambda x: (x=='male').sum()}).reset_index()
data.rename(columns={'who':'total', 'sex':'male'}, inplace=True)
data.sort_values('total', inplace=True)

```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>embark_town</th>
      <th>total</th>
      <th>male</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Queenstown</td>
      <td>77</td>
      <td>41</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Cherbourg</td>
      <td>168</td>
      <td>95</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Southampton</td>
      <td>644</td>
      <td>441</td>
    </tr>
  </tbody>
</table>
</div>

```python
# Barplot Showing Part of Total
sns.set_color_codes("pastel")
sns.barplot(x="total", y="embark_town", data=data,
            label="Female", color="b")
sns.set_color_codes("muted")
sns.barplot(x="male", y="embark_town", data=data,
            label="Male", color="b")
plt.title('Barplot Showing Part of Total')
plt.xlabel('Number of Persons')
plt.legend(loc='upper right')
plt.show()
```

![png](images/output_13_0.png)

- Comparison of Subcategories within each category through Clustered Bar Plots

```python
# Clustered barplot
sns.barplot(data=titanic, x='class', y='age', hue='sex', estimator='mean', errorbar=None, palette='viridis')
plt.title('Clustered Barplot')
plt.xlabel('Class')
plt.ylabel('Average Age')
plt.show()
```

![png](images/output_15_1.png)

# 2) Count plots

A count plot exhibits the occurrences of each category within a categorical variable.

On the x-axis lie the variable's categories, while the y-axis displays the count or frequency of each category.

- Frequency Distribution of categorical variables

```python
# Simple Countplot
sns.countplot(data=titanic, x='alive', palette='viridis')
plt.title('Simple Countplot')
plt.show()
```

![png](images/output_19_1.png)

- Relationship between different categorical variables

```python
# Clustered Countplot
sns.countplot(data=titanic, y="who", hue="alive", palette='viridis')
plt.title('Clustered Countplot')
plt.show()
```

![png](images/output_21_1.png)

# 3) Histograms

Histograms visually depict the distribution of a dataset, offering insights into its key characteristics such as normality, skewness, or presence of multiple peaks.

They showcase the frequency or count of observations across various intervals or "bins" of the data.

Let's use iris dataset.

```python
iris.head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
      <th>species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
  </tbody>
</table>
</div>

- Visualize the shape, centre, range and spread of a continuous/numeric variable and to identify any patterns or outliers

```python
# Histogram with KDE
sns.histplot(data=iris, x='sepal_width', kde=True)
plt.title('Histogram with KDE')
plt.show()
```

![png](images/output_26_0.png)

- Compare the distribution of many continuous variables

```python
# Histogram with multiple features
sns.histplot(data=iris[['sepal_length','sepal_width']])
plt.title('Multi-Column Histogram')
plt.show()
```

![png](images/output_28_1.png)

- Compare the distribution of a continuous variable for different categories

```python
sns.histplot(iris, x='sepal_length', hue='species', multiple='stack', linewidth=0.5)
plt.title('Stacked Histogram')
plt.show()
```

![png](images/output_30_1.png)

# 4) Cat Plots (Box, Violin, Swarm, Boxen)

Catplot is a flexible higher-level function that integrates various categorical seaborn plots including boxplots, violinplots, swarmplots, pointplots, barplots, and countplots.

Now, let's use tips dataset.

```python
tips.head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
      <th>sex</th>
      <th>smoker</th>
      <th>day</th>
      <th>time</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16.99</td>
      <td>1.01</td>
      <td>Female</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.34</td>
      <td>1.66</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.01</td>
      <td>3.50</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.68</td>
      <td>3.31</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24.59</td>
      <td>3.61</td>
      <td>Female</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>

- Boxplot

```python
# Boxplot
sns.boxplot(data=tips, x='time', y='total_bill', hue='sex', palette='viridis')
plt.title('Boxplot')
plt.show()
```

![png](images/output_35_1.png)

- Violin plot

```python
# Violinplot
sns.violinplot(data=tips, x='day', y='total_bill', palette='viridis')
plt.title('Violinplot')
plt.show()
```

![png](images/output_37_1.png)

- Swarm plot

```python
sns.swarmplot(data=tips, x='time', y='tip', dodge=True, palette='viridis', hue='sex', s=6)
plt.title('SwarmPlot')
plt.show()
```

![png](images/output_39_0.png)

- StripPlot

```python
#StripPlot
sns.stripplot(data=tips, x='tip', hue='size', y='day', s=25, alpha=0.2, jitter=False, marker='D',palette='viridis')
plt.title('StripPlot')
plt.show()
```

![png](images/output_41_1.png)

# 5) Multiple Plots using FacetGrid

FacetGrid, a component of the seaborn library, enables the creation of multiple data subsets arranged in a grid format. Each plot within the grid represents a specific category, determined by the column names specified in the 'col' and 'row' attributes of FacetGrid().

The plots within the grid can encompass various plot types supported by seaborn, including scatter plots, line plots, bar plots, and histograms.

For example, we utilized exercise dataset

```python
exercise.head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>id</th>
      <th>diet</th>
      <th>pulse</th>
      <th>time</th>
      <th>kind</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1</td>
      <td>low fat</td>
      <td>85</td>
      <td>1 min</td>
      <td>rest</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>low fat</td>
      <td>85</td>
      <td>15 min</td>
      <td>rest</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>1</td>
      <td>low fat</td>
      <td>88</td>
      <td>30 min</td>
      <td>rest</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>2</td>
      <td>low fat</td>
      <td>90</td>
      <td>1 min</td>
      <td>rest</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>2</td>
      <td>low fat</td>
      <td>92</td>
      <td>15 min</td>
      <td>rest</td>
    </tr>
  </tbody>
</table>
</div>

- Boxplots for pulse rate during different activities

```python
# Creating subplots using FacetGrid
g = sns.FacetGrid(exercise, col='kind', palette='Paired')

# Drawing a plot on every facet
g.map(sns.boxplot, 'pulse')
g.set_titles(col_template="Pulse rate for {col_name}")

```

![png](images/output_46_2.png)

- Scatter plots for flipper length and body mass of Penguins from different islands

For this plot, we utilized penguins dataset

```python
penguins.head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>species</th>
      <th>island</th>
      <th>bill_length_mm</th>
      <th>bill_depth_mm</th>
      <th>flipper_length_mm</th>
      <th>body_mass_g</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Adelie</td>
      <td>Torgersen</td>
      <td>39.1</td>
      <td>18.7</td>
      <td>181.0</td>
      <td>3750.0</td>
      <td>Male</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Adelie</td>
      <td>Torgersen</td>
      <td>39.5</td>
      <td>17.4</td>
      <td>186.0</td>
      <td>3800.0</td>
      <td>Female</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Adelie</td>
      <td>Torgersen</td>
      <td>40.3</td>
      <td>18.0</td>
      <td>195.0</td>
      <td>3250.0</td>
      <td>Female</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Adelie</td>
      <td>Torgersen</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Adelie</td>
      <td>Torgersen</td>
      <td>36.7</td>
      <td>19.3</td>
      <td>193.0</td>
      <td>3450.0</td>
      <td>Female</td>
    </tr>
  </tbody>
</table>
</div>

```python
# Creating subplots using FacetGrid
g = sns.FacetGrid(penguins, col='island', hue='sex', palette='Paired')

# Drawing a plot on every facet
g.map(sns.scatterplot, 'flipper_length_mm', 'body_mass_g')
g.set_titles(template="Penguins of {col_name} Island")
g.add_legend()
```

![png](images/output_50_1.png)

# 6) Joint Plots

A joint plot consolidates various univariate and bivariate plots within one figure. The focal point usually features a scatter plot or a hexbin plot, illustrating the joint distribution of the two variables.

Alongside, additional plots along the axes, such as histograms or Kernel Density Estimations (KDEs), depict the individual distributions of each variable.

Let's use mpg dataset to see some examples.

- Comparison of the displacement and mpg for cars

```python
# Hex Plot with Histogram margins
sns.jointplot(x="mpg", y="displacement", data=mpg, height=5, kind='hex', ratio=2, marginal_ticks=True)
```

![png](images/output_54_1.png)

- Comparison of acceleration and horsepower for cars from different countries

```python
# Scatter Plot with KDE Margins
sns.jointplot(x="horsepower", y="acceleration", data=mpg, hue="origin", height=5, ratio=2, marginal_ticks=True)
```

![png](images/output_56_2.png)

# 7) KDE Plots

A KDE (Kernel Density Estimate) plot is a smoothed rendition of a histogram, showcasing the probability density function of a continuous random variable.

The y-axis denotes the density or probability of observing a specific value of the variable, while the x-axis signifies the values of the variable itself.

- Comparing the horsepower of cars with respect to number of cylinders

```python
#Overlapping KDE Plots
sns.kdeplot(data=mpg, x='horsepower', hue='cylinders', fill=True,
           palette='viridis', alpha=.5, linewidth=0)
plt.title('Overlapping KDE Plot')
plt.show()
```

![png](images/output_60_1.png)

- Comparing the weight of cars across different countries

```python
#Stacked KDE Plots
sns.kdeplot(data=mpg, x="weight", hue="origin", multiple="stack")
plt.title('Stacked KDE Plot')
plt.show()
```

![png](images/output_62_1.png)

# 8) Pairplots

A pair plot is a visualization technique that enables exploration of relationships between multiple variables within a dataset. It comprises a grid of scatter plots, where each variable is plotted against every other variable.

Along the diagonal, histograms or density plots for each variable illustrate the distribution of values.

- Visualisation of relationship between different features of penguins

```python
#Simple Pairplot
sns.pairplot(data=penguins, corner=True)
```

![png](images/output_66_1.png)

```python
# Pairplot with hues
sns.pairplot(data=penguins, hue='species')
```

![png](images/output_67_2.png)

# 9) Heatmaps

Heatmaps serve as visual representations utilizing color-coded cells to exhibit the values within a matrix or data table.

Within a heatmap, the rows and columns of the matrix represent distinct variables, while the intensity of each cell's color depicts the magnitude or value of the data point at the intersection of those variables.

```python
#Selection of numeric columns from the dataset
num_cols = list(mpg.select_dtypes(include='number'))
fig = plt.figure(figsize=(12,7))

#Correlation Heatmap
sns.heatmap(data=mpg[num_cols].corr(),
            annot=True, cmap=sns.cubehelix_palette(as_cmap=True))
plt.title('Heatmap of Correlation matrix')
plt.show()
```

![png](images/output_70_0.png)

# 10) Scatter Plots

A scatterplot illustrates the correlation between two continuous variables by plotting individual data points on a graph, where one variable is depicted on the x-axis and the other on the y-axis.

The resulting plot displays multiple points scattered across the graph, hence earning the name "scatterplot."

Scatter plots serve several purposes in data analysis and visualization:

- Visualizing Relationships: They help in understanding the relationship between two continuous variables. For example, they can reveal if there's a positive, negative, or no correlation between the variables.

- Identifying Patterns: Scatter plots can help identify patterns or trends in data, such as clusters or outliers, which may not be apparent from summary statistics alone.

- Assessing Correlation: They allow for a quick assessment of the strength and direction of the relationship between variables. Strong correlations often result in a more structured or linear arrangement of points, while weak correlations may result in a more scattered arrangement.

- Checking for Linearity: Scatter plots are useful for assessing whether a linear model is appropriate for the data. If the points form a clear linear pattern, linear regression may be a suitable modeling technique.

- Visualizing Distribution: Scatter plots provide insights into the distribution of data points along both the x-axis and y-axis, which can help in understanding the overall shape of the data distribution.

```python
# Simple Scatterplot
sns.scatterplot(data=mpg, x='weight', y='horsepower', s=150, alpha=0.7)
plt.title('Simple Scatterplot')
plt.show()
```

![png](images/output_73_0.png)

```python
# Scatterplot with Hue
sns.scatterplot(data=mpg, x='weight', y='horsepower', s=150, alpha=0.7,
               hue='origin', palette='viridis')
plt.title('Scatterplot with Hue')
plt.show()
```

![png](images/output_74_0.png)

```python
# Scatterplot with Hue and Markers
sns.scatterplot(data=mpg, x='weight', y='horsepower', s=150, alpha=0.7,
              style='origin',palette='viridis', hue='origin')
plt.title('Scatterplot with Hue and Markers')
plt.show()
```

![png](images/output_75_0.png)

```python
# Scatterplot with Hue & Size
sns.scatterplot(data=mpg, x='weight', y='horsepower', sizes=(40, 400), alpha=.5,
              palette='viridis', hue='origin', size='cylinders')
plt.title('Scatterplot with Hue & Size')
plt.show()
```

![png](images/output_76_0.png)

# More details can be seen in the links below:

1. [Bar Plots](https://seaborn.pydata.org/generated/seaborn.barplot.html)

2. [Count Plots](https://seaborn.pydata.org/generated/seaborn.countplot.html)

3. [Histograms](https://seaborn.pydata.org/generated/seaborn.histplot.html)

4. [Cat Plots (Box, Violin, Swarm, Boxen)](https://seaborn.pydata.org/generated/seaborn.catplot.html)

5. [Multiple Plots using FacetGrid](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html)

6. [Joint Plots](https://seaborn.pydata.org/generated/seaborn.jointplot.html)

7. [KDE Plots](https://seaborn.pydata.org/generated/seaborn.kdeplot.html)

8. [Pairplots](https://seaborn.pydata.org/generated/seaborn.pairplot.html)

9. [Heatmaps](https://seaborn.pydata.org/generated/seaborn.heatmap.html)

   10.[ Scatter Plots](https://seaborn.pydata.org/generated/seaborn.scatterplot.html)

## References

1 - [Ten Must-Know Seaborn Plots](https://medium.com/@snehabajaj108/ten-must-know-seaborn-plots-1f3a82dc99c5);

2 - [Seaborn: statistical data visualization](https://seaborn.pydata.org/index.html)
