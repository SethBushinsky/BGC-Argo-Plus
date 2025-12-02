---
layout: single
title: Code Repository
toc: True
toc_label: "Main Sections"
toc_icon: "cog"
permalink: /code-repository/
title: "Getting started with BGC Argo data"
author_profile: false
breadcrumbs: true
---
## Github repository
As part of this dataset effort we would like to make it easy for people to get started with BGC Argo data. All scripts referenced on this page can be found in our [github repository](
https://github.com/Hi-Cycles/BGC_Argo_Plus_Code_Repository)

If you are brand new to Python, take a look at the "Getting started with Python section" below for tips on getting Python initially installed and some of the basics of working with environments, scripts, git, etc.

## Lessons and activities
As I develop lessons / data activities for working with BGC Argo data, I'll post them here. 

### 1. [Setting up your BGC Argo Python environment](https://www.bgc-argo-plus.info/code-pages/bgc-argo-plus-env)
### 2. Download example example float data and make preliminary plots
### 3. Calculate seasonal NCP via Nitrate Drawdown

## Scripts and example output:

{% capture notice-2 %}
**Investigate float and Glodap observation density in a given region**:
[Float_Glodap_Obs_Density.ipynb](https://github.com/Hi-Cycles/BGC_Argo_Plus_Code_Repository/blob/main/Float_Glodap_Obs_Density.ipynb)
- Requires: 
  - some amount of float data
  - Preprocessed Glodap netcdf file: 
![image-left](https://www.bgc-argo-plus.info/assets/images/example_figures/Argo_GDAC_Sampling_Density and num_profiles_v_1.pdf){: .align-left "width: 80%;"}

{% endcapture %}

<div class="notice">{{ notice-2 | markdownify }}</div>

**Have a script that you think could be useful to the community? Send me an e-mail and we can add it to the repository or link to it here. Find a bug? Please let the author know.**

## New to Python? Here are some links and recommendations for getting started:

### Getting started with Python:
You might get better guidance googling "how to get started with python" but there are a million avenues out there, so here's one approach:

- Download anaconda or miniconda as a package manager: https://conda.io/projects/conda/en/latest/user-guide/install/index.html
- Create a virtual environment to save new packages into. Using a virtual environment avoids messing up your computer by installing a million packages into your base environment. If you screw up a virtual environment, just make a new one. I make a different one for most projects.
  - I've created a .yml file for this class that you can use to install the packages I use for the code examples shown here. You can create a new virtual environment from this .yml file by calling:
    - "conda env create -f ocn_623.yml"
- Activate your new virtual environment:
  - "conda activate ocn_623
- You can now start coding in your program of choice. Many people use jupyter notebook:
  - "jupyter notebook"
- I like VS code: https://code.visualstudio.com/

Links I've found useful:
- Geoscience computing and virtual environments: https://rabernat.github.io/research_computing_2018/python-environments.html 
- Managing environments: https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#
- Git version control: https://swcarpentry.github.io/git-novice/ and https://www.earthdatascience.org/workshops/setup-earth-analytics-python/
- Setting up github on a new machine: https://gist.github.com/qin-yu/bc26a2d280ee2e93b2d7860a1bfbd0c5 

### Matlab and R
For now I am not posting my old Matlab code and I've never used R, so I can't help much there for now. However, I will post links to useful resources as I find them and if you write scripts that use the BGC-Argo+ dataset and would like to share it with the community, let me know and we can add it to the repository or link to it here.
