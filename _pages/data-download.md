---
layout: single
title: Data Download
toc: True
toc_sticky: true
toc_label: "Main Sections"
toc_icon: "cog"
permalink: /data-download/
title: "Data Download"
author_profile: false
breadcrumbs: true
---

**We have submitted the BGC-Argo+ manuscript to ESSD.**\
**- Submitted [dataset archive](https://doi.org/10.5281/zenodo.19709012)**\
**- Submitted [code archive](https://doi.org/10.5281/zenodo.19705310)**

## Float data files - version 0.1_2026_04
**Grouped by region or individually. One file per float, similar format as GDAC files (details below)**

Region | Link | File Size | Date Updated
--- | --- | --- | --- 
Global dataset| [BGC_Argo_Plus_Global](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/BGC_Argo_Plus_Global_v0.1_2026_04.tar.gz) | 14.2 GB | 2026-04
Individual Float Files | [FTP Directory](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/Individual_Floats/) | 2,427 files, 32.8 GB total | 2026-04

## Gridded data 
**(X x Y x Z grid, not mapped or interpolated)**

Variable | Link | File Size | Date Updated
--- | --- | --- | --- 
Oxygen | [Tar.gz file](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/Gridded/BGC_Argo_monthly_gridded_DOXY_ADJUSTED_BGCArgoPlus_v0.1_2026_04.nc) | 116 MB | 2026-04
Nitrate | [Tar.gz file](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/Gridded/BGC_Argo_monthly_gridded_NITRATE_ADJUSTED_BGCArgoPlus_v0.1_2026_04.nc) | 293 MB | 2026-04
DIC | [Tar.gz file](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/Gridded/BGC_Argo_monthly_gridded_DIC_BGCArgoPlus_v0.1_2026_04.nc) | 65 MB | 2026-04


<a href="https://forms.gle/FFHy8kGoGW8W2Vit5" target="_blank" style="display: inline-block; background-color: #0073e6; color: white; padding: 10px 20px; text-decoration: none; border-radius: 5px; font-weight: bold; margin: 10px 0;">📧 Click here for emails about future updates. </a>\
*We'll only send updates about new data releases, important corrections, and publication announcements.*


## Float file details

### Variables originally included in the Sprof files that are modified:

Variable name | Type | Description 
--- | --- | ---
[VAR] | Unchanged | Raw variable copied directly from DAC 
[VAR]_ADJUSTED | Unchanged | ADJUSTED variable copied directly from DAC 
[VAR]_BGCArgoPlus | Biogeochemical Argo+ version | Variable modified from DAC according to the processing level description 
[VAR]_BGCArgoPlus_ProcessingLevel | Biogeochemical Argo+ version | Processing level of variable 
 
**Processing Level Descriptors:** \
"RTA/DM" - Mode allowed (RTA = Real Time Adjusted and delayed mode allowed, DM = only delayed mode allowed)\
"F" - DAC QC flags applied (Bad data flagged as NaN)\
"RO" - Outlier removed\
"O2Bias" - O2 Bias correction applied (Bushinsky et al., 2025, Nachod et al., in prep)\
"Thermo" - Thermodynamic correction applied to pK1/pK2 (Johnson et al., in prep)



### Newly calculated derived variables added to the BGCArgoPlus files:

Variable name | Description
--- | --- 
TALK_BGCArgoPlus | Alkalinity estimated from ESPER Mixed, which is an average of ESPER neural network and ESPER multiple linear regression (Carter et al. 2023)
DIC_BGCArgoPlus | DIC calculated from float pH and TALK_BGCArgoPlus. O2 Bias Correction applied where possible and the Johnson et al. XXXX adjustment to pk1 and pk2 used. 
sigma0 | Potential Density calculated using gsw.SA_from_SP, gsw.CT_from_t, gsw.sigma0
cons_temp | Conservative temperature calculated using gsw.CT_from_t
spiciness0 | Spiciness calculated using using gsw.SA_from_SP, gsw.CT_from_t, gsw.spiciness0
gamma | Neutral density calculated using Matlab eos80_legacy_gamma_n from **XXXX**
depth | Depth calculated using gsw.conversions.z_from_p
MLD | MLD calculated using De Boyer Montegue et al. 2004 (modified to work w/ under ice data as well)
DOXY_SAT | The saturation concentration of oxygen using Garcia and Gordon 1992 (gsw.O2sol_SP_pt). Assumes standard atmospheric pressure. 

## Outliers removed
[CSV](https://ftp.soest.hawaii.edu/bgc_argo_plus/outliers_removed/v0.1_2026_04/merged_outlier_meta_with_latlon_V2026_04.csv) of all outliers removed. [Column headers: Float Number, Variable, N_PROF, Date, N_LEVELS, Pressure (dbar), Deletion reason, Variable_short, Variable_base, Project, Data_Centre, DOXY_MODEL, NITRATE_MODEL, PH_MODEL, Deployment_Date, Dep_Lat, Dep_Lon, Last_Date, Lat, Lon.]

## Version notes
v0.1_2026_04 - Additional outlier removals, cleaned up data files, gridded files added, manuscript submitted\
v0.1_2025_12 - Changed to "BGCArgoPlus" variable suffixes. \
v0.0 - Preliminary upload. 
