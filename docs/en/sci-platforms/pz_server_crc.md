# Combine Redshift Catalogs

The **Combine Redshift Catalogs** pipeline allows users to generate a single, unified redshift sample by combining multiple individual redshift catalogs. It uses the [LSDB](https://docs.lsdb.io/en/stable/index.html) library, developed by the [LINCC Frameworks](https://lsstdiscoveryalliance.org/programs/lincc-frameworks/), to perform spatial crossmatching between catalogs and identify multiple measurements of the same galaxy. The pipeline offers flexible options to resolve or retain duplicates.

This process is especially useful for preparing clean redshift samples that can be used as training sets, validation sets, or calibration inputs for photometric redshift (photo-z) estimation.

## How to Run the Pipeline

### Photo-z Server website 

When running the pipeline from the [Photo-z Server](https://pzserver.linea.org.br/) GUI, users must provide the following:

1. **Combined catalog name**  
    A short mnemonic name that will be used to register the result in the system, and to find it in the products list in future searches. There is no need to choose a unique name, as the system will automatically append the ID number (automatically generated) to the product's internal name to ensure uniqueness.

2. **Description** *(optional)*  
   Any description or notes about the sample.

3. **Select Redshift Catalogs**  
   Choose two or more catalogs already available in the system.

4. **Resolve duplicates**
     - **No**: Simply concatenate the catalogs, stacking the columns according to the meaning assigned during column association at upload time ([step 3 of the "Upload a new data product" section](http://127.0.0.1:8000/en/sci-platforms/pz_server.html#upload-a-new-data-product)).
     - **Yes, but keep all**: Identify duplicates and add `tie_result` information, but retain all entries.
     - **Yes, and remove duplicates**: Identify and **keep only the best measurement** per galaxy (only rows with `tie_result == 1`).

5. **Output format**  
   Choose one: Parquet, HDF5, CSV, or FITS.

Once these fields are filled in, users can click **Run** to execute the pipeline.

### Photo-z Server API

!!! warning
    section under construction 

### Interpreting `tie_result`

When duplicate resolution is enabled, the pipeline assigns a value to the `tie_result` column:

- `1`: Best entry (unique object or the winner of a tie-break)
- `0`: Discarded entry (loser of a tie-break)
- `2`: Hard tie (unresolved)

This information is especially relevant when choosing the “Yes, but keep all” or “Yes, and remove duplicates” options.

!!! warning
    To enable duplicate resolution, your input catalogs **must contain either**:

    - The columns `z_flag_homogenized` (numeric) and `instrument_type_homogenized` (string),  
    **or**
    - A column named `survey` with survey names supported by the internal translation system  
      ([see the `flag_translation.yaml` file](https://github.com/linea-it/pzserver_pipelines/blob/main/combine_redshift_dedup/flags_translation.yaml)).

    These fields are used in the tie-breaking logic. If neither the homogenized columns nor a valid `survey` column are present, **the pipeline will fail**.

    When a `survey` column is provided, the pipeline attempts to infer the homogenized columns on a row-by-row basis using internal translation rules. Therefore, different survey names can coexist in the same file — as long as **each individual value** is recognized.  
    If at least one row contains a survey name that is not listed in the translation file, **the pipeline will raise an error**.

    **Standard system for tie-breaking**:

    - `z_flag_homogenized` must be a numeric column using the following values:
        - `0`: No redshift  
        - `1`: Low confidence (<70%)  
        - `2`: Medium confidence (70–90%)  
        - `3`: High confidence (90–99%)  
        - `4`: Very high confidence (>99%)  
        - `6`: Star (non-extragalactic)

    - `instrument_type_homogenized` must be a string column using:
        - `s`: Spectroscopic  
        - `g`: Grism  
        - `p`: Photometric

    If these columns exist but are incorrectly typed, the pipeline may crash or behave incorrectly.

    ---
     
    💡 **Future improvement**:  
    In future versions, we plan to support user-defined translation files and tie-breaking rules. This will allow users to:

    - Customize the order and content of tie-breaker columns (via `tiebreaking_priority`)
    - Use additional or alternative numeric columns (e.g., quality scores or custom metrics) for resolving ties
    - Add support for new surveys by providing their own translation rules for quality flags and instrument types

    ---
    

