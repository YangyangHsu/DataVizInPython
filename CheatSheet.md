#CheatSheet

#####Two Libraries: matplotlib, seaborn

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
`plt.xlim((0,6)) //Only choose those from 0 - 6`
`plt.xscale('log') //Use log to show the result evenly`