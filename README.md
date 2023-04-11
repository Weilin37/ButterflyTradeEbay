# Guide for eBay Butterfly Scraping

### Pre-requisites

As a pre-requisite, install python 3 (version 3.8 is used at the time of writing) and jupyterlab to run the jupyter notebooks. There will also be several python packages which are imported in the beginning of each notebook that will need to be installed.

### Introduction

This guide is to run the web scraper for butterflies sold in eBay. It is meant to scrape butterfies worldwide based on their search terms (typically species name) and listings that are instant and auction-based.

Inside the "Butterfly Scraping" folder, you'll see a list of jupyter ipynb python files.

The list of python files are split into the following:

1. "Past Sales List Only.ipynb" file
2. "Past Sales Fill In.ipynb" file
3. "Download Images.ipynb" file

The files are split into three because the scraping works in three phases:

### Phase 1: List Only
The "List Only" phase uses the "Past Sales List Only.ipynb" Jupyter notebook file. What this accomplishes is getting an initial scraping of the search results in eBay, without going into each listing in detail. This phase is usually the most time consuming phase. This script needs a few steps to run:

1. Base folder to contain the files. This is usually a folder like "June 2022" to indicate the month and year of scraping. For the purpose of this tutorial the folder will be called "Example Base Folder"
2. A folder called "past sales" inside the base folder. This is to contain scraping results on an individual species basis
3. A folder called "images" inside the "past sales" folder. This is to contain scraped images from each listing
4. A CSV file with a list of species names that need to be searched scraped. the file must be called "species.csv" with one column named "Species"
5. A "www.ProxyCrawl.com" subscription. The scraper utilizes a third-party service called "proxycrawl" that automatically solves "Captchas" to get past eBay scraping captchas. 
6. In the 2nd cell, change the parameters to include your proxycrawl API key, your base folder name, the date of scraping, the "part" and "part_size" (see next step)
7. Due to the sheer volume of species that need to be scraped, your "species.csv" may be tens of thousands of rows large. It is recommended to run mulitple versions of this notebook in parallel. The "part" variable in the 2nd cell splits up the "species.csv" list into several parts of size 500 so that the work is divided in each parallel run. The part size can be changed in the "part_size" variable in the 2nd cell
8. After the parameters are set, run each cell in the notebook up to the last.
9. While this script is running, it will create a CSV file that keeps track of the scraping progress. It will include information on the species scraped, the URL of the search term, whether any results are found, the number of found results, and the number of attempts made to solve the captcha. Do note move or modify this file as this file is also used to resume progress in the case that the script is interrupted for any reason. Additionally, this script will create CSV files in the "past sales" folder for each species, prefilling any high level scraped information found in the search results. Some columns will be blank and that will be filled in during Phase 2.

### Phase 2: Fill In
The "Fill In" phase takes the search results from the "List Only" phase and goes into each listing and fills in detailed information on a listing by listing basis. This phase is slightly less time consuming than phase 1. In a similar fashion as the "List Only" phase, running this script will involve a few steps and parameters to be set (2nd cell of the script):

1. The base folder will need to be defined. In this tutorial, we will continu to use "Example Base Folder"
2. The script can also be run in parallel parts, and thus the "part" and "part_size" variable will need to be set.
3. Make sure that the "past sales" folder exists in the base folder
4. While this script is running, it will fill in more information in the existing CSV files in the "past sales" folder that were generated during Phase 1. There is no resume function, and so any interrupts means that the script will start from the beginning. For that reason, run the script in parallel parts and keep the part size relatively small (i.e. 200)

### Phase 3: Download Images
The "Download Images" phase is the last step. After all the information is scraped from each species, the script will go into each CSV file in "past sales" and download the primary image in each listing. This phase is relatively quick. The images will be saved in the "past sales\images" folder, so make sure the folder exists.

# Guide for Butterfly Clustering

### Pre-requisites

As a pre-requisite, install python 3 (version 3.8 is used at the time of writing) and jupyterlab to run the jupyter notebooks. There will also be several python packages which are imported in the beginning of each notebook that will need to be installed.

### Introduction

This guide is to run the clustering algorithm for clustering butterfly images based on feature similarity. Inside the "Butterfly Image Clustering" folder, you will see two folders and two notebook scripts.

The "Clusters" folder is where clustering visual results are saved. The "images" folder is where input images ready for clustering are stored.

The two notebooks are described below:

### Image Processing
The "Image Processing.ipynb" jupyter notebook is to process images and prepare them for clustering. There are three primary cells aiming at three different tasks:

1. Convert white backgrounds into transparent backgrounds
2. Crop images so that it crops to the minimum boundary size
3. Add paddings to the images so that the butterflies are proportional in size relative to other butterflies. The proportions are determined via a separate file (in this tutorial, it is wingspan.csv) that contains wingspan information for the butterflies in question. The script will read this information and adjust the paddings proportionally.

It is up to the user to determine which of theses processing steps would be needed

### Image Clustering
The "Image Clustering.ipynb" notebook is the primary notebook to conduct clustering after any processing step. The script will read images fro mthe images folder, load the clustering VVG16() model, collect clustering features, perform k-means clustering, and save the clustering results in a CSV file (clusters.csv) and the visual results in the "Clusters" folder.
