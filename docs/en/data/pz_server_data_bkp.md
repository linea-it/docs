# Photo-z Server Data 

The Photo-z Server administrators regularly upload and update a list of data resources to support the LSST Community with relevant photo-z-related data products. These include both officially released products from Rubin Observatory's Data Management team, datasets prepared by LIneA's Data Management team, and public datasets from other surveys of interest.   

The data product links in the documentation below directs to the product details and download pages. Each page includes a detailed sample description and characterization of the respective data product.  


!!! info "Photo-z Server Documentation "
     The Photo-z Server documentation for users with instructions and examples is available on a [separate page](../sci-platforms/pz_server.md).


 
## External datasets 

Public data collected from the literature and hosted on the Photo-z Server.

### Public individual reference redshift catalogs  

| Data product | Survey Name  | Measurement Type | Redshift Range | # of Redshifts| Standard flags translation | Reference   |  
|---           |---           |---               |---             |---            |---                         |---          |
|              | [COSMOS 2025](https://github.com/cosmosastro/speczcompilation/tree/main)       |                  | $z < 8$        |  165,312      |                            | [Khostovan et al. 2025](https://arxiv.org/abs/2503.00120) |
|              |              |                  |                |               |                            |             |
|              |              |                  |                |               |                            |             |
|              |              |                  |                |               |                            |             | 


### Public redshifts compilations 

Combined catalog of reference redshifts from the literature (primarily spectroscopic). 


| Data product | Duplicates handling | # of Surveys | Redshift Range | # of Redshifts | Last update | 
|---           |---                  |---           |---             |---             |---          |
|              | Keep all            |              |                |                |             |
|              | Flag duplicates     |              |                |                |             |
|              | Remove duplicates   |              |                |                |             |

Standard flags meaning adopted {0: , 1: , 2: , 3: , 4: } 

Quick access via `pzserver` library: 

```python
pz_server.get_product('') 
``` 

### Public training sets  

The result of the spatial cross-matching between the public redshifts compilations and public photometric data.   

| Data product | Photo. Catalog  | Duplicates handling | # of matched Spec. Surveys | Redshift Range | # of Galaxies | Last update | 
|---           | ---             | --                  |---                         |---             |---            |---          |             
|              | DES DR2         | Keep all            |                            |                |               |             |             
|              | DES DR2         | Flag duplicates     |                            |                |               |             |             
|              | DES DR2         | Remove duplicates   |                            |                |               |             |             
|              | DES Y6 Gold     | Keep all            |                            |                |               |             |             
|              | DES Y6 Gold     | Flag duplicates     |                            |                |               |             |             
|              | DES Y6 Gold     | Remove duplicates   |                            |                |               |             | 

Quick access via `pzserver` library: 

```python
pz_server.get_product('') 
``` 


## Data Preview 0


!!! danger "ATTENTION: Example Datasets"  
   These datasets were prepared by the LIneA team for educational purposes, to serve as use case examples for the Photo-z Server tutorials. **They are not classified as Official Datasets released by Rubin's Data Management team.**


#### Redshift Catalogs 

|       | Redshift Catalogs | 
|-------|-------------------|
|       |                   |
|       |                   | 
|       |                   | 

#### Training Sets 

|       | Training Sets    | 
|-------|------------------|
|       |                  |
|       |                  | 
|       |                  | 

#### Training Results 

| Algorithm  | Training Results | 
|------------|------------------|
| BPZ        |                  |
| CMNN       |                  | 
| DNF        |                  | 
| FlexZBoost |                  | 
| KNN        |                  | 
| LePHARE    |                  | 

#### Photo-z Estimates 

| Algorithm  | Photo-z Estimates | 
|------------|-------------------|
| BPZ        |                   |
| CMNN       |                   | 
| DNF        |                   | 
| FlexZBoost |                   | 
| KNN        |                   | 
| LePHARE    |                   | 



## Data Preview 1 


!!! danger "ATTENTION: Preliminary Datasets"  
    These datasets were produced by the PZ Science Unit — a working group from Rubin’s Commissioning Team — during the _Initial studies of photometric redshifts with LSSTComCam from DP1_. All results, along with detailed dataset descriptions, are available in the tech note [SITCOMTN-154](https://sitcomtn-154.lsst.io/). **These datasets are not classified as Official Datasets released by Rubin's DM team.**  

#### Redshift Catalogs 

|       | Redshift Catalogs | 
|-------|-------------------|
|       |                   |
|       |                   | 
|       |                   | 

#### Training Sets 

|       | Training Sets    | 
|-------|------------------|
|       |                  |
|       |                  | 
|       |                  | 

#### Training Results 

| Algorithm  | Training Results | 
|------------|------------------|
| BPZ        |                  |
| CMNN       |                  | 
| DNF        |                  | 
| FlexZBoost |                  | 
| KNN        |                  | 
| LePHARE    |                  | 

#### Photo-z Estimates 

| Algorithm  | Photo-z Estimates | 
|------------|-------------------|
| BPZ        |                   |
| CMNN       |                   | 
| DNF        |                   | 
| FlexZBoost |                   | 
| KNN        |                   | 
| LePHARE    |                   | 


  
:white_check_mark: 

:heavy_multiplication_x: unavailable 


:lock: private 

:busts_in_silhouette: public 



