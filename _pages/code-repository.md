---
layout: single
title: "Getting started with BGC Argo data"
classes: wide
toc: true
toc_sticky: true
toc_label: "Main Sections"
toc_icon: "cog"
permalink: /code-repository/
---
## Github repository
As part of this dataset effort we would like to make it easy for people to get started with BGC Argo data. All scripts referenced on this page can be found in our [github repository](
https://github.com/Hi-Cycles/BGC_Argo_Plus_Code_Repository)

If you are brand new to Python, take a look at the "Getting started with Python section" below for tips on getting Python initially installed and some of the basics of working with environments, scripts, git, etc.

## Lessons/Activities/Example code
As I develop lessons / data activities for working with BGC Argo data, I'll post them here. 

### [1. Setting up your BGC Argo Python environment](https://www.bgc-argo-plus.info/code-pages/bgc-argo-plus-env)
### [2. Download example example float data and make preliminary plots](https://www.bgc-argo-plus.info/code-pages/Float_file_exploration/)
### [3. Calculate seasonal NCP via Nitrate Drawdown](https://www.bgc-argo-plus.info/code-pages/NCP_nitrate_drawdown/)
### [4. Find the amount of ship/float data available in a given region (currently unfinished)](https://www.bgc-argo-plus.info/code-pages/NCP_nitrate_drawdown/)




**Have a script that you think could be useful to the community? Send me an e-mail and we can add it to the repository or link to it here. Find a bug? Please let the author know.**

## New to Python? Here are some links and recommendations for getting started:

### Getting started with Python:
You might get better guidance googling "how to get started with python" but there are a million avenues out there, so here's one approach:

- Download anaconda or miniconda as a package manager: https://conda.io/projects/conda/en/latest/user-guide/install/index.html
- Create a virtual environment to save new packages into. Using a virtual environment avoids messing up your computer by installing a million packages into your base environment. If you screw up a virtual environment, just make a new one. I make a different one for most projects.
  - Instructions for making the "bgc_argo_env" environment can be found in Lesson #1 above.  
- You can now start coding in your program of choice. Many people use jupyter notebook, which will open a browser tab that you can use to create different notebooks. You need to have activated your python environment first though, otherwise your notebook will not have access to any of the packages you installed:

```
(base) smb-uh@smb-uh BGC-Argo-Plus % conda activate bgc_argo_env
(bgc_argo_env) smb-uh@smb-uh BGC-Argo-Plus % jupyter notebook
[I 2025-12-11 10:07:41.453 LabApp] JupyterLab extension loaded from /Users/smb-uh/opt/anaconda3/lib/python3.9/site-packages/jupyterlab
[I 2025-12-11 10:07:41.453 LabApp] JupyterLab application directory is /Users/smb-uh/opt/anaconda3/share/jupyter/lab
[I 10:07:41.457 NotebookApp] Serving notebooks from local directory: /Users/smb-uh/UHM_Ocean_BGC_Group Dropbox/Seth Bushinsky/Work/Projects/2023_10 UHM_BGC_float_processing/BGC-Argo-Plus
[I 10:07:41.457 NotebookApp] Jupyter Notebook 6.4.8 is running at:
[I 10:07:41.457 NotebookApp] http://localhost:8888/?token=d75d554e66b178b1767be9d971143ccc91ac88af6adbd7f5
[I 10:07:41.457 NotebookApp]  or http://127.0.0.1:8888/?token=d75d554e66b178b1767be9d971143ccc91ac88af6adbd7f5
[I 10:07:41.457 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:07:41.460 NotebookApp] 
    
    To access the notebook, open this file in a browser:
        file:///Users/smb-uh/Library/Jupyter/runtime/nbserver-55003-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/?token=d75d554e66b178b1767be9d971143ccc91ac88af6adbd7f5
     or http://127.0.0.1:8888/?token=d75d554e66b178b1767be9d971143ccc91ac88af6adbd7f5

```
- I personally like VS code and primarily use it for coding: https://code.visualstudio.com/

## Links I've found useful:
- Geoscience computing and virtual environments: https://rabernat.github.io/research_computing_2018/python-environments.html 
- Managing environments: https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#
- Git version control: https://swcarpentry.github.io/git-novice/ and https://www.earthdatascience.org/workshops/setup-earth-analytics-python/
- Setting up github on a new machine: https://gist.github.com/qin-yu/bc26a2d280ee2e93b2d7860a1bfbd0c5 

### Matlab and R
For now I am not posting my old Matlab code and I've never used R, so I can't help much there for now. However, I will post links to useful resources as I find them and if you write scripts that use the BGC-Argo+ dataset and would like to share it with the community, let me know and we can add it to the repository or link to it here.
