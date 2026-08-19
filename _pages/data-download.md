---
layout: single
# title: Data Download
toc: True
toc_sticky: true
toc_label: "Main Sections"
toc_icon: "cog"
permalink: /data-download/
title: "Data Download"
author_profile: false
# breadcrumbs: true
---

**We have submitted the BGC-Argo+ manuscript to ESSD:**\
**- Manuscript [Preprint](https://doi.org/10.5194/essd-2026-311)**\
**- Submitted [dataset archive](https://doi.org/10.5281/zenodo.19709012)**\
**- Submitted [code archive](https://doi.org/10.5281/zenodo.19705310)**

<!-- ## Float data files - version 0.1_2026_04
**Grouped by region or individually. One file per float, similar format as GDAC files (details below)**

Region | Link | File Size | Date Updated
--- | --- | --- | --- 
Global dataset| [BGC_Argo_Plus_Global](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/BGC_Argo_Plus_Global_v0.1_2026_04.tar.gz) | 14.2 GB | 2026-04
Individual Float Files | [FTP Directory](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/Individual_Floats/) | 2,427 files, 32.8 GB total | 2026-04

***When using these data, in addition to citing this work please make sure to follow the [Argo](https://argo.ucsd.edu/data/acknowledging-argo/), [GO-BGC](https://www.go-bgc.org/data/citing-go-bgc), or [SOCCOM](https://soccom.org/about-us/acknowledgment-text/) acknowledgments as appropriate. 
## Gridded data 
**(X x Y x Z grid, not mapped or interpolated)**

Variable | Link | File Size | Date Updated
--- | --- | --- | --- 
Oxygen | [Tar.gz file](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/Gridded/BGC_Argo_monthly_gridded_DOXY_ADJUSTED_BGCArgoPlus_v0.1_2026_04.nc) | 116 MB | 2026-04
Nitrate | [Tar.gz file](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/Gridded/BGC_Argo_monthly_gridded_NITRATE_ADJUSTED_BGCArgoPlus_v0.1_2026_04.nc) | 293 MB | 2026-04
DIC | [Tar.gz file](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/Gridded/BGC_Argo_monthly_gridded_DIC_BGCArgoPlus_v0.1_2026_04.nc) | 65 MB | 2026-04 -->

# Data files - version 0.1_2026_04
**Float files are grouped by region or individually, one file per float, similar format as GDAC files (details below). Gridded files are on an X x Y x Z grid, not mapped or interpolated.**

Version notes: v0.1_2026_04 - Additional outlier removals, cleaned up data files, gridded files added, manuscript submitted

Type | Region/Variable | Link | File Size | Date Updated
--- | --- | --- | --- | ---
Tar ball of all float files  | Global | [BGC_Argo_Plus_Global](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/BGC_Argo_Plus_Global_v0.1_2026_04.tar.gz) | 14.2 GB | 2026-04
Individual float files       | Global | [FTP Directory](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/Individual_Floats/) | 2,427 files, 32.8 GB total | 2026-04
Gridded                      | Oxygen | [Tar.gz file](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/Gridded/BGC_Argo_monthly_gridded_DOXY_ADJUSTED_BGCArgoPlus_v0.1_2026_04.nc) | 116 MB | 2026-04
Gridded                      | Nitrate| [Tar.gz file](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/Gridded/BGC_Argo_monthly_gridded_NITRATE_ADJUSTED_BGCArgoPlus_v0.1_2026_04.nc) | 293 MB | 2026-04
Gridded                      | DIC    | [Tar.gz file](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/Gridded/BGC_Argo_monthly_gridded_DIC_BGCArgoPlus_v0.1_2026_04.nc) | 65 MB | 2026-04
.csv of outliers removed <sup>1</sup>      | Global | [CSV](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/merged_outlier_meta_with_latlon_V2026_04.csv) | 139 MB | 2026-04

**When using these data, in addition to citing this work please make sure to follow the [Argo](https://argo.ucsd.edu/data/acknowledging-argo/), [GO-BGC](https://www.go-bgc.org/data/citing-go-bgc), or [SOCCOM](https://soccom.org/about-us/acknowledgment-text/) acknowledgments as appropriate.** \
<sup>1</sup>Column headers: Float Number, Variable, N_PROF, Date, N_LEVELS, Pressure (dbar), Deletion reason, Variable_short, Variable_base, Project, Data_Centre, DOXY_MODEL, NITRATE_MODEL, PH_MODEL, Deployment_Date, Dep_Lat, Dep_Lon, Last_Date, Lat, Lon.

<a href="https://forms.gle/FFHy8kGoGW8W2Vit5" target="_blank" style="display: inline-block; background-color: #0073e6; color: white; padding: 10px 20px; text-decoration: none; border-radius: 5px; font-weight: bold; margin: 10px 0;">📧 Click here for emails about future updates. </a>\
*We'll only send updates about new data releases, important corrections, and publication announcements.*


## Float file details

### Variables originally included in the Sprof files that are modified:

Variable name | Type | Description 
--- | --- | ---
[VAR] | Unchanged | Raw variable copied directly from DAC 
[VAR]_ADJUSTED | Unchanged | ADJUSTED variable copied directly from DAC 
[VAR]_ADJUSTED_BGCArgoPlus | BGC-Argo+ version | Variable modified from DAC according to the processing level description 
[VAR]_ADJUSTED_BGCArgoPlus_flag | BGC-Argo+ version | A string listing each of the processing steps<sup>*</sup> that have been applied to the variable. 
 
**<sup>*</sup>Processing Level Descriptors:** \
F = data with Argo QC flag(s) 3 and 4 set to NaN\
S = surface data set to NaN \
Inv = density inversions set to NaN \
DMonly = non-delayed mode data removed
<!-- 
"RTA/DM" - Mode allowed (RTA = Real Time Adjusted and delayed mode allowed, DM = only delayed mode allowed)\
"F" - DAC QC flags applied (Bad data flagged as NaN)\
"RO" - Outlier removed\
"O2Bias" - O2 Bias correction applied (Bushinsky et al., 2025, Nachod et al., in prep)\
"Thermo" - Thermodynamic correction applied to pK1/pK2 (Johnson et al., in prep) -->



### New derived and other helpful parameters added to the BGCArgoPlus files:

Variable name | Description
--- | --- 
[SENSOR]\_model | Sensor model type retrieved from the associated meta file
TALK\_BGCArgoPlus | Total alkalinity estimated from ESPER Mixed, which is an average of ESPER neural network and ESPER multiple linear regression (Carter et al. 2021)
PCO2\_BGCArgoPlus| The partial pressure of CO<sub>2</sub> calculated from float PH\_IN\_SITU\_TOTAL\_ADJUSTED\_BGCArgoPlus and TALK\_BGCArgoPlus
DIC\_BGCArgoPlus  | Dissolved inorganic carbon (DIC) calculated from float PH\_IN\_SITU\_TOTAL\_ADJUSTED\_BGCArgoPlus and TALK\_BGCArgoPlus
PH\_25C\_TOTAL\_ADJUSTED\_BGCArgoPlus | PH recalculated at a constant 25<sup>o</sup>C using PYCO2SYS
sigma0 | Potential density with 0db reference, from 'gsw.SA\_from\_SP', 'gsw.CT\_from\_t', 'gsw.sigma0'
cons\_temp | Conservative temperature calculated using 'gsw.CT\_from\_t'
spiciness0 | Spiciness at 0db calculated using gsw.SA\_from\_SP, gsw.CT\_from\_t, gsw.spiciness0
gamma | Neutral density calculated using EOS-80 seawater properties Matlab toolbox (eos\_legacy\_gamma\_n script) via the Python Matlab engine (Jackett and McDougall, 1997)
depth | Depth calculated using 'gsw.conversions.z\_from\_p'
MLD | MLD calculated using De Boyer Montegue et al. 2004 (modified to work w/ under ice data as well)
DOXY\_SAT | The saturation concentration of oxygen using Garcia and Gordon 1992 using the gsw 'O2sol\_SP\_pt' Python package. Assumes standard atmospheric pressure
O2\_cal\_type | Type of oxygen calibration performed on the float, from the "SCIENTIFIC\_CALIB\_COMMENT" field in the Sprof file. "air\_cal" = float was calibrated in air, "not\_air" = float was not calibrated in air\
profiler\_type | Type of float, from the synthetic Argo profile index file. Table of Argo profiler types can be found at: https://vocab.nerc.ac.uk/collection/R08/current/
ocean | Ocean basin(s) where the float has drifted, from the synthetic Argo profile index file

<!-- {[SENSOR]}\_model | Sensor model type retrieved from the associated meta file
{[VARIABLE]}\_ADJUSTED\_BGCArgoPlus | The new "BGC-Argo+" version of a variable, with processing steps applied
{[VARIABLE]}\_ADJUSTED\_BGCArgoPlus\_flag | A string listing each of the processing steps that have been applied to the variable. F = data with Argo QC flag(s) 3 and 4 set to NaN, S = surface data set to NaN, Inv = density inversions set to NaN, DMonly = non-delayed mode data removed -->


## Past version notes
v0.1_2025_12 - Changed to "BGCArgoPlus" variable suffixes. \
v0.0 - Preliminary upload. 
