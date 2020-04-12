---
date: 2020-04-12 16:40:58
layout: post
title: Visualise Missing Data
subtitle: A short and simple code in Python
description: Have you ever wondered how to visualise missing values in your data? Let's do it in a two lines of code...
image: https://photos.app.goo.gl/9dAuJE39MLHn5aU29
optimized_image: https://photos.app.goo.gl/h9gWV4WTmxZJGzBC8
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

## Datasource: [Sleep_Data](https://github.com/opendatavis/opendatavis.github.io/Data/Sleep_Data.csv)

## Code:

### Import Libraries
```python
# Import Libraries
import os
import datetime
import numpy as np
import pandas as pd
from itertools import product
import plotly.graph_objects as go
```

### Read Data

```python
df_data = pd.read_csv("Sleep_Missing_Data.csv")
```

### Create custom function
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


### Call the custom function and pass your dataframe
```python
fn_vis_msng_data(df_data)
```