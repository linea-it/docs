# Training Set Maker

El **Training Set Maker** es un *pipeline* que realiza el *crossmatch* espacial entre un Catálogo de Redshifts previamente registrado y el catálogo **LSST Object**, con el objetivo de crear *training sets* para algoritmos de *photo-z* basados en aprendizaje automático. Utiliza la biblioteca [LSDB](https://docs.lsdb.io/en/stable/index.html), desarrollada por el [LINCC Frameworks](https://lsstdiscoveryalliance.org/programs/lincc-frameworks/), para manejar eficientemente los datos distribuidos espacialmente y ofrecer flexibilidad en los observables incluidos en el *training set*.



### Ejecución mediante el sitio web del Photo-z Server

El *pipeline* puede ejecutarse a través del sitio web del **Photo-z Server**, que proporciona una interfaz fácil de usar para configurar los parámetros del *crossmatch*, seleccionar los contenidos deseados y elegir el formato de salida.

Al ejecutar el *pipeline* desde la interfaz del Photo-z Server, el usuario debe proporcionar la siguiente información:

1. **Nombre del training set**  
    Un nombre corto y descriptivo que se utilizará para registrar el resultado en el sistema y encontrarlo en la lista de productos en futuras búsquedas.  
    No es necesario elegir un nombre único — el sistema añadirá automáticamente un número de ID interno al nombre del producto para garantizar su unicidad.

2. **Descripción (opcional)**  
    Cualquier observación o nota sobre la muestra.

3. **Seleccionar el Catálogo de Redshifts para el crossmatch**  
    Elija entre los Catálogos de Redshift de Referencia ya registrados y listados en el menú.  
    Puede ser el resultado de una ejecución previa del *pipeline* **Combine Redshift Catalogs** o un catálogo personalizado previamente cargado por el usuario.

4. **Seleccionar el Catálogo de Objetos (datos fotométricos)**  
    Elija entre las versiones disponibles del LSST, por ejemplo, **DP0.2**, **DP1**, etc.  

    * **Tipo de flujo (Flux type)**: seleccione qué columnas de flujo se usarán durante el *crossmatch*. Las opciones se generan dinámicamente según el catálogo seleccionado, por ejemplo, `cModel`, `gaap1p0`, `psf`, etc.
    * **Aplicar corrección por extinción (dustmaps)**  
    Seleccione qué mapa de polvo (si lo hay) se utilizará para corregir los flujos fotométricos.
    * **Convertir flujos en magnitudes**  
    Marque esta opción para convertir los flujos en magnitudes. La conversión se realiza mediante la fórmula:

    $$
    mag = -2.5 * \log_{10}(flux) + 31.5
    $$

    donde $flux$ es el valor en la columna de flujo seleccionada.

5. **Seleccionar las configuraciones del crossmatch**
      * **Distancia umbral (arcsec)**: distancia máxima permitida entre las fuentes emparejadas.
      * **Número de vecinos**: número de coincidencias más cercanas que se recuperarán.  
        Seleccione **1** para una coincidencia uno a uno o un número mayor para recuperar múltiples coincidencias.
      
6. **Seleccionar galaxias únicas (próximamente)**  
    En casos de múltiples coincidencias, aplicar un criterio de deduplicación (esta opción aún no está implementada).

7. **Formato de salida**  
    Elija entre los formatos compatibles: **Parquet**, **CSV**, **FITS** o **HDF5**.


#### Ejemplo 1: Training Set con galaxias simuladas del DP0.2

!!! warning
    sección en construcción 
    
    
#### Ejemplo 2: Training Set con galaxias observadas del DP1

!!! warning
    sección en construcción 
    

### Ejecución mediante la API del Photo-z Server

!!! warning
    sección en construcción 
