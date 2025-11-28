# Combine Redshift Catalogs

El *Combine Redshift Catalogs* es un *pipeline* que permite generar una muestra única y unificada de *redshifts* combinando múltiples catálogos individuales. Utiliza la biblioteca [LSDB](https://docs.lsdb.io/en/stable/index.html), desarrollada por [LINCC Frameworks](https://lsstdiscoveryalliance.org/programs/lincc-frameworks/), para realizar el *crossmatch* espacial entre catálogos e identificar múltiples mediciones de una misma galaxia. El *pipeline* ofrece opciones flexibles para resolver o mantener duplicados.

Este proceso es especialmente útil para preparar muestras de *redshift* limpias que pueden utilizarse como *training sets*, *validation sets* o entradas de calibración para estimaciones de *redshift* fotométrico (*photo-z*).

## Cómo ejecutar el pipeline

### Sitio web de Photo-z Server

Al ejecutar el *pipeline* desde la interfaz gráfica del [*Photo-z Server*](https://pzserver.linea.org.br/), el usuario debe proporcionar la siguiente información:

1. **Nombre del catálogo combinado**  
    Un nombre corto y descriptivo que se utilizará para registrar el resultado en el sistema y encontrarlo en la lista de productos en búsquedas futuras.  
    No es necesario elegir un nombre único; el sistema añadirá automáticamente un número de ID interno al nombre del producto para garantizar su unicidad.

2. **Descripción** *(opcional)*  
   Cualquier observación o nota sobre la muestra.

3. **Seleccionar catálogos de redshift**  
   Seleccione dos o más catálogos ya disponibles en el sistema.

4. **Resolver duplicados**
     - **No**: Simplemente concatena los catálogos, apilando las columnas según el significado asignado durante la asociación de columnas en el momento de la carga ([paso 3 de la sección «Upload a new data product»](./pz_server.md#upload-a-new-data-product)).  
       **Importante:** Incluso en este modo, la etapa de preparación aún se ejecuta e intenta producir `z_flag_homogenized` e `instrument_type_homogenized` en caso de que aún no existan. Por lo tanto, aún pueden ocurrir errores de validación (véanse las advertencias más abajo).
     - **Sí, pero mantener todos**: Identifica duplicados y añade la información `tie_result`, pero conserva todas las entradas.
     - **Sí, y eliminar duplicados**: Identifica y **mantiene únicamente la mejor medición** por galaxia (solo filas con `tie_result == 1`).

5. **Formato de salida**  
   Elija uno entre: Parquet, HDF5, CSV o FITS.

Tras completar los campos, haga clic en **Run** para ejecutar el *pipeline*.

### API de Photo-z Server

Para obtener instrucciones sobre cómo ejecutar el *pipeline* mediante la biblioteca Python (API) del *Photo-z Server*, consulte:  
https://github.com/linea-it/pzserver/blob/main/docs/notebooks/pzserver_tutorial.ipynb

### Interpretación de `tie_result`

Cuando la resolución de duplicados está habilitada, el *pipeline* asigna un valor a la columna `tie_result`:

- `1`: Mejor entrada (objeto único o ganador del *tie-break*)  
- `0`: Entrada descartada (perdedora del *tie-break*)  
- `2`: *Hard tie* (empate no resuelto)  
- `3`: Estrella (objeto no extragaláctico; no participa en el *tie-breaking*)

Esta información es especialmente relevante al seleccionar las opciones «Sí, pero mantener todos» o «Sí, y eliminar duplicados».

!!! warning
    Para habilitar la resolución de duplicados, los catálogos de entrada **deben** proporcionar señales de *tie-breaking* mediante al menos una de las rutas admitidas a continuación.  
    La etapa de preparación **siempre** se ejecuta (incluso al elegir la simple concatenación) y validará/calculará las columnas homogeneizadas según sea necesario.

    **Ruta A — Columnas homogeneizadas proporcionadas por el usuario**

    Proporcione ambas:

    - `z_flag_homogenized` (numérica) con los valores permitidos:
        - `0`: Sin *redshift*  
        - `1`: Baja confianza (<70%)  
        - `2`: Confianza media (70–90%)  
        - `3`: Alta confianza (90–99%)  
        - `4`: Confianza muy alta (>99%)  
        - `6`: Estrella (objeto no extragaláctico)
    - `instrument_type_homogenized` (cadena de texto) con:
        - `s`: Espectroscópico  
        - `g`: *Grism*  
        - `p`: Fotométrico

    Observaciones:

    - Los valores `NaN` están permitidos y serán ignorados por el *tie-breaking*.  
    - Cualquier valor **fuera** de los conjuntos anteriores provocará que el *pipeline* **falle**.

    **Ruta B — Inferencia rápida a partir de columnas de calidad y tipo (estilo SITCOMTN-154)**

    - Si la columna asociada a `z_flag` contiene valores en **[0, 1]** con **al menos un valor estrictamente entre 0 y 1** (es decir, una puntuación de calidad), el *pipeline* la asigna automáticamente a `z_flag_homogenized` utilizando umbrales internos.  
    - Si existe una columna denominada `type` que contiene únicamente los valores **`"s"`, `"p"` o `"g"`** (sin distinción de mayúsculas o minúsculas), se asigna a `instrument_type_homogenized`.  
    - Este es el caso de los catálogos que siguen el estándar SITCOMTN-154.  
    - Si `z_flag` no es del tipo calidad o si `type` contiene valores no válidos (p. ej., `"m"`), dichas columnas son **ignoradas**, y el *pipeline* recurre a la traducción basada en los nombres de `survey`.

    **Ruta C — Traducción mediante nombres de `survey`**

    - Si existe una columna `survey`, el *pipeline* intenta inferir `z_flag_homogenized` e `instrument_type_homogenized` fila por fila utilizando las reglas internas definidas en el archivo de traducción:  
      [flags_translation.yaml](https://github.com/linea-it/pzserver_pipelines/blob/main/combine_redshift_dedup/flags_translation.yaml)
    - Diferentes nombres de `survey` pueden coexistir en el mismo archivo. Sin embargo, **cada fila** debe hacer referencia a un *survey* presente en el archivo de traducción.  
      Si al menos una fila contiene un *survey* no admitido, el *pipeline* **generará un error**.
    - Casos especiales tratados mediante asignaciones predefinidas:
        - `JADES_LETTER_TO_SCORE = {"A": 4.0, "B": 3.0, "C": 2.0, "D": 1.0, "E": 0.0}`
        - `VIMOS_FLAG_TO_SCORE = {"LRB_X": 0.0, "MR_X": 0.0, "LRB_B": 1.0, "LRB_C": 1.0, "MR_C": 1.0, "MR_B": 3.0, "LRB_A": 4.0, "MR_A": 4.0}`  

      Para estos dos *surveys*, el valor del indicador de entrada **debe coincidir exactamente** con las cadenas mostradas anteriormente para ser reconocido.

    **Condiciones de fallo (cualquiera de las siguientes):**

    - `z_flag_homogenized` o `instrument_type_homogenized` existen pero contienen valores fuera de los conjuntos permitidos.  
    - No se proporcionan columnas homogeneizadas **y**:
        - `z_flag` **no** es del tipo calidad (sin valores estrictamente entre 0 y 1), **o** la columna `type` está ausente o contiene valores fuera de `{"s","p","g"}`; **y**
        - El *pipeline* recurre a la traducción, pero la columna `survey` está ausente **o** contiene al menos un *survey* no admitido.  
    - Tras la preparación/homogeneización, **todos los valores** en `z_flag_homogenized` o en `instrument_type_homogenized` son `NaN` (estas columnas son obligatorias para el criterio de *tie-breaking* y deben contener al menos un valor válido).

    ---

    💡 **Mejora futura**

    En versiones futuras, se prevé admitir archivos de traducción y reglas de *tie-breaking* definidas por el usuario. Esto permitirá:

    - Personalizar el orden y contenido de las columnas utilizadas en el *tie-breaking* (`tiebreaking_priority`)
    - Utilizar columnas numéricas adicionales o alternativas (p. ej., puntuaciones de calidad o métricas personalizadas) para resolver empates
    - Añadir soporte para nuevos *surveys* proporcionando reglas de traducción propias para indicadores de calidad y tipos de instrumento
