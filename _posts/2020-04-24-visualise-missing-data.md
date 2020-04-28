---
layout: post
title:  "Visualise Missing Data Patterns in Python"
author: sai
categories: [ Python, tutorial, Plotly ]
image: assets/images/Missing_Data.jpg
tags: [sticky]
---

Missing values in a data set is quite common and have significance effect on the analysis and training of a machine learning model.

> Missing data can be either "Missing At Random (MAR)" or "Missing Completely At Random (MCAR)" or "Missing Not At Random (MNAR)"

The easiest way to know which category your missing data falls in to is by visualising it. Have you ever wondered how to visualise missing values in your data? Let's do it using basic python libraries `pandas` and `plotly`.

#### Let's start coding
##### Import Libraries and read data

As mentioned above, let's import libraries pandas and plotly.
<br/>
<!-- Gist: Import libraries Start-->
<script src="https://gist.github.com/opendatavis/108f3ebfc2bf4835329340fd354d58f1.js"></script>
<!-- Gist: Import libraries End-->
Read the dataset [Sleep](https://raw.githubusercontent.com/opendatavis/opendatavis.github.io/master/Data/Sleep_Data.csv) hosted on github. Missing values are represented as NA in the csv file. `pandas` converts NA to NaN.
<br/>

<!-- Gist: Read data Start-->
<script src="https://gist.github.com/opendatavis/40a95f13ea7fcb069d90f579f10ba5bf.js"></script>
<!-- Gist: Read data End-->
<!-- HTML: Head Data Start-->
<center>
    <div>
        <style type="text/css">
            .tg  {border-collapse:collapse;border-color:black;border-spacing:0;border-style:solid;border-width:1px;}
            .tg td{border-style:solid;border-width:0px;font-family:Arial, sans-serif;font-size:14px;overflow:hidden;
              padding:10px 5px;word-break:normal;}
              .tg th{border-style:solid;border-width:0px;font-family:Arial, sans-serif;font-size:14px;font-weight:normal;
                  overflow:hidden;padding:10px 5px;word-break:normal;}
                  .tg .tg-ayo4{background-color:#efefef;border-color:#ffffff;color:rgb(0, 0, 0);text-align:right;vertical-align:top}
                  .tg .tg-06ar{border-color:#ffffff;color:rgb(0, 0, 0);font-weight:bold;text-align:right;vertical-align:top}
                  .tg .tg-h25s{border-color:#ffffff;font-weight:bold;text-align:right;vertical-align:top}
                  .tg .tg-zyvm{border-color:#ffffff;color:rgb(0, 0, 0);text-align:right;vertical-align:top}
              </style>
              <table class="tg">
                  <tr>
                    <th class="tg-06ar">BodyWgt</th>
                    <th class="tg-06ar">BrainWgt</th>
                    <th class="tg-06ar">NonD</th>
                    <th class="tg-06ar">Dream</th>
                    <th class="tg-06ar">Sleep</th>
                    <th class="tg-06ar">Span</th>
                    <th class="tg-06ar">Gest</th>
                    <th class="tg-06ar">Pred</th>
                    <th class="tg-06ar">Exp</th>
                    <th class="tg-h25s">Danger</th>
                </tr>
                <tr>
                    <td class="tg-ayo4">6654.000</td>
                    <td class="tg-ayo4">5712.0</td>
                    <td class="tg-ayo4">NaN</td>
                    <td class="tg-ayo4">NaN</td>
                    <td class="tg-ayo4">3.3</td>
                    <td class="tg-ayo4">38.6</td>
                    <td class="tg-ayo4">645.0</td>
                    <td class="tg-ayo4">3</td>
                    <td class="tg-ayo4">5</td>
                    <td class="tg-ayo4">3</td>
                </tr>
                <tr>
                    <td class="tg-zyvm">1.000</td>
                    <td class="tg-zyvm">6.6</td>
                    <td class="tg-zyvm">6.3</td>
                    <td class="tg-zyvm">2.0</td>
                    <td class="tg-zyvm">8.3</td>
                    <td class="tg-zyvm">4.5</td>
                    <td class="tg-zyvm">42.0</td>
                    <td class="tg-zyvm">3</td>
                    <td class="tg-zyvm">1</td>
                    <td class="tg-zyvm">3</td>
                </tr>
                <tr>
                    <td class="tg-ayo4">3.385</td>
                    <td class="tg-ayo4">44.5</td>
                    <td class="tg-ayo4">NaN</td>
                    <td class="tg-ayo4">NaN</td>
                    <td class="tg-ayo4">12.5</td>
                    <td class="tg-ayo4">14.0</td>
                    <td class="tg-ayo4">60.0</td>
                    <td class="tg-ayo4">1</td>
                    <td class="tg-ayo4">1</td>
                    <td class="tg-ayo4">1</td>
                </tr>
                <tr>
                    <td class="tg-zyvm">0.920</td>
                    <td class="tg-zyvm">5.7</td>
                    <td class="tg-zyvm">NaN</td>
                    <td class="tg-zyvm">NaN</td>
                    <td class="tg-zyvm">16.5</td>
                    <td class="tg-zyvm">NaN</td>
                    <td class="tg-zyvm">25.0</td>
                    <td class="tg-zyvm">5</td>
                    <td class="tg-zyvm">2</td>
                    <td class="tg-zyvm">3</td>
                </tr>
                <tr>
                    <td class="tg-ayo4">2547.000</td>
                    <td class="tg-ayo4">4603.0</td>
                    <td class="tg-ayo4">2.1</td>
                    <td class="tg-ayo4">1.8</td>
                    <td class="tg-ayo4">3.9</td>
                    <td class="tg-ayo4">69.0</td>
                    <td class="tg-ayo4">624.0</td>
                    <td class="tg-ayo4">3</td>
                    <td class="tg-ayo4">5</td>
                    <td class="tg-ayo4">4</td>
                </tr>
            </table>
        </div>
    </center>
<!-- HTML: Head Data End-->

##### Identify Missing Values

Let's identify the columns with missing values and number of missing records in each of them.
<br/>
<!-- Gist: Find Missing data Start-->
<script src="https://gist.github.com/opendatavis/169f1c6fcd44885a2841b314b745fdd9.js"></script>
<!-- Gist: Find Missing data End-->
<!-- HTML: Missing Data Table Start-->
<center>
    <div>
        <style type="text/css">
            .tg  {border-collapse:collapse;border-spacing:0;}
            .tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
              overflow:hidden;padding:10px 5px;word-break:normal;}
              .tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
                  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
                  .tg .tg-g873{color:rgb(0, 0, 0);text-align:right;vertical-align:top}
                  .tg .tg-zweo{color:rgb(0, 0, 0);font-weight:bold;text-align:right;vertical-align:top}
                  .tg .tg-cmkb{background-color:rgb(239, 239, 239);color:rgb(0, 0, 0);text-align:right;vertical-align:top}
              </style>
              <table class="tg">
                  <tr>
                    <th class="tg-zweo">Column</th>
                    <th class="tg-zweo">No_Records</th>
                </tr>
                <tr>
                    <td class="tg-cmkb">NonD</td>
                    <td class="tg-cmkb">14</td>
                </tr>
                <tr>
                    <td class="tg-g873">Dream</td>
                    <td class="tg-g873">12</td>
                </tr>
                <tr>
                    <td class="tg-cmkb">Sleep</td>
                    <td class="tg-cmkb">4</td>
                </tr>
                <tr>
                    <td class="tg-g873">Span</td>
                    <td class="tg-g873">4</td>
                </tr>
                <tr>
                    <td class="tg-cmkb">Gest</td>
                    <td class="tg-cmkb">4</td>
                </tr>
            </table>
        </div>
    </center>
<br/>
<!-- HTML: Missing Data Table End-->
Add Row_Index as a column and use it to identify missing rows in the plot. We need only missing columns from the dataframe. Also, let us binarize data by mapping missing values to 0 and non-missing values to 1.
<br/>
<!-- Gist: Binarize data Start-->
<script src="https://gist.github.com/opendatavis/c4d507db6a6b4b0653643f901cf3bedd.js"></script>
<!-- Gist: Binarize data End-->
<!-- HTML: Binarize Data Table Start-->
<center>
    <div>
        <style type="text/css">
            .tg  {border-collapse:collapse;border-color:black;border-spacing:0;border-style:solid;border-width:1px;}
            .tg td{border-style:solid;border-width:0px;font-family:Arial, sans-serif;font-size:14px;overflow:hidden;
              padding:10px 5px;word-break:normal;}
              .tg th{border-style:solid;border-width:0px;font-family:Arial, sans-serif;font-size:14px;font-weight:normal;
                  overflow:hidden;padding:10px 5px;word-break:normal;}
                  .tg .tg-efhx{border-color:inherit;color:rgb(0, 0, 0);font-weight:bold;text-align:right;vertical-align:top}
                  .tg .tg-6ic8{border-color:inherit;font-weight:bold;text-align:right;vertical-align:top}
                  .tg .tg-ehic{background-color:#efefef;border-color:inherit;color:rgb(0, 0, 0);text-align:right;vertical-align:top}
                  .tg .tg-pf22{border-color:inherit;color:rgb(0, 0, 0);text-align:right;vertical-align:top}
              </style>
              <table class="tg">
                  <tr>
                    <th class="tg-efhx">NonD</th>
                    <th class="tg-efhx">Dream</th>
                    <th class="tg-efhx">Sleep</th>
                    <th class="tg-efhx">Span</th>
                    <th class="tg-efhx">Gest</th>
                    <th class="tg-6ic8">Row_Index</th>
                </tr>
                <tr>
                    <td class="tg-ehic">0</td>
                    <td class="tg-ehic">0</td>
                    <td class="tg-ehic">1</td>
                    <td class="tg-ehic">1</td>
                    <td class="tg-ehic">1</td>
                    <td class="tg-ehic">0</td>
                </tr>
                <tr>
                    <td class="tg-pf22">1</td>
                    <td class="tg-pf22">1</td>
                    <td class="tg-pf22">1</td>
                    <td class="tg-pf22">1</td>
                    <td class="tg-pf22">1</td>
                    <td class="tg-pf22">1</td>
                </tr>
                <tr>
                    <td class="tg-ehic">0</td>
                    <td class="tg-ehic">0</td>
                    <td class="tg-ehic">1</td>
                    <td class="tg-ehic">1</td>
                    <td class="tg-ehic">1</td>
                    <td class="tg-ehic">2</td>
                </tr>
                <tr>
                    <td class="tg-pf22">0</td>
                    <td class="tg-pf22">0</td>
                    <td class="tg-pf22">1</td>
                    <td class="tg-pf22">0</td>
                    <td class="tg-pf22">1</td>
                    <td class="tg-pf22">3</td>
                </tr>
                <tr>
                    <td class="tg-ehic">1</td>
                    <td class="tg-ehic">1</td>
                    <td class="tg-ehic">1</td>
                    <td class="tg-ehic">1</td>
                    <td class="tg-ehic">1</td>
                    <td class="tg-ehic">4</td>
                </tr>
            </table>
        </div>
    </center>
<!-- HTML: Binarize Data Table End-->

##### Transform data
Re-structure data using the pandas function `melt` to use it for plotting.
<br/>
<!-- Gist: Melt Data Start-->
<script src="https://gist.github.com/opendatavis/ba0490bfa635440d47b5863241791213.js"></script>
<!-- Gist: Melt Data End-->
<!-- HTML: Melt Data Table Start-->
<center>
    <div>
        <style type="text/css">
            .tg  {border-collapse:collapse;border-color:black;border-spacing:0;border-style:solid;border-width:1px;}
            .tg td{border-style:solid;border-width:0px;font-family:Arial, sans-serif;font-size:14px;overflow:hidden;
              padding:10px 5px;word-break:normal;}
              .tg th{border-style:solid;border-width:0px;font-family:Arial, sans-serif;font-size:14px;font-weight:normal;
                  overflow:hidden;padding:10px 5px;word-break:normal;}
                  .tg .tg-zgm3{background-color:#efefef;border-color:inherit;color:rgb(33, 37, 41);text-align:right;vertical-align:top}
                  .tg .tg-706p{background-color:rgb(255, 255, 255);border-color:inherit;color:rgb(33, 37, 41);text-align:right;vertical-align:top}
                  .tg .tg-7d3y{background-color:rgb(255, 255, 255);border-color:inherit;color:rgb(33, 37, 41);font-weight:bold;text-align:right;
                      vertical-align:top}
                      .tg .tg-6ic8{border-color:inherit;font-weight:bold;text-align:right;vertical-align:top}
                  </style>
                  <table class="tg">
                      <tr>
                        <th class="tg-7d3y">Row_Index</th>
                        <th class="tg-7d3y">Miss_Col</th>
                        <th class="tg-6ic8">Miss_Val</th>
                    </tr>
                    <tr>
                        <td class="tg-zgm3">0</td>
                        <td class="tg-zgm3">NonD</td>
                        <td class="tg-zgm3">0</td>
                    </tr>
                    <tr>
                        <td class="tg-706p">1</td>
                        <td class="tg-706p">NonD</td>
                        <td class="tg-706p">1</td>
                    </tr>
                    <tr>
                        <td class="tg-zgm3">2</td>
                        <td class="tg-zgm3">NonD</td>
                        <td class="tg-zgm3">0</td>
                    </tr>
                    <tr>
                        <td class="tg-706p">3</td>
                        <td class="tg-706p">NonD</td>
                        <td class="tg-706p">0</td>
                    </tr>
                    <tr>
                        <td class="tg-zgm3">4</td>
                        <td class="tg-zgm3">NonD</td>
                        <td class="tg-zgm3">1</td>
                    </tr>
                    <tr>
                        <td class="tg-706p">5</td>
                        <td class="tg-706p">NonD</td>
                        <td class="tg-706p">1</td>
                    </tr>
                    <tr>
                        <td class="tg-zgm3">6</td>
                        <td class="tg-zgm3">NonD</td>
                        <td class="tg-zgm3">1</td>
                    </tr>
                    <tr>
                        <td class="tg-706p">7</td>
                        <td class="tg-706p">NonD</td>
                        <td class="tg-706p">1</td>
                    </tr>
                    <tr>
                        <td class="tg-zgm3">8</td>
                        <td class="tg-zgm3">NonD</td>
                        <td class="tg-zgm3">1</td>
                    </tr>
                    <tr>
                        <td class="tg-706p">9</td>
                        <td class="tg-706p">NonD</td>
                        <td class="tg-706p">1</td>
                    </tr>
                </table>
            </div>
        </center>
<!-- HTML: Melt Data Table End-->

##### Plot - Heat map

Plot the data as Heat map to visualise the pattern of missing and non-missing values. 
<br/>
<!-- Gist: Plot Heat map Start-->
<script src="https://gist.github.com/opendatavis/48b37ed6e3140c6202daea7e90d25668.js"></script>
<!-- Gist: Plot Heat map End-->
<!-- Plotly: java script Start-->
<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<!-- Plotly: java script End-->
<br/>
Here you go...!!!
We have an interactive plot with row index on x-axis and columns on y-axis. We can hover over the plot to see the column, row index and the missing index which tells if the data is missing or not.

> Missing value of 0 indicates data is missing and 1 indicates data as not.

<!-- HTML: Plotly chart Start-->
<div>        
    <div id="3074447b-2c17-47bb-8102-01a8b4c429d8" class="plotly-graph-div" style="height:100%; width:100%;"></div>
    <script type="text/javascript">
        
        window.PLOTLYENV=window.PLOTLYENV || {};
        
        if (document.getElementById("3074447b-2c17-47bb-8102-01a8b4c429d8")) {
            Plotly.newPlot(
                '3074447b-2c17-47bb-8102-01a8b4c429d8',
                [{"colorbar": {"title": {"text": "Missing<br>(0=Yes, 1=No)<br>"}}, "colorscale": [[0.0, "rgb(0, 0, 0)"], [0.09090909090909091, "rgb(16, 16, 16)"], [0.18181818181818182, "rgb(38, 38, 38)"], [0.2727272727272727, "rgb(59, 59, 59)"], [0.36363636363636365, "rgb(81, 80, 80)"], [0.45454545454545453, "rgb(102, 101, 101)"], [0.5454545454545454, "rgb(124, 123, 122)"], [0.6363636363636364, "rgb(146, 146, 145)"], [0.7272727272727273, "rgb(171, 171, 170)"], [0.8181818181818182, "rgb(197, 197, 195)"], [0.9090909090909091, "rgb(224, 224, 223)"], [1.0, "rgb(254, 254, 253)"]], "hovertemplate": "Row Index: %{x}<br>Column: %{y}<br>Missing: %{z}<br><extra></extra>", "reversescale": true, "showscale": true, "type": "heatmap", "x": [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61], "y": ["NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "NonD", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Dream", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Sleep", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Span", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest", "Gest"], "z": [0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1]}],
                {"font": {"color": "#000000", "family": "Segoe UI", "size": 12}, "images": [{"sizex": 0.2, "sizey": 0.2, "source": "data:image/png;base64,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", "x": 1, "xanchor": "right", "xref": "paper", "y": 1.05, "yanchor": "bottom", "yref": "paper"}], "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "title": {"text": "Missing Data Visualisation", "x": 0.5, "xanchor": "center", "y": 0.9, "yanchor": "top"}, "xaxis": {"linecolor": "black", "linewidth": 1, "showline": true, "title": {"text": "Row Index"}}, "yaxis": {"linecolor": "black", "linewidth": 1, "showline": true, "title": {"text": "Missing Columns"}}},
                {"responsive": true}
                )
};

</script>
</div>
<!-- HTML: Plotly chart End-->

This is an interactive plot. It has options to zoom, pan, save as image. We can also use lasso selection from point to point to zoom to specific area. 

This is inspired from a post by fellow Kaggler [AiO](https://www.kaggle.com/notaapple) in which he created a function to Visualize missing data in R. It is very simple yet quick and powerful.

Here, I have recreated it in Python.