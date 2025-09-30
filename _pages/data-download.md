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

## Float data files - version 0.0
**Grouped by region or individually. One file per float, similar format as GDAC files (details below)**

Region | Link | File Size | Date Updated
--- | --- | --- | --- 
Global | [BGC_Argo_Plus_Global](https://ftp.soest.hawaii.edu/bgc_argo_plus/BGC_Argo_Plus_Global_v0_2025_09.tar.gz) | 15.0 GB | 2025-05-13
Southern Ocean | [BGC_Argo_Plus_SO](https://ftp.soest.hawaii.edu/bgc_argo_plus/Basins/BGC_Argo_Plus_SO.tgz) | X GB | 2025-05-13
Atlantic | [BGC_Argo_Plus_Atl](https://ftp.soest.hawaii.edu/bgc_argo_plus/Basins/BGC_Argo_Plus_Atl.tgz) | X GB | 2025-05-13
Pacific | [BGC_Argo_Plus_Pac](https://ftp.soest.hawaii.edu/bgc_argo_plus/Basins/BGC_Argo_Plus_Pac.tgz) | X GB | 2025-05-13
Indian | [BGC_Argo_Plus_Ind](https://ftp.soest.hawaii.edu/bgc_argo_plus/Basins/BGC_Argo_Plus_Ind.tgz) | X GB | 2025-05-13
All Floats | [FTP Directory](https://ftp.soest.hawaii.edu/bgc_argo_plus/Individual_Floats) | 1934 files, 29.8 GB total | 2025-05

## Gridded data 
**(X x Y x Z grid, not mapped or interpolated)**

Region | Link | File Size | Date Updated
--- | --- | --- | --- 
N/A | N/A | N/A | N/A

## Float file details
**Data variables contained in each file:**

Variable name | Type | Description
--- | --- | ---
[VAR] | Unchanged | Raw variable copied directly from DAC
[VAR]_ADJUSTED | Unchanged | ADJUSTED variable copied directly from DAC
[VAR]_BGCAP | Biogeochemical Argo+ version | Variable modified from DAC according to the processing level description
[VAR]_BGCAP_Processing_Level | Biogeochemical Argo+ version | Processing level flags

**Flag descriptors:** \
"M" - Mode filter applied (delayed mode only typically)\
"F" - DAC QC flags applied (Bad data flagged as NaN)\
"RO" - Outlier removed\
"Corr" - O2 Bias correction applied (Bushinsky et al., 2025, Nachod et al., in prep)\
"Thermo" - Thermodynamic correction applied to pK1/pK2 (Johnson et al., in prep)

