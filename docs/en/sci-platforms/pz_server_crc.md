# Combine Redshift Catalogs

The **Combine Redshift Catalogs** pipeline allows users to generate a single, unified redshift sample by combining multiple individual redshift catalogs. It uses the [LSDB library](https://docs.lsdb.io/en/stable/index.html), developed by the [LINCC Frameworks](https://lsstdiscoveryalliance.org/programs/lincc-frameworks/) team, to perform spatial crossmatching between catalogs and identify multiple measurements of the same galaxy. The pipeline offers flexible options to resolve or retain duplicates.

This process is especially useful for preparing clean redshift samples that can be used as training sets, validation sets, or calibration inputs for photometric redshift (photo-z) estimation.

## How to Run the Pipeline

### Photo-z Server website 

When running the pipeline from the [Photo-z Server](https://pzserver.linea.org.br/) GUI, users must provide the following:

1. **Combined catalog name**  
   Name that will be used to register the result in the system.

2. **Description** *(optional)*  
   Any description or notes about the sample.

3. **Select Redshift Catalogs**  
   Choose one or more catalogs already available in the system.

4. **Resolve duplicates**
     - **No**: Simply concatenate the catalogs.
     - **Yes, but keep all**: Identify duplicates and add `tie_result` information, but retain all entries.
     - **Yes, and remove duplicates**: Identify and **keep only the best measurement** per galaxy (only rows with `tie_result == 1`).

5. **Output format**  
   Choose one: Parquet, HDF5, CSV, or FITS.

Once these fields are filled in, users can click **Run** to execute the pipeline.

### Interpreting `tie_result`

When duplicate resolution is enabled, the pipeline assigns a value to the `tie_result` column:

- `1`: Best entry (selected and retained)
- `0`: Discarded entry (loser of the tie-break)
- `2`: Hard tie (multiple entries remain indistinguishable and are all kept)

This information is especially relevant when choosing the “Yes, but keep all” or “Yes, and remove duplicates” options.

!!! warning
    To enable duplicate resolution, your input catalogs **must contain either**:
    
    - The columns `z_flag_homogenized` (numeric) and `instrument_type_homogenized` (string),  
    **or**
    - A column named `survey` with the exact survey name (e.g., `2DFGRS`, `VVDS`, etc.) supported by the internal translation system.

    These fields are used in the tie-breaking logic. If neither the homogenized columns nor a valid `survey` column are present, **the pipeline will fail**.

    When a supported `survey` column is provided, the pipeline attempts to infer the homogenized columns using internal rules and translation tables. However, this process will also fail if the survey is unknown or required auxiliary columns are missing from your dataset.

    **Standard quality flag system** (for `z_flag_homogenized`):
    
    - `0`: No redshift  
    - `1`: Low confidence (<70%)  
    - `2`: Medium confidence (70–90%)  
    - `3`: High confidence (90–99%)  
    - `4`: Very high confidence (>99%)  
    - `6`: Star (non-extragalactic)
    
    **Instrument types** (for `instrument_type_homogenized`):
    
    - `s`: Spectroscopic  
    - `g`: Grism  
    - `p`: Photometric

    Make sure `z_flag_homogenized` is a numeric column and `instrument_type_homogenized` is a string column. Otherwise, the pipeline may crash or behave incorrectly.

### Photo-z Server API

!!! warning
    section under construction 
    

