# Combine Redshift Catalogs

The **Combine Redshift Catalogs** pipeline allows users to generate a single, unified redshift sample by combining multiple individual redshift catalogs. It uses the [LSDB library](https://docs.lsdb.io/en/stable/index.html), developed by the [LINCC Frameworks](https://lsstdiscoveryalliance.org/programs/lincc-frameworks/) team, to perform spatial crossmatching between catalogs and identify multiple measurements of the same galaxy. The pipeline offers flexible options to resolve or retain duplicates.

This process is especially useful for preparing clean redshift samples that can be used as training sets, validation sets, or calibration inputs for photometric redshift (photo-z) estimation.


## How the Pipeline Works

### 1. Input Catalog Preparation

Each selected redshift catalog goes through a preparation step:

- The catalog is read using Dask for efficient processing.
- A unique identifier called `CRD_ID` is assigned to each object. This ID is sequential across all catalogs (e.g., the second catalog starts counting where the first ended).
- The pipeline attempts to translate redshift quality flags and instrument types to the internal homogenized system. For this to work, the catalog must contain a `survey` column with uppercase string values (e.g., `2DFGRS`, `2DFLENS`, etc.) that match a survey supported in the internal translation YAML. 
- If the user selects to resolve duplicates, each catalog is checked for internal duplicates based on exact matches in RA and DEC (rounded to 6 decimal places).

For administrators uploading official spectroscopic catalogs to the PZServer, please contact the LIneA team so we can update the internal YAML file used to translate quality flags and instrument types. This ensures that the original flags are properly mapped and interpreted by the pipeline. This process maps original survey-specific values into a standardized system inspired by VVDS:

<font size=4>Standard quality flag system</font>

- 0: No redshift
- 1: Low confidence (<70%)
- 2: Medium confidence (70–90%)
- 3: High confidence (90–99%)
- 4: Very high confidence (>99%)
- 6: Star (non-extragalactic)

<font size=4>Instrument types</font>

- `s`: Spectroscopic
- `g`: Grism
- `p`: Photometric

If the catalog is **user-uploaded**, users are responsible for ensuring that the columns `z_flag_homogenized` and `instrument_type_homogenized` are present in the dataset. These column names must match exactly and follow the standard system described above. Even if the catalog includes a `survey` column and the original quality flags, automatic translation may not work if the survey is not yet supported in the internal YAML — or if the translation for that survey depends on conditions involving specific columns that may be missing from the user’s dataset. Providing homogenized columns directly is the only way to guarantee correct behavior for user-uploaded catalogs.

Additionally, the pipeline flags objects that fall within key Deep Drilling Fields ([LSST DP1](https://dp1.lsst.io/index.html) regions), such as ECDFS, EDFS, 47 Tuc, Fornax, and others.

---

### 2. Duplicate Resolution Within a Single Catalog
When the user selects to resolve duplicates, each catalog is checked for objects with identical sky positions (RA and DEC rounded to 6 decimal places). The following logic is used:

- Objects with the same rounded RA/DEC are grouped.
- Tie-breaking is applied in fixed priority order:
    - `z_flag_homogenized` (higher is better, 6 = star is eliminated)
    - `instrument_type_homogenized` (priority: `s > g > p`)
- In each round:
    - Objects with the best score are retained for the next round.
    - Others are eliminated.
- If a tie persists after all columns:
    - The pipeline applies the fixed threshold `delta_z_threshold = 0.0005`. If the redshift difference is smaller than 0.0005, one entry is selected and the other eliminated. If the redshift difference is greater than 0.0005, the pair is kept as a hard tie (`tie_result = 2`).
- Final labels:
    - `tie_result = 1`: selected object
    - `tie_result = 0`: eliminated
    - `tie_result = 2`: unresolved tie

This process aims to ensure that each sky position in a catalog contributes at most one object to the final sample, according to scientifically-motivated priorities. However, in some cases unresolved ties (hard ties) may remain if no clear winner can be determined, and those will be labeled with `tie_result = 2`.

---

### 3. Cross-Catalog Duplicate Resolution
When combining multiple catalogs with duplicate resolution enabled, the pipeline uses LSDB to perform crossmatches between catalogs. For each matched pair:

- Tie-breaking is applied in fixed priority order:
    1. `z_flag_homogenized` (higher is better; entries with value 6, indicating stars, are eliminated).
    2. `instrument_type_homogenized` (priority order: `s > g > p`).

- Missing values (NaNs) are handled with fallback logic:
    - If both sides have valid values, the higher one wins.
    - If only one side has a value, it's remembered as a fallback.
    - If no column resolves the tie, the fallback is used.
    - If all values are NaN or equal, the pair is marked as a hard tie (`tie_result = 2`).

- If a hard tie remains, the pipeline applies the fixed `delta_z_threshold = 0.0005`:
    - If the redshift difference is smaller than 0.0005, one of the entries is kept.
    - If the redshift difference is greater than 0.0005, the pair is kept as a hard tie (`tie_result = 2`).

- Final decisions are stored in `tie_result`, and the `compared_to` field lists all other IDs that were considered during tie-breaking.

---

### 4. Output Options
After combining and resolving duplicates (if selected), the final dataset is cleaned and saved.

- The main columns (`id`, `ra`, `dec`, `z`, `z_flag`, `z_err`, `survey`, `z_flag_homogenized`, `instrument_type_homogenized`) are preserved if present and not entirely missing. Additionally, the column `source` (containing the internal product name) is always included, as well as `tie_result` and `compared_to` — but only if the duplicate resolution option was enabled.
- If the option **"Yes, and remove duplicates"** is selected, **only rows with `tie_result == 1` are retained**.
- The final file can be saved in one of the following formats:
    - Parquet
    - HDF5
    - CSV
    - FITS



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

### Photo-z Server API

!!! warning
    section under construction 
    

