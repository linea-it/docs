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
       **Important:** Even in this mode, the preparation stage still runs and attempts to produce `z_flag_homogenized` and `instrument_type_homogenized` if they are not already present. Therefore, validation errors can still occur (see warnings below).
     - **Yes, but keep all**: Identify duplicates and add `tie_result` information, but retain all entries.
     - **Yes, and remove duplicates**: Identify and **keep only the best measurement** per galaxy (only rows with `tie_result == 1`).

5. **Output format**  
   Choose one: Parquet, HDF5, CSV, or FITS.

Once these fields are filled in, users can click **Run** to execute the pipeline.

### Photo-z Server API

For instructions on running this pipeline via the Photo-z Server Python library (API), see:  
https://github.com/linea-it/pzserver/blob/main/docs/notebooks/pzserver_tutorial.ipynb

### Interpreting `tie_result`

When duplicate resolution is enabled, the pipeline assigns a value to the `tie_result` column:

- `1`: Best entry (unique object or the winner of a tie-break)
- `0`: Discarded entry (loser of a tie-break)
- `2`: Hard tie (unresolved)
- `3`: Star (non-extragalactic; stars do **not** participate in the tie-breaking)

This information is especially relevant when choosing the “Yes, but keep all” or “Yes, and remove duplicates” options.

!!! warning
    To enable duplicate resolution, your input catalogs **must** provide tie-breaking signals through at least one of the following supported paths.  
    The preparation stage **always** runs (even when choosing simple concatenation), and will validate/compute the homogenized columns accordingly.

    **Path A — User-provided homogenized columns**

    Provide both:

    - `z_flag_homogenized` (numeric) using the allowed values:
        - `0`: No redshift  
        - `1`: Low confidence (<70%)  
        - `2`: Medium confidence (70–90%)  
        - `3`: High confidence (90–99%)  
        - `4`: Very high confidence (>99%)  
        - `6`: Star (non-extragalactic)
    - `instrument_type_homogenized` (string) using:
        - `s`: Spectroscopic  
        - `g`: Grism  
        - `p`: Photometric

    Notes:

    - `NaN` values are allowed and will be ignored by tie-breaking.  
    - Any value **outside** the sets above will cause the pipeline to **fail**.

    **Path B — Fast-path inference from quality-like flags and type (SITCOMTN-154-style)**

    - If the column associated to `z_flag` contains values in **[0, 1]** with **at least one value strictly between 0 and 1** (i.e., a quality-like score), the pipeline automatically maps it to `z_flag_homogenized` via internal thresholds.  
    - If a column named `type` exists and contains only the values **`"s"`, `"p"`, or `"g"`** (case-insensitive), the pipeline assigns it to `instrument_type_homogenized`.  
    - This is the case for catalogs following SITCOMTN-154 patterns.  
    - If the `z_flag` column is not quality-like, or if `type` contains invalid values (e.g., `"m"`), these columns are **ignored**, and the pipeline falls back to the translation stage based on `survey` names.

    **Path C — Translation via `survey` names**

    - If a `survey` column is present, the pipeline tries to infer `z_flag_homogenized` and `instrument_type_homogenized` on a row-by-row basis using internal translation rules defined in the translation file:  
      [flags_translation.yaml](https://github.com/linea-it/pzserver_pipelines/blob/main/combine_redshift_dedup/flags_translation.yaml)
    - Different `survey` names can coexist in the same file. However, **each row** must reference a survey that appears in the translation file.  
      If at least one row contains an unsupported `survey`, the pipeline will **raise an error**.
    - Special cases handled by hardcoded mappings:
        - `JADES_LETTER_TO_SCORE = {"A": 4.0, "B": 3.0, "C": 2.0, "D": 1.0, "E": 0.0}`
        - `VIMOS_FLAG_TO_SCORE = {"LRB_X": 0.0, "MR_X": 0.0, "LRB_B": 1.0, "LRB_C": 1.0, "MR_C": 1.0, "MR_B": 3.0, "LRB_A": 4.0, "MR_A": 4.0}`  

      For these two surveys, the input flag **must exactly match** the strings shown above to be recognized.

    **Failure conditions (any of the following will fail):**

    - `z_flag_homogenized` or `instrument_type_homogenized` exist but contain values outside the allowed sets.  
    - No homogenized columns are provided **and**:
        - `z_flag` is **not** quality-like (no values strictly between 0 and 1), **or** the `type` column is missing or contains values outside `{"s","p","g"}`; **and**
        - The pipeline falls back to translation but the `survey` column is missing **or** contains at least one unsupported survey.  
    - After preparation/homogenization, **all values** in `z_flag_homogenized` or in `instrument_type_homogenized` are `NaN` (these columns are required by the tie-breaking priority and must contain at least one non-NaN).

    ---

    💡 **Future improvement**

    In future versions, we plan to support user-defined translation files and tie-breaking rules. This will allow users to:

    - Customize the order and content of tie-breaker columns (via `tiebreaking_priority`)
    - Use additional or alternative numeric columns (e.g., quality scores or custom metrics) for resolving ties
    - Add support for new surveys by providing their own translation rules for quality flags and instrument types
