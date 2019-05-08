#CheatSheet

#####Two Libraries: matplotlib, seaborn

####subplot: plt.subplot(1, 2, 2)

###Bar Chart

Barchart: `sb.countplot(data =  , x = ' ', color =  )` 

Uniform color: 
`sb.sb.color_palette()[0]`

Order and calculate proportion:

1.Get order by frequency: 

```python
type_counts = data['type'].value_counts()
type_order = type_counts.index
```
2.Get largestest proportion:

```python
number = data['species'].unique().shape[0]
max_type_count = type_counts[0]
max_prop = max_type_count / number
```

3.Establish tick locations and create plot

```python
tick_props = np.arange(0, max_prop, 0.02)
tick_names = ['{:0.2f}'.format(v) for v in tick_props]
plt.xticks(tick_props * number, tick_names)
plt.xlabel('Proportion')
```
###Histogram

```python
bins = np.arange(20, data['count'].max() + 5, 5)
plt.hist(data['count'], bins = bins)
```

###Scale

```python
plt.xlim((0,6)) //Only choose those from 0 - 6
plt.xscale('log') //Use log to show the result evenly
```

###Scatterplots

```python
plt.scatter(data = df, x = 'num_var1', y = 'num_var2')
//For transparency, add parameter alpha (.range[0, 1], .value[not visible, fully opaque]
//For jitter (clustering together), add parameter to move the position of each point slightly from its true value
sb.regplot(data = df, x = 'disc_var1', y = 'disc_var2', fit_reg = False,
           x_jitter = 0.2, y_jitter = 0.2, scatter_kws = {'alpha' : 1/3})
```

Scatterplot with a regression line in the middle.

```python
sb.regplot(data = df, x = 'num_var1', y = 'num_var2')
```

###Heatmap

```python
plt.subplot(1, 2, 2)
bins_x = np.arange(0.5, 10.5+1, 1)
bins_y = np.arange(-0.5, 10.5+1, 1)
plt.hist2d(data = df, x = 'disc_var1', y = 'disc_var2',
           bins = [bins_x, bins_y])
plt.colorbar();
```

###Violin Plots

x-axis for categorical variable and y-axis for quantitative. The violin plots show the distribution among each category.

```python
sb.violinplot(data = df, x = 'cat_var', y = 'num_var')
//Inside each curve, there is a black shape with a white dot inside.
//The black line shows 95% of the data, black shape from 25% to 75% and the white dot shows the median.
//There is also a parameter called inner, indicating the contents inside each figure.
```

###Box Plots

```python
sb.boxplot(data = df, x = 'cat_var', y = 'num_var', color = base_color)
```

###Clustered Bar Charts

```python
ax = sb.countplot(data = df, x = 'cat_var1', hue = 'cat_var2')
ax.legend(loc = 8, ncol = 3, framealpha = 1, title = 'cat_var2')
//In a heatmap
ct_counts = df.groupby(['cat_var1', 'cat_var2']).size()
ct_counts = ct_counts.reset_index(name = 'count')
ct_counts = ct_counts.pivot(index = 'cat_var2', columns = 'cat_var1', values = 'count')
sb.heatmap(ct_counts)
sb.heatmap(ct_counts, annot = True, fmt = 'd') //label
```

###Faceting

**Axises are consistent throughout the whole graph**

```python
bin_edges = np.arange(-3, df['num_var'].max()+1/3, 1/3)
g = sb.FacetGrid(data = df, col = 'cat_var')
g.map(plt.hist, "num_var", bins = bin_edges) //bins = np.arange(5, 15+1, 1)
g.set_titles('{col_name}')
```

###Adapted Bar Charts

```python
base_color = sb.color_palette()[0]
sb.barplot(data = df, x = 'cat_var', y = 'num_var', color = base_color)
//The bar heights indicate the mean value on the numeric variable, and the lines means the range of of certain category.
```

###Line Plot

```python
#normal line plot
plt.errorbar(data = df, x = 'num_var1', y = 'num_var2')

# set bin edges, compute centers
bin_size = 0.25
xbin_edges = np.arange(0.5, df['num_var1'].max()+bin_size, bin_size)
xbin_centers = (xbin_edges + bin_size/2)[:-1]

# plot the summarized data
plt.errorbar(x = xbin_centers)
plt.xlabel('num_var1')
plt.ylabel('num_var2')
```

###Stacked Plots

```python
cat1_order = ['East', 'South', 'West', 'North']
cat2_order = ['Type X', 'Type Y', 'Type Z', 'Type O']

artists = [] # for storing references to plot elements
baselines = np.zeros(len(cat1_order))
cat1_counts = df['cat_var1'].value_counts()

# for each second-variable category:
for i in range(len(cat2_order)):
    # isolate the counts of the first category,
    cat2 = cat2_order[i]
    inner_counts = df[df['cat_var2'] == cat2]['cat_var1'].value_counts()
    inner_props = inner_counts / cat1_counts
    # then plot those counts on top of the accumulated baseline
    bars = plt.bar(x = np.arange(len(cat1_order)),
                   height = inner_props[cat1_order],
                   bottom = baselines)
    artists.append(bars)
    baselines += inner_props[cat1_order]

plt.xticks(np.arange(len(cat1_order)), cat1_order)
plt.legend(reversed(artists), reversed(cat2_order), framealpha = 1, bbox_to_anchor = (1, 0.5), loc = 6);
```

###Encoding

#####via Shape

```python
cat_markers = [['A', 'o'],  //circle
               ['B', 's']]  //square
# triangle -> ^, X -> X

for cat, marker in cat_markers:
    df_cat = df[df['cat_var1'] == cat]
    plt.scatter(data = df_cat, x = 'num_var1', y = 'num_var2', marker = marker)
plt.legend(['A','B'])
```

#####via Size

```python
plt.scatter(data = df, x = 'num_var1', y = 'num_var2', s = 'num_var3')

# dummy series for adding legend
sizes = [20, 35, 50]
base_color = sb.color_palette()[0]
legend_obj = []
for s in sizes:
    legend_obj.append(plt.scatter([], [], s = s, color = base_color))
plt.legend(legend_obj, sizes)
```

#####via Color

```python
sb.palplot(sb.color_palette(n_colors=9))
sb.palplot(sb.color_palette('vlag', 9))

g = sb.FacetGrid(data = df, hue = 'cat_var1', size = 5, palette = 'colorblind')
g.map(plt.scatter, 'num_var1', 'num_var2')
g.add_legend()
```