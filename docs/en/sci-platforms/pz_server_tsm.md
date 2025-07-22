# Training Set Maker 


The **Training Set Maker** pipeline creates high-quality training samples for machine-learning based photometric redshift (photo-z) estimation. It performs a spatial crossmatch between a pre-registered spectroscopic redshift catalog and one of the available photometric catalogs — including [LSST DP0.2](https://dp0-2.lsst.io/), [LSST DP1](https://dp1.lsst.io/index.html), or DES DR2 (from the [Dark Energy Survey](https://www.darkenergysurvey.org/)) — using the [LSDB](https://docs.lsdb.io/en/stable/index.html) library, developed by the [LINCC Frameworks](https://lsstdiscoveryalliance.org/programs/lincc-frameworks/) team.

The left-side catalog (redshift) is loaded directly from a Pandas DataFrame using `lsdb.from_dataframe(...)`, while the right-side photometric catalog is read in HATS format with `lsdb.read_hats(...)`. Margin caching is supported and applied by default to avoid edge effects during the crossmatch. For more on margin cache, refer to the [LSDB margins tutorial](https://docs.lsdb.io/en/stable/tutorials/margins.html).

Although the ideal behavior is to match each spectroscopic object to a unique photometric counterpart, deduplication is not yet implemented, and it's possible for duplicates to appear in the final result due to original redshift catalog duplications or overlapping matches caused by the margin cache.


## How the Pipeline Works


!!! warning
    section under construction 
    



## How to Run the Pipeline

### Photo-z Server website 


When running the pipeline from the Photo-z Server UI, users must provide the following:

1. **Training set name**  
    Name that will be used to register the result in the system.

2. **Description (optional)**  
    Notes or summary about the training sample.

3. **Select the Redshift Catalog for the cross-matching**  
    Must be a previously registered spectroscopic catalog.

4. **Select the Objects catalog (photometric data)**  
    Choose between DES DR2, DP0.2, or DP1.

5. **Flux type**  
    Select which flux columns to use during the crossmatch.

6. **Apply dereddening from dustmaps**  
    Select which dust map (if any) to use for dereddening the photometric fluxes.

7. **Cross-matching configuration**  
      - Threshold distance (arcsec): maximum allowed distance between matched sources.
      - Number of neighbors: number of closest matches to retrieve.
      - Convert fluxes into magnitudes: checkbox to apply flux-to-mag conversion.

8. **Select unique galaxies (coming soon)**  
      This option is not yet implemented.

9. **Output format**  
    Choose from: Parquet, CSV, FITS, or HDF5.


### Photo-z Server API

!!! warning
    section under construction 
    


