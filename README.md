
# Read the data😂


```python
import pandas as pd
import numpy as np
```


```python
data = pd.read_csv('./data/BreadBasket_DMS.csv')
data.head()
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
      <th>Date</th>
      <th>Time</th>
      <th>Transaction</th>
      <th>Item</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-10-30</td>
      <td>09:58:11</td>
      <td>1</td>
      <td>Bread</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-10-30</td>
      <td>10:05:34</td>
      <td>2</td>
      <td>Scandinavian</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-10-30</td>
      <td>10:05:34</td>
      <td>2</td>
      <td>Scandinavian</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-10-30</td>
      <td>10:07:57</td>
      <td>3</td>
      <td>Hot chocolate</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-10-30</td>
      <td>10:07:57</td>
      <td>3</td>
      <td>Jam</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.shape
```




    (21293, 4)



# Clead the data😥


```python
len(data[data["Item"] == "NONE"])
```




    786




```python
data['Item'] = data['Item'].replace("NONE",np.nan).dropna()
```


```python
data = data.dropna()
data.shape
```




    (20507, 4)



# Extract Hour & insert into column😀


```python
import re
from pandas import Series

pattern_hour = '[0-9][0-9]'

hourlist = []
for item in data['Time']:
    hour = re.findall(pattern_hour,item)[0]
    hourlist.append(hour)
#hourlist
```


```python
data.insert(2,'Hour',Series(hourlist))
```


```python
data.head()
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
      <th>Date</th>
      <th>Time</th>
      <th>Hour</th>
      <th>Transaction</th>
      <th>Item</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-10-30</td>
      <td>09:58:11</td>
      <td>09</td>
      <td>1</td>
      <td>Bread</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-10-30</td>
      <td>10:05:34</td>
      <td>10</td>
      <td>2</td>
      <td>Scandinavian</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-10-30</td>
      <td>10:05:34</td>
      <td>10</td>
      <td>2</td>
      <td>Scandinavian</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-10-30</td>
      <td>10:07:57</td>
      <td>10</td>
      <td>3</td>
      <td>Hot chocolate</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-10-30</td>
      <td>10:07:57</td>
      <td>10</td>
      <td>3</td>
      <td>Jam</td>
    </tr>
  </tbody>
</table>
</div>



---

# Analysis methods😀

- Busy Hours Analysis
- Popular Items Analysis
- Transaction Analysis

---

# Busy Hours Analysis


```python
hour_counts = data['Hour'].value_counts().sort_index()
```

## Busy Hour Bar Chart


```python
from pyecharts import Bar

attr = list(map(int, hour_counts.index))
v =  hour_counts.values

bar = Bar("Busy Hour")
bar.add("", attr, v, mark_line=["max"],is_label_show=True,is_more_utils=True)

bar.render('./charts/Busy-Hour.html')
bar
```




<script>
    require.config({
        paths: {
            'echarts': '/nbextensions/echarts/echarts.min'
        }
    });
</script>
    <div id="868d1a2dfb7b4ba8bb3e4e9fca5acf86" style="width:800px;height:400px;"></div>


<script>
    require(['echarts'], function(echarts) {
        
var myChart_868d1a2dfb7b4ba8bb3e4e9fca5acf86 = echarts.init(document.getElementById('868d1a2dfb7b4ba8bb3e4e9fca5acf86'), 'light', {renderer: 'canvas'});

var option_868d1a2dfb7b4ba8bb3e4e9fca5acf86 = {
    "title": [
        {
            "text": "Busy Hour",
            "left": "auto",
            "top": "auto",
            "textStyle": {
                "fontSize": 18
            },
            "subtextStyle": {
                "fontSize": 12
            }
        }
    ],
    "toolbox": {
        "show": true,
        "orient": "vertical",
        "left": "95%",
        "top": "center",
        "feature": {
            "saveAsImage": {
                "show": true,
                "title": "save as image"
            },
            "restore": {
                "show": true,
                "title": "restore"
            },
            "dataView": {
                "show": true,
                "title": "data view"
            },
            "magicType": {
                "show": true,
                "type": [
                    "line",
                    "bar",
                    "stack",
                    "tiled"
                ],
                "title": {
                    "line": "\u6298\u7ebf\u56fe",
                    "bar": "\u67f1\u72b6\u56fe",
                    "stack": "\u5806\u53e0",
                    "tiled": "\u5e73\u94fa"
                }
            },
            "dataZoom": {
                "show": true,
                "title": {
                    "zoom": "\u533a\u57df\u7f29\u653e",
                    "back": "\u7f29\u653e\u8fd8\u539f"
                }
            }
        }
    },
    "series_id": 727277,
    "tooltip": {
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "backgroundColor": "rgba(50,50,50,0.7)",
        "borderColor": "#333",
        "borderWidth": 0
    },
    "series": [
        {
            "type": "bar",
            "data": [
                1.0,
                24.0,
                625.0,
                1892.0,
                2560.0,
                3002.0,
                2753.0,
                2505.0,
                2530.0,
                2037.0,
                1295.0,
                353.0,
                80.0,
                47.0,
                22.0,
                3.0,
                8.0,
                3.0
            ],
            "barCategoryGap": "20%",
            "label": {
                "normal": {
                    "show": true,
                    "position": "top",
                    "textStyle": {
                        "fontSize": 12
                    }
                },
                "emphasis": {
                    "show": true,
                    "textStyle": {
                        "fontSize": 12
                    }
                }
            },
            "markPoint": {
                "data": []
            },
            "markLine": {
                "data": [
                    {
                        "type": "max",
                        "name": "Maximum"
                    }
                ],
                "symbolSize": 10
            },
            "seriesId": 727277
        }
    ],
    "legend": [
        {
            "data": [
                ""
            ],
            "selectedMode": "multiple",
            "show": true,
            "left": "center",
            "top": "top",
            "orient": "horizontal",
            "textStyle": {
                "fontSize": 12
            }
        }
    ],
    "animation": true,
    "xAxis": [
        {
            "show": true,
            "nameLocation": "middle",
            "nameGap": 25,
            "nameTextStyle": {
                "fontSize": 14
            },
            "axisTick": {
                "alignWithLabel": false
            },
            "inverse": false,
            "boundaryGap": true,
            "type": "category",
            "splitLine": {
                "show": false
            },
            "axisLine": {
                "lineStyle": {
                    "width": 1
                }
            },
            "axisLabel": {
                "interval": "auto",
                "rotate": 0,
                "margin": 8,
                "textStyle": {
                    "fontSize": 12
                }
            },
            "data": [
                1,
                7,
                8,
                9,
                10,
                11,
                12,
                13,
                14,
                15,
                16,
                17,
                18,
                19,
                20,
                21,
                22,
                23
            ]
        }
    ],
    "yAxis": [
        {
            "show": true,
            "nameLocation": "middle",
            "nameGap": 25,
            "nameTextStyle": {
                "fontSize": 14
            },
            "axisTick": {
                "alignWithLabel": false
            },
            "inverse": false,
            "boundaryGap": true,
            "type": "value",
            "splitLine": {
                "show": true
            },
            "axisLine": {
                "lineStyle": {
                    "width": 1
                }
            },
            "axisLabel": {
                "interval": "auto",
                "formatter": "{value} ",
                "rotate": 0,
                "margin": 8,
                "textStyle": {
                    "fontSize": 12
                }
            }
        }
    ],
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597",
        "#f6f5ec"
    ]
};
myChart_868d1a2dfb7b4ba8bb3e4e9fca5acf86.setOption(option_868d1a2dfb7b4ba8bb3e4e9fca5acf86);

    });
</script>




- Busy Hours start at **10a.m.** ,end at **14p.m.**

---

# Popular Items Analysis

### TOP 10


```python
item_counts = data['Item'].value_counts()
```


```python
hot_items = item_counts[:10]
hot_items
```




    Coffee           5471
    Bread            3325
    Tea              1435
    Cake             1025
    Pastry            856
    Sandwich          771
    Medialuna         616
    Hot chocolate     590
    Cookies           540
    Brownie           379
    Name: Item, dtype: int64



### Others


```python
other_items_count = data.Item.count() - hot_items.sum()
item_list = hot_items.append(pd.Series([other_items_count], index=["Others"]))
```


```python
item_list
```




    Coffee           5471
    Bread            3325
    Tea              1435
    Cake             1025
    Pastry            856
    Sandwich          771
    Medialuna         616
    Hot chocolate     590
    Cookies           540
    Brownie           379
    Others           5499
    dtype: int64



## Popular Items Pie Chart


```python
from pyecharts import Pie, Grid

attr_items = item_list.index
count_items = item_list.values

pie1 = Pie("Popular Items",title_pos='center')
pie1.add("",attr_items,count_items,
         is_label_show=True,legend_orient="vertical",legend_pos="left")

pie1.render('./charts/Popular-Items.html')
pie1
```




<script>
    require.config({
        paths: {
            'echarts': '/nbextensions/echarts/echarts.min'
        }
    });
</script>
    <div id="aa0eca169bbc4c59b594567062d77656" style="width:800px;height:400px;"></div>


<script>
    require(['echarts'], function(echarts) {
        
var myChart_aa0eca169bbc4c59b594567062d77656 = echarts.init(document.getElementById('aa0eca169bbc4c59b594567062d77656'), 'light', {renderer: 'canvas'});

var option_aa0eca169bbc4c59b594567062d77656 = {
    "title": [
        {
            "text": "Popular Items",
            "left": "center",
            "top": "auto",
            "textStyle": {
                "fontSize": 18
            },
            "subtextStyle": {
                "fontSize": 12
            }
        }
    ],
    "toolbox": {
        "show": true,
        "orient": "vertical",
        "left": "95%",
        "top": "center",
        "feature": {
            "saveAsImage": {
                "show": true,
                "title": "save as image"
            },
            "restore": {
                "show": true,
                "title": "restore"
            },
            "dataView": {
                "show": true,
                "title": "data view"
            }
        }
    },
    "series_id": 4819781,
    "tooltip": {
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "backgroundColor": "rgba(50,50,50,0.7)",
        "borderColor": "#333",
        "borderWidth": 0
    },
    "series": [
        {
            "type": "pie",
            "data": [
                {
                    "name": "Coffee",
                    "value": 5471.0
                },
                {
                    "name": "Bread",
                    "value": 3325.0
                },
                {
                    "name": "Tea",
                    "value": 1435.0
                },
                {
                    "name": "Cake",
                    "value": 1025.0
                },
                {
                    "name": "Pastry",
                    "value": 856.0
                },
                {
                    "name": "Sandwich",
                    "value": 771.0
                },
                {
                    "name": "Medialuna",
                    "value": 616.0
                },
                {
                    "name": "Hot chocolate",
                    "value": 590.0
                },
                {
                    "name": "Cookies",
                    "value": 540.0
                },
                {
                    "name": "Brownie",
                    "value": 379.0
                },
                {
                    "name": "Others",
                    "value": 5499.0
                }
            ],
            "radius": [
                "0%",
                "75%"
            ],
            "center": [
                "50%",
                "50%"
            ],
            "label": {
                "normal": {
                    "show": true,
                    "position": "outside",
                    "textStyle": {
                        "fontSize": 12
                    },
                    "formatter": "{b}: {d}%"
                },
                "emphasis": {
                    "show": true,
                    "textStyle": {
                        "fontSize": 12
                    },
                    "formatter": "{b}: {d}%"
                }
            },
            "seriesId": 4819781
        }
    ],
    "legend": [
        {
            "data": [
                "Coffee",
                "Bread",
                "Tea",
                "Cake",
                "Pastry",
                "Sandwich",
                "Medialuna",
                "Hot chocolate",
                "Cookies",
                "Brownie",
                "Others"
            ],
            "selectedMode": "multiple",
            "show": true,
            "left": "left",
            "top": "top",
            "orient": "vertical",
            "textStyle": {
                "fontSize": 12
            }
        }
    ],
    "animation": true,
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597",
        "#f6f5ec"
    ]
};
myChart_aa0eca169bbc4c59b594567062d77656.setOption(option_aa0eca169bbc4c59b594567062d77656);

    });
</script>




- Apperently, **Coffee** is the most popular and the **bread** is second.

---

# Transaction Combination Analysis


```python
coffee_transaction_list = data[data['Item'] == "Coffee"]["Transaction"].tolist()
```


```python
# make a copy
data_copy = data.copy()
```


```python
data_copy = data_copy[data_copy['Transaction'].isin(coffee_transaction_list)]
```


```python
data_copy.head()
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
      <th>Date</th>
      <th>Time</th>
      <th>Hour</th>
      <th>Transaction</th>
      <th>Item</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>2016-10-30</td>
      <td>10:13:03</td>
      <td>10</td>
      <td>5</td>
      <td>Coffee</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2016-10-30</td>
      <td>10:13:03</td>
      <td>10</td>
      <td>5</td>
      <td>Pastry</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2016-10-30</td>
      <td>10:13:03</td>
      <td>10</td>
      <td>5</td>
      <td>Bread</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2016-10-30</td>
      <td>10:19:12</td>
      <td>10</td>
      <td>7</td>
      <td>Medialuna</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2016-10-30</td>
      <td>10:19:12</td>
      <td>10</td>
      <td>7</td>
      <td>Pastry</td>
    </tr>
  </tbody>
</table>
</div>



### with-coffee-item list


```python
with_coffee_items = data_copy.Item.value_counts()[1:10]
with_coffee_items
```




    Bread            923
    Cake             540
    Tea              482
    Pastry           474
    Sandwich         421
    Medialuna        345
    Hot chocolate    293
    Cookies          283
    Toast            224
    Name: Item, dtype: int64



### hot-item list


```python
#item_list[1:10]
```


```python
hot_items = item_list.drop(labels=["Coffee"])[:9]
hot_items
```




    Bread            3325
    Tea              1435
    Cake             1025
    Pastry            856
    Sandwich          771
    Medialuna         616
    Hot chocolate     590
    Cookies           540
    Brownie           379
    dtype: int64




```python
values = hot_items.tolist()
values
```




    [3325, 1435, 1025, 856, 771, 616, 590, 540, 379]




```python
values_coffee_combine = with_coffee_items.tolist()
values_coffee_combine
```




    [923, 540, 482, 474, 421, 345, 293, 283, 224]




```python
values_without_coffee = [values[i]-v for i,v in enumerate(values_coffee_combine)]
values_without_coffee
```




    [2402, 895, 543, 382, 350, 271, 297, 257, 155]




```python
from pandas import DataFrame
coffee_compare = pd.DataFrame({'with_coffee':values_coffee_combine, 'without_coffee':values_without_coffee})
coffee_compare.index = with_coffee_items.index
```


```python
coffee_compare
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
      <th>with_coffee</th>
      <th>without_coffee</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bread</th>
      <td>923</td>
      <td>2402</td>
    </tr>
    <tr>
      <th>Cake</th>
      <td>540</td>
      <td>895</td>
    </tr>
    <tr>
      <th>Tea</th>
      <td>482</td>
      <td>543</td>
    </tr>
    <tr>
      <th>Pastry</th>
      <td>474</td>
      <td>382</td>
    </tr>
    <tr>
      <th>Sandwich</th>
      <td>421</td>
      <td>350</td>
    </tr>
    <tr>
      <th>Medialuna</th>
      <td>345</td>
      <td>271</td>
    </tr>
    <tr>
      <th>Hot chocolate</th>
      <td>293</td>
      <td>297</td>
    </tr>
    <tr>
      <th>Cookies</th>
      <td>283</td>
      <td>257</td>
    </tr>
    <tr>
      <th>Toast</th>
      <td>224</td>
      <td>155</td>
    </tr>
  </tbody>
</table>
</div>



## Coffee Combination Line Chart


```python
from pyecharts import Line

x1 = coffee_compare.index
y1 =  (coffee_compare['with_coffee'].values/coffee_compare['without_coffee'].values).round(2)

x2 = coffee_compare.index
y2 = coffee_compare['without_coffee'].values

line = Line("Coffee Combination")
line.add("with coffee", x1, y1,mark_point=["max"],is_smooth=True,xaxis_interval=0, xaxis_rotate=30, yaxis_rotate=30)
#line.add("without coffee", x2, y2,mark_point=["max"],is_smooth=True)

line.render('./charts/Coffee-Combination.html')
line
```




<script>
    require.config({
        paths: {
            'echarts': '/nbextensions/echarts/echarts.min'
        }
    });
</script>
    <div id="84712cb5dd114aad8be94ee7449707d5" style="width:800px;height:400px;"></div>


<script>
    require(['echarts'], function(echarts) {
        
var myChart_84712cb5dd114aad8be94ee7449707d5 = echarts.init(document.getElementById('84712cb5dd114aad8be94ee7449707d5'), 'light', {renderer: 'canvas'});

var option_84712cb5dd114aad8be94ee7449707d5 = {
    "title": [
        {
            "text": "Coffee Combination",
            "left": "auto",
            "top": "auto",
            "textStyle": {
                "fontSize": 18
            },
            "subtextStyle": {
                "fontSize": 12
            }
        }
    ],
    "toolbox": {
        "show": true,
        "orient": "vertical",
        "left": "95%",
        "top": "center",
        "feature": {
            "saveAsImage": {
                "show": true,
                "title": "save as image"
            },
            "restore": {
                "show": true,
                "title": "restore"
            },
            "dataView": {
                "show": true,
                "title": "data view"
            }
        }
    },
    "series_id": 1815698,
    "tooltip": {
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "backgroundColor": "rgba(50,50,50,0.7)",
        "borderColor": "#333",
        "borderWidth": 0
    },
    "series": [
        {
            "type": "line",
            "name": "with coffee",
            "symbol": "emptyCircle",
            "symbolSize": 4,
            "smooth": true,
            "step": false,
            "showSymbol": true,
            "data": [
                [
                    "Bread",
                    0.38
                ],
                [
                    "Cake",
                    0.6
                ],
                [
                    "Tea",
                    0.89
                ],
                [
                    "Pastry",
                    1.24
                ],
                [
                    "Sandwich",
                    1.2
                ],
                [
                    "Medialuna",
                    1.27
                ],
                [
                    "Hot chocolate",
                    0.99
                ],
                [
                    "Cookies",
                    1.1
                ],
                [
                    "Toast",
                    1.45
                ]
            ],
            "label": {
                "normal": {
                    "show": false,
                    "position": "top",
                    "textStyle": {
                        "fontSize": 12
                    }
                },
                "emphasis": {
                    "show": true,
                    "textStyle": {
                        "fontSize": 12
                    }
                }
            },
            "lineStyle": {
                "normal": {
                    "width": 1,
                    "opacity": 1,
                    "curveness": 0,
                    "type": "solid"
                }
            },
            "areaStyle": {
                "opacity": 0
            },
            "markPoint": {
                "data": [
                    {
                        "type": "max",
                        "name": "Maximum",
                        "symbol": "pin",
                        "symbolSize": 50,
                        "label": {
                            "normal": {
                                "textStyle": {
                                    "color": "#fff"
                                }
                            }
                        }
                    }
                ]
            },
            "markLine": {
                "data": []
            },
            "seriesId": 1815698
        }
    ],
    "legend": [
        {
            "data": [
                "with coffee"
            ],
            "selectedMode": "multiple",
            "show": true,
            "left": "center",
            "top": "top",
            "orient": "horizontal",
            "textStyle": {
                "fontSize": 12
            }
        }
    ],
    "animation": true,
    "xAxis": [
        {
            "show": true,
            "nameLocation": "middle",
            "nameGap": 25,
            "nameTextStyle": {
                "fontSize": 14
            },
            "axisTick": {
                "alignWithLabel": false
            },
            "inverse": false,
            "boundaryGap": true,
            "type": "category",
            "splitLine": {
                "show": false
            },
            "axisLine": {
                "lineStyle": {
                    "width": 1
                }
            },
            "axisLabel": {
                "interval": 0,
                "rotate": 30,
                "margin": 8,
                "textStyle": {
                    "fontSize": 12
                }
            },
            "data": [
                "Bread",
                "Cake",
                "Tea",
                "Pastry",
                "Sandwich",
                "Medialuna",
                "Hot chocolate",
                "Cookies",
                "Toast"
            ]
        }
    ],
    "yAxis": [
        {
            "show": true,
            "nameLocation": "middle",
            "nameGap": 25,
            "nameTextStyle": {
                "fontSize": 14
            },
            "axisTick": {
                "alignWithLabel": false
            },
            "inverse": false,
            "boundaryGap": true,
            "type": "value",
            "splitLine": {
                "show": true
            },
            "axisLine": {
                "lineStyle": {
                    "width": 1
                }
            },
            "axisLabel": {
                "interval": "auto",
                "formatter": "{value} ",
                "rotate": 30,
                "margin": 8,
                "textStyle": {
                    "fontSize": 12
                }
            }
        }
    ],
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597",
        "#f6f5ec"
    ]
};
myChart_84712cb5dd114aad8be94ee7449707d5.setOption(option_84712cb5dd114aad8be94ee7449707d5);

    });
</script>




- Clearly, customers are most likely to buy **toast** with **coffee**
- Customers don't like **bread**/**Cake** plus **coffee** combine

---

# Predict Coffee Sales


```python
coffee_data = data_copy[data_copy['Item']=='Coffee']
coffee_data.head()
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
      <th>Date</th>
      <th>Time</th>
      <th>Hour</th>
      <th>Transaction</th>
      <th>Item</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>2016-10-30</td>
      <td>10:13:03</td>
      <td>10</td>
      <td>5</td>
      <td>Coffee</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2016-10-30</td>
      <td>10:19:12</td>
      <td>10</td>
      <td>7</td>
      <td>Coffee</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2016-10-30</td>
      <td>10:30:14</td>
      <td>10</td>
      <td>12</td>
      <td>Coffee</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2016-10-30</td>
      <td>10:31:24</td>
      <td>10</td>
      <td>13</td>
      <td>Coffee</td>
    </tr>
    <tr>
      <th>44</th>
      <td>2016-10-30</td>
      <td>10:37:08</td>
      <td>10</td>
      <td>16</td>
      <td>Coffee</td>
    </tr>
  </tbody>
</table>
</div>




```python
grouped = coffee_data['Item'].groupby(coffee_data['Date'])
pd_coffee = DataFrame(grouped.count())
```


```python
pd_coffee.columns = ['Sales']
```


```python
pd_coffee.head()
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
      <th>Sales</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-10-30</th>
      <td>33</td>
    </tr>
    <tr>
      <th>2016-10-31</th>
      <td>60</td>
    </tr>
    <tr>
      <th>2016-11-01</th>
      <td>38</td>
    </tr>
    <tr>
      <th>2016-11-02</th>
      <td>42</td>
    </tr>
    <tr>
      <th>2016-11-03</th>
      <td>40</td>
    </tr>
  </tbody>
</table>
</div>



## Coffee-Sales Line Chart


```python
from pyecharts import Line

date = pd_coffee.index
sales =  pd_coffee.values


line = Line("Coffee-Sales")
line.add("", date, sales,mark_point=["max"],mark_line=["average"],xaxis_interval=7, xaxis_rotate=30, yaxis_rotate=30)

line.render('./charts/Coffee-Sales.html')
line
```




<script>
    require.config({
        paths: {
            'echarts': '/nbextensions/echarts/echarts.min'
        }
    });
</script>
    <div id="0c36b741ffec4047bd0618f71573d85b" style="width:800px;height:400px;"></div>


<script>
    require(['echarts'], function(echarts) {
        
var myChart_0c36b741ffec4047bd0618f71573d85b = echarts.init(document.getElementById('0c36b741ffec4047bd0618f71573d85b'), 'light', {renderer: 'canvas'});

var option_0c36b741ffec4047bd0618f71573d85b = {
    "title": [
        {
            "text": "Coffee-Sales",
            "left": "auto",
            "top": "auto",
            "textStyle": {
                "fontSize": 18
            },
            "subtextStyle": {
                "fontSize": 12
            }
        }
    ],
    "toolbox": {
        "show": true,
        "orient": "vertical",
        "left": "95%",
        "top": "center",
        "feature": {
            "saveAsImage": {
                "show": true,
                "title": "save as image"
            },
            "restore": {
                "show": true,
                "title": "restore"
            },
            "dataView": {
                "show": true,
                "title": "data view"
            }
        }
    },
    "series_id": 6504146,
    "tooltip": {
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "backgroundColor": "rgba(50,50,50,0.7)",
        "borderColor": "#333",
        "borderWidth": 0
    },
    "series": [
        {
            "type": "line",
            "symbol": "emptyCircle",
            "symbolSize": 4,
            "smooth": false,
            "step": false,
            "showSymbol": true,
            "data": [
                [
                    "2016-10-30",
                    [
                        33.0
                    ]
                ],
                [
                    "2016-10-31",
                    [
                        60.0
                    ]
                ],
                [
                    "2016-11-01",
                    [
                        38.0
                    ]
                ],
                [
                    "2016-11-02",
                    [
                        42.0
                    ]
                ],
                [
                    "2016-11-03",
                    [
                        40.0
                    ]
                ],
                [
                    "2016-11-04",
                    [
                        51.0
                    ]
                ],
                [
                    "2016-11-05",
                    [
                        71.0
                    ]
                ],
                [
                    "2016-11-06",
                    [
                        48.0
                    ]
                ],
                [
                    "2016-11-07",
                    [
                        41.0
                    ]
                ],
                [
                    "2016-11-08",
                    [
                        44.0
                    ]
                ],
                [
                    "2016-11-09",
                    [
                        32.0
                    ]
                ],
                [
                    "2016-11-10",
                    [
                        37.0
                    ]
                ],
                [
                    "2016-11-11",
                    [
                        40.0
                    ]
                ],
                [
                    "2016-11-12",
                    [
                        55.0
                    ]
                ],
                [
                    "2016-11-13",
                    [
                        41.0
                    ]
                ],
                [
                    "2016-11-14",
                    [
                        38.0
                    ]
                ],
                [
                    "2016-11-15",
                    [
                        39.0
                    ]
                ],
                [
                    "2016-11-16",
                    [
                        30.0
                    ]
                ],
                [
                    "2016-11-17",
                    [
                        38.0
                    ]
                ],
                [
                    "2016-11-18",
                    [
                        43.0
                    ]
                ],
                [
                    "2016-11-19",
                    [
                        64.0
                    ]
                ],
                [
                    "2016-11-20",
                    [
                        37.0
                    ]
                ],
                [
                    "2016-11-21",
                    [
                        30.0
                    ]
                ],
                [
                    "2016-11-22",
                    [
                        25.0
                    ]
                ],
                [
                    "2016-11-23",
                    [
                        26.0
                    ]
                ],
                [
                    "2016-11-24",
                    [
                        30.0
                    ]
                ],
                [
                    "2016-11-25",
                    [
                        38.0
                    ]
                ],
                [
                    "2016-11-26",
                    [
                        46.0
                    ]
                ],
                [
                    "2016-11-27",
                    [
                        47.0
                    ]
                ],
                [
                    "2016-11-28",
                    [
                        29.0
                    ]
                ],
                [
                    "2016-11-29",
                    [
                        24.0
                    ]
                ],
                [
                    "2016-11-30",
                    [
                        25.0
                    ]
                ],
                [
                    "2016-12-01",
                    [
                        22.0
                    ]
                ],
                [
                    "2016-12-02",
                    [
                        30.0
                    ]
                ],
                [
                    "2016-12-03",
                    [
                        45.0
                    ]
                ],
                [
                    "2016-12-04",
                    [
                        35.0
                    ]
                ],
                [
                    "2016-12-05",
                    [
                        41.0
                    ]
                ],
                [
                    "2016-12-06",
                    [
                        33.0
                    ]
                ],
                [
                    "2016-12-07",
                    [
                        22.0
                    ]
                ],
                [
                    "2016-12-08",
                    [
                        21.0
                    ]
                ],
                [
                    "2016-12-09",
                    [
                        28.0
                    ]
                ],
                [
                    "2016-12-10",
                    [
                        37.0
                    ]
                ],
                [
                    "2016-12-11",
                    [
                        29.0
                    ]
                ],
                [
                    "2016-12-12",
                    [
                        21.0
                    ]
                ],
                [
                    "2016-12-13",
                    [
                        27.0
                    ]
                ],
                [
                    "2016-12-14",
                    [
                        31.0
                    ]
                ],
                [
                    "2016-12-15",
                    [
                        37.0
                    ]
                ],
                [
                    "2016-12-16",
                    [
                        44.0
                    ]
                ],
                [
                    "2016-12-17",
                    [
                        28.0
                    ]
                ],
                [
                    "2016-12-18",
                    [
                        34.0
                    ]
                ],
                [
                    "2016-12-19",
                    [
                        42.0
                    ]
                ],
                [
                    "2016-12-20",
                    [
                        32.0
                    ]
                ],
                [
                    "2016-12-21",
                    [
                        41.0
                    ]
                ],
                [
                    "2016-12-22",
                    [
                        23.0
                    ]
                ],
                [
                    "2016-12-23",
                    [
                        33.0
                    ]
                ],
                [
                    "2016-12-24",
                    [
                        38.0
                    ]
                ],
                [
                    "2016-12-27",
                    [
                        25.0
                    ]
                ],
                [
                    "2016-12-28",
                    [
                        34.0
                    ]
                ],
                [
                    "2016-12-29",
                    [
                        43.0
                    ]
                ],
                [
                    "2016-12-30",
                    [
                        29.0
                    ]
                ],
                [
                    "2016-12-31",
                    [
                        27.0
                    ]
                ],
                [
                    "2017-01-03",
                    [
                        34.0
                    ]
                ],
                [
                    "2017-01-04",
                    [
                        26.0
                    ]
                ],
                [
                    "2017-01-05",
                    [
                        23.0
                    ]
                ],
                [
                    "2017-01-06",
                    [
                        29.0
                    ]
                ],
                [
                    "2017-01-07",
                    [
                        55.0
                    ]
                ],
                [
                    "2017-01-08",
                    [
                        27.0
                    ]
                ],
                [
                    "2017-01-09",
                    [
                        28.0
                    ]
                ],
                [
                    "2017-01-10",
                    [
                        18.0
                    ]
                ],
                [
                    "2017-01-11",
                    [
                        27.0
                    ]
                ],
                [
                    "2017-01-12",
                    [
                        23.0
                    ]
                ],
                [
                    "2017-01-13",
                    [
                        32.0
                    ]
                ],
                [
                    "2017-01-14",
                    [
                        46.0
                    ]
                ],
                [
                    "2017-01-15",
                    [
                        29.0
                    ]
                ],
                [
                    "2017-01-16",
                    [
                        16.0
                    ]
                ],
                [
                    "2017-01-17",
                    [
                        32.0
                    ]
                ],
                [
                    "2017-01-18",
                    [
                        16.0
                    ]
                ],
                [
                    "2017-01-19",
                    [
                        31.0
                    ]
                ],
                [
                    "2017-01-20",
                    [
                        41.0
                    ]
                ],
                [
                    "2017-01-21",
                    [
                        43.0
                    ]
                ],
                [
                    "2017-01-22",
                    [
                        42.0
                    ]
                ],
                [
                    "2017-01-23",
                    [
                        29.0
                    ]
                ],
                [
                    "2017-01-24",
                    [
                        28.0
                    ]
                ],
                [
                    "2017-01-25",
                    [
                        11.0
                    ]
                ],
                [
                    "2017-01-26",
                    [
                        27.0
                    ]
                ],
                [
                    "2017-01-27",
                    [
                        43.0
                    ]
                ],
                [
                    "2017-01-28",
                    [
                        56.0
                    ]
                ],
                [
                    "2017-01-29",
                    [
                        31.0
                    ]
                ],
                [
                    "2017-01-30",
                    [
                        20.0
                    ]
                ],
                [
                    "2017-01-31",
                    [
                        29.0
                    ]
                ],
                [
                    "2017-02-01",
                    [
                        33.0
                    ]
                ],
                [
                    "2017-02-02",
                    [
                        23.0
                    ]
                ],
                [
                    "2017-02-03",
                    [
                        31.0
                    ]
                ],
                [
                    "2017-02-04",
                    [
                        72.0
                    ]
                ],
                [
                    "2017-02-05",
                    [
                        55.0
                    ]
                ],
                [
                    "2017-02-06",
                    [
                        36.0
                    ]
                ],
                [
                    "2017-02-07",
                    [
                        28.0
                    ]
                ],
                [
                    "2017-02-08",
                    [
                        26.0
                    ]
                ],
                [
                    "2017-02-09",
                    [
                        27.0
                    ]
                ],
                [
                    "2017-02-10",
                    [
                        36.0
                    ]
                ],
                [
                    "2017-02-11",
                    [
                        38.0
                    ]
                ],
                [
                    "2017-02-12",
                    [
                        33.0
                    ]
                ],
                [
                    "2017-02-13",
                    [
                        41.0
                    ]
                ],
                [
                    "2017-02-14",
                    [
                        29.0
                    ]
                ],
                [
                    "2017-02-15",
                    [
                        21.0
                    ]
                ],
                [
                    "2017-02-16",
                    [
                        24.0
                    ]
                ],
                [
                    "2017-02-17",
                    [
                        37.0
                    ]
                ],
                [
                    "2017-02-18",
                    [
                        52.0
                    ]
                ],
                [
                    "2017-02-19",
                    [
                        48.0
                    ]
                ],
                [
                    "2017-02-20",
                    [
                        30.0
                    ]
                ],
                [
                    "2017-02-21",
                    [
                        28.0
                    ]
                ],
                [
                    "2017-02-22",
                    [
                        28.0
                    ]
                ],
                [
                    "2017-02-23",
                    [
                        29.0
                    ]
                ],
                [
                    "2017-02-24",
                    [
                        38.0
                    ]
                ],
                [
                    "2017-02-25",
                    [
                        47.0
                    ]
                ],
                [
                    "2017-02-26",
                    [
                        57.0
                    ]
                ],
                [
                    "2017-02-27",
                    [
                        24.0
                    ]
                ],
                [
                    "2017-02-28",
                    [
                        33.0
                    ]
                ],
                [
                    "2017-03-01",
                    [
                        23.0
                    ]
                ],
                [
                    "2017-03-02",
                    [
                        32.0
                    ]
                ],
                [
                    "2017-03-03",
                    [
                        41.0
                    ]
                ],
                [
                    "2017-03-04",
                    [
                        57.0
                    ]
                ],
                [
                    "2017-03-05",
                    [
                        40.0
                    ]
                ],
                [
                    "2017-03-06",
                    [
                        27.0
                    ]
                ],
                [
                    "2017-03-07",
                    [
                        33.0
                    ]
                ],
                [
                    "2017-03-08",
                    [
                        18.0
                    ]
                ],
                [
                    "2017-03-09",
                    [
                        27.0
                    ]
                ],
                [
                    "2017-03-10",
                    [
                        38.0
                    ]
                ],
                [
                    "2017-03-11",
                    [
                        48.0
                    ]
                ],
                [
                    "2017-03-12",
                    [
                        40.0
                    ]
                ],
                [
                    "2017-03-13",
                    [
                        33.0
                    ]
                ],
                [
                    "2017-03-14",
                    [
                        27.0
                    ]
                ],
                [
                    "2017-03-15",
                    [
                        25.0
                    ]
                ],
                [
                    "2017-03-16",
                    [
                        25.0
                    ]
                ],
                [
                    "2017-03-17",
                    [
                        40.0
                    ]
                ],
                [
                    "2017-03-18",
                    [
                        44.0
                    ]
                ],
                [
                    "2017-03-19",
                    [
                        35.0
                    ]
                ],
                [
                    "2017-03-20",
                    [
                        31.0
                    ]
                ],
                [
                    "2017-03-21",
                    [
                        32.0
                    ]
                ],
                [
                    "2017-03-22",
                    [
                        31.0
                    ]
                ],
                [
                    "2017-03-23",
                    [
                        28.0
                    ]
                ],
                [
                    "2017-03-24",
                    [
                        42.0
                    ]
                ],
                [
                    "2017-03-25",
                    [
                        54.0
                    ]
                ],
                [
                    "2017-03-26",
                    [
                        35.0
                    ]
                ],
                [
                    "2017-03-27",
                    [
                        29.0
                    ]
                ],
                [
                    "2017-03-28",
                    [
                        32.0
                    ]
                ],
                [
                    "2017-03-29",
                    [
                        30.0
                    ]
                ],
                [
                    "2017-03-30",
                    [
                        33.0
                    ]
                ],
                [
                    "2017-03-31",
                    [
                        41.0
                    ]
                ],
                [
                    "2017-04-01",
                    [
                        39.0
                    ]
                ],
                [
                    "2017-04-02",
                    [
                        32.0
                    ]
                ],
                [
                    "2017-04-03",
                    [
                        35.0
                    ]
                ],
                [
                    "2017-04-04",
                    [
                        40.0
                    ]
                ],
                [
                    "2017-04-05",
                    [
                        30.0
                    ]
                ],
                [
                    "2017-04-06",
                    [
                        27.0
                    ]
                ],
                [
                    "2017-04-07",
                    [
                        29.0
                    ]
                ],
                [
                    "2017-04-08",
                    [
                        41.0
                    ]
                ],
                [
                    "2017-04-09",
                    [
                        17.0
                    ]
                ]
            ],
            "label": {
                "normal": {
                    "show": false,
                    "position": "top",
                    "textStyle": {
                        "fontSize": 12
                    }
                },
                "emphasis": {
                    "show": true,
                    "textStyle": {
                        "fontSize": 12
                    }
                }
            },
            "lineStyle": {
                "normal": {
                    "width": 1,
                    "opacity": 1,
                    "curveness": 0,
                    "type": "solid"
                }
            },
            "areaStyle": {
                "opacity": 0
            },
            "markPoint": {
                "data": [
                    {
                        "type": "max",
                        "name": "Maximum",
                        "symbol": "pin",
                        "symbolSize": 50,
                        "label": {
                            "normal": {
                                "textStyle": {
                                    "color": "#fff"
                                }
                            }
                        }
                    }
                ]
            },
            "markLine": {
                "data": [
                    {
                        "type": "average",
                        "name": "mean-Value"
                    }
                ],
                "symbolSize": 10
            },
            "seriesId": 6504146
        }
    ],
    "legend": [
        {
            "data": [
                ""
            ],
            "selectedMode": "multiple",
            "show": true,
            "left": "center",
            "top": "top",
            "orient": "horizontal",
            "textStyle": {
                "fontSize": 12
            }
        }
    ],
    "animation": true,
    "xAxis": [
        {
            "show": true,
            "nameLocation": "middle",
            "nameGap": 25,
            "nameTextStyle": {
                "fontSize": 14
            },
            "axisTick": {
                "alignWithLabel": false
            },
            "inverse": false,
            "boundaryGap": true,
            "type": "category",
            "splitLine": {
                "show": false
            },
            "axisLine": {
                "lineStyle": {
                    "width": 1
                }
            },
            "axisLabel": {
                "interval": 7,
                "rotate": 30,
                "margin": 8,
                "textStyle": {
                    "fontSize": 12
                }
            },
            "data": [
                "2016-10-30",
                "2016-10-31",
                "2016-11-01",
                "2016-11-02",
                "2016-11-03",
                "2016-11-04",
                "2016-11-05",
                "2016-11-06",
                "2016-11-07",
                "2016-11-08",
                "2016-11-09",
                "2016-11-10",
                "2016-11-11",
                "2016-11-12",
                "2016-11-13",
                "2016-11-14",
                "2016-11-15",
                "2016-11-16",
                "2016-11-17",
                "2016-11-18",
                "2016-11-19",
                "2016-11-20",
                "2016-11-21",
                "2016-11-22",
                "2016-11-23",
                "2016-11-24",
                "2016-11-25",
                "2016-11-26",
                "2016-11-27",
                "2016-11-28",
                "2016-11-29",
                "2016-11-30",
                "2016-12-01",
                "2016-12-02",
                "2016-12-03",
                "2016-12-04",
                "2016-12-05",
                "2016-12-06",
                "2016-12-07",
                "2016-12-08",
                "2016-12-09",
                "2016-12-10",
                "2016-12-11",
                "2016-12-12",
                "2016-12-13",
                "2016-12-14",
                "2016-12-15",
                "2016-12-16",
                "2016-12-17",
                "2016-12-18",
                "2016-12-19",
                "2016-12-20",
                "2016-12-21",
                "2016-12-22",
                "2016-12-23",
                "2016-12-24",
                "2016-12-27",
                "2016-12-28",
                "2016-12-29",
                "2016-12-30",
                "2016-12-31",
                "2017-01-03",
                "2017-01-04",
                "2017-01-05",
                "2017-01-06",
                "2017-01-07",
                "2017-01-08",
                "2017-01-09",
                "2017-01-10",
                "2017-01-11",
                "2017-01-12",
                "2017-01-13",
                "2017-01-14",
                "2017-01-15",
                "2017-01-16",
                "2017-01-17",
                "2017-01-18",
                "2017-01-19",
                "2017-01-20",
                "2017-01-21",
                "2017-01-22",
                "2017-01-23",
                "2017-01-24",
                "2017-01-25",
                "2017-01-26",
                "2017-01-27",
                "2017-01-28",
                "2017-01-29",
                "2017-01-30",
                "2017-01-31",
                "2017-02-01",
                "2017-02-02",
                "2017-02-03",
                "2017-02-04",
                "2017-02-05",
                "2017-02-06",
                "2017-02-07",
                "2017-02-08",
                "2017-02-09",
                "2017-02-10",
                "2017-02-11",
                "2017-02-12",
                "2017-02-13",
                "2017-02-14",
                "2017-02-15",
                "2017-02-16",
                "2017-02-17",
                "2017-02-18",
                "2017-02-19",
                "2017-02-20",
                "2017-02-21",
                "2017-02-22",
                "2017-02-23",
                "2017-02-24",
                "2017-02-25",
                "2017-02-26",
                "2017-02-27",
                "2017-02-28",
                "2017-03-01",
                "2017-03-02",
                "2017-03-03",
                "2017-03-04",
                "2017-03-05",
                "2017-03-06",
                "2017-03-07",
                "2017-03-08",
                "2017-03-09",
                "2017-03-10",
                "2017-03-11",
                "2017-03-12",
                "2017-03-13",
                "2017-03-14",
                "2017-03-15",
                "2017-03-16",
                "2017-03-17",
                "2017-03-18",
                "2017-03-19",
                "2017-03-20",
                "2017-03-21",
                "2017-03-22",
                "2017-03-23",
                "2017-03-24",
                "2017-03-25",
                "2017-03-26",
                "2017-03-27",
                "2017-03-28",
                "2017-03-29",
                "2017-03-30",
                "2017-03-31",
                "2017-04-01",
                "2017-04-02",
                "2017-04-03",
                "2017-04-04",
                "2017-04-05",
                "2017-04-06",
                "2017-04-07",
                "2017-04-08",
                "2017-04-09"
            ]
        }
    ],
    "yAxis": [
        {
            "show": true,
            "nameLocation": "middle",
            "nameGap": 25,
            "nameTextStyle": {
                "fontSize": 14
            },
            "axisTick": {
                "alignWithLabel": false
            },
            "inverse": false,
            "boundaryGap": true,
            "type": "value",
            "splitLine": {
                "show": true
            },
            "axisLine": {
                "lineStyle": {
                    "width": 1
                }
            },
            "axisLabel": {
                "interval": "auto",
                "formatter": "{value} ",
                "rotate": 30,
                "margin": 8,
                "textStyle": {
                    "fontSize": 12
                }
            }
        }
    ],
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597",
        "#f6f5ec"
    ]
};
myChart_0c36b741ffec4047bd0618f71573d85b.setOption(option_0c36b741ffec4047bd0618f71573d85b);

    });
</script>



