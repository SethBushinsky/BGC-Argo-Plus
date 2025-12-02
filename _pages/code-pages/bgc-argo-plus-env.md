---
layout: single
permalink: /code-pages/bgc-argo-plus-env/
title: "Creating a Python Environment for working with BGC Argo data"
author_profile: false
last_created_on: 2025-12-01
toc: True
toc_label: "Contents"
toc_icon: "cog"
breadcrumbs: true

---
 
Maintaining different Python environments is key, as you will eventually install enough packages that conflicts will likely arise between some. If you do this all in your "base" environment, you'll probably be stuck reinstalling Python and (if you're like me) put off learning Python for a few more years. 

Before you begin working on a new project, decide where you are going to keep the code for that project. I recommend learning to use version control software like github, but I'm not going to get into that here. Check out the Python Basics info on the code-repository page of this site to find some links to git resources. For now I'm going to assume you've some directory I'll refer to as "[PROJECT_DIRECTORY]/code/" where "PROJECT_DIRECTORY" is your project name and "code" is a subfolder where you have your (potentially git linked) code for your project work.

Now, open terminal or equivalent. First, double check that conda is installed:

```
(base) smb-uh@smb-uh BGC-Argo-Plus % conda -V
conda 23.3.1
```
Notice the `(base)` at the beginning of my prompt. This indicates that I am working in the "base" Python environment. I don't want to do anything here. Instead, I want to create a new environment to use where I will install all new packages. I personally tend to create a new environment for each project. 

There are two possible ways to proceed: **Option A**, where you install packages one at a time or **Option B**, where you use a .yml file that gives a list of many packages at once, along with specifying which installer to use, etc. **I'd probably try Option B first.** If you run into trouble try **Option A instead**, usually you can get things to work that way. 

## Option A - installing packages one at a time
 you can create an environment and then manually add packages as needed. This will create a new python environment called "bgc_argo_env" and install python, pandas, numpy, and xarray in it. 
```
(base) smb-uh@smb-uh BGC-Argo-Plus % conda create --name bgc_argo_env python numpy pandas xarray
```

Conda will try to install all four packages, checking for compatability between them. 

```
Retrieving notices: ...working... done
Collecting package metadata (current_repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
  current version: 23.3.1
  latest version: 25.7.0

Please update conda by running

    $ conda update -n base -c defaults conda

Or to minimize the number of packages updated during conda update use

     conda install conda=25.7.0



Package Plan

  environment location: /Users/smb-uh/opt/anaconda3/envs/bgc_argo_env

  added / updated specs:
    - numpy
    - pandas
    - python
    - xarray


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    _openmp_mutex-4.5          |       7_kmp_llvm           8 KB  conda-forge
    bzip2-1.0.8                |       h500dc9f_8         129 KB  conda-forge
    ca-certificates-2025.11.12 |       hbd8a1cb_0         149 KB  conda-forge
    libblas-3.11.0             |2_he492b99_openblas          18 KB  conda-forge
    libcblas-3.11.0            |2_h9b27e0a_openblas          18 KB  conda-forge
    libcxx-21.1.6              |       h3d58e20_0         556 KB  conda-forge
    libexpat-2.7.3             |       heffb93a_0          72 KB  conda-forge
    libffi-3.5.2               |       h750e83c_0          51 KB  conda-forge
    libgcc-15.2.0              |      h08519bb_14         414 KB  conda-forge
    libgfortran-15.2.0         |      h7e5c614_14         136 KB  conda-forge
    libgfortran5-15.2.0        |      hd16e46c_14         1.0 MB  conda-forge
    liblapack-3.11.0           |2_h859234e_openblas          18 KB  conda-forge
    liblzma-5.8.1              |       hd471939_2         102 KB  conda-forge
    libmpdec-4.0.0             |       h6e16a3a_0          76 KB  conda-forge
    libopenblas-0.3.30         |openmp_h6006d49_4         6.0 MB  conda-forge
    libsqlite-3.51.1           |       h86bffb9_0         964 KB  conda-forge
    llvm-openmp-21.1.6         |       h472b3d1_0         304 KB  conda-forge
    numpy-2.3.5                |  py314hf08249b_0         7.8 MB  conda-forge
    openssl-3.6.0              |       h230baf5_0         2.7 MB  conda-forge
    pandas-2.3.3               |  py314hc4308db_2        13.7 MB  conda-forge
    pip-25.3                   |     pyh145f28c_0         1.1 MB  conda-forge
    python-3.14.0              |hf88997e_102_cp314        13.8 MB  conda-forge
    python-dateutil-2.9.0.post0|     pyhe01879c_2         228 KB  conda-forge
    python_abi-3.14            |          8_cp314           7 KB  conda-forge
    six-1.17.0                 |     pyhe01879c_1          18 KB  conda-forge
    tk-8.6.13                  |       hf689a15_3         3.1 MB  conda-forge
    xarray-2025.11.0           |     pyhcf101f3_0         966 KB  conda-forge
    zstd-1.5.7                 |       h281d3d1_5         465 KB  conda-forge
    ------------------------------------------------------------
                                           Total:        53.7 MB

The following NEW packages will be INSTALLED:

  _openmp_mutex      conda-forge/osx-64::_openmp_mutex-4.5-7_kmp_llvm 
  bzip2              conda-forge/osx-64::bzip2-1.0.8-h500dc9f_8 
  ca-certificates    conda-forge/noarch::ca-certificates-2025.11.12-hbd8a1cb_0 
  libblas            conda-forge/osx-64::libblas-3.11.0-2_he492b99_openblas 
  libcblas           conda-forge/osx-64::libcblas-3.11.0-2_h9b27e0a_openblas 
  libcxx             conda-forge/osx-64::libcxx-21.1.6-h3d58e20_0 
  libexpat           conda-forge/osx-64::libexpat-2.7.3-heffb93a_0 
  libffi             conda-forge/osx-64::libffi-3.5.2-h750e83c_0 
  libgcc             conda-forge/osx-64::libgcc-15.2.0-h08519bb_14 
  libgfortran        conda-forge/osx-64::libgfortran-15.2.0-h7e5c614_14 
  libgfortran5       conda-forge/osx-64::libgfortran5-15.2.0-hd16e46c_14 
  liblapack          conda-forge/osx-64::liblapack-3.11.0-2_h859234e_openblas 
  liblzma            conda-forge/osx-64::liblzma-5.8.1-hd471939_2 
  libmpdec           conda-forge/osx-64::libmpdec-4.0.0-h6e16a3a_0 
  libopenblas        conda-forge/osx-64::libopenblas-0.3.30-openmp_h6006d49_4 
  libsqlite          conda-forge/osx-64::libsqlite-3.51.1-h86bffb9_0 
  libzlib            conda-forge/osx-64::libzlib-1.3.1-hd23fc13_2 
  llvm-openmp        conda-forge/osx-64::llvm-openmp-21.1.6-h472b3d1_0 
  ncurses            conda-forge/osx-64::ncurses-6.5-h0622a9a_3 
  numpy              conda-forge/osx-64::numpy-2.3.5-py314hf08249b_0 
  openssl            conda-forge/osx-64::openssl-3.6.0-h230baf5_0 
  packaging          conda-forge/noarch::packaging-25.0-pyh29332c3_1 
  pandas             conda-forge/osx-64::pandas-2.3.3-py314hc4308db_2 
  pip                conda-forge/noarch::pip-25.3-pyh145f28c_0 
  python             conda-forge/osx-64::python-3.14.0-hf88997e_102_cp314 
  python-dateutil    conda-forge/noarch::python-dateutil-2.9.0.post0-pyhe01879c_2 
  python-tzdata      conda-forge/noarch::python-tzdata-2025.2-pyhd8ed1ab_0 
  python_abi         conda-forge/noarch::python_abi-3.14-8_cp314 
  pytz               conda-forge/noarch::pytz-2025.2-pyhd8ed1ab_0 
  readline           conda-forge/osx-64::readline-8.2-h7cca4af_2 
  six                conda-forge/noarch::six-1.17.0-pyhe01879c_1 
  tk                 conda-forge/osx-64::tk-8.6.13-hf689a15_3 
  tzdata             conda-forge/noarch::tzdata-2025b-h78e105d_0 
  xarray             conda-forge/noarch::xarray-2025.11.0-pyhcf101f3_0 
  zstd               conda-forge/osx-64::zstd-1.5.7-h281d3d1_5 


Proceed ([y]/n)? 
```
 
 Typing "y" and hitting enter should give you the following result:

 ```
 Proceed ([y]/n)? y


Downloading and Extracting Packages
                                                                                                                                                                                                                                                                                                                                         
Preparing transaction: done                                                                                                                                                                                                                                                                                                              
Verifying transaction: done                                                                                                                                                                                                                                                                                                              
Executing transaction: done                                                                                                                                                                                                                                                                                                              
#                                                                                                                                                                                                                                                                                                                                        
# To activate this environment, use                                                                                                                                                                                                                                                                                                      
#                                                                                                                                                                                                                                                                                                                                        
#     $ conda activate bgc_argo_env                                                                                                                                                                                                                                                                                                      
#                                                                                                                                                                                                                                                                                                                                        
# To deactivate an active environment, use                                                                                                                                                                                                                                                                                               
#                                                                                                                                                                                                                                                                                                                                        
#     $ conda deactivate                                                                                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                                                                                                         
(base) smb-uh@smb-uh BGC-Argo-Plus % 
```

You can then follow the instructions to activate your new environment:
```
(base) smb-uh@smb-uh BGC-Argo-Plus % conda activate bgc_argo_env
(bgc_argo_env) smb-uh@smb-uh BGC-Argo-Plus % 
```
Note the change from `(base)` to `(bgc_argo_env)`. Now any new packages that you install will be added only to your new environment. Let's try adding matplotlib.
```
(bgc_argo_env) smb-uh@smb-uh BGC-Argo-Plus % conda install matplotlib
Collecting package metadata (current_repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
  current version: 23.3.1
  latest version: 25.7.0

Please update conda by running

    $ conda update -n base -c defaults conda

Or to minimize the number of packages updated during conda update use

     conda install conda=25.7.0



## Package Plan ##

  environment location: /Users/smb-uh/opt/anaconda3/envs/bgc_argo_env

  added / updated specs:
    - matplotlib


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    brotli-1.2.0               |       hf139dec_1          20 KB  conda-forge
    brotli-bin-1.2.0           |       h8616949_1          18 KB  conda-forge
    contourpy-1.3.3            |  py314h00ed6fe_3         266 KB  conda-forge
    cycler-0.12.1              |     pyhcf101f3_2          14 KB  conda-forge
    freetype-2.14.1            |       h694c41f_0         170 KB  conda-forge
    kiwisolver-1.4.9           |  py314hf3ac25a_2          68 KB  conda-forge
    libbrotlicommon-1.2.0      |       h8616949_1          77 KB  conda-forge
    libbrotlidec-1.2.0         |       h8616949_1          30 KB  conda-forge
    libbrotlienc-1.2.0         |       h8616949_1         303 KB  conda-forge
    libdeflate-1.25            |       h517ebb2_0          69 KB  conda-forge
    libfreetype-2.14.1         |       h694c41f_0           8 KB  conda-forge
    libfreetype6-2.14.1        |       h6912278_0         366 KB  conda-forge
    libjpeg-turbo-3.1.2        |       h8616949_0         572 KB  conda-forge
    libpng-1.6.51              |       h380d223_0         292 KB  conda-forge
    libtiff-4.7.1              |       ha0a348c_1         395 KB  conda-forge
    libwebp-base-1.6.0         |       hb807250_0         357 KB  conda-forge
    matplotlib-3.10.8          |  py314hee6578b_0          17 KB  conda-forge
    matplotlib-base-3.10.8     |  py314hd47142c_0         7.8 MB  conda-forge
    munkres-1.1.4              |     pyhd8ed1ab_1          15 KB  conda-forge
    openjpeg-2.5.4             |       h87e8dc5_0         327 KB  conda-forge
    pillow-12.0.0              |  py314hedf0282_2         979 KB  conda-forge
    pyparsing-3.2.5            |     pyhcf101f3_0         102 KB  conda-forge
    tornado-6.5.2              |  py314h6482030_2         883 KB  conda-forge
    xorg-libxau-1.0.12         |       h8616949_1          13 KB  conda-forge
    xorg-libxdmcp-1.1.5        |       h8616949_1          19 KB  conda-forge
    zlib-ng-2.3.1              |       h53ec75d_1         132 KB  conda-forge
    ------------------------------------------------------------
                                           Total:        13.2 MB

The following NEW packages will be INSTALLED:

  brotli             conda-forge/osx-64::brotli-1.2.0-hf139dec_1 
  brotli-bin         conda-forge/osx-64::brotli-bin-1.2.0-h8616949_1 
  contourpy          conda-forge/osx-64::contourpy-1.3.3-py314h00ed6fe_3 
  cycler             conda-forge/noarch::cycler-0.12.1-pyhcf101f3_2 
  fonttools          pkgs/main/noarch::fonttools-4.25.0-pyhd3eb1b0_0 
  freetype           conda-forge/osx-64::freetype-2.14.1-h694c41f_0 
  kiwisolver         conda-forge/osx-64::kiwisolver-1.4.9-py314hf3ac25a_2 
  lcms2              conda-forge/osx-64::lcms2-2.17-h72f5680_0 
  lerc               conda-forge/osx-64::lerc-4.0.0-hcca01a6_1 
  libbrotlicommon    conda-forge/osx-64::libbrotlicommon-1.2.0-h8616949_1 
  libbrotlidec       conda-forge/osx-64::libbrotlidec-1.2.0-h8616949_1 
  libbrotlienc       conda-forge/osx-64::libbrotlienc-1.2.0-h8616949_1 
  libdeflate         conda-forge/osx-64::libdeflate-1.25-h517ebb2_0 
  libfreetype        conda-forge/osx-64::libfreetype-2.14.1-h694c41f_0 
  libfreetype6       conda-forge/osx-64::libfreetype6-2.14.1-h6912278_0 
  libjpeg-turbo      conda-forge/osx-64::libjpeg-turbo-3.1.2-h8616949_0 
  libpng             conda-forge/osx-64::libpng-1.6.51-h380d223_0 
  libtiff            conda-forge/osx-64::libtiff-4.7.1-ha0a348c_1 
  libwebp-base       conda-forge/osx-64::libwebp-base-1.6.0-hb807250_0 
  libxcb             conda-forge/osx-64::libxcb-1.17.0-hf1f96e2_0 
  matplotlib         conda-forge/osx-64::matplotlib-3.10.8-py314hee6578b_0 
  matplotlib-base    conda-forge/osx-64::matplotlib-base-3.10.8-py314hd47142c_0 
  munkres            conda-forge/noarch::munkres-1.1.4-pyhd8ed1ab_1 
  openjpeg           conda-forge/osx-64::openjpeg-2.5.4-h87e8dc5_0 
  pillow             conda-forge/osx-64::pillow-12.0.0-py314hedf0282_2 
  pthread-stubs      conda-forge/osx-64::pthread-stubs-0.4-h00291cd_1002 
  pyparsing          conda-forge/noarch::pyparsing-3.2.5-pyhcf101f3_0 
  qhull              conda-forge/osx-64::qhull-2020.2-h3c5361c_5 
  tornado            conda-forge/osx-64::tornado-6.5.2-py314h6482030_2 
  xorg-libxau        conda-forge/osx-64::xorg-libxau-1.0.12-h8616949_1 
  xorg-libxdmcp      conda-forge/osx-64::xorg-libxdmcp-1.1.5-h8616949_1 
  zlib-ng            conda-forge/osx-64::zlib-ng-2.3.1-h53ec75d_1 


Proceed ([y]/n)? y


Downloading and Extracting Packages
                                                                                                                                                                                                                                                                                                                                         
Preparing transaction: done                                                                                                                                                                                                                                                                                                              
Verifying transaction: done                                                                                                                                                                                                                                                                                                              
Executing transaction: done                                                                                                                                                                                                                                                                                                              
(bgc_argo_env) smb-uh@smb-uh BGC-Argo-Plus % 
```
## Option B - using a .yml file
Instead of installing packages one at a time, you can try to install everything from a ".yml" file. This can be handy, especially if there are a certain set of package versions that work together. Sometimes Conda has a hard time getting everything to work well together and this fails and/or it takes forever. Either option A or B are fine as long as you get the packages you need installed. 

First, download the .yml file from the BGC Argo+ code repository: [](https://github.com/Hi-Cycles/BGC_Argo_Plus_Code_Repository/blob/main/bgc_argo_env.yml). Put this in the directory where you will store your code ("[PROJECT_DIRECTORY]/code/"). 

bgc_argo_env.yml should look like this:
```
name: bgc_argo_env
channels:
 - conda-forge
 - defaults
dependencies:
 - numpy # general numerical computing
 - pandas # general data handling
 - xarray # useful for working with model data or other multi-dimensional datasets, especially netcdf files
 - netCDF4 # required for xarray and general use of netcdf files
 - matplotlib # plotting package
 - jupyter # needed for running notebooks
 - python
 - scipy # statistical functions
 - gsw # for calculating different seawater properties
 - cartopy # a mapping package
 - tqdm # for helpful progress bars
 - pip # install packages that are not available through conda
 - pip:
   - PyCO2SYS # for calculating carbonate system parameters
```

This file will be used to create a new Python environment with the name in the .yml file (note that this is NOT the name of the file, but what comes after "name: " in the first link of the file). After entering the next command you might want to go for a walk, get a cup of coffee, or pet your cat - it will likely take a long time for Conda to sort out the various dependencies here. If you'd rather actively be working on things, then you can go back to Option A, but work down this list of packages one by one. Note that after you install "pip" using `conda install pip`, you'll then use "pip" to install PyCO2SYS. PyCO2SYS cannot be installed through conda, so we use PIP (`pip install PyCO2SYS`). Don't ask me why. 

If you're continuing with the .yml file:
```
(base) smb-uh@smb-uh code % conda env create -f bgc_argo_env.yml   
Collecting package metadata (repodata.json): done
Solving environment: / 
```
It'll likely sit spinning for a while. 

## General tips for working with Python environments

### List all installed environments
Forget whether you already made an environment to work on a specific project? Use `conda info --envs`
```
(bgc_argo_env) smb-uh@smb-uh BGC-Argo-Plus % conda info --envs  
# conda environments:
#
base                     /Users/smb-uh/opt/anaconda3
argo_base                /Users/smb-uh/opt/anaconda3/envs/argo_base
argo_base_2024           /Users/smb-uh/opt/anaconda3/envs/argo_base_2024
bgc_argo_env          *  /Users/smb-uh/opt/anaconda3/envs/bgc_argo_env
cds_environment          /Users/smb-uh/opt/anaconda3/envs/cds_environment
cmip_processing          /Users/smb-uh/opt/anaconda3/envs/cmip_processing
dashboard                /Users/smb-uh/opt/anaconda3/envs/dashboard
float_bgc_synthesis_products     /Users/smb-uh/opt/anaconda3/envs/float_bgc_synthesis_products
float_oxygen_offset_impacts     /Users/smb-uh/opt/anaconda3/envs/float_oxygen_offset_impacts
float_processing         /Users/smb-uh/opt/anaconda3/envs/float_processing
float_processing_new     /Users/smb-uh/opt/anaconda3/envs/float_processing_new
mayavi_env               /Users/smb-uh/opt/anaconda3/envs/mayavi_env
ocn_623                  /Users/smb-uh/opt/anaconda3/envs/ocn_623
oxygen_env               /Users/smb-uh/opt/anaconda3/envs/oxygen_env
parcels                  /Users/smb-uh/opt/anaconda3/envs/parcels
som_ffn                  /Users/smb-uh/opt/anaconda3/envs/som_ffn
ventilation              /Users/smb-uh/opt/anaconda3/envs/ventilation

(bgc_argo_env) smb-uh@smb-uh BGC-Argo-Plus % 
```
The '*' tells you which environment is currently active. 

### Check what packages have been installed
This will tell you where your environment is installed, what is 
Want to check to make sure something did, in fact, install? Use `conda list`
```
(bgc_argo_env) smb-uh@smb-uh BGC-Argo-Plus % conda list
# packages in environment at /Users/smb-uh/opt/anaconda3/envs/bgc_argo_env:
#
# Name                    Version                   Build  Channel
_openmp_mutex             4.5                  7_kmp_llvm    conda-forge
brotli                    1.2.0                hf139dec_1    conda-forge
brotli-bin                1.2.0                h8616949_1    conda-forge
bzip2                     1.0.8                h500dc9f_8    conda-forge
ca-certificates           2025.11.12           hbd8a1cb_0    conda-forge
contourpy                 1.3.3           py314h00ed6fe_3    conda-forge
cycler                    0.12.1             pyhcf101f3_2    conda-forge
fonttools                 4.25.0             pyhd3eb1b0_0  
freetype                  2.14.1               h694c41f_0    conda-forge
kiwisolver                1.4.9           py314hf3ac25a_2    conda-forge
lcms2                     2.17                 h72f5680_0    conda-forge
lerc                      4.0.0                hcca01a6_1    conda-forge
libblas                   3.11.0          2_he492b99_openblas    conda-forge
libbrotlicommon           1.2.0                h8616949_1    conda-forge
libbrotlidec              1.2.0                h8616949_1    conda-forge
libbrotlienc              1.2.0                h8616949_1    conda-forge
libcblas                  3.11.0          2_h9b27e0a_openblas    conda-forge
libcxx                    21.1.6               h3d58e20_0    conda-forge
libdeflate                1.25                 h517ebb2_0    conda-forge
libexpat                  2.7.3                heffb93a_0    conda-forge
libffi                    3.5.2                h750e83c_0    conda-forge
libfreetype               2.14.1               h694c41f_0    conda-forge
libfreetype6              2.14.1               h6912278_0    conda-forge
libgcc                    15.2.0              h08519bb_14    conda-forge
libgfortran               15.2.0              h7e5c614_14    conda-forge
libgfortran5              15.2.0              hd16e46c_14    conda-forge
libjpeg-turbo             3.1.2                h8616949_0    conda-forge
liblapack                 3.11.0          2_h859234e_openblas    conda-forge
liblzma                   5.8.1                hd471939_2    conda-forge
libmpdec                  4.0.0                h6e16a3a_0    conda-forge
libopenblas               0.3.30          openmp_h6006d49_4    conda-forge
libpng                    1.6.51               h380d223_0    conda-forge
libsqlite                 3.51.1               h86bffb9_0    conda-forge
libtiff                   4.7.1                ha0a348c_1    conda-forge
libwebp-base              1.6.0                hb807250_0    conda-forge
libxcb                    1.17.0               hf1f96e2_0    conda-forge
libzlib                   1.3.1                hd23fc13_2    conda-forge
llvm-openmp               21.1.6               h472b3d1_0    conda-forge
matplotlib                3.10.8          py314hee6578b_0    conda-forge
matplotlib-base           3.10.8          py314hd47142c_0    conda-forge
munkres                   1.1.4              pyhd8ed1ab_1    conda-forge
ncurses                   6.5                  h0622a9a_3    conda-forge
numpy                     2.3.5           py314hf08249b_0    conda-forge
openjpeg                  2.5.4                h87e8dc5_0    conda-forge
openssl                   3.6.0                h230baf5_0    conda-forge
packaging                 25.0               pyh29332c3_1    conda-forge
pandas                    2.3.3           py314hc4308db_2    conda-forge
pillow                    12.0.0          py314hedf0282_2    conda-forge
pip                       25.3               pyh145f28c_0    conda-forge
pthread-stubs             0.4               h00291cd_1002    conda-forge
pyparsing                 3.2.5              pyhcf101f3_0    conda-forge
python                    3.14.0          hf88997e_102_cp314    conda-forge
python-dateutil           2.9.0.post0        pyhe01879c_2    conda-forge
python-tzdata             2025.2             pyhd8ed1ab_0    conda-forge
python_abi                3.14                    8_cp314    conda-forge
pytz                      2025.2             pyhd8ed1ab_0    conda-forge
qhull                     2020.2               h3c5361c_5    conda-forge
readline                  8.2                  h7cca4af_2    conda-forge
six                       1.17.0             pyhe01879c_1    conda-forge
tk                        8.6.13               hf689a15_3    conda-forge
tornado                   6.5.2           py314h6482030_2    conda-forge
tzdata                    2025b                h78e105d_0    conda-forge
xarray                    2025.11.0          pyhcf101f3_0    conda-forge
xorg-libxau               1.0.12               h8616949_1    conda-forge
xorg-libxdmcp             1.1.5                h8616949_1    conda-forge
zlib-ng                   2.3.1                h53ec75d_1    conda-forge
zstd                      1.5.7                h281d3d1_5    conda-forge
(bgc_argo_env) smb-uh@smb-uh BGC-Argo-Plus % 
```
