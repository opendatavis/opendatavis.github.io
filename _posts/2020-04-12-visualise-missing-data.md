---
date: 2020-04-12 16:40:58
layout: post
title: Visualise Missing Data
subtitle: A short and simple code in Python
description: Have you ever wondered how to visualise missing values in your data? Let's do it in a two lines of code...
image: https://drive.google.com/uc?export=view&id=1F6abOcwAUtZLfzW2I178KwLlFpIZj6dC
optimized_image: https://drive.google.com/uc?export=view&id=1Jip12OO68IPFlZwRmt9TUV5-0-U-8iYN
category: data
tags: 
  - Missing Data
  - Python Missing Data
author: saikrupan
paginate: false
---

Have you ever wondered how to visualise missing values in your data? Let's do it in a two lines of code with the help of a custom function created using python libraries.

This is inspired from a post by fellow Kaggler [AiO](https://www.kaggle.com/notaapple) in which he created a function to Visualize missing data in R. The function gives an amazing plot representing the non missing data and missing  in black & white colors as output. It's capability of handling huge amounts of data is quite impressive. I have created a plot for a dataset of 500000 observations within few seconds using it.

And then I thought why not recreate the same in Python and here it is.
### Datasource: [Sleep_Data](https://raw.githubusercontent.com/opendatavis/opendatavis.github.io/master/Data/Sleep_Data.csv)
### Code:
#### Import Libraries

<script src="https://gist.github.com/opendatavis/108f3ebfc2bf4835329340fd354d58f1.js"></script>

```python
# Import Libraries
import os
import datetime
import numpy as np
import pandas as pd
from itertools import product
import plotly.graph_objects as go
```
#### Read Data
```python
df_data = pd.read_csv("Sleep_Missing_Data.csv")
df_data.head(5)
```
#### Create custom function
```python
def fn_vis_msng_data(df_):
  # Identify Missing Data
  len_data = len(df_)
  df_['Row_Index'] = df_.index
  ms_cols = df_.isna().sum()
  ms_cols = ms_cols[ms_cols > 0]
  ms_cols = list(ms_cols.index)

  req_cols = ms_cols + ['Row_Index']

  df_ = df_.loc[:, req_cols]
  df_.loc[:, ms_cols] = df_.loc[:, ms_cols].notnull().astype(int)

  ord_cols = list(df_.sum().sort_values(ascending=True).index)
  df_ = df_.loc[:, ord_cols]

  dictionary = {'Row_Index': list(range(0, len(df_))),
                'Miss_Cols': ms_cols}

  df_temp = pd.DataFrame([row for row in product(*dictionary.values())], 
                         columns=dictionary.keys())
  
  df_ = df_.melt(id_vars=['Row_Index'], var_name='Miss_Cols', value_name='Miss_Index')
  df_ = df_.merge(df_temp, on=['Row_Index', 'Miss_Cols'])
  df_['Miss_Index'] = df_['Miss_Index'].astype(str)

  # Plot data
  list_mis_cols = df_['Miss_Cols']
  row_ids = df_['Row_Index']
  mis_index = df_['Miss_Index']

  fig = go.Figure(data=go.Heatmap(
          z=mis_index,
          x=row_ids,
          y=list_mis_cols,
          reversescale=True,
          colorscale='gray',
          showscale=True))

  fig.update_layout(
      title={
          'text': "Ever wondered how to visualise missing values in your data? Not anymore...!",
          'y':0.9,
          'x':0.5,
          'xanchor': 'center',
          'yanchor': 'top',
          },
      xaxis_nticks=len_data,
      xaxis_title='Row Index',
      yaxis_title='Missing Columns',
      font=dict(
          family="Segoe UI",
          size=20,
          color="#000000"
      )
  )

  fig.show()
```
#### Call the custom function and pass your dataframe
```python
fn_vis_msng_data(df_data)
```

### Missing Data Visualization

<script src="https://cdn.plot.ly/plotly-latest.min.js"></script> 

<div>\n        \n        \n            <div id="0b680a07-89f9-42d5-8572-a44280646e44" class="plotly-graph-div" style="height:100%; width:100%;"></div>\n            <script type="text/javascript">\n                \n                    window.PLOTLYENV=window.PLOTLYENV || {};\n                    \n                if (document.getElementById("0b680a07-89f9-42d5-8572-a44280646e44")) {\n                    Plotly.newPlot(\n                        \'0b680a07-89f9-42d5-8572-a44280646e44\',\n                        [{"colorbar": {"title": {"text": "Missing<br>(0=Yes, 1=No)<br>"}}, "colorscale": [[0.0, "rgb(0, 0, 0)"], [0.09090909090909091, "rgb(16, 16, 16)"], [0.18181818181818182, "rgb(38, 38, 38)"], [0.2727272727272727, "rgb(59, 59, 59)"], [0.36363636363636365, "rgb(81, 80, 80)"], [0.45454545454545453, "rgb(102, 101, 101)"], [0.5454545454545454, "rgb(124, 123, 122)"], [0.6363636363636364, "rgb(146, 146, 145)"], [0.7272727272727273, "rgb(171, 171, 170)"], [0.8181818181818182, "rgb(197, 197, 195)"], [0.9090909090909091, "rgb(224, 224, 223)"], [1.0, "rgb(254, 254, 253)"]], "reversescale": true, "showscale": true, "type": "heatmap", "x": [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61], "y": ["NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest"], "z": [0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1]}],\n                        {"font": {"color": "#000000", "family": "Segoe UI", "size": 12}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "title": {"text": "Missing Data Visualisation", "x": 0.5, "xanchor": "center", "y": 0.9, "yanchor": "top"}, "xaxis": {"linecolor": "black", "linewidth": 1, "showline": true, "title": {"text": "Row Index"}}, "yaxis": {"linecolor": "black", "linewidth": 1, "showline": true, "title": {"text": "Missing Columns"}}},\n                        {"responsive": true}\n                    )\n                };\n                \n            </script>\n        </div>

