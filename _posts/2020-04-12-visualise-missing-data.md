---
date: 2020-04-12 16:40:58
layout: post
title: Visualise Missing Data and identify the missing pattern in Python.
subtitle: An interactive heat map in less than 25 lines of code. 
description: Missing Data! Is it MAR or MCAR or MNAR? Find it out in less than a minute by this interactive heat map.
image: https://drive.google.com/uc?export=view&id=1F6abOcwAUtZLfzW2I178KwLlFpIZj6dC
optimized_image: https://drive.google.com/uc?export=view&id=1Jip12OO68IPFlZwRmt9TUV5-0-U-8iYN
category: data
tags: 
  - Missing Data
  - Python
  - Data Visualization
  - Plotly
author: saikrupan
paginate: false
---

Missing values in a data set is quite common and have significance effect on the analysis and machine learning model training.

> Missing data can be either "Missing At Random (MAR)" or "Missing Completely At Random (MCAR)" or "Missing Not At Random (MNAR)"

The easiest way to check which category your missing data falls in to is by visualising it. Have you ever wondered how to visualise missing values in your data? Let's do it in less than 25 lines of code using basic `pandas` and `plotly` functions. 

This is inspired from a post by fellow Kaggler [AiO](https://www.kaggle.com/notaapple) in which he created a function to Visualize missing data in R. It is very simple yet quick and powerful.

Here I am recreating it same in Python.

## Let's start coding:

#### Import Libraries
<br>
<!-- Gist: Import libraries -->
<script src="https://gist.github.com/opendatavis/108f3ebfc2bf4835329340fd354d58f1.js"></script>


Read the dataset [Sleep_Data](https://raw.githubusercontent.com/opendatavis/opendatavis.github.io/master/Data/Sleep_Data.csv) hosted in github. Missing values are represented as NaN.
<br>
<br>

<!-- Gist: Read data -->
<script src="https://gist.github.com/opendatavis/40a95f13ea7fcb069d90f579f10ba5bf.js"></script>

<!-- HTML: Head of data -->
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
                <th>BodyWgt</th>
                <th>BrainWgt</th>
                <th>NonD</th>
                <th>Dream</th>
                <th>Sleep</th>
                <th>Span</th>
                <th>Gest</th>
                <th>Pred</th>
                <th>Exp</th>
                <th>Danger</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <th>0</th>
                <td>6654.000</td>
                <td>5712.0</td>
                <td>NaN</td>
                <td>NaN</td>
                <td>3.3</td>
                <td>38.6</td>
                <td>645.0</td>
                <td>3</td>
                <td>5</td>
                <td>3</td>
            </tr>
            <tr>
                <th>1</th>
                <td>1.000</td>
                <td>6.6</td>
                <td>6.3</td>
                <td>2.0</td>
                <td>8.3</td>
                <td>4.5</td>
                <td>42.0</td>
                <td>3</td>
                <td>1</td>
                <td>3</td>
            </tr>
            <tr>
                <th>2</th>
                <td>3.385</td>
                <td>44.5</td>
                <td>NaN</td>
                <td>NaN</td>
                <td>12.5</td>
                <td>14.0</td>
                <td>60.0</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
            </tr>
            <tr>
                <th>3</th>
                <td>0.920</td>
                <td>5.7</td>
                <td>NaN</td>
                <td>NaN</td>
                <td>16.5</td>
                <td>NaN</td>
                <td>25.0</td>
                <td>5</td>
                <td>2</td>
                <td>3</td>
            </tr>
            <tr>
                <th>4</th>
                <td>2547.000</td>
                <td>4603.0</td>
                <td>2.1</td>
                <td>1.8</td>
                <td>3.9</td>
                <td>69.0</td>
                <td>624.0</td>
                <td>3</td>
                <td>5</td>
                <td>4</td>
            </tr>
        </tbody>
    </table>
</div>

#### Identify Missing Values

Let us identify the columns with missing values and number of missing records.
<br>
<br>

<!-- Gist: Find Missing data -->
<script src="https://gist.github.com/opendatavis/169f1c6fcd44885a2841b314b745fdd9.js"></script>


<!-- HTML: Missing Data -->
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
				<th>Column</th>
				<th>No_Records</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<th>0</th>
				<td>NonD</td>
				<td>14</td>
			</tr>
			<tr>
				<th>1</th>
				<td>Dream</td>
				<td>12</td>
			</tr>
			<tr>
				<th>2</th>
				<td>Sleep</td>
				<td>4</td>
			</tr>
			<tr>
				<th>3</th>
				<td>Span</td>
				<td>4</td>
			</tr>
			<tr>
				<th>4</th>
				<td>Gest</td>
				<td>4</td>
			</tr>
		</tbody>
	</table>
</div>


Add Row_Index as a column and use it to identify missing rows in the plot. We need only missing columns from the dataframe. Also, let us binarize data by mapping missing values to 0 and non-missing values to 1.
<br>
<br>
<!-- Gist: Binarize data -->
<script src="https://gist.github.com/opendatavis/c4d507db6a6b4b0653643f901cf3bedd.js"></script>

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
        <th>NonD</th>
        <th>Dream</th>
        <th>Sleep</th>
        <th>Span</th>
        <th>Gest</th>
        <th>Row_Index</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>0</th>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>1</td>
        <td>1</td>
        <td>0</td>
      </tr>
      <tr>
        <th>1</th>
        <td>1</td>
        <td>1</td>
        <td>1</td>
        <td>1</td>
        <td>1</td>
        <td>1</td>
      </tr>
      <tr>
        <th>2</th>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>1</td>
        <td>1</td>
        <td>2</td>
      </tr>
      <tr>
        <th>3</th>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>1</td>
        <td>3</td>
      </tr>
      <tr>
        <th>4</th>
        <td>1</td>
        <td>1</td>
        <td>1</td>
        <td>1</td>
        <td>1</td>
        <td>4</td>
      </tr>
    </tbody>
  </table>
</div>


#### Transform data
Re-structure data using the pandas function `melt` and use it for plotting.

<!-- Gist: Melt Data -->
<script src="https://gist.github.com/opendatavis/ba0490bfa635440d47b5863241791213.js"></script>

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
        <th>Row_Index</th>
        <th>Miss_Col</th>
        <th>Miss_Val</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>0</th>
        <td>0</td>
        <td>NonD</td>
        <td>0</td>
      </tr>
      <tr>
        <th>1</th>
        <td>1</td>
        <td>NonD</td>
        <td>1</td>
      </tr>
      <tr>
        <th>2</th>
        <td>2</td>
        <td>NonD</td>
        <td>0</td>
      </tr>
      <tr>
        <th>3</th>
        <td>3</td>
        <td>NonD</td>
        <td>0</td>
      </tr>
      <tr>
        <th>4</th>
        <td>4</td>
        <td>NonD</td>
        <td>1</td>
      </tr>
      <tr>
        <th>5</th>
        <td>5</td>
        <td>NonD</td>
        <td>1</td>
      </tr>
      <tr>
        <th>6</th>
        <td>6</td>
        <td>NonD</td>
        <td>1</td>
      </tr>
      <tr>
        <th>7</th>
        <td>7</td>
        <td>NonD</td>
        <td>1</td>
      </tr>
      <tr>
        <th>8</th>
        <td>8</td>
        <td>NonD</td>
        <td>1</td>
      </tr>
      <tr>
        <th>9</th>
        <td>9</td>
        <td>NonD</td>
        <td>1</td>
      </tr>
    </tbody>
  </table>
</div>


### Plot - Heat map

Let us plot the data as Heat map to visualise the pattern of missing and non-missing values. 

Here you go...!!!

We have an interactive plot with row index on x-axis and columns on y-axis. We can hover over the plot to see the column, row index and the missing index which tells if the data is missing or not. 

<!-- Gist: Plot Heat map -->
<script src="https://gist.github.com/opendatavis/48b37ed6e3140c6202daea7e90d25668.js"></script>

<!-- Plotly java script -->
<script src="https://cdn.plot.ly/plotly-latest.min.js"></script> 

> Missing value of 0 indicates data is missing and 1 indicates data as not.


<!-- HTML: Plotly chart  -->
<div>
  <div id="e29bc7a6-61de-46ab-b946-b182d658a811" class="plotly-graph-div" style="height:100%; width:100%;"></div>
  <script type="text/javascript">
    
    window.PLOTLYENV=window.PLOTLYENV || {};
    
    if (document.getElementById("e29bc7a6-61de-46ab-b946-b182d658a811")) {
      Plotly.newPlot(
        'e29bc7a6-61de-46ab-b946-b182d658a811',
        [{"colorbar": {"title": {"text": "Missing<br>(0=Yes, 1=No)<br>"}}, "colorscale": [[0.0, "rgb(0, 0, 0)"], [0.09090909090909091, "rgb(16, 16, 16)"], [0.18181818181818182, "rgb(38, 38, 38)"], [0.2727272727272727, "rgb(59, 59, 59)"], [0.36363636363636365, "rgb(81, 80, 80)"], [0.45454545454545453, "rgb(102, 101, 101)"], [0.5454545454545454, "rgb(124, 123, 122)"], [0.6363636363636364, "rgb(146, 146, 145)"], [0.7272727272727273, "rgb(171, 171, 170)"], [0.8181818181818182, "rgb(197, 197, 195)"], [0.9090909090909091, "rgb(224, 224, 223)"], [1.0, "rgb(254, 254, 253)"]], "hovertemplate": "Row Index: %{x}<br>Column: %{y}<br>Missing: %{z}<br><extra></extra>", "reversescale": true, "showscale": true, "type": "heatmap", "x": [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61], "y": ["NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest"], "z": [0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1]}],
        {"font": {"color": "#000000", "family": "Segoe UI", "size": 12}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "title": {"text": "Missing Data Visualisation", "x": 0.5, "xanchor": "center", "y": 0.9, "yanchor": "top"}, "xaxis": {"linecolor": "black", "linewidth": 1, "showline": true, "title": {"text": "Row Index"}}, "yaxis": {"linecolor": "black", "linewidth": 1, "showline": true, "title": {"text": "Missing Columns"}}},
        {"responsive": true}
        )
};

</script>
</div>


Plotly provides several customization options to the charts interactively. You can play around and make the charts more interesting. 