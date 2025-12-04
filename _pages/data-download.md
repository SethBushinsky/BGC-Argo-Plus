---
layout: single
title: Data Download
toc: True
toc_label: "Main Sections"
toc_icon: "cog"
permalink: /data-download/
title: "Data Download"
author_profile: false
breadcrumbs: true
---

**This is a preliminary dataset that is undergoing changes and is not currently meant for public use**

## Float data files - version 0.1_2025_12
**Grouped by region or individually. One file per float, similar format as GDAC files (details below)**

Region | Link | File Size | Date Updated
--- | --- | --- | --- 
Global dataset| [BGC_Argo_Plus_Global](https://ftp.soest.hawaii.edu/bgc_argo_plus/BGC_Argo_Plus_Global_v0_2025_09.tar.gz) | 15.0 GB | 2025-05-13
Individual Float Files | [FTP Directory](https://ftp.soest.hawaii.edu/bgc_argo_plus/Individual_Floats/outliers_removed/v0.1_2025_12/) | 2,228 files, 29.9 GB total | 2025-05

## Gridded data 
**(X x Y x Z grid, not mapped or interpolated)**

Region | Link | File Size | Date Updated
--- | --- | --- | --- 
N/A | N/A | N/A | N/A

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

## Version notes
v0.1_2025_12 - Changed to "BGCArgoPlus" variable suffixes. 
v0.0 - Preliminary upload. 