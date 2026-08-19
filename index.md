---
layout: splash
title:  
permalink: /
hidden: true
header:
  overlay_color: "#ffffff"
  overlay_image: /assets/images/Map_all_bgc_floats_no_legend.png
  overlay_filter: rgba(0, 0, 0, 0.6)
  caption: "BGC float locations globally"
  actions:
    - label: "Data download"
      url: "data-download/"
excerpt: A dataset containing all available biogeochemical Argo data with a secondary quality control applied. Data available as individual float files or in monthly 1x1 gridded options. 
  
---
**Biogeochemical Argo** floats have been deployed by dozens of nations and represent a paradigm change in how we observe ocean biogeochemistry. Major U.S. programs responsible for a significant fraction of the BGC dataset are the Southern Ocean Carbon and Climate Observations and Modeling ([SOCCOM](https://soccom.org/)) program and the Global Ocean Biogeochemistry Array ([GO-BGC](https://www.go-bgc.org/)). Both of these programs have been primarily funded by the National Science Foundation, with significant support from NOAA and NASA. 

The **BGC-Argo+ dataset** is a product of the HI-Cycles group at the University of Hawaiʻi at Mānoa, led by Seth Bushinsky. Our group began work on this product in October of 2023 to develop a global float data product that represented our combined group efforts to make the biogeochemical data as "good" and useable as possible. This effort included several main components:

- Find and remove outliers from the oxygen, nitrate, pH, temperature, or salinity data currently in the "ADJUSTED" float data
    - details about specific types of outliers removed and the approach followed can be found in Bushinsky et al. ([submitted to ESSD](/assets/images/BGC_Argo_Plus_Dataset_Manuscript-submission.pdf)). 

- Calculate common derived or ancillary parameters

- **Future work, Nachod et al. (in prep.):** Correct deep oxygen bias first noted in [Bushinsky et al. (2016)](https://doi.org/10.1002/lom3.10107) and [Drucker and Riser (2016)](https://doi.org/10.1016/j.mio.2016.09.007). Subsequently seen in [Gouretski et al. (2024)](https://essd.copernicus.org/articles/16/5503/2024/) and [Bushinsky et al. (2025)](https://doi.org/10.1029/2024GB008185). The approach for finding this bias is described in [Bushinsky et al. (2025)](https://doi.org/10.1029/2024GB008185).


As part of this effort we are discussing these issues with float PIs and researchers involved in the current QC at the DAC levels. Eventually we hope that this sort of secondary QC effort is no longer necessary. For now we believe it is important to keep looking into the data in as much detail as possible to improve its utility for all users. It is one thing to deploy new floats, but improving existing data adds measurements we can never make again. Both are necessary to move this area of oceanography forward. 

Please send an email to [seth.bushinsky at hawaii.edu](mailto:seth.bushinsky@hawaii.edu) with any errors, issues, or concerns. If you'd like to find out more about the work that the HI-Cycles group does, please take a look at our [group website](https://bushinskyoceanlab.org).

This work was originally funded by NOAA grant NA21OAR4310260 to PIs Seth Bushinsky, Nancy Williams (USF), and Andrea Fassbender (NOAA-PMEL). Subsequent support has come from Schmidt Sciences via the Ocean Biogeochemistry Virtual Institute [InMOS Project](https://inmos-obvi.github.io/). 

<figure class="half">
  <a href="/assets/images/noaa.png">
  <img src="/assets/images/noaa.png"></a>

  <a href="/assets/images/Schmidt_Sciences.png">
  <img src="/assets/images/Schmidt_Sciences.png"></a>

  <!-- <figcaption>Gallery with a two image per row grid.</figcaption> -->
</figure>

<!-- ![image-left](https://sethbushinsky.github.io/BGC-Argo-Plus/assets/images/noaa.png){: .align-left "width: 20%;"} -->

