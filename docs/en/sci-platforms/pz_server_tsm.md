# Training Set Maker 

The Training Set Maker pipeline performs the spatial cross-matching between a pre-registered Redshift Catalog and the LSST Object catalog in order to create training sets for machine-learning based photo-z algorithms. It relies on the [LSDB](https://docs.lsdb.io/en/stable/index.html) library, developed by the [LINCC Frameworks](https://lsstdiscoveryalliance.org/programs/lincc-frameworks/) to handle the spatially distributed data efficiently and provide flexibility in the observables included in the training set. 



### Run via Photo-z Server website 

The pipeline can be run through the Photo-z Server website, which provides a user-friendly interface for configuring the cross-matching parameters, selecting the desired output contents and format.

When running the pipeline from the Photo-z Server UI, users must provide the following information: 

1. **Training Set name**  
    A short mnemonic name that will be used to register the result in the system, and to find it in the products list in future searches. There is no need to choose a unique name, as the system will automatically append the ID number (automatically generated) to the product's internal name to ensure uniqueness.  

2. **Description (optional)**  
    Notes or summary about the training sample.

3. **Select the Redshift Catalog for the cross-matching**  
    Choose from the available registered Reference Redshift Catalogs listed on the menu. It can be either the result of a previous run of the Combine Redshift Catalogs pipeline or a custom catalog previously uploaded by the user. 

4. **Select the Object Catalog (photometric data)**   
    Choose between the LSST releases available, e.g., DP0.2, DP1, etc.  

    * **Flux type**: Select which flux columns to use during the crossmatch. The options are dynamically populated based on the selected Object Catalog, e.g., `cModel`, `gaap1p0`, `psf`, etc.
    * **Apply dereddening from dustmaps**  
    Select which dust map (if any) to use for dereddening the photometric fluxes.
    * **Convert fluxes into magnitudes** 
    Check this box to convert the fluxes into magnitudes. The conversion is done using the formula: 
    
    $$
    mag = -2.5 * \log_{10}(flux) + 31.5 
    $$ 
    
    where $flux$ is the value in the selected flux column.  

5. **Select the cross-matching configuration choices**  
      * **Threshold distance (arcsec)**: maximum allowed distance between matched sources.
      * **Number of neighbors**: number of closest matches to retrieve. Select 1 for a one-to-one match, or a higher number to retrieve multiple matches.
      
6. **Select unique galaxies (coming soon)**  
    In case of multiple matchings, apply a given deduplication criteria (this option is not yet implemented).  

7. **Output format**  
    Choose from the list of supported formats: Parquet, CSV, FITS, or HDF5.


#### Example 1: Training Set with simulated galaxies from DP0.2

!!! warning
    section under construction 
    
    
#### Example 2: Training Set with observed galaxies from DP1

!!! warning
    section under construction 
    

### Run via Photo-z Server API

!!! warning
    section under construction 
    


