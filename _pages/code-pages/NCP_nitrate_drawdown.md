---
layout: splash
permalink: /code-pages/NCP_nitrate_drawdown/
title: "Perform example calculations of nitrate drawdown using a simplified approach"
author_profile: false
last_modified_at: 2025-12-11
toc: false
breadcrumbs: true
---

# Calculation of spring bloom NCP (bNCP) from nitrate drawdown
#### Contents:
1. Importing packages and setting directories
2. Loading in a single float file
3. Exploring the data
    - Plotting the float track
    - Section plots
4. Mixed layer means

----
Seth Bushinsky - Dec. 2025

# 1. Setting up your notebook: packages and directories
---


```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
# import os
# import matplotlib.ticker as mticker
import cartopy.crs as ccrs
import cartopy.feature as cfeature
# from scipy import stats
# from tqdm import tqdm
import xarray as xr
import gsw 
```


```python
# Set directories
base_dir = "/Users/sethbushinsky/"

# I store my data and code  in separate directories, adjust as needed
data_dir = base_dir + "UHM_Ocean_BGC_Group Dropbox/Datasets/"
argo_path = data_dir + "Data_Products/BGC_ARGO_GLOBAL/2025_01_24/processed/for_external_sharing/"
glodap_path = data_dir + "Data_Products/GLODAP/"

# path for saving figures, etc
home_dir = base_dir + "UHM_Ocean_BGC_Group Dropbox/Seth Bushinsky/Work/"
figure_dir = home_dir + "Projects/2025_10_BGC_Argo_Plus_Code_examples/plots/"
plot_ver = 'v_1'
```

# 2. Loading an Argo file and examine its contents
--- 
Let's try to reproduce Plant et al. 2016's numbers. Download 1 or more of these floats here: https://www.bgc-argo-plus.info/float_meta_table/ or access the ftp site directly here: https://ftp.soest.hawaii.edu/bgc_argo_plus/Individual_Floats/outliers_removed/


5902128, 5903274, 5903405, 5903714, 5903891, 5904125. 


```python
# Choose the float file you want to plot:
float_file = '5902128_Sprof_BGCArgoPlus.nc'

# load the file and explore the data
argo_n = xr.open_dataset(argo_path + float_file) 
argo_n
```




<div><svg style="position: absolute; width: 0; height: 0; overflow: hidden">
<defs>
<symbol id="icon-database" viewBox="0 0 32 32">
<path d="M16 0c-8.837 0-16 2.239-16 5v4c0 2.761 7.163 5 16 5s16-2.239 16-5v-4c0-2.761-7.163-5-16-5z"></path>
<path d="M16 17c-8.837 0-16-2.239-16-5v6c0 2.761 7.163 5 16 5s16-2.239 16-5v-6c0 2.761-7.163 5-16 5z"></path>
<path d="M16 26c-8.837 0-16-2.239-16-5v6c0 2.761 7.163 5 16 5s16-2.239 16-5v-6c0 2.761-7.163 5-16 5z"></path>
</symbol>
<symbol id="icon-file-text2" viewBox="0 0 32 32">
<path d="M28.681 7.159c-0.694-0.947-1.662-2.053-2.724-3.116s-2.169-2.030-3.116-2.724c-1.612-1.182-2.393-1.319-2.841-1.319h-15.5c-1.378 0-2.5 1.121-2.5 2.5v27c0 1.378 1.122 2.5 2.5 2.5h23c1.378 0 2.5-1.122 2.5-2.5v-19.5c0-0.448-0.137-1.23-1.319-2.841zM24.543 5.457c0.959 0.959 1.712 1.825 2.268 2.543h-4.811v-4.811c0.718 0.556 1.584 1.309 2.543 2.268zM28 29.5c0 0.271-0.229 0.5-0.5 0.5h-23c-0.271 0-0.5-0.229-0.5-0.5v-27c0-0.271 0.229-0.5 0.5-0.5 0 0 15.499-0 15.5 0v7c0 0.552 0.448 1 1 1h7v19.5z"></path>
<path d="M23 26h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
<path d="M23 22h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
<path d="M23 18h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
</symbol>
</defs>
</svg>
<style>/* CSS stylesheet for displaying xarray objects in jupyterlab.
 *
 */

:root {
  --xr-font-color0: var(--jp-content-font-color0, rgba(0, 0, 0, 1));
  --xr-font-color2: var(--jp-content-font-color2, rgba(0, 0, 0, 0.54));
  --xr-font-color3: var(--jp-content-font-color3, rgba(0, 0, 0, 0.38));
  --xr-border-color: var(--jp-border-color2, #e0e0e0);
  --xr-disabled-color: var(--jp-layout-color3, #bdbdbd);
  --xr-background-color: var(--jp-layout-color0, white);
  --xr-background-color-row-even: var(--jp-layout-color1, white);
  --xr-background-color-row-odd: var(--jp-layout-color2, #eeeeee);
}

html[theme="dark"],
html[data-theme="dark"],
body[data-theme="dark"],
body.vscode-dark {
  --xr-font-color0: rgba(255, 255, 255, 1);
  --xr-font-color2: rgba(255, 255, 255, 0.54);
  --xr-font-color3: rgba(255, 255, 255, 0.38);
  --xr-border-color: #1f1f1f;
  --xr-disabled-color: #515151;
  --xr-background-color: #111111;
  --xr-background-color-row-even: #111111;
  --xr-background-color-row-odd: #313131;
}

.xr-wrap {
  display: block !important;
  min-width: 300px;
  max-width: 700px;
}

.xr-text-repr-fallback {
  /* fallback to plain text repr when CSS is not injected (untrusted notebook) */
  display: none;
}

.xr-header {
  padding-top: 6px;
  padding-bottom: 6px;
  margin-bottom: 4px;
  border-bottom: solid 1px var(--xr-border-color);
}

.xr-header > div,
.xr-header > ul {
  display: inline;
  margin-top: 0;
  margin-bottom: 0;
}

.xr-obj-type,
.xr-array-name {
  margin-left: 2px;
  margin-right: 10px;
}

.xr-obj-type {
  color: var(--xr-font-color2);
}

.xr-sections {
  padding-left: 0 !important;
  display: grid;
  grid-template-columns: 150px auto auto 1fr 0 20px 0 20px;
}

.xr-section-item {
  display: contents;
}

.xr-section-item input {
  display: inline-block;
  opacity: 0;
  height: 0;
}

.xr-section-item input + label {
  color: var(--xr-disabled-color);
}

.xr-section-item input:enabled + label {
  cursor: pointer;
  color: var(--xr-font-color2);
}

.xr-section-item input:focus + label {
  border: 2px solid var(--xr-font-color0);
}

.xr-section-item input:enabled + label:hover {
  color: var(--xr-font-color0);
}

.xr-section-summary {
  grid-column: 1;
  color: var(--xr-font-color2);
  font-weight: 500;
}

.xr-section-summary > span {
  display: inline-block;
  padding-left: 0.5em;
}

.xr-section-summary-in:disabled + label {
  color: var(--xr-font-color2);
}

.xr-section-summary-in + label:before {
  display: inline-block;
  content: "►";
  font-size: 11px;
  width: 15px;
  text-align: center;
}

.xr-section-summary-in:disabled + label:before {
  color: var(--xr-disabled-color);
}

.xr-section-summary-in:checked + label:before {
  content: "▼";
}

.xr-section-summary-in:checked + label > span {
  display: none;
}

.xr-section-summary,
.xr-section-inline-details {
  padding-top: 4px;
  padding-bottom: 4px;
}

.xr-section-inline-details {
  grid-column: 2 / -1;
}

.xr-section-details {
  display: none;
  grid-column: 1 / -1;
  margin-bottom: 5px;
}

.xr-section-summary-in:checked ~ .xr-section-details {
  display: contents;
}

.xr-array-wrap {
  grid-column: 1 / -1;
  display: grid;
  grid-template-columns: 20px auto;
}

.xr-array-wrap > label {
  grid-column: 1;
  vertical-align: top;
}

.xr-preview {
  color: var(--xr-font-color3);
}

.xr-array-preview,
.xr-array-data {
  padding: 0 5px !important;
  grid-column: 2;
}

.xr-array-data,
.xr-array-in:checked ~ .xr-array-preview {
  display: none;
}

.xr-array-in:checked ~ .xr-array-data,
.xr-array-preview {
  display: inline-block;
}

.xr-dim-list {
  display: inline-block !important;
  list-style: none;
  padding: 0 !important;
  margin: 0;
}

.xr-dim-list li {
  display: inline-block;
  padding: 0;
  margin: 0;
}

.xr-dim-list:before {
  content: "(";
}

.xr-dim-list:after {
  content: ")";
}

.xr-dim-list li:not(:last-child):after {
  content: ",";
  padding-right: 5px;
}

.xr-has-index {
  font-weight: bold;
}

.xr-var-list,
.xr-var-item {
  display: contents;
}

.xr-var-item > div,
.xr-var-item label,
.xr-var-item > .xr-var-name span {
  background-color: var(--xr-background-color-row-even);
  margin-bottom: 0;
}

.xr-var-item > .xr-var-name:hover span {
  padding-right: 5px;
}

.xr-var-list > li:nth-child(odd) > div,
.xr-var-list > li:nth-child(odd) > label,
.xr-var-list > li:nth-child(odd) > .xr-var-name span {
  background-color: var(--xr-background-color-row-odd);
}

.xr-var-name {
  grid-column: 1;
}

.xr-var-dims {
  grid-column: 2;
}

.xr-var-dtype {
  grid-column: 3;
  text-align: right;
  color: var(--xr-font-color2);
}

.xr-var-preview {
  grid-column: 4;
}

.xr-index-preview {
  grid-column: 2 / 5;
  color: var(--xr-font-color2);
}

.xr-var-name,
.xr-var-dims,
.xr-var-dtype,
.xr-preview,
.xr-attrs dt {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  padding-right: 10px;
}

.xr-var-name:hover,
.xr-var-dims:hover,
.xr-var-dtype:hover,
.xr-attrs dt:hover {
  overflow: visible;
  width: auto;
  z-index: 1;
}

.xr-var-attrs,
.xr-var-data,
.xr-index-data {
  display: none;
  background-color: var(--xr-background-color) !important;
  padding-bottom: 5px !important;
}

.xr-var-attrs-in:checked ~ .xr-var-attrs,
.xr-var-data-in:checked ~ .xr-var-data,
.xr-index-data-in:checked ~ .xr-index-data {
  display: block;
}

.xr-var-data > table {
  float: right;
}

.xr-var-name span,
.xr-var-data,
.xr-index-name div,
.xr-index-data,
.xr-attrs {
  padding-left: 25px !important;
}

.xr-attrs,
.xr-var-attrs,
.xr-var-data,
.xr-index-data {
  grid-column: 1 / -1;
}

dl.xr-attrs {
  padding: 0;
  margin: 0;
  display: grid;
  grid-template-columns: 125px auto;
}

.xr-attrs dt,
.xr-attrs dd {
  padding: 0;
  margin: 0;
  float: left;
  padding-right: 10px;
  width: auto;
}

.xr-attrs dt {
  font-weight: normal;
  grid-column: 1;
}

.xr-attrs dt:hover span {
  display: inline-block;
  background: var(--xr-background-color);
  padding-right: 10px;
}

.xr-attrs dd {
  grid-column: 2;
  white-space: pre-wrap;
  word-break: break-all;
}

.xr-icon-database,
.xr-icon-file-text2,
.xr-no-icon {
  display: inline-block;
  vertical-align: middle;
  width: 1em;
  height: 1.5em !important;
  stroke-width: 0;
  stroke: currentColor;
  fill: currentColor;
}
</style><pre class='xr-text-repr-fallback'>&lt;xarray.Dataset&gt; Size: 28MB
Dimensions:                            (N_PROF: 216, N_PARAM: 5, N_CALIB: 1,
                                        N_LEVELS: 548)
Coordinates:
    JULD                               (N_PROF) datetime64[ns] 2kB ...
    LATITUDE                           (N_PROF) float64 2kB ...
    LONGITUDE                          (N_PROF) float64 2kB ...
    PRES_ADJUSTED_BGCArgoPlus          (N_PROF, N_LEVELS) float32 473kB ...
  * N_LEVELS                           (N_LEVELS) int64 4kB 0 1 2 ... 546 547
  * N_PROF                             (N_PROF) int64 2kB 0 1 2 ... 213 214 215
Dimensions without coordinates: N_PARAM, N_CALIB
Data variables: (12/86)
    DATA_TYPE                          object 8B ...
    FORMAT_VERSION                     object 8B ...
    HANDBOOK_VERSION                   object 8B ...
    REFERENCE_DATE_TIME                object 8B ...
    DATE_CREATION                      object 8B ...
    DATE_UPDATE                        object 8B ...
    ...                                 ...
    spiciness0                         (N_PROF, N_LEVELS) float64 947kB ...
    cons_temp                          (N_PROF, N_LEVELS) float64 947kB ...
    gamma                              (N_PROF, N_LEVELS) float64 947kB ...
    depth                              (N_PROF, N_LEVELS) float64 947kB ...
    MLD                                (N_PROF) float64 2kB ...
    DOXY_SAT                           (N_PROF, N_LEVELS) float64 947kB ...
Attributes:
    title:                Argo float vertical profile
    institution:          AOML
    source:               Argo float
    history:              2024-12-18T06:05:20Z creation (software version 1.1...
    references:           http://www.argodatamgt.org/Documentation
    user_manual_version:  1.0
    Conventions:          Argo-3.1 CF-1.6
    featureType:          trajectoryProfile
    software_version:     1.18 (version 11.01.2024 for ARGO_simplified_profile)
    id:                   https://doi.org/10.17882/42182</pre><div class='xr-wrap' style='display:none'><div class='xr-header'><div class='xr-obj-type'>xarray.Dataset</div></div><ul class='xr-sections'><li class='xr-section-item'><input id='section-43c2095a-4fd2-4f26-bbd5-20f7c07b6c63' class='xr-section-summary-in' type='checkbox' disabled ><label for='section-43c2095a-4fd2-4f26-bbd5-20f7c07b6c63' class='xr-section-summary'  title='Expand/collapse section'>Dimensions:</label><div class='xr-section-inline-details'><ul class='xr-dim-list'><li><span class='xr-has-index'>N_PROF</span>: 216</li><li><span>N_PARAM</span>: 5</li><li><span>N_CALIB</span>: 1</li><li><span class='xr-has-index'>N_LEVELS</span>: 548</li></ul></div><div class='xr-section-details'></div></li><li class='xr-section-item'><input id='section-ec3dced5-988e-42aa-9ffb-6cab6f5ff72e' class='xr-section-summary-in' type='checkbox'  checked><label for='section-ec3dced5-988e-42aa-9ffb-6cab6f5ff72e' class='xr-section-summary' >Coordinates: <span>(6)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><ul class='xr-var-list'><li class='xr-var-item'><div class='xr-var-name'><span>JULD</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>datetime64[ns]</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f70b602d-e932-445a-9ec3-cc97c4330862' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f70b602d-e932-445a-9ec3-cc97c4330862' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-8ec16eea-07ea-499a-ad24-aed77517e2fc' class='xr-var-data-in' type='checkbox'><label for='data-8ec16eea-07ea-499a-ad24-aed77517e2fc' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Julian day (UTC) of the station relative to REFERENCE_DATE_TIME</dd><dt><span>standard_name :</span></dt><dd>time</dd><dt><span>conventions :</span></dt><dd>Relative julian days with decimal part (as parts of day)</dd><dt><span>axis :</span></dt><dd>T</dd><dt><span>resolution :</span></dt><dd>1e-08</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=datetime64[ns]]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>LATITUDE</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f9bfa309-1a4e-441b-8e0d-14b328430f8c' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f9bfa309-1a4e-441b-8e0d-14b328430f8c' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-5b114ba5-34e3-48a6-9f8d-20c7f83a48b7' class='xr-var-data-in' type='checkbox'><label for='data-5b114ba5-34e3-48a6-9f8d-20c7f83a48b7' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Latitude of the station, best estimate</dd><dt><span>standard_name :</span></dt><dd>latitude</dd><dt><span>units :</span></dt><dd>degree_north</dd><dt><span>valid_min :</span></dt><dd>-90.0</dd><dt><span>valid_max :</span></dt><dd>90.0</dd><dt><span>axis :</span></dt><dd>Y</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=float64]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>LONGITUDE</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-9cd45c6e-9fde-4199-af2d-37a87389ed13' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-9cd45c6e-9fde-4199-af2d-37a87389ed13' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-2275f93c-3994-47ec-ab03-1a197cf79a37' class='xr-var-data-in' type='checkbox'><label for='data-2275f93c-3994-47ec-ab03-1a197cf79a37' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Longitude of the station, best estimate</dd><dt><span>standard_name :</span></dt><dd>longitude</dd><dt><span>units :</span></dt><dd>degree_east</dd><dt><span>valid_min :</span></dt><dd>-180.0</dd><dt><span>valid_max :</span></dt><dd>180.0</dd><dt><span>axis :</span></dt><dd>X</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=float64]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PRES_ADJUSTED_BGCArgoPlus</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-5af903d1-0c06-4cb5-8f57-31cb2f9901a1' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-5af903d1-0c06-4cb5-8f57-31cb2f9901a1' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-90a8f2d7-27db-49f5-aa5c-89d487224ca7' class='xr-var-data-in' type='checkbox'><label for='data-90a8f2d7-27db-49f5-aa5c-89d487224ca7' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Sea water pressure, equals 0 at sea-level</dd><dt><span>standard_name :</span></dt><dd>sea_water_pressure</dd><dt><span>units :</span></dt><dd>decibar</dd><dt><span>valid_min :</span></dt><dd>0.0</dd><dt><span>valid_max :</span></dt><dd>12000.0</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd><dt><span>axis :</span></dt><dd>Z</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>N_LEVELS</span></div><div class='xr-var-dims'>(N_LEVELS)</div><div class='xr-var-dtype'>int64</div><div class='xr-var-preview xr-preview'>0 1 2 3 4 5 ... 543 544 545 546 547</div><input id='attrs-3277a8a5-003a-4e4d-8384-4640545a86af' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-3277a8a5-003a-4e4d-8384-4640545a86af' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-908852ce-baad-400e-aacb-315ae9fe0554' class='xr-var-data-in' type='checkbox'><label for='data-908852ce-baad-400e-aacb-315ae9fe0554' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([  0,   1,   2, ..., 545, 546, 547], shape=(548,))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>N_PROF</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>int64</div><div class='xr-var-preview xr-preview'>0 1 2 3 4 5 ... 211 212 213 214 215</div><input id='attrs-a0853f02-955e-4fef-a531-0495cecb8057' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-a0853f02-955e-4fef-a531-0495cecb8057' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-6347aac7-0021-42fa-adb5-f8e60d344109' class='xr-var-data-in' type='checkbox'><label for='data-6347aac7-0021-42fa-adb5-f8e60d344109' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([  0,   1,   2, ..., 213, 214, 215], shape=(216,))</pre></div></li></ul></div></li><li class='xr-section-item'><input id='section-5280057a-1d7f-4fc6-9eb8-5b2359d90a14' class='xr-section-summary-in' type='checkbox'  ><label for='section-5280057a-1d7f-4fc6-9eb8-5b2359d90a14' class='xr-section-summary' >Data variables: <span>(86)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><ul class='xr-var-list'><li class='xr-var-item'><div class='xr-var-name'><span>DATA_TYPE</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f2d456de-37ca-4567-b3f3-d2c8abc0656c' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f2d456de-37ca-4567-b3f3-d2c8abc0656c' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-475fe355-b2f5-4679-9789-29911edb531d' class='xr-var-data-in' type='checkbox'><label for='data-475fe355-b2f5-4679-9789-29911edb531d' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Data type</dd><dt><span>conventions :</span></dt><dd>Argo reference table 1</dd></dl></div><div class='xr-var-data'><pre>[1 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>FORMAT_VERSION</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-32d31081-b995-4084-bbc5-a9963bbbe04f' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-32d31081-b995-4084-bbc5-a9963bbbe04f' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-76bccf83-60fe-42af-953b-f4f4a7394cf4' class='xr-var-data-in' type='checkbox'><label for='data-76bccf83-60fe-42af-953b-f4f4a7394cf4' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>File format version</dd></dl></div><div class='xr-var-data'><pre>[1 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>HANDBOOK_VERSION</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-5694ba61-007f-48f6-8437-3ae8ba0de89c' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-5694ba61-007f-48f6-8437-3ae8ba0de89c' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-51ebda74-e6b5-438d-b372-9cf1225705ea' class='xr-var-data-in' type='checkbox'><label for='data-51ebda74-e6b5-438d-b372-9cf1225705ea' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Data handbook version</dd></dl></div><div class='xr-var-data'><pre>[1 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>REFERENCE_DATE_TIME</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-c818015d-cb5a-4eb9-8d1b-af878d4ec84f' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-c818015d-cb5a-4eb9-8d1b-af878d4ec84f' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-fff5dcf5-6680-4b86-a830-1626a578cb02' class='xr-var-data-in' type='checkbox'><label for='data-fff5dcf5-6680-4b86-a830-1626a578cb02' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Date of reference for Julian days</dd><dt><span>conventions :</span></dt><dd>YYYYMMDDHHMISS</dd></dl></div><div class='xr-var-data'><pre>[1 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DATE_CREATION</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-012ae3ba-b139-445d-9c21-357eede639e5' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-012ae3ba-b139-445d-9c21-357eede639e5' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-2520627f-348f-4c1e-9052-2f3420e0cf2b' class='xr-var-data-in' type='checkbox'><label for='data-2520627f-348f-4c1e-9052-2f3420e0cf2b' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Date of file creation</dd><dt><span>conventions :</span></dt><dd>YYYYMMDDHHMISS</dd></dl></div><div class='xr-var-data'><pre>[1 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DATE_UPDATE</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-7c9e05c5-8def-4154-9562-7eae939fec73' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-7c9e05c5-8def-4154-9562-7eae939fec73' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-2e47584a-1c02-4de9-88e9-c01007e820f4' class='xr-var-data-in' type='checkbox'><label for='data-2e47584a-1c02-4de9-88e9-c01007e820f4' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Date of update of this file</dd><dt><span>conventions :</span></dt><dd>YYYYMMDDHHMISS</dd></dl></div><div class='xr-var-data'><pre>[1 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PLATFORM_NUMBER</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-ab627445-b65a-4af1-aea2-3130755baa09' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-ab627445-b65a-4af1-aea2-3130755baa09' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-95eccf5c-0391-43b0-b744-a848cb4833d0' class='xr-var-data-in' type='checkbox'><label for='data-95eccf5c-0391-43b0-b744-a848cb4833d0' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Float unique identifier</dd><dt><span>conventions :</span></dt><dd>WMO float identifier : A9IIIII</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PROJECT_NAME</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f3540163-e599-41ee-aa55-126ce5af6e3f' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f3540163-e599-41ee-aa55-126ce5af6e3f' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-89df456b-9d1f-4833-ac9b-062a38ae2196' class='xr-var-data-in' type='checkbox'><label for='data-89df456b-9d1f-4833-ac9b-062a38ae2196' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Name of the project</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PI_NAME</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-7e25863d-d243-45d1-9368-b74aabdff976' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-7e25863d-d243-45d1-9368-b74aabdff976' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-87ea7f2d-d9a1-4060-b371-3c558a5af14d' class='xr-var-data-in' type='checkbox'><label for='data-87ea7f2d-d9a1-4060-b371-3c558a5af14d' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Name of the principal investigator</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>STATION_PARAMETERS</span></div><div class='xr-var-dims'>(N_PROF, N_PARAM)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-6d579404-36cc-418b-9462-44aa4353abb8' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-6d579404-36cc-418b-9462-44aa4353abb8' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-e9cecd27-d714-452f-afd7-5058c0cfd451' class='xr-var-data-in' type='checkbox'><label for='data-e9cecd27-d714-452f-afd7-5058c0cfd451' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>List of available parameters for the station</dd><dt><span>conventions :</span></dt><dd>Argo reference table 3</dd></dl></div><div class='xr-var-data'><pre>[1080 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>CYCLE_NUMBER</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-75cb8d80-abf5-410e-9d24-2587027492bd' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-75cb8d80-abf5-410e-9d24-2587027492bd' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-4fdd6726-4971-4a46-9059-b7f55446166c' class='xr-var-data-in' type='checkbox'><label for='data-4fdd6726-4971-4a46-9059-b7f55446166c' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Float cycle number</dd><dt><span>conventions :</span></dt><dd>0...N, 0 : launch cycle (if exists), 1 : first complete cycle</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=float64]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DIRECTION</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-b3163fc0-208b-4ad6-985d-103a7188a37a' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-b3163fc0-208b-4ad6-985d-103a7188a37a' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c0e3e9f0-3db5-44cb-b1ac-1830b461cb0b' class='xr-var-data-in' type='checkbox'><label for='data-c0e3e9f0-3db5-44cb-b1ac-1830b461cb0b' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Direction of the station profiles</dd><dt><span>conventions :</span></dt><dd>A: ascending profiles, D: descending profiles</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DATA_CENTRE</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-09fbd09d-79c1-4feb-8656-cb99ef731948' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-09fbd09d-79c1-4feb-8656-cb99ef731948' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-7f8ac8e5-35c2-4a0e-8c00-745380ebbe87' class='xr-var-data-in' type='checkbox'><label for='data-7f8ac8e5-35c2-4a0e-8c00-745380ebbe87' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Data centre in charge of float data processing</dd><dt><span>conventions :</span></dt><dd>Argo reference table 4</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PARAMETER_DATA_MODE</span></div><div class='xr-var-dims'>(N_PROF, N_PARAM)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-6858da0c-2b1b-4974-95f5-8f283c221b15' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-6858da0c-2b1b-4974-95f5-8f283c221b15' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-3222b61c-3530-46e4-aa6c-13dcd5273514' class='xr-var-data-in' type='checkbox'><label for='data-3222b61c-3530-46e4-aa6c-13dcd5273514' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Delayed mode or real time data</dd><dt><span>conventions :</span></dt><dd>R : real time; D : delayed mode; A : real time with adjustment</dd></dl></div><div class='xr-var-data'><pre>[1080 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PLATFORM_TYPE</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-a32e72ca-c22f-47ed-b9ce-d7fc0059b295' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-a32e72ca-c22f-47ed-b9ce-d7fc0059b295' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-312fbf2e-b771-4c6d-82a2-adc356fb017d' class='xr-var-data-in' type='checkbox'><label for='data-312fbf2e-b771-4c6d-82a2-adc356fb017d' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Type of float</dd><dt><span>conventions :</span></dt><dd>Argo reference table 23</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>FLOAT_SERIAL_NO</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-3b43badd-2554-41ba-b0a2-5b9556fbac36' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-3b43badd-2554-41ba-b0a2-5b9556fbac36' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-e3fb3083-9ab2-44cb-bcf1-ed274a813184' class='xr-var-data-in' type='checkbox'><label for='data-e3fb3083-9ab2-44cb-bcf1-ed274a813184' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Serial number of the float</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>FIRMWARE_VERSION</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-0bfeb5c6-4a35-424d-a780-090bbcbfa83b' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-0bfeb5c6-4a35-424d-a780-090bbcbfa83b' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-113e6e9a-acfe-4d5b-a3c6-090078877f1f' class='xr-var-data-in' type='checkbox'><label for='data-113e6e9a-acfe-4d5b-a3c6-090078877f1f' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Instrument firmware version</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>WMO_INST_TYPE</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-e34bd705-f60f-4573-bf8f-f64a64ddb975' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-e34bd705-f60f-4573-bf8f-f64a64ddb975' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-54c6200d-1634-428e-b6c6-fec19416089b' class='xr-var-data-in' type='checkbox'><label for='data-54c6200d-1634-428e-b6c6-fec19416089b' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Coded instrument type</dd><dt><span>conventions :</span></dt><dd>Argo reference table 8</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>JULD_QC</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-01412c6a-de35-4aa9-a8a8-22730f5329f6' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-01412c6a-de35-4aa9-a8a8-22730f5329f6' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c1e45a05-389c-409d-938f-afea46230e37' class='xr-var-data-in' type='checkbox'><label for='data-c1e45a05-389c-409d-938f-afea46230e37' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Quality on date and time</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>JULD_LOCATION</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>datetime64[ns]</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f09d807d-516e-4581-965a-27192525e8e5' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f09d807d-516e-4581-965a-27192525e8e5' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-67d0a8bd-2e50-4897-a7ff-c84d13644085' class='xr-var-data-in' type='checkbox'><label for='data-67d0a8bd-2e50-4897-a7ff-c84d13644085' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Julian day (UTC) of the location relative to REFERENCE_DATE_TIME</dd><dt><span>conventions :</span></dt><dd>Relative julian days with decimal part (as parts of day)</dd><dt><span>resolution :</span></dt><dd>1e-08</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=datetime64[ns]]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>POSITION_QC</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-5ec648ec-ddba-42cd-90fc-adaa2cdd8866' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-5ec648ec-ddba-42cd-90fc-adaa2cdd8866' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-1ab44cff-2216-4460-9262-a0ac708948b8' class='xr-var-data-in' type='checkbox'><label for='data-1ab44cff-2216-4460-9262-a0ac708948b8' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Quality on position (latitude and longitude)</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>POSITIONING_SYSTEM</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-b7009003-3aa7-4e8d-b1d8-536e1b2659f2' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-b7009003-3aa7-4e8d-b1d8-536e1b2659f2' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-ab0774ec-96aa-4fca-aefd-30614a3a6ff7' class='xr-var-data-in' type='checkbox'><label for='data-ab0774ec-96aa-4fca-aefd-30614a3a6ff7' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Positioning system</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>CONFIG_MISSION_NUMBER</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-ba2ea2fa-1560-4741-9c0d-623ff9e94ec8' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-ba2ea2fa-1560-4741-9c0d-623ff9e94ec8' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-2ea5208a-9179-4652-a26f-b0315374a9d4' class='xr-var-data-in' type='checkbox'><label for='data-2ea5208a-9179-4652-a26f-b0315374a9d4' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Unique number denoting the missions performed by the float</dd><dt><span>conventions :</span></dt><dd>1...N, 1 : first complete mission</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=float64]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PARAMETER</span></div><div class='xr-var-dims'>(N_PROF, N_CALIB, N_PARAM)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f9fb08d9-db93-43d8-ab57-1d130eb535df' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f9fb08d9-db93-43d8-ab57-1d130eb535df' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-05693fcb-e1da-417d-ac9b-fc2c40a0b856' class='xr-var-data-in' type='checkbox'><label for='data-05693fcb-e1da-417d-ac9b-fc2c40a0b856' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>List of parameters with calibration information</dd><dt><span>conventions :</span></dt><dd>Argo reference table 3</dd></dl></div><div class='xr-var-data'><pre>[1080 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>SCIENTIFIC_CALIB_EQUATION</span></div><div class='xr-var-dims'>(N_PROF, N_CALIB, N_PARAM)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-ba7b64a0-1db6-4153-a6c2-fbfb16d307a4' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-ba7b64a0-1db6-4153-a6c2-fbfb16d307a4' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-8f2a41f0-83b5-4fbc-a84f-c1b92f996a63' class='xr-var-data-in' type='checkbox'><label for='data-8f2a41f0-83b5-4fbc-a84f-c1b92f996a63' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Calibration equation for this parameter</dd></dl></div><div class='xr-var-data'><pre>[1080 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>SCIENTIFIC_CALIB_COEFFICIENT</span></div><div class='xr-var-dims'>(N_PROF, N_CALIB, N_PARAM)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-d5f59fb8-2b41-4401-ae40-5d314afe4325' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-d5f59fb8-2b41-4401-ae40-5d314afe4325' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-a662d24b-8533-444f-a627-e14c7d6b372a' class='xr-var-data-in' type='checkbox'><label for='data-a662d24b-8533-444f-a627-e14c7d6b372a' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Calibration coefficients for this equation</dd></dl></div><div class='xr-var-data'><pre>[1080 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>SCIENTIFIC_CALIB_COMMENT</span></div><div class='xr-var-dims'>(N_PROF, N_CALIB, N_PARAM)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-1ff9c2ea-15b2-4010-8676-c1a9d09f396b' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-1ff9c2ea-15b2-4010-8676-c1a9d09f396b' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-406d242f-5b6d-419f-a637-6fc3b4aa58c7' class='xr-var-data-in' type='checkbox'><label for='data-406d242f-5b6d-419f-a637-6fc3b4aa58c7' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Comment applying to this parameter calibration</dd></dl></div><div class='xr-var-data'><pre>[1080 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>SCIENTIFIC_CALIB_DATE</span></div><div class='xr-var-dims'>(N_PROF, N_CALIB, N_PARAM)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f4cfc180-e34c-498d-9b51-17a9404e3f61' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f4cfc180-e34c-498d-9b51-17a9404e3f61' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-9fb52d7b-acfb-478c-bda2-03c103fb0ac0' class='xr-var-data-in' type='checkbox'><label for='data-9fb52d7b-acfb-478c-bda2-03c103fb0ac0' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Date of calibration</dd><dt><span>conventions :</span></dt><dd>YYYYMMDDHHMISS</dd></dl></div><div class='xr-var-data'><pre>[1080 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PROFILE_PRES_QC</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-761e9966-4a00-4286-98c3-2616b4d5a12f' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-761e9966-4a00-4286-98c3-2616b4d5a12f' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-e715d4fa-0f20-461b-a6e8-e1dfdaf0c8e5' class='xr-var-data-in' type='checkbox'><label for='data-e715d4fa-0f20-461b-a6e8-e1dfdaf0c8e5' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Global quality flag of PRES profile</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2a</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PROFILE_TEMP_QC</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f6e4579a-bab7-4c8a-bf74-56181a3fc9c4' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f6e4579a-bab7-4c8a-bf74-56181a3fc9c4' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c5e10021-29bc-408e-97f2-f34fd810764c' class='xr-var-data-in' type='checkbox'><label for='data-c5e10021-29bc-408e-97f2-f34fd810764c' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Global quality flag of TEMP profile</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2a</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PROFILE_PSAL_QC</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-565dd46c-be3a-42de-a229-93345691fe18' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-565dd46c-be3a-42de-a229-93345691fe18' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-2e535167-936f-43a6-8a0f-98af7332911c' class='xr-var-data-in' type='checkbox'><label for='data-2e535167-936f-43a6-8a0f-98af7332911c' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Global quality flag of PSAL profile</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2a</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PROFILE_DOXY_QC</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-4a3a7fff-5420-4f9b-abc9-24f47eed82cb' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-4a3a7fff-5420-4f9b-abc9-24f47eed82cb' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c10b9c3e-7f55-470f-9ec3-144c8839f47d' class='xr-var-data-in' type='checkbox'><label for='data-c10b9c3e-7f55-470f-9ec3-144c8839f47d' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Global quality flag of DOXY profile</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2a</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PROFILE_NITRATE_QC</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-ac0e7d55-9548-43e6-9bdb-995bdbc10566' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-ac0e7d55-9548-43e6-9bdb-995bdbc10566' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-8e7aa552-6cbd-47a9-84bd-1bf784c087f4' class='xr-var-data-in' type='checkbox'><label for='data-8e7aa552-6cbd-47a9-84bd-1bf784c087f4' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Global quality flag of NITRATE profile</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2a</dd></dl></div><div class='xr-var-data'><pre>[216 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PRES</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-0c5486f8-e460-451e-9f73-2b810838fc6f' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-0c5486f8-e460-451e-9f73-2b810838fc6f' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-839138af-61ed-4c96-aaef-872b73b4d87b' class='xr-var-data-in' type='checkbox'><label for='data-839138af-61ed-4c96-aaef-872b73b4d87b' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Sea water pressure, equals 0 at sea-level</dd><dt><span>standard_name :</span></dt><dd>sea_water_pressure</dd><dt><span>units :</span></dt><dd>decibar</dd><dt><span>valid_min :</span></dt><dd>0.0</dd><dt><span>valid_max :</span></dt><dd>12000.0</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd><dt><span>axis :</span></dt><dd>Z</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PRES_QC</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f7fa65dd-24b2-422f-b9d0-f495aa1fcef3' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f7fa65dd-24b2-422f-b9d0-f495aa1fcef3' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-1e67023f-e1e1-45d2-a020-1492c055a079' class='xr-var-data-in' type='checkbox'><label for='data-1e67023f-e1e1-45d2-a020-1492c055a079' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>quality flag</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PRES_ADJUSTED</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-3edd2bac-83ef-443e-930b-2a95bddef711' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-3edd2bac-83ef-443e-930b-2a95bddef711' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-35a14c8a-4330-4bd6-bbae-42a0b328ef1c' class='xr-var-data-in' type='checkbox'><label for='data-35a14c8a-4330-4bd6-bbae-42a0b328ef1c' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Sea water pressure, equals 0 at sea-level</dd><dt><span>standard_name :</span></dt><dd>sea_water_pressure</dd><dt><span>units :</span></dt><dd>decibar</dd><dt><span>valid_min :</span></dt><dd>0.0</dd><dt><span>valid_max :</span></dt><dd>12000.0</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd><dt><span>axis :</span></dt><dd>Z</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PRES_ADJUSTED_QC</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-12d18eb9-6122-45a3-a074-02492d1285cd' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-12d18eb9-6122-45a3-a074-02492d1285cd' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-f83eca40-abf2-4469-a90f-298dc28c9e26' class='xr-var-data-in' type='checkbox'><label for='data-f83eca40-abf2-4469-a90f-298dc28c9e26' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>quality flag</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PRES_ADJUSTED_ERROR</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-a1fa704c-3322-4ccf-afe2-129455232d25' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-a1fa704c-3322-4ccf-afe2-129455232d25' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-13174d44-27ab-4cf1-8b85-3bd30ee2390a' class='xr-var-data-in' type='checkbox'><label for='data-13174d44-27ab-4cf1-8b85-3bd30ee2390a' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Contains the error on the adjusted values as determined by the delayed mode QC process</dd><dt><span>units :</span></dt><dd>decibar</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>TEMP</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-d2d3b510-66aa-48a7-844d-48851582b08f' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-d2d3b510-66aa-48a7-844d-48851582b08f' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-d6f16bd0-0704-4f1c-b28b-0a9fe2453256' class='xr-var-data-in' type='checkbox'><label for='data-d6f16bd0-0704-4f1c-b28b-0a9fe2453256' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Sea temperature in-situ ITS-90 scale</dd><dt><span>standard_name :</span></dt><dd>sea_water_temperature</dd><dt><span>units :</span></dt><dd>degree_Celsius</dd><dt><span>valid_min :</span></dt><dd>-2.5</dd><dt><span>valid_max :</span></dt><dd>40.0</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>TEMP_QC</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-2afe10fb-5583-4c24-a3e2-703499c13e33' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-2afe10fb-5583-4c24-a3e2-703499c13e33' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c56e0333-093a-4d6b-89bc-233656ad44c3' class='xr-var-data-in' type='checkbox'><label for='data-c56e0333-093a-4d6b-89bc-233656ad44c3' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>quality flag</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>TEMP_dPRES</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-eee9bb10-40bd-43f9-b54d-e984e1ebe043' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-eee9bb10-40bd-43f9-b54d-e984e1ebe043' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-9220488e-65b9-4404-8078-1d86fd392b6f' class='xr-var-data-in' type='checkbox'><label for='data-9220488e-65b9-4404-8078-1d86fd392b6f' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>TEMP pressure displacement from original sampled value</dd><dt><span>units :</span></dt><dd>decibar</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>TEMP_ADJUSTED</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-ed93ca0c-b38f-4a00-bbca-85944f4ce6b7' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-ed93ca0c-b38f-4a00-bbca-85944f4ce6b7' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-ed3786d4-e802-4f46-a72e-7a175f8d50fa' class='xr-var-data-in' type='checkbox'><label for='data-ed3786d4-e802-4f46-a72e-7a175f8d50fa' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Sea temperature in-situ ITS-90 scale</dd><dt><span>standard_name :</span></dt><dd>sea_water_temperature</dd><dt><span>units :</span></dt><dd>degree_Celsius</dd><dt><span>valid_min :</span></dt><dd>-2.5</dd><dt><span>valid_max :</span></dt><dd>40.0</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>TEMP_ADJUSTED_QC</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-c45a2151-9ccb-4921-83d2-29e184a606de' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-c45a2151-9ccb-4921-83d2-29e184a606de' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-f60ef476-ea00-47d3-bdf4-8098ac440f7e' class='xr-var-data-in' type='checkbox'><label for='data-f60ef476-ea00-47d3-bdf4-8098ac440f7e' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>quality flag</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>TEMP_ADJUSTED_ERROR</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-69db8082-0d73-441d-b011-25cdcb73a241' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-69db8082-0d73-441d-b011-25cdcb73a241' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-5c369c08-77b2-408e-808f-08259ebd8683' class='xr-var-data-in' type='checkbox'><label for='data-5c369c08-77b2-408e-808f-08259ebd8683' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Contains the error on the adjusted values as determined by the delayed mode QC process</dd><dt><span>units :</span></dt><dd>degree_Celsius</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PSAL</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-fc40f437-0d64-4457-a677-e19ea9871bf6' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-fc40f437-0d64-4457-a677-e19ea9871bf6' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-d1ff8980-e0f0-4f99-96e8-40549b151c33' class='xr-var-data-in' type='checkbox'><label for='data-d1ff8980-e0f0-4f99-96e8-40549b151c33' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Practical salinity</dd><dt><span>standard_name :</span></dt><dd>sea_water_salinity</dd><dt><span>units :</span></dt><dd>psu</dd><dt><span>valid_min :</span></dt><dd>2.0</dd><dt><span>valid_max :</span></dt><dd>41.0</dd><dt><span>C_format :</span></dt><dd>%.4f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.4</dd><dt><span>resolution :</span></dt><dd>1e-04</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PSAL_QC</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-60d3e455-515d-46b2-9147-6ccc0ef64116' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-60d3e455-515d-46b2-9147-6ccc0ef64116' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-a237fb39-afa2-4370-85fa-49a703d2f261' class='xr-var-data-in' type='checkbox'><label for='data-a237fb39-afa2-4370-85fa-49a703d2f261' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>quality flag</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PSAL_dPRES</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-e8ddf879-2a52-48b6-800b-babb50781753' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-e8ddf879-2a52-48b6-800b-babb50781753' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-cb7c679e-c4bf-49b5-9dad-f5a3437aa471' class='xr-var-data-in' type='checkbox'><label for='data-cb7c679e-c4bf-49b5-9dad-f5a3437aa471' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>PSAL pressure displacement from original sampled value</dd><dt><span>units :</span></dt><dd>decibar</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PSAL_ADJUSTED</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-c6ef33cc-f0f9-4d89-957a-683d17f1f5db' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-c6ef33cc-f0f9-4d89-957a-683d17f1f5db' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-b718b807-f933-471a-90e6-608ff47f6738' class='xr-var-data-in' type='checkbox'><label for='data-b718b807-f933-471a-90e6-608ff47f6738' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Practical salinity</dd><dt><span>standard_name :</span></dt><dd>sea_water_salinity</dd><dt><span>units :</span></dt><dd>psu</dd><dt><span>valid_min :</span></dt><dd>2.0</dd><dt><span>valid_max :</span></dt><dd>41.0</dd><dt><span>C_format :</span></dt><dd>%.4f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.4</dd><dt><span>resolution :</span></dt><dd>1e-04</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PSAL_ADJUSTED_QC</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-dcfd44bd-2839-4482-8daf-99b7a629dbf6' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-dcfd44bd-2839-4482-8daf-99b7a629dbf6' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-5afba924-cb74-476f-873a-0f908f83b7c0' class='xr-var-data-in' type='checkbox'><label for='data-5afba924-cb74-476f-873a-0f908f83b7c0' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>quality flag</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PSAL_ADJUSTED_ERROR</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f7d22dee-43b2-4103-b828-d3b28b459eed' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f7d22dee-43b2-4103-b828-d3b28b459eed' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-785c8710-ccf6-4e74-b1cf-7cd55e8fb522' class='xr-var-data-in' type='checkbox'><label for='data-785c8710-ccf6-4e74-b1cf-7cd55e8fb522' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Contains the error on the adjusted values as determined by the delayed mode QC process</dd><dt><span>units :</span></dt><dd>psu</dd><dt><span>C_format :</span></dt><dd>%.4f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.4</dd><dt><span>resolution :</span></dt><dd>1e-04</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DOXY</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-34c6c4c0-ccfd-4105-9f3a-9a16803ce1d5' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-34c6c4c0-ccfd-4105-9f3a-9a16803ce1d5' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c290231f-a321-4b53-b623-97c513212580' class='xr-var-data-in' type='checkbox'><label for='data-c290231f-a321-4b53-b623-97c513212580' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Dissolved oxygen</dd><dt><span>standard_name :</span></dt><dd>moles_of_oxygen_per_unit_mass_in_sea_water</dd><dt><span>units :</span></dt><dd>micromole/kg</dd><dt><span>valid_min :</span></dt><dd>-5.0</dd><dt><span>valid_max :</span></dt><dd>600.0</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DOXY_QC</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-5b677976-0564-47a5-8f70-e4f55aef873d' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-5b677976-0564-47a5-8f70-e4f55aef873d' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-14af7cf0-7853-4087-b6ed-b1fd2a47890b' class='xr-var-data-in' type='checkbox'><label for='data-14af7cf0-7853-4087-b6ed-b1fd2a47890b' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>quality flag</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DOXY_dPRES</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-e5cac1e8-ee9b-4e1f-9741-dcc4dee44cc5' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-e5cac1e8-ee9b-4e1f-9741-dcc4dee44cc5' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-78723c6b-ac18-44d0-a9c4-d8d1ea004b41' class='xr-var-data-in' type='checkbox'><label for='data-78723c6b-ac18-44d0-a9c4-d8d1ea004b41' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>DOXY pressure displacement from original sampled value</dd><dt><span>units :</span></dt><dd>decibar</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DOXY_ADJUSTED</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-cf83ebac-963b-4053-aec5-08cf94256ba2' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-cf83ebac-963b-4053-aec5-08cf94256ba2' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-f7d75de6-d0f6-49c2-b7d7-642d2c1449d2' class='xr-var-data-in' type='checkbox'><label for='data-f7d75de6-d0f6-49c2-b7d7-642d2c1449d2' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Dissolved oxygen</dd><dt><span>standard_name :</span></dt><dd>moles_of_oxygen_per_unit_mass_in_sea_water</dd><dt><span>units :</span></dt><dd>micromole/kg</dd><dt><span>valid_min :</span></dt><dd>-5.0</dd><dt><span>valid_max :</span></dt><dd>600.0</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DOXY_ADJUSTED_QC</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f7007e8c-7aac-4844-9b46-8a084ab6563b' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f7007e8c-7aac-4844-9b46-8a084ab6563b' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-90c7073c-c448-4867-923a-61bec9b47f58' class='xr-var-data-in' type='checkbox'><label for='data-90c7073c-c448-4867-923a-61bec9b47f58' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>quality flag</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DOXY_ADJUSTED_ERROR</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-cd464b69-c3f9-40c3-beca-a4b7494d2d22' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-cd464b69-c3f9-40c3-beca-a4b7494d2d22' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-7b191dd0-64bb-4f21-9848-59e258c070ed' class='xr-var-data-in' type='checkbox'><label for='data-7b191dd0-64bb-4f21-9848-59e258c070ed' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Contains the error on the adjusted values as determined by the delayed mode QC process</dd><dt><span>units :</span></dt><dd>micromole/kg</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>NITRATE</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f6f906b9-727c-4117-8c80-f762d848888d' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f6f906b9-727c-4117-8c80-f762d848888d' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-12449ad1-021f-4b70-9f47-e30c7044ee9a' class='xr-var-data-in' type='checkbox'><label for='data-12449ad1-021f-4b70-9f47-e30c7044ee9a' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Nitrate</dd><dt><span>standard_name :</span></dt><dd>moles_of_nitrate_per_unit_mass_in_sea_water</dd><dt><span>units :</span></dt><dd>micromole/kg</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>NITRATE_QC</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-1d59d9ec-3371-4f5a-b0a7-aca2c4687c29' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-1d59d9ec-3371-4f5a-b0a7-aca2c4687c29' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-cfe9cb77-b10e-4c55-899b-35d375c358a0' class='xr-var-data-in' type='checkbox'><label for='data-cfe9cb77-b10e-4c55-899b-35d375c358a0' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>quality flag</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>NITRATE_dPRES</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-cf6c8c42-cce8-4306-b4be-a5288ffda197' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-cf6c8c42-cce8-4306-b4be-a5288ffda197' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c1550b09-79bb-46ca-9e2f-a889c81dcd67' class='xr-var-data-in' type='checkbox'><label for='data-c1550b09-79bb-46ca-9e2f-a889c81dcd67' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>NITRATE pressure displacement from original sampled value</dd><dt><span>units :</span></dt><dd>decibar</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>NITRATE_ADJUSTED</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-2e1c6cfc-b94c-48b6-952e-47fff8294979' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-2e1c6cfc-b94c-48b6-952e-47fff8294979' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c85dc404-9efb-46e5-a2a1-b03494fcdd20' class='xr-var-data-in' type='checkbox'><label for='data-c85dc404-9efb-46e5-a2a1-b03494fcdd20' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Nitrate</dd><dt><span>standard_name :</span></dt><dd>moles_of_nitrate_per_unit_mass_in_sea_water</dd><dt><span>units :</span></dt><dd>micromole/kg</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>NITRATE_ADJUSTED_QC</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-17e56897-abac-4ac9-830e-3c93c30a194b' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-17e56897-abac-4ac9-830e-3c93c30a194b' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-fd8884ad-1821-4baa-b730-94bc4924ce01' class='xr-var-data-in' type='checkbox'><label for='data-fd8884ad-1821-4baa-b730-94bc4924ce01' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>quality flag</dd><dt><span>conventions :</span></dt><dd>Argo reference table 2</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=object]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>NITRATE_ADJUSTED_ERROR</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-a4bba938-b5e2-4f66-9e19-074c3271d674' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-a4bba938-b5e2-4f66-9e19-074c3271d674' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-1e1e7f21-bdf3-4e6c-a312-15335d97a346' class='xr-var-data-in' type='checkbox'><label for='data-1e1e7f21-bdf3-4e6c-a312-15335d97a346' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Contains the error on the adjusted values as determined by the delayed mode QC process</dd><dt><span>units :</span></dt><dd>micromole/kg</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>CTD_CNDC_model</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>&lt;U7</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-fe57d57d-5236-442f-81bf-e6f147d6921c' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-fe57d57d-5236-442f-81bf-e6f147d6921c' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-d44c60bc-6986-461d-8a84-a64fe62b1b7f' class='xr-var-data-in' type='checkbox'><label for='data-d44c60bc-6986-461d-8a84-a64fe62b1b7f' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[1 values with dtype=&lt;U7]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>CTD_TEMP_model</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>&lt;U7</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-0dfa2924-084d-430b-a0b4-a650dc8fce18' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-0dfa2924-084d-430b-a0b4-a650dc8fce18' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-bc4a0dbe-f4e7-43b5-9b28-9bc76b0133f3' class='xr-var-data-in' type='checkbox'><label for='data-bc4a0dbe-f4e7-43b5-9b28-9bc76b0133f3' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[1 values with dtype=&lt;U7]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>CTD_PRES_model</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>&lt;U14</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-7b3a893f-efc2-48a6-a9a8-49c23f9a34c9' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-7b3a893f-efc2-48a6-a9a8-49c23f9a34c9' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-ae2bc1b5-e0c1-4eea-b739-ef9329f38c73' class='xr-var-data-in' type='checkbox'><label for='data-ae2bc1b5-e0c1-4eea-b739-ef9329f38c73' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[1 values with dtype=&lt;U14]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>OPTODE_DOXY_model</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>&lt;U20</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-2bbc96ab-9d03-4496-b690-0f366b5b47f3' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-2bbc96ab-9d03-4496-b690-0f366b5b47f3' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-04b75e87-96a7-4d2c-8f7b-cafeb46e8ccb' class='xr-var-data-in' type='checkbox'><label for='data-04b75e87-96a7-4d2c-8f7b-cafeb46e8ccb' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[1 values with dtype=&lt;U20]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>SPECTROPHOTOMETER_NITRATE_model</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>&lt;U4</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-599ecd1e-2d84-4df0-af69-a5141266ade3' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-599ecd1e-2d84-4df0-af69-a5141266ade3' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-767b8dc1-fb03-4003-81b7-6a4f5654f20f' class='xr-var-data-in' type='checkbox'><label for='data-767b8dc1-fb03-4003-81b7-6a4f5654f20f' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[1 values with dtype=&lt;U4]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>O2_cal_type</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>&lt;U7</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-8081dacd-32df-408d-964d-240576265c86' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-8081dacd-32df-408d-964d-240576265c86' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-331e241e-1571-4132-a5f7-b23485110e81' class='xr-var-data-in' type='checkbox'><label for='data-331e241e-1571-4132-a5f7-b23485110e81' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[1 values with dtype=&lt;U7]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>WMO_ID</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>int64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-01908b11-9026-4433-a69d-3a66ae15d81e' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-01908b11-9026-4433-a69d-3a66ae15d81e' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-cbf5402f-551d-4c6a-861b-2510da3e1ff3' class='xr-var-data-in' type='checkbox'><label for='data-cbf5402f-551d-4c6a-861b-2510da3e1ff3' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[1 values with dtype=int64]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PRES_ADJUSTED_BGCArgoPlus_flag</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>&lt;U2</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-31a21e3e-4e8e-40c4-88f3-075c242cd2f1' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-31a21e3e-4e8e-40c4-88f3-075c242cd2f1' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-28fba36e-dc6a-4c10-bac1-935025715599' class='xr-var-data-in' type='checkbox'><label for='data-28fba36e-dc6a-4c10-bac1-935025715599' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[1 values with dtype=&lt;U2]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>TEMP_ADJUSTED_BGCArgoPlus</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-c1263a3a-4a8d-4575-9418-20896f3f98c4' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-c1263a3a-4a8d-4575-9418-20896f3f98c4' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-3d60e236-c23e-4a4e-94f4-330ea3ade699' class='xr-var-data-in' type='checkbox'><label for='data-3d60e236-c23e-4a4e-94f4-330ea3ade699' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Sea temperature in-situ ITS-90 scale</dd><dt><span>standard_name :</span></dt><dd>sea_water_temperature</dd><dt><span>units :</span></dt><dd>degree_Celsius</dd><dt><span>valid_min :</span></dt><dd>-2.5</dd><dt><span>valid_max :</span></dt><dd>40.0</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>TEMP_ADJUSTED_BGCArgoPlus_flag</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>&lt;U7</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-4f99b2c6-6561-4e2e-b5be-41748f40a615' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-4f99b2c6-6561-4e2e-b5be-41748f40a615' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c47ad6d1-f67b-4ae3-a9d8-063029cfbc2b' class='xr-var-data-in' type='checkbox'><label for='data-c47ad6d1-f67b-4ae3-a9d8-063029cfbc2b' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[1 values with dtype=&lt;U7]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PSAL_ADJUSTED_BGCArgoPlus</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-f34fa7ff-046a-4cb5-bd84-7423f1ca2066' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f34fa7ff-046a-4cb5-bd84-7423f1ca2066' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-62339f8e-4b62-45a1-a1e9-3545fe40f92b' class='xr-var-data-in' type='checkbox'><label for='data-62339f8e-4b62-45a1-a1e9-3545fe40f92b' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Practical salinity</dd><dt><span>standard_name :</span></dt><dd>sea_water_salinity</dd><dt><span>units :</span></dt><dd>psu</dd><dt><span>valid_min :</span></dt><dd>2.0</dd><dt><span>valid_max :</span></dt><dd>41.0</dd><dt><span>C_format :</span></dt><dd>%.4f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.4</dd><dt><span>resolution :</span></dt><dd>1e-04</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>PSAL_ADJUSTED_BGCArgoPlus_flag</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>&lt;U11</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-e385933b-d203-48c5-a1b2-5428ceb911af' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-e385933b-d203-48c5-a1b2-5428ceb911af' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c8f3966b-2407-4370-8916-be37d0f6c843' class='xr-var-data-in' type='checkbox'><label for='data-c8f3966b-2407-4370-8916-be37d0f6c843' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[1 values with dtype=&lt;U11]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DOXY_ADJUSTED_BGCArgoPlus</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-50620dbd-cbd3-4725-892c-3bbd05ac00a0' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-50620dbd-cbd3-4725-892c-3bbd05ac00a0' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-fd7df046-43df-4c63-b0b5-7aeb348ae7a5' class='xr-var-data-in' type='checkbox'><label for='data-fd7df046-43df-4c63-b0b5-7aeb348ae7a5' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Dissolved oxygen</dd><dt><span>standard_name :</span></dt><dd>moles_of_oxygen_per_unit_mass_in_sea_water</dd><dt><span>units :</span></dt><dd>micromole/kg</dd><dt><span>valid_min :</span></dt><dd>-5.0</dd><dt><span>valid_max :</span></dt><dd>600.0</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DOXY_ADJUSTED_BGCArgoPlus_flag</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>&lt;U14</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-bdfe70f7-db29-4cb6-a51d-5562c93ba838' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-bdfe70f7-db29-4cb6-a51d-5562c93ba838' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-dd49a290-7ae7-49d3-85cf-4a44205ab6af' class='xr-var-data-in' type='checkbox'><label for='data-dd49a290-7ae7-49d3-85cf-4a44205ab6af' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[1 values with dtype=&lt;U14]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>NITRATE_ADJUSTED_BGCArgoPlus</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-61de2556-2138-4e75-a76a-c7b4fac2825a' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-61de2556-2138-4e75-a76a-c7b4fac2825a' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-15d19c65-2e45-4cf5-ad27-8c8d64b54846' class='xr-var-data-in' type='checkbox'><label for='data-15d19c65-2e45-4cf5-ad27-8c8d64b54846' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Nitrate</dd><dt><span>standard_name :</span></dt><dd>moles_of_nitrate_per_unit_mass_in_sea_water</dd><dt><span>units :</span></dt><dd>micromole/kg</dd><dt><span>C_format :</span></dt><dd>%.3f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.3</dd><dt><span>resolution :</span></dt><dd>0.001</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float32]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>NITRATE_ADJUSTED_BGCArgoPlus_flag</span></div><div class='xr-var-dims'>()</div><div class='xr-var-dtype'>&lt;U14</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-eecbbde4-43a0-4042-ae7e-01aa77a2d6ff' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-eecbbde4-43a0-4042-ae7e-01aa77a2d6ff' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-aff778ca-c297-4d4f-bc5f-3754e8ca2b3a' class='xr-var-data-in' type='checkbox'><label for='data-aff778ca-c297-4d4f-bc5f-3754e8ca2b3a' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[1 values with dtype=&lt;U14]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>Sigma_theta_gsw</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-53104aaf-dd6c-4661-9154-d7551b9f45a8' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-53104aaf-dd6c-4661-9154-d7551b9f45a8' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-ccb43e2a-8c9b-4e9b-b890-efff8a67b6a3' class='xr-var-data-in' type='checkbox'><label for='data-ccb43e2a-8c9b-4e9b-b890-efff8a67b6a3' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float64]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>sigma0</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-531aa640-7e2e-4a29-b7bf-78483f159b19' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-531aa640-7e2e-4a29-b7bf-78483f159b19' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-3ada77f1-f356-4a0a-8d25-30b24a164cda' class='xr-var-data-in' type='checkbox'><label for='data-3ada77f1-f356-4a0a-8d25-30b24a164cda' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float64]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>spiciness0</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-33f8397e-aae1-4fcd-b097-225a7293ea3d' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-33f8397e-aae1-4fcd-b097-225a7293ea3d' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-f2895f00-fd35-452b-a927-7a3e3e9edac5' class='xr-var-data-in' type='checkbox'><label for='data-f2895f00-fd35-452b-a927-7a3e3e9edac5' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float64]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>cons_temp</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-7369089c-63ce-416e-8b0c-ae88a1da955c' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-7369089c-63ce-416e-8b0c-ae88a1da955c' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-fe140483-652c-4820-9603-f5e0cd414a65' class='xr-var-data-in' type='checkbox'><label for='data-fe140483-652c-4820-9603-f5e0cd414a65' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float64]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>gamma</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-00f39130-0445-4d4b-9b79-d956a0419db0' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-00f39130-0445-4d4b-9b79-d956a0419db0' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-f5b00424-5910-4231-9506-85649a23e87c' class='xr-var-data-in' type='checkbox'><label for='data-f5b00424-5910-4231-9506-85649a23e87c' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float64]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>depth</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-eedc3937-c347-4114-baa5-6f1a94fe9b57' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-eedc3937-c347-4114-baa5-6f1a94fe9b57' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-bff6f277-3015-411c-9ad6-130fea237460' class='xr-var-data-in' type='checkbox'><label for='data-bff6f277-3015-411c-9ad6-130fea237460' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float64]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>MLD</span></div><div class='xr-var-dims'>(N_PROF)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-44024e5d-8e73-4e06-a535-303486b069d3' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-44024e5d-8e73-4e06-a535-303486b069d3' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-98486efa-d021-43dc-8440-172f87132d72' class='xr-var-data-in' type='checkbox'><label for='data-98486efa-d021-43dc-8440-172f87132d72' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>[216 values with dtype=float64]</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>DOXY_SAT</span></div><div class='xr-var-dims'>(N_PROF, N_LEVELS)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>...</div><input id='attrs-6477b6e1-107e-48c9-8fe0-a634ca87d40e' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-6477b6e1-107e-48c9-8fe0-a634ca87d40e' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-144fba45-477d-4095-a948-da834af986e7' class='xr-var-data-in' type='checkbox'><label for='data-144fba45-477d-4095-a948-da834af986e7' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Practical salinity</dd><dt><span>standard_name :</span></dt><dd>sea_water_salinity</dd><dt><span>units :</span></dt><dd>psu</dd><dt><span>valid_min :</span></dt><dd>2.0</dd><dt><span>valid_max :</span></dt><dd>41.0</dd><dt><span>C_format :</span></dt><dd>%.4f</dd><dt><span>FORTRAN_format :</span></dt><dd>F.4</dd><dt><span>resolution :</span></dt><dd>1e-04</dd></dl></div><div class='xr-var-data'><pre>[118368 values with dtype=float64]</pre></div></li></ul></div></li><li class='xr-section-item'><input id='section-37a03a7a-41b4-4135-8ffc-a75397613834' class='xr-section-summary-in' type='checkbox'  ><label for='section-37a03a7a-41b4-4135-8ffc-a75397613834' class='xr-section-summary' >Indexes: <span>(2)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><ul class='xr-var-list'><li class='xr-var-item'><div class='xr-index-name'><div>N_LEVELS</div></div><div class='xr-index-preview'>PandasIndex</div><input type='checkbox' disabled/><label></label><input id='index-56e1dee7-c900-4762-b5cb-c13c49e16ee1' class='xr-index-data-in' type='checkbox'/><label for='index-56e1dee7-c900-4762-b5cb-c13c49e16ee1' title='Show/Hide index repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-index-data'><pre>PandasIndex(Index([  0,   1,   2,   3,   4,   5,   6,   7,   8,   9,
       ...
       538, 539, 540, 541, 542, 543, 544, 545, 546, 547],
      dtype=&#x27;int64&#x27;, name=&#x27;N_LEVELS&#x27;, length=548))</pre></div></li><li class='xr-var-item'><div class='xr-index-name'><div>N_PROF</div></div><div class='xr-index-preview'>PandasIndex</div><input type='checkbox' disabled/><label></label><input id='index-7ed32bdf-d816-4ced-89b7-d4651a6353a5' class='xr-index-data-in' type='checkbox'/><label for='index-7ed32bdf-d816-4ced-89b7-d4651a6353a5' title='Show/Hide index repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-index-data'><pre>PandasIndex(Index([  0,   1,   2,   3,   4,   5,   6,   7,   8,   9,
       ...
       206, 207, 208, 209, 210, 211, 212, 213, 214, 215],
      dtype=&#x27;int64&#x27;, name=&#x27;N_PROF&#x27;, length=216))</pre></div></li></ul></div></li><li class='xr-section-item'><input id='section-32d7e100-a58f-4153-b337-c106e4318cae' class='xr-section-summary-in' type='checkbox'  ><label for='section-32d7e100-a58f-4153-b337-c106e4318cae' class='xr-section-summary' >Attributes: <span>(10)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><dl class='xr-attrs'><dt><span>title :</span></dt><dd>Argo float vertical profile</dd><dt><span>institution :</span></dt><dd>AOML</dd><dt><span>source :</span></dt><dd>Argo float</dd><dt><span>history :</span></dt><dd>2024-12-18T06:05:20Z creation (software version 1.18 (version 11.01.2024 for ARGO_simplified_profile))</dd><dt><span>references :</span></dt><dd>http://www.argodatamgt.org/Documentation</dd><dt><span>user_manual_version :</span></dt><dd>1.0</dd><dt><span>Conventions :</span></dt><dd>Argo-3.1 CF-1.6</dd><dt><span>featureType :</span></dt><dd>trajectoryProfile</dd><dt><span>software_version :</span></dt><dd>1.18 (version 11.01.2024 for ARGO_simplified_profile)</dd><dt><span>id :</span></dt><dd>https://doi.org/10.17882/42182</dd></dl></div></li></ul></div></div>



# 3. Take a first look at the data
---
To do this we will use the Cartopy package, which has handy geographic plotting functions. A few things to keep in mind when dealing with spatial plots:

- Longitude conventions (-180/180 or 0/360). Both are commonly used, but some packages accept only one.

- Projections: The projection is how a set of coordinates is displayed on a map - basically how do you display data from an oblate spheroid (the Earth) onto a flat screen. There are many projections out there, right now I'm a fan of Interrupted Goode Homolosime projectsion (see below). 

- Data Projection vs. Map Projection. Cartopy needs you to pass a simple lat/lon projection type (PlateCarree shown below) for your data, while your map Projection can be anything you choose. I mention this because if you use the map projection for both your data "transform" and your map projection, your data will not show up in the correct place. I've done this enough that I assume others will make the same mistake.


```python
# First let's make a map of the figure track so we know where it is: 

# For plotting it will be easier if we create a time variable in decimal years
argo_n['decimal_year'] = (['N_PROF'],np.empty(argo_n.PRES_ADJUSTED.shape[0])) # add an empty variable 
argo_n.decimal_year[:] = np.nan # set all values to nan
date_time = pd.to_datetime(argo_n.JULD.values) # get out the time, converted to a Pandas datetime
year = date_time.year # extract the year
decimal_year = year + (date_time.day_of_year - 1) / 365.25 # extract the day of year, convert to decimal year and add the year
argo_n.decimal_year[:] = decimal_year # save into the array. This step isn't really necessary here, but keeps things organized and you could save it out for later use. I should probably just add decimal year to the files..

data_proj = ccrs.PlateCarree(central_longitude=0)
median_lon = np.nanmedian(argo_n['LONGITUDE'].values)
median_lat = np.nanmedian(argo_n['LATITUDE'].values)
map_proj =ccrs.InterruptedGoodeHomolosine(central_longitude=median_lon, globe=None, emphasis='ocean')

fig = plt.figure(figsize=(7,5))

ax0 = fig.add_subplot(1,1,1, projection=map_proj)
ax0.set_global()
ax0.add_feature(cfeature.NaturalEarthFeature('physical', 'land', '110m', edgecolor='k', facecolor=[.5, .5 ,.5]))

map = ax0.scatter(argo_n.LONGITUDE.values, argo_n.LATITUDE.values, s=10, c=argo_n.decimal_year, transform=data_proj, cmap='cool')
lat_lon_padding = 20
ax0.set_extent([median_lon-lat_lon_padding, median_lon+lat_lon_padding, median_lat-lat_lon_padding, median_lat+lat_lon_padding], crs=data_proj)
# # add lat/lon gridlines and labels
gl = ax0.gridlines(draw_labels=True, dms=True, x_inline=False, y_inline=False)
gl.top_labels = False
gl.right_labels = False
plt.colorbar(map, label='Decimal Year', orientation='horizontal')
plt.title('Float ' + str(argo_n['WMO_ID'].values) + ' Track')

```




    Text(0.5, 1.0, 'Float 5902128 Track')




    
![png](NCP_nitrate_drawdown_files/NCP_nitrate_drawdown_7_1.png)
    


## Section plots
---


```python
# While we should have the variables we want, since we picked our float file, let's double check that everything is there and has some valid data:

var_suffix = '_ADJUSTED_BGCArgoPlus'
pres_name = 'PRES' + var_suffix
vars_to_plot_all = ['TEMP' + var_suffix, 'NITRATE' + var_suffix, 'DOXY' + var_suffix, 'CHLA' + var_suffix, 'BBP700' + var_suffix]
vars_to_plot = []
# check that these variables exist in the float file and have valid data:
for var in vars_to_plot_all:
    if var not in argo_n.variables:
        print(f"Variable {var} not found in dataset.")
    elif np.all(np.isnan(argo_n[var].values)):
        print(f"Variable {var} contains only NaN values.")
    else:
        vars_to_plot.append(var)
    
print("Variables to plot:", vars_to_plot)
```

    Variable CHLA_ADJUSTED_BGCArgoPlus not found in dataset.
    Variable BBP700_ADJUSTED_BGCArgoPlus not found in dataset.
    Variables to plot: ['TEMP_ADJUSTED_BGCArgoPlus', 'NITRATE_ADJUSTED_BGCArgoPlus', 'DOXY_ADJUSTED_BGCArgoPlus']



```python
max_depth = 250 # we're interested in the upper ocean, so let's limit our figures to that depth
# We'll also overlay the MLD since that will be important for our analysis
color_map = 'viridis'
num_rows = int(np.ceil((len(vars_to_plot))/2))
fig = plt.figure(figsize=(15, 6*num_rows))

plot_num = 0

for var_name in vars_to_plot:
    plot_num += 1
    ax2 = fig.add_subplot(num_rows,2,plot_num)

    if ~np.all(np.isnan(argo_n[var_name])):
        var_min = np.nanmin(argo_n[var_name].where(argo_n[pres_name]<=max_depth))
        var_max = np.nanmax(argo_n[var_name].where(argo_n[pres_name]<=max_depth))    

    for p in range(0, len(argo_n.N_PROF)):
        # p_p = argo_n[pres_name][p, ~np.isnan(argo_n[pres_name][p,:])].values
        t_p = np.array([argo_n.decimal_year[p].item(), argo_n.decimal_year[p].values + np.nanmedian(np.diff(argo_n.decimal_year))]) # try padding with median difference between profile times instead of next profile in case of large gaps

        # t_p = argo_n.decimal_year[p:p+2].values
        if t_p.size==1:
            t_p = np.tile(t_p, (2,1))
            t_p[1] = t_p[1] + (argo_n.decimal_year[p] - argo_n.decimal_year[p-1]).values
        elif np.isnan(t_p).any(): # if any values in t_p are nans
            if np.isnan(t_p).all(): # if all are nans, continue
                continue
            elif np.isnan(t_p[0]):
                t_p[0] = t_p[1] - 10/365
            else:
                t_p[1] = t_p[0] + 10/365
        # xl,yl = np.meshgrid(t_p, p_p)

        # extracting the variable that we want to plot
        # c = argo_n[var_name][p,~np.isnan(argo_n[pres_name][p,:])].values

        # extracting the variable, this time removing any vertical gaps
        c2= argo_n[var_name][p,np.logical_and(~np.isnan(argo_n[var_name][p,:]), ~np.isnan(argo_n[pres_name][p,:]))].values
        
        # if c.size==0:
        #     continue
        # else:
        #     c = np.tile(c, (2,1))
        #     c = c.T
        #     pc = ax.pcolormesh(xl, yl, c[0:-1,0:-1], cmap=color_map, shading='flat', vmin=var_min, vmax=var_max)
       
        # we'll need a different shape x/y array to match the size of c2 (it is likely smaller in the vertical direction due to removed gaps)
        if c2.size==0:
            continue
        else:
            p_p2 = argo_n[pres_name][p,np.logical_and(~np.isnan(argo_n[var_name][p,:]), ~np.isnan(argo_n[pres_name][p,:]))].values
            xl2, yl2 = np.meshgrid(t_p, p_p2)

            c2 = np.tile(c2, (2,1))
            c2 = c2.T

            pc2 = ax2.pcolormesh(xl2, yl2, c2[0:-1,0:-1], cmap=color_map, shading='flat', vmin=var_min, vmax=var_max)

      
    ax2.plot(argo_n.decimal_year, argo_n['MLD'].values, 'k-', linewidth=2)
    # cl1 = plt.colorbar(pc, ax=ax)
    # ax.set_ylim(argo_n[pres_name].max().values, 0)
    # ax.set_xlim(argo_n['decimal_year'][0], argo_n['decimal_year'][-1])
    # ax.set_title(argo_n[var_name].long_name + '\nProcessing: ' + argo_n[var_name + '_flag'].values.item())
    # cl1.set_label(f'{argo_n[var_name].standard_name} \n({argo_n[var_name].units})')

    cl2 = plt.colorbar(pc2, ax=ax2)
    ax2.set_ylim(max_depth, 0)
    ax2.set_xlim(argo_n['decimal_year'][0], argo_n['decimal_year'][-1])
    ax2.set_title(argo_n[var_name].long_name + '\nProcessing: ' + argo_n[var_name + '_flag'].values.item())
    cl2.set_label(f'{argo_n[var_name].units}')

fig.suptitle(f"Float {argo_n['WMO_ID'].values}")
# fig.savefig(figure_dir + f"Float_{argo_n['WMO_ID'].values}_BGCArgoPlus_Section_Plots_{plot_ver}.png", dpi=300)

# print(f"Figure saved to {figure_dir}, with the filename: Float_{argo_n['WMO_ID'].values}_BGCArgoPlus_Section_Plots_{plot_ver}.png")
```




    Text(0.5, 0.98, 'Float 5902128')




    
![png](NCP_nitrate_drawdown_files/NCP_nitrate_drawdown_10_1.png)
    


This float looks promising, let's pick just one "float-year" and look more closely at the seasonal cycle.

I'm going to copy the cell above and zoom in on a specific time range. While I'm at it I'm going to manually specify color ranges so that the upper ocean features stand out in all variables. **You should modify your time range, max depth, and variable ranges to better show your own float's data.**


```python
min_time = 2009 
max_time = 2010 # plotting a bit over 1 year so that I have a better reference for what is going on

var_min_max = {'TEMP' + var_suffix: (5,14),
               'NITRATE' + var_suffix: (10, 25),
               'DOXY' + var_suffix: (250, 350),
               'CHLA' + var_suffix: (0, 0.3),
               'BBP700' + var_suffix: (0, 0.001)}

max_depth = 250 # we're interested in the upper ocean, so let's limit our figures to that depth
# We'll also overlay the MLD since that will be important for our analysis

num_rows = int(np.ceil((len(vars_to_plot))/2))
fig = plt.figure(figsize=(15, 6*num_rows))

plot_num = 0

for var_name in vars_to_plot:
    plot_num += 1
    ax2 = fig.add_subplot(num_rows,2,plot_num)

    if ~np.all(np.isnan(argo_n[var_name])):
        var_min = var_min_max[var_name][0]
        var_max = var_min_max[var_name][1]

    for p in range(0, len(argo_n.N_PROF)):
        # p_p = argo_n[pres_name][p, ~np.isnan(argo_n[pres_name][p,:])].values
        t_p = np.array([argo_n.decimal_year[p].item(), argo_n.decimal_year[p].values + np.nanmedian(np.diff(argo_n.decimal_year))]) # try padding with median difference between profile times instead of next profile in case of large gaps

        # t_p = argo_n.decimal_year[p:p+2].values
        if t_p.size==1:
            t_p = np.tile(t_p, (2,1))
            t_p[1] = t_p[1] + (argo_n.decimal_year[p] - argo_n.decimal_year[p-1]).values
        elif np.isnan(t_p).any(): # if any values in t_p are nans
            if np.isnan(t_p).all(): # if all are nans, continue
                continue
            elif np.isnan(t_p[0]):
                t_p[0] = t_p[1] - 10/365
            else:
                t_p[1] = t_p[0] + 10/365
        # xl,yl = np.meshgrid(t_p, p_p)

        # extracting the variable that we want to plot
        # c = argo_n[var_name][p,~np.isnan(argo_n[pres_name][p,:])].values

        # extracting the variable, this time removing any vertical gaps
        c2= argo_n[var_name][p,np.logical_and(~np.isnan(argo_n[var_name][p,:]), ~np.isnan(argo_n[pres_name][p,:]))].values
        
        # if c.size==0:
        #     continue
        # else:
        #     c = np.tile(c, (2,1))
        #     c = c.T
        #     pc = ax.pcolormesh(xl, yl, c[0:-1,0:-1], cmap=color_map, shading='flat', vmin=var_min, vmax=var_max)
       
        # we'll need a different shape x/y array to match the size of c2 (it is likely smaller in the vertical direction due to removed gaps)
        if c2.size==0:
            continue
        else:
            p_p2 = argo_n[pres_name][p,np.logical_and(~np.isnan(argo_n[var_name][p,:]), ~np.isnan(argo_n[pres_name][p,:]))].values
            xl2, yl2 = np.meshgrid(t_p, p_p2)

            c2 = np.tile(c2, (2,1))
            c2 = c2.T

            pc2 = ax2.pcolormesh(xl2, yl2, c2[0:-1,0:-1], cmap=color_map, shading='flat', vmin=var_min, vmax=var_max)

      
    ax2.plot(argo_n.decimal_year, argo_n['MLD'].values, 'k-', linewidth=2)
    # cl1 = plt.colorbar(pc, ax=ax)
    # ax.set_ylim(argo_n[pres_name].max().values, 0)
    # ax.set_xlim(argo_n['decimal_year'][0], argo_n['decimal_year'][-1])
    # ax.set_title(argo_n[var_name].long_name + '\nProcessing: ' + argo_n[var_name + '_flag'].values.item())
    # cl1.set_label(f'{argo_n[var_name].standard_name} \n({argo_n[var_name].units})')

    cl2 = plt.colorbar(pc2, ax=ax2)
    ax2.set_ylim(max_depth, 0)
    ax2.set_xlim(min_time, max_time)
    ax2.set_title(argo_n[var_name].long_name + '\nProcessing: ' + argo_n[var_name + '_flag'].values.item())
    cl2.set_label(f'{argo_n[var_name].units}')

fig.suptitle(f"Float {argo_n['WMO_ID'].values}")
# fig.savefig(figure_dir + f"Float_{argo_n['WMO_ID'].values}_BGCArgoPlus_Section_Plots_{plot_ver}.png", dpi=300)

# print(f"Figure saved to {figure_dir}, with the filename: Float_{argo_n['WMO_ID'].values}_BGCArgoPlus_Section_Plots_{plot_ver}.png")
```




    Text(0.5, 0.98, 'Float 5902128')




    
![png](NCP_nitrate_drawdown_files/NCP_nitrate_drawdown_12_1.png)
    


# Signals of production

What do we expect to see if there is production? Where should we see changes in nitrate, oxygen, CHLA or Backscatter?

What are the important fluxes to expect for nitrate and oxygen? What would we need to know to calculate production from these data?

Let's add some boxes onto these figures to help us think through the changes we would expect over time. We'll choose three time points: t0, t1, and t2, which should correspond to when the mixed layer first shoals, when it first starts to deepen, and when it is at its maximum depth


```python
# You'll probably have to adjust these time periods iteratively until you get something that looks good for your float data
t0 = 2009 +2/12
t1 = 2009 + 5/12
t2 = 2009 + 9/12

# we'll set our box depths according the the MLDs during those time periods and a deeper depth for the box below the maximum winter MLD
MLD1 =  argo_n['MLD'].where((argo_n.decimal_year>=t0) & (argo_n.decimal_year<t1) ).max().values # argo_n['MLD'].where((argo_n.decimal_year>=t0) & (argo_n.decimal_year<t1) ).mean().values
MLD2 = argo_n['MLD'].where((argo_n.decimal_year>=t1) & (argo_n.decimal_year<t2) ).mean().values
max_box_depth = 250
max_depth = 300 # we'll extend the depth of our plot so that we can see the maximum box depth

vars_to_plot = ['TEMP' + var_suffix, 'NITRATE' + var_suffix, 'DOXY' + var_suffix] # I'm going to focus on T, N, and O2 for now
min_time = t0-2/12 #2019 + 6/12
max_time = t2 + 5/12 # 2021 # plotting a bit over 1 year so that I have a better reference for what is going on

# var_min_max = {'TEMP' + var_suffix: (17,25),
#                'NITRATE' + var_suffix: (1, 4),
#                'DOXY' + var_suffix: (200, 235),
#                'CHLA' + var_suffix: (0, 0.3),
#                'BBP700' + var_suffix: (0, 0.001)}

# We'll also overlay the MLD since that will be important for our analysis

num_rows = int(np.ceil((len(vars_to_plot))/2))
fig = plt.figure(figsize=(15, 6*num_rows))

plot_num = 0



for var_name in vars_to_plot:
    plot_num += 1
    ax2 = fig.add_subplot(num_rows,2,plot_num)

    if ~np.all(np.isnan(argo_n[var_name])):
        var_min = var_min_max[var_name][0]
        var_max = var_min_max[var_name][1]

    for p in range(0, len(argo_n.N_PROF)):
        # p_p = argo_n[pres_name][p, ~np.isnan(argo_n[pres_name][p,:])].values
        t_p = np.array([argo_n.decimal_year[p].item(), argo_n.decimal_year[p].values + np.nanmedian(np.diff(argo_n.decimal_year))]) # try padding with median difference between profile times instead of next profile in case of large gaps

        # t_p = argo_n.decimal_year[p:p+2].values
        if t_p.size==1:
            t_p = np.tile(t_p, (2,1))
            t_p[1] = t_p[1] + (argo_n.decimal_year[p] - argo_n.decimal_year[p-1]).values
        elif np.isnan(t_p).any(): # if any values in t_p are nans
            if np.isnan(t_p).all(): # if all are nans, continue
                continue
            elif np.isnan(t_p[0]):
                t_p[0] = t_p[1] - 10/365
            else:
                t_p[1] = t_p[0] + 10/365
        # xl,yl = np.meshgrid(t_p, p_p)

        # extracting the variable that we want to plot
        # c = argo_n[var_name][p,~np.isnan(argo_n[pres_name][p,:])].values

        # extracting the variable, this time removing any vertical gaps
        c2= argo_n[var_name][p,np.logical_and(~np.isnan(argo_n[var_name][p,:]), ~np.isnan(argo_n[pres_name][p,:]))].values
        
        # if c.size==0:
        #     continue
        # else:
        #     c = np.tile(c, (2,1))
        #     c = c.T
        #     pc = ax.pcolormesh(xl, yl, c[0:-1,0:-1], cmap=color_map, shading='flat', vmin=var_min, vmax=var_max)
       
        # we'll need a different shape x/y array to match the size of c2 (it is likely smaller in the vertical direction due to removed gaps)
        if c2.size==0:
            continue
        else:
            p_p2 = argo_n[pres_name][p,np.logical_and(~np.isnan(argo_n[var_name][p,:]), ~np.isnan(argo_n[pres_name][p,:]))].values
            xl2, yl2 = np.meshgrid(t_p, p_p2)

            c2 = np.tile(c2, (2,1))
            c2 = c2.T

            pc2 = ax2.pcolormesh(xl2, yl2, c2[0:-1,0:-1], cmap=color_map, shading='flat', vmin=var_min, vmax=var_max)

      
    ax2.plot(argo_n.decimal_year, argo_n['MLD'].values, 'k-', linewidth=2)

    # t1 boxes
    ax2.plot([t0, t1, t1, t0, t0], [MLD1, MLD1, 0 , 0, MLD1], '-', color='cyan', linewidth=2)
    ax2.plot([t0, t1, t1, t0, t0], [MLD2, MLD2, MLD1, MLD1, MLD2], '-', color='cyan', linewidth=2)
    ax2.plot([t0, t1, t1, t0, t0], [MLD2, MLD2, max_box_depth , max_box_depth, MLD2], '-', color='cyan', linewidth=2)

     # t2 boxes
    ax2.plot([t1, t2, t2, t1, t1], [MLD1, MLD1, 0 , 0, MLD1], '-', color='cyan', linewidth=2)
    ax2.plot([t1, t2, t2, t1, t1], [MLD2, MLD2, MLD1, MLD1, MLD2], '-', color='cyan', linewidth=2)
    ax2.plot([t1, t2, t2, t1, t1], [MLD2, MLD2, max_box_depth , max_box_depth, MLD2], '-', color='cyan', linewidth=2)

    # label the boxes 
    ax2.text((t0 + t1)/2, MLD2/2, 'Box 1,1', color='cyan', fontsize=12, fontweight='bold', ha='center')
    ax2.text((t1 + t2)/2, MLD2/2, 'Box 1,2', color='cyan', fontsize=12, fontweight='bold', ha='center')
    ax2.text((t0 + t1)/2, (MLD2-MLD1)/2+MLD1, 'Box 2,1', color='cyan', fontsize=12, fontweight='bold', ha='center')
    ax2.text((t1 + t2)/2, (MLD2-MLD1)/2+MLD1, 'Box 2,2', color='cyan', fontsize=12, fontweight='bold', ha='center')

    ax2.text((t0 + t1)/2, (max_box_depth-MLD2)/2+MLD2, 'Box 3,1', color='cyan', fontsize=12, fontweight='bold', ha='center')
    ax2.text((t1 + t2)/2, (max_box_depth-MLD2)/2+MLD2, 'Box 3,2', color='cyan', fontsize=12, fontweight='bold', ha='center')


    cl2 = plt.colorbar(pc2, ax=ax2)
    ax2.set_ylim(max_depth, 0)
    ax2.set_xlim(min_time, max_time)
    ax2.set_title(argo_n[var_name].long_name + '\nProcessing: ' + argo_n[var_name + '_flag'].values.item())
    cl2.set_label(f'{argo_n[var_name].units}')
    
fig.suptitle(f"Float {argo_n['WMO_ID'].values}")
# fig.savefig(figure_dir + f"Float_{argo_n['WMO_ID'].values}_BGCArgoPlus_Section_Plots_{plot_ver}.png", dpi=300)

# print(f"Figure saved to {figure_dir}, with the filename: Float_{argo_n['WMO_ID'].values}_BGCArgoPlus_Section_Plots_{plot_ver}.png")
```




    Text(0.5, 0.98, 'Float 5902128')




    
![png](NCP_nitrate_drawdown_files/NCP_nitrate_drawdown_14_1.png)
    


1. Write out the mass balance equations for these 3 boxes. What sign / 1st order changes in each tracer do you expect to see in each box?
2. Calculate the back of the envelope NCP in each box from nitrate and oxygen
    - What is different between the two? What would we need to account for with oxygen that we don't need to for nitrate? How would you expect that to change your nitrate calculation? 
3. Compare your answers to the groups with the other floats and to Plant et al. 2016 - how do they differ? 
4. Do you see any discernible signal in your deepest box? If you have time, try to add a diapycnal diffusivity flux and see if that helps


```python
# Finding nitrate concentrations in the different boxes
t0_1 = argo_n['NITRATE_ADJUSTED_BGCArgoPlus'].where((argo_n['decimal_year']>=t0) &  (argo_n['decimal_year']<t1))
t1_2 = argo_n['NITRATE_ADJUSTED_BGCArgoPlus'].where((argo_n['decimal_year']>=t1) &  (argo_n['decimal_year']<t2))

box_11_mask = t0_1['PRES_ADJUSTED_BGCArgoPlus']<MLD2
box_12_mask = (t0_1['PRES_ADJUSTED_BGCArgoPlus']>=MLD2) & (t0_1['PRES_ADJUSTED_BGCArgoPlus']<MLD1)
box_13_mask = (t0_1['PRES_ADJUSTED_BGCArgoPlus']>=MLD1) & (t0_1['PRES_ADJUSTED_BGCArgoPlus']<max_box_depth)

box_21_mask = t1_2['PRES_ADJUSTED_BGCArgoPlus']<MLD2
box_22_mask = (t1_2['PRES_ADJUSTED_BGCArgoPlus']>=MLD2) & (t1_2['PRES_ADJUSTED_BGCArgoPlus']<MLD1)
box_23_mask = (t1_2['PRES_ADJUSTED_BGCArgoPlus']>=MLD1) & (t1_2['PRES_ADJUSTED_BGCArgoPlus']<max_box_depth)


box_11_no3 = t0_1.where(box_11_mask).mean().values
box_12_no3 = t0_1.where(box_12_mask).mean().values
box_13_no3 = t0_1.where(box_13_mask).mean().values
print(f'box 11 no3: {box_11_no3:.2f}, box 12 no3: {box_12_no3:.2f}, box 13 no3: {box_13_no3:.2f}')

box_21_no3 = t1_2.where(box_21_mask).mean().values
box_22_no3 = t1_2.where(box_22_mask).mean().values
box_23_no3 = t1_2.where(box_23_mask).mean().values
print(f'box 21 no3: {box_21_no3:.2f}, box 22 no3: {box_22_no3:.2f}, box 23 no3: {box_23_no3:.2f}')
print(f'MLDs: {MLD2}, {MLD1}')
```

    box 11 no3: 12.51, box 12 no3: 13.32, box 13 no3: 28.31
    box 21 no3: 9.01, box 22 no3: 12.93, box 23 no3: 27.65
    MLDs: 24.675, 107.44



```python
# calculate NCP from the nitrate changes between box 1 and box 2
d_yr = (t1+t2)/2 - (t0+t1)/2 
d_t_days = d_yr*365.25

box_1_ncp = (box_11_no3 - box_21_no3)*1025*1e-6*MLD2 / d_t_days * 106/16 # umol / kg * kg/m3 * mol/umol * m / days * 106C / 16N
box_1_ncp_mmol_m2_d = box_1_ncp*1000
print(f'NCP in mmol C m-2 d-1: {box_1_ncp_mmol_m2_d:.2f}' )
```


```python
# comparison of group calculations to Plant et al. 

f5904125_2015 = [4.3*365/1000, 1.3]
f5902128_2009 = [5.5*365/1000, 1.7]
f5902128_2010 = [1.8, 0.8]

plt.plot(f5904125_2015[1], f5904125_2015[0], 'x', label='5904125_2015')
plt.plot(f5902128_2009[1], f5902128_2009[0], 'x', label='5902128_2009')
plt.plot(f5902128_2010[1], f5902128_2010[0], 'x', label='5902128_2010')
plt.grid('on')
plt.plot([0, 5], [0,5], '--k')
plt.legend()
plt.xlabel('Plant 2016, mol C /m2 /yr')
plt.ylabel('Our estimates, mol C /m2 /yr')

```




    Text(0, 0.5, 'Our estimates, mol C /m2 /yr')




    
![png](NCP_nitrate_drawdown_files/NCP_nitrate_drawdown_18_1.png)
    

