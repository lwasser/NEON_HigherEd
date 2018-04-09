---
syncID: a1388c25d16342cca2643bc2df3fbd8e
title: "Use the neonUtilities Package to Access NEON Data"
description: "Convert data downloaded from the NEON Data Portal in zipped month-by-site files into a table with all data of interest. Temperature data are used as an example. "
dateCreated: 2017-08-01
authors: [Megan A. Jones ]
contributors: [Claire K. Lunch ]
estimatedTime: 5 minutes
packagesLibraries: neonUtilities
topics: data-management, rep-sci
languageTool: R
code1: R/neonUtilities/neonDataStackR.R
tutorialSeries:
urlTitle: neonDataStackR

---

This tutorial goes over how to use the neonUtilities R package to 
convert data downloaded from the NEON Data Portal 
in zipped month-by-site files into individual files with all data from the 
given site(s) and months. Temperature data are used as an example.

## Download the Data
To start, you must have your data of interest downloaded from the 
<a href="http://data.neonscience.org" target="_blank"> NEON Data Portal</a>.  

The stacking function will only work on zipped Comma Seperated Value (.csv) 
files and not the NEON data stored in other formats (HDF5, etc). 

Your data will download in a single zipped file. 

The example data below are any single-aspirated air temperature available from 
1 January 2015 to 31 December 2016. 

## neonUtilities package

This package was written to stack data downloaded in month-by-site files into a 
full table with all the data of interest from all sites in the downloaded date
range.  

More information on the package see the README in the associated GitHub repo 
<a href="https://github.com/NEONScience/NEON-utilities/tree/master/neonUtilities" target="_blank"> NEONScience/NEON-utilities</a>. 

First, we must install the package from the GitHub repo. You must have the 
**devtools** package installed and loaded to do this. Then load the package. 


    # install devtools - can skip if already installed
    install.packages("devtools")

    ## Installing package into '/Users/clunch/Library/R/3.4/library'
    ## (as 'lib' is unspecified)

    ## Warning in install.packages :
    ##   cannot open URL 'https://cran.rstudio.com/bin/macosx/el-capitan/contrib/3.4/PACKAGES.rds': HTTP status was '404 Not Found'
    ## 
    ## The downloaded binary packages are in
    ## 	/var/folders/_k/gbjn452j1h3fk7880d5ppkx1_9xf6m/T//Rtmpic9d04/downloaded_packages

    # load devtools
    library(devtools)
    
    # install neonUtilities from GitHub
    install_github("NEONScience/NEON-utilities/neonUtilities", dependencies=TRUE)

    ## Downloading GitHub repo NEONScience/NEON-utilities@master
    ## from URL https://api.github.com/repos/NEONScience/NEON-utilities/zipball/master

    ## Installing neonUtilities

    ## '/Library/Frameworks/R.framework/Resources/bin/R' --no-site-file  \
    ##   --no-environ --no-save --no-restore --quiet CMD INSTALL  \
    ##   '/private/var/folders/_k/gbjn452j1h3fk7880d5ppkx1_9xf6m/T/Rtmpic9d04/devtools151984b0342/NEONScience-NEON-utilities-860d978/neonUtilities'  \
    ##   --library='/Users/clunch/Library/R/3.4/library' --install-tests

    ## 

    # load neonUtilities
    library (neonUtilities)


## stackByTable
There is a single function to run in this package `stackByTable()`. The output
will yield data grouped into new files by table name. For example the single 
aspirated air temperature data product contains 1 minute and 30 minute interval
data. The output from this function is one .csv with 1 minute data and one .csv 
with 30 minute data. 

Depending on your file size this function may run for a while. The 2015 and 2016
single aspirated air temperature from two sites that I used for a 2017 workshop
took about 25 minutes to complete. A progress bar will display while the 
stacking is in progress. 

To run the `stackByTable()` function, input the data product ID (DPID) of the 
data you downloaded, and the file path to the downloaded and zipped file. 
The DPID can be found in the data product box on the 
[new data browse page](http://data.neonscience.org/static/browse.html), or in 
the [data product catalog](http://data.neonscience.org/data-product-catalog). 
It will be in the form DP#.#####.001; the DPID of single aspirated air 
temperature is DP1.00002.001.


    # stack files - Mac OSX file path shown
    stackByTable("DP1.00002.001","~neon/Documents/data/NEON_temp-air-single.zip")


    Unpacking zip files
      |=========================================================================================| 100%
    Stacking table SAAT_1min
      |=========================================================================================| 100%
    Stacking table SAAT_30min
      |=========================================================================================| 100%
    Finished: All of the data are stacked into  2  tables!
    Copied the first available variable definition file to /stackedFiles and renamed as variables.csv
    Stacked SAAT_1min which has 424800 out of the expected 424800 rows (100%).
    Stacked SAAT_30min which has 14160 out of the expected 14160 rows (100%).
    Stacking took 6.233922 secs
    All unzipped monthly data folders have been removed.

From the single-aspirated air temperature data we are given two final tables. 
One with 1 minute intervals: **SAAT_1min** and one for 30 minute intervals: 
**SAAT_30min**.  

In the same directory as the zipped file, you should now have an unzipped 
directory of the same name. When you open this you will see a new directory 
called **stackedFiles**. This directory contains one or more .csv files 
(depends on the data product you are working with) with all the data from 
the months & sites you downloaded. There will also be a single copy of the 
associated varibles.csv and validation.csv files, if applicable (validation 
files are only available for observational data products).

These .csv files are now ready for use with the program of your choice. 
