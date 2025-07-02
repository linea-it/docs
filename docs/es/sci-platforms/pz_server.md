### Introducción

Inspirado por el DES Science Portal ([Gschwend et al., 2018](https://www.sciencedirect.com/science/article/abs/pii/S2213133718300891?via%3Dihub){:target="_blank"}; [Fausti Neto et al., 2018](https://www.sciencedirect.com/science/article/abs/pii/S2213133717300975){:target="_blank"}), el [Photo-z Server](https://pzserver.linea.org.br/) es un servicio en línea complementario a la Rubin Science Platform (RSP) para alojar y generar productos de datos ligeros relacionados con photo-z, y ofrecer herramientas de gestión de datos que permiten compartir productos entre los usuarios de la RSP, adjuntar y compartir metadatos relevantes, y ayudar en el seguimiento de procedencia.

El servicio está alojado en el Centro Brasileño de Acceso Independiente a Datos (IDAC) y está abierto a toda la Comunidad LSST sin restricciones geográficas. Está diseñado para ser lo más amplio y genérico posible para ser útil para todas las Colaboraciones Científicas del LSST que trabajan con productos de datos photo-z. Como exige el programa in-kind del LSST, el código fuente estará disponible públicamente en [GitHub](https://github.com/linea-it/pzserver_app){:target="_blank"}.

El Photo-z Server fue diseñado para ayudar a los usuarios de la RSP a participar en la Photo-z (PZ) Validation Cooperative. Esta iniciativa del equipo de DM tendrá lugar durante la fase de puesta en marcha del LSST (ver la nota técnica [dmtn-049](https://dmtn-049.lsst.io/) para más detalles). El Grupo de Coordinación PZ recibirá credenciales de usuario "admin" con permisos especiales para agregar productos de datos marcados como "productos oficiales".

Durante la PZ Validation Cooperative, el Grupo de Coordinación podrá usar el Photo-z Server para alojar y distribuir conjuntos estandarizados de entrenamiento y validación para experimentos de comparación de algoritmos, y recopilar los resultados de diferentes usuarios. Sin embargo, el Photo-z Server continuará sirviendo a la Comunidad LSST en los años siguientes. Más allá de la PZ Validation Cooperative, los usuarios de la RSP podrán usar el Photo-z Server para rastrear y compartir fácilmente archivos ligeros con diversos resultados de pruebas.

### Primeros pasos

#### Sitio web del Photo-z Server

La interfaz principal del usuario es su sitio web en [pzserver.linea.org.br](https://pzserver.linea.org.br/). 

<p align="center">
  <img src="../../images/pz-server-landing-page.png" alt="Página principal del Photo-z Server">
</p>

Las tres tarjetas en la página de inicio conducen a la lista de productos de datos (izquierda y centro) o a los pipelines del Photo-z Server (derecha).

En la página de productos de datos, los usuarios pueden explorar, buscar y filtrar productos cargados por usuarios o generados mediante pipelines. Los productos cargados se vuelven automáticamente visibles, descargables y compartibles para todos los usuarios registrados.

<p align="center">
  <img src="../../images/pz-server-user-data-products.png" alt="Lista de Productos de Datos del Photo-z Server">
</p>

#### Tipos de productos de datos

Los productos relacionados con photo-z se organizan en cuatro categorías:

* **Reference Redshift Catalogs:** Catálogo con redshifts de referencia y posiciones de galaxias (normalmente redshifts espectroscópicos y coordenadas ecuatoriales).
* **Training Sets:** Conjunto de entrenamiento para algoritmos photo-z (datos tabulares), que usualmente contiene magnitudes, errores y redshifts de referencia.
* **Training Results:** Resultados de un procedimiento de entrenamiento photo-z (formato libre). Usualmente un archivo pickle generado por el submódulo RAIL Inform.
* **Validation Results:** Resultados de un procedimiento de validación photo-z (formato libre). Usualmente contiene estimaciones photo-z (puntuales y/o pdf), métricas de validación y gráficos.
* **Photo-z Estimates:** Resultados de un procedimiento de estimación photo-z (usualmente la salida del módulo RAIL Estimate). Si los datos exceden 200 MB, solo se almacenan los metadatos.



#### Subir un nuevo producto de datos

Haz clic en el botón **NEW PRODUCT** en la esquina superior derecha de la [página de productos del usuario](https://pzserver.linea.org.br/user_products) y completa el formulario de carga con los metadatos correspondientes. La descripción y los archivos auxiliares son opcionales y se pueden editar después.

<p align="center">
  <img src="../../images/pz-server-upload-form.png" alt="Formulario de carga del Photo-z Server">
</p>

Dependiendo del tipo de producto, si es tabular, puede requerirse un formato específico: CSV, FITS, HDF5 o Parquet[^dagger].

[^dagger]: Contacta al equipo de desarrollo si necesitas otro formato.

#### Compartir productos

Cada producto tiene un nombre único, "**internal_name**", compuesto por un **id** más el nombre proporcionado por el usuario con espacios reemplazados por guiones bajos. Este nombre corresponde a la URL del producto (pzserver.linea.org.br/product_id) y es la clave de acceso desde la API de Python. Compartir el **internal_name** o el enlace permite acceso directo.

#### Descargar un producto

En la página de detalles del producto se muestra metadatos, una vista previa de la tabla (si aplica) y el archivo auxiliar HTML si está disponible.

<p align="center">
  <img src="../../images/pz-server-product-details-page.png" alt="Página de detalles del producto del Photo-z Server">
</p>

El botón de descarga genera un archivo `.zip` con el contenido completo del producto, incluyendo archivos auxiliares.

### API del Photo-z Server

El paquete `pzserver` permite acceso a los productos de datos vía línea de comandos en Python. Está disponible en [GitHub](https://github.com/linea-it/pzserver){:target="_blank"} y se instala con:

```bash
pip install pzserver
```

#### Notebook de ejemplo

Un [notebook de ejemplo](https://github.com/linea-it/pzserver/blob/main/docs/notebooks/pzserver_tutorial.ipynb) está disponible en el repositorio de [`pzserver`](https://github.com/linea-it/pzserver). También existe una [página de documentación](https://linea-it.github.io/pzserver){:target="_blank"} con información técnica adicional.

#### Token de acceso

Luego de importar el paquete, puedes conectarte al servidor con:

```python
from pzserver import PzServer
pz_server = PzServer(token="<pega tu token de acceso aquí>")
```

Genera el token desde el sitio web (menú superior derecho).

<img src="../../images/pz-server-token-menu.png" width=200pt align="top"/> <img src="../../images/pz-server-generate-token.png" width=350pt />

#### Comandos básicos

Visualizar tipos de productos:

```python
pz_server.display_product_types()
```

Visualizar releases disponibles:

```python
pz_server.display_releases()
```

Listar todos los productos:

```python
pz_server.display_products_list()
```

Filtrar productos por release y tipo:

```python
pz_server.display_products_list(filters={"release": "DP1", "product_type": "Training Set"})
```

Buscar productos:

```python
search_results = pz_server.get_products_list(filters={"product_type": "training results"})
```

Mostrar metadatos:

```python
pz_server.display_product_metadata(<product_id>)
```

Descargar producto:

```python
pz_server.download_product(<product_id>, save_in=".")
```

Cargar producto a memoria:

```
training_set = pz_server.get_product(<training_set_id>)
training_set.display_metadata()
```

Consulta el [notebook de ejemplo](https://github.com/linea-it/pzserver/blob/main/docs/notebooks/pzserver_tutorial.ipynb) para más ejemplos y métodos avanzados.