### Introducción

Inspirado en el Portal Científico DES ([Gschwend et al., 2018](https://www.sciencedirect.com/science/article/abs/pii/S2213133718300891?via%3Dihub){:target="_blank"}; [Fausti Neto et al., 2018](https://www.sciencedirect.com/science/article/abs/pii/S2213133717300975){:target="_blank"}), el Servidor Photo-z es un servicio en línea complementario a la Plataforma Científica Rubin (RSP) para alojar y producir productos de datos ligeros relacionados con photo-z y ofrecer herramientas de gestión de datos que permiten compartir productos de datos entre usuarios de la RSP, adjuntar y compartir metadatos relevantes y facilitar el rastreo de procedencia.

El servicio se aloja en el Centro Independiente de Acceso a Datos (IDAC) de Brasil y está abierto a toda la comunidad LSST, sin restricciones geográficas. Su diseño es lo más amplio y genérico posible para facilitar la colaboración científica de LSST con productos de datos de photo-z. Según lo exige el programa de donaciones en especie de LSST, el código fuente estará disponible públicamente en [GitHub](https://github.com/linea-it/pzserver_app){:target="_blank"}.

El servidor Photo-z se diseñó para ayudar a los usuarios de RSP a participar en la Cooperativa de Validación de Photo-z (PZ). Esta iniciativa del equipo DM se llevará a cabo durante la fase de puesta en marcha de LSST (consulte la nota técnica [dmtn-049](https://dmtn-049.lsst.io/) para obtener más información). El grupo de coordinación de PZ recibirá credenciales de usuario "admin" con permisos especiales para agregar productos de datos etiquetados como "productos de datos oficiales".

Durante la Cooperativa de Validación de PZ, el Grupo de Coordinación de PZ puede usar el Servidor Photo-z para alojar y distribuir conjuntos estandarizados de entrenamiento y validación para experimentos de comparación del rendimiento de algoritmos y para recopilar los resultados de diferentes usuarios. No obstante, el Servidor Photo-z seguirá prestando servicio a la Comunidad LSST en los próximos años. Además de la Cooperativa de Validación de PZ, los usuarios de RSP pueden usar el Servidor Photo-z para realizar un seguimiento y compartir fácilmente archivos ligeros que contienen diversos resultados de pruebas.

¡Información sobre "Conjuntos de datos"!

Los administradores del Servidor Photo-z mantienen y actualizan periódicamente una lista seleccionada de recursos de datos para apoyar a la Comunidad LSST con productos de datos relacionados con Photo-z. Las descripciones detalladas y los enlaces a cada producto de datos están disponibles en una [página aparte](../data/pz_server_data.md).


### Sitio web del Servidor Photo-z

La interfaz de usuario principal del sitio web del Servidor Photo-z es [pzserver.linea.org.br](https://pzserver.linea.org.br/).

<p align="center">
<img src="../../images/pz-server-landing-page.png" alt="Página de inicio del servidor Photo-z">
</p>

Las tres tarjetas de la página de inicio conducen a la lista de productos de datos (izquierda y centro) o a las canalizaciones del servidor Photo-z (derecha).

En la página de la lista de productos de datos, los usuarios pueden explorar, buscar y filtrar los productos subidos por otros usuarios o creados con la canalización del servidor Photo-z. Los productos de datos subidos al servidor PZ se vuelven automáticamente visibles, descargables y compartibles para todos los usuarios registrados.

<p align="center">
<img src="../../images/pz-server-user-data-products.png" alt="Photo-z Server Data Products List Page">
</p>

### Cargar un nuevo producto de datos

Para cargar un nuevo producto de datos, haga clic en el botón **NUEVO PRODUCTO** en la esquina superior derecha de la [página de Productos de Datos Generados por el Usuario](https://pzserver.linea.org.br/user_products) y complete el formulario de carga con los metadatos relevantes en cuatro pasos:

**Paso 1:** Ingrese un nombre corto y mnemotécnico para su nuevo producto de datos. Seleccione el tipo de producto de datos que va a cargar (p. ej., Catálogo de Redshift de Referencia, Conjunto de Entrenamiento, etc.) y la versión de datos a la que pertenece (si corresponde).

<p align="center">
<img src="../../images/pz-server-upload-form-step1.png" alt="Formulario de Carga del Servidor Photo-z - Paso 1">
</p>

**Paso 2:** Seleccione su archivo principal y todos los archivos auxiliares que desee cargar. El archivo principal contiene el producto de datos, mientras que los archivos auxiliares pueden incluir documentación, descripción o cualquier otra información relevante sobre el producto.

Si el producto de datos es tabular, la herramienta de carga podría requerir formatos de archivo específicos según su tipo. Los formatos actualmente compatibles son: CSV, FITS, HDF5 y Parquet. Póngase en contacto con el equipo de desarrollo (mailto:pzserver-admin@linea.org.br) si su caso científico requiere un formato de archivo diferente o si su archivo supera el límite de 200 MB.

<p align="center">
<img src="../../images/pz-server-upload-form-step2.png" alt="Formulario de carga del servidor Photo-z - Paso 2">
</p>

**Paso 3:** Si el producto de datos es un Catálogo de Redshift de Referencia o un Conjunto de Entrenamiento, algunas columnas son obligatorias. Los nombres de las columnas son libres, pero debe proporcionar la asociación con su significado y [UCD en el estándar IVOA](https://www.ivoa.net/documents/REC/UCD/UCD-20050812.html), como se muestra en la figura siguiente.

<p align="center">
<img src="../../images/pz-server-upload-form-step3.png" alt="Formulario de carga del servidor Photo-z - Paso 3">
</p>

**Paso 4:** Revise su información y vuelva a laSi es necesario, revise los pasos anteriores. No olvide pulsar el botón FINALIZAR al final de la página para enviar su producto de datos.

<p align="center">
<img src="../../images/pz-server-upload-form-step4.png" alt="Formulario de carga del servidor Photo-z - Paso 4">
</p>

### Descargar un producto de datos

Para descargar un producto de datos, haga clic en el icono <img src="../../images/pz-server-download-icon.png" width="30" style="vertical-align: middle;"> en la fila del producto en la página [Productos de datos generados por el usuario](https://pzserver.linea.org.br/user_products). Al hacer clic, se generará un archivo comprimido .zip con todo el contenido del producto, incluyendo los archivos de descripción auxiliares.

También hay un botón en la página de detalles del producto, al que se puede acceder haciendo clic en el nombre del producto en la lista.

<p align="center">
<img src="../../images/pz-server-product-details-page.png" alt="Photo-z Server Product Details Page">
</p>

### Compartir productos de datos

Para compartir un producto de datos, haga clic en el icono <img src="../../images/pz-server-share-icon.png" width="30" style="vertical-align: middle;"> en la fila del producto en la [página de Productos de Datos Generados por el Usuario](https://pzserver.linea.org.br/user_products) o en la página de detalles del producto. Al hacer clic, se abrirá una ventana emergente con el **nombre_interno** y la URL del producto. Puede copiar la información para compartirla con otros usuarios.

!!! Información "nombre_interno"
Cada producto de datos tiene un nombre único ("**nombre_interno**"), compuesto automáticamente por el sistema como un número **id** único seguido del nombre elegido por el usuario, con los espacios en blanco sustituidos por guiones bajos. Este nombre es la URL de la página de detalles del producto de datos en el sitio web de Photo-z Server:

<p align="center"> https://pzserver.linea.org.br/product/nombre_interno </p>

y es la clave para acceder a los datos mediante la API de Python de Photo-z Server (ver detalles a continuación). La forma más sencilla de compartir un producto de datos es proporcionar el **nombre_interno** o la URL del producto, que lleva a la página de descarga del producto.

## Tipos de productos

### Catálogo de corrimiento al rojo de referencia

En el contexto de PZ Server, los catálogos de corrimiento al rojo de referencia se definen como cualquier catálogo que contenga coordenadas ecuatoriales esféricas y mediciones de corrimiento al rojo (generalmente espectroscópico o corrimiento al rojo verdadero para simulaciones).

Columnas obligatorias:

* Ascensión recta [grados] - `float`
* Declinación [grados] - `float`
* Desplazamiento al rojo - `float`

Columna recomendada:

* Error de desplazamiento al rojo - `float`

Un Catálogo de Desplazamiento al Rojo de Referencia puede incluir datos de un único estudio espectroscópico o una combinación de datos de varias fuentes.

¡¡¡!!! Peligro "ATENCIÓN: Requisitos de la canalización"

Si se pretende utilizar el Catálogo de Redshift de Referencia como datos de entrada para los _Catálogos de Redshift Combinados_, aplicando la función de resolución de duplicados (consulte [detalles de la canalización aquí](./pz_server_crc.md)), se recomienda incluir las siguientes columnas:

* Indicador de calidad (asociar con **z_flag** en el paso 3 de la carga) - `integer`, `float` o `string` (el indicador de calidad original del catálogo fuente, si está disponible).

* Tipo de medición - `string` (p. ej., "s" para "espectroscópico", "g" para "grisma/prisma", "p" para "fotométrico", según lo adoptado en [SITCOMTN-154](https://sitcomtn-154.lsst.io/){:target="_blank"})

* Nombre de la encuesta (asociar con **survey** en el paso 3 de la carga) - `string` (p. ej., "DESI", "COSMOS2025", "JADES", etc.)
* Otras columnas con información adicional que desee utilizar para la resolución de duplicados (p. ej., resolución del instrumento).

### Conjunto de Entrenamiento

En el contexto del Servidor PZ, los Conjuntos de Entrenamiento se definen como el producto del cruce espacial entre un Catálogo de Desplazamiento al Rojo de Referencia (levantamiento individual o compilación) y los datos fotométricos, en este caso, el Catálogo de Objetos LSST. El flujo de trabajo *Generador de Conjuntos de Entrenamiento* del Servidor Photo-z permite a los usuarios crear Conjuntos de Entrenamiento personalizados basados en los Catálogos de Desplazamiento al Rojo de Referencia disponibles (consulte [detalles del flujo de trabajo aquí](./pz_server_tsm.md)).

¡Información sobre "subconjuntos de entrenamiento/prueba"!

Los conjuntos de entrenamiento se suelen dividir en dos o más subconjuntos para la validación de Photo-z. Si el propietario del conjunto de entrenamiento ha definido previamente qué objetos deben pertenecer a cada subconjunto (conjuntos de entrenamiento y validación/prueba), esta información debe estar disponible como una columna adicional en la tabla o como instrucciones claras para reproducir la separación de los subconjuntos en la descripción del producto de datos. Para dos archivos separados, cada uno debe cargarse por separado y se convertirá en un producto de datos independiente, ambos con el tipo de producto "Conjunto de entrenamiento".

¡Información "conjuntos de entrenamiento basados en imágenes"!

El servidor PZ solo admite conjuntos de entrenamiento a nivel de catálogo. Los conjuntos de entrenamiento basados en imágenes, por ejemplo, para algoritmos de aprendizaje profundo, no son compatibles. En este caso, utilice el tipo de producto "Otro" y proporcione una descripción clara del formato de los datos en la descripción del producto.

Para garantizar la flexibilidad en los observables, la única columna obligatoria es el corrimiento al rojo ("float"). Otras columnas esperadas sone:

* ID de objeto del Catálogo de Objetos LSST - `integer`
* Observables: magnitudes (y/o colores, o flujos) del Catálogo de Objetos LSST - `float`
* Errores observables: errores de magnitud (y/o errores de color, o errores de flujo) del Catálogo de Objetos LSST - `float`
* Ascensión recta [grados] - `float`
* Declinación [grados] - `float`
* Indicador de calidad - `integer`, `float` o `string`
* Indicador de subconjunto - `integer`, `float` o `string`

### Resultados de Entrenamiento

Los resultados de entrenamiento de los algoritmos PZ basados en aprendizaje automático también se pueden alojar en el Servidor PZ para su uso compartido y reutilización. Este tipo de producto permite archivos en formato libre. Cuando los resultados de entrenamiento se generan con el método `inform` de RAIL (https://rail-hub.readthedocs.io/en/latest/source/overview.html#estimation), se almacenan como archivos *pickle*.

### Resultados de Validación

El tipo de producto "Resultados de Validación" está diseñado para etiquetar los resultados de cualquier procedimiento de validación de foto-z. Puede utilizarse para almacenar los resultados de la Cooperativa de Validación PZ o cualquier otra tarea de validación.

Este tipo de producto es bastante genérico. Puede contener estimaciones de foto-z (estimaciones individuales o PDF) de un conjunto de prueba, métricas de validación de foto-z, gráficos QQ-PIT, etc. Los usuarios pueden cargar un archivo principal y una lista de archivos auxiliares en cualquier formato.

### Estimaciones de Photo-z

Las estimaciones de Photo-z son el resultado de un procedimiento de estimación de Photo-z, generalmente la salida del módulo [método `estimate` de RAIL](https://rail-hub.readthedocs.io/en/latest/source/overview.html#estimation). Si los datos superan el límite de carga de archivos (200 MB), la entrada del producto solo almacena los metadatos y se deben proporcionar instrucciones para acceder a ellos en el campo de descripción.

### Otros

Cualquier otro producto de datos que no se ajuste a las categorías anteriores se puede cargar como producto de tipo "Otro". Este es un tipo de producto genérico que permite a los usuarios cargar cualquier formato de archivo y proporcionar una descripción del producto de datos en el campo de descripción.

## API del servidor de Photo-z

El servidor de Photo-z también ofrece una API como paquete de Python para facilitar el acceso a datos y metadatos mediante la línea de comandos. La API contiene funciones para explorar los productos de datos disponibles, recuperar el contenido de un producto de datos determinado para trabajar en la memoria o descargar los archivos de interés.

El paquete de Python `pzserver` es de código abierto y está disponible en [GitHub](https://github.com/linea-it/pzserver){:target="_blank"}. Se puede instalar mediante pip con:

```bash
pip install pzserver
```

### Cuaderno de tutoriales

Un cuaderno de tutoriales [https://github.com/linea-it/pzserver/blob/main/docs/notebooks/pzserver_tutorial.ipynb] con ejemplos de todos los métodos de `pzserver` está disponible en el repositorio de la biblioteca [`pzserver` en GitHub](https://github.com/linea-it/pzserver). También está disponible la página de documentación de la API de Photo-z Server [https://linea-it.github.io/pzserver){:target="_blank"} con más detalles para desarrolladores.

### Token de acceso

Una vez instalada e importada en un entorno Python, la clase `PzServer` abre la conexión remota a la base de datos de PZ Server.

```python
from pzserver import PzServer
pz_server = PzServer(token="<pegue aquí su token de acceso>")
```

Se requiere un token de acceso para la autenticación. Los usuarios pueden generarlo en el sitio web de PZ Server (menú de la esquina superior derecha de la página de inicio).

<img src="../../images/pz-server-token-menu.png" width=200pt align="top"/> <img src="../../images/pz-server-generate-token.png" width=350pt />

### Comandos básicos

Comandos básicos para mostrar datos y metadatos en una celda de Jupyter Notebook (si no está en una Jupyter Notebook, reemplace `display` por `get` para devolver los resultados como diccionarios de Python):

```python
pz_server.display_product_types()
```

```python
pz_server.display_releases()
```

```python
pz_server.display_products_list()
```

```python
pz_server.display_products_list(filters={"release": "DP1",
"product_type": "Training Set"})
```

```python
search_results = pz_server.get_products_list(filters={"product_type": "results"})
```

```python
pz_server.display_product_metadata(<product_id>)
```

Comandos básicos para descargar o recuperar datos en memoria:

```python
pz_server.download_product(<product_id>, save_in=".")
```

```
data = pz_server.get_product(<product_id>)
```

Consulta el [tutorial] notebook](https://github.com/linea-it/pzserver/blob/main/docs/notebooks/pzserver_tutorial.ipynb) para obtener la lista completa de ejemplos, incluyendo instrucciones para cargar y modificar productos de datos mediante la biblioteca `pzserver`.

## Canalizaciones de Photo-z Server

Las canalizaciones de Photo-z Server son un conjunto de herramientas que ayudan a los usuarios a crear y gestionar productos de datos. Las canalizaciones están disponibles en la página de Canalizaciones del sitio web de Photo-z Server.


[<font size=4>Combinar catálogos de Redshift</font>](./crc.md)


[<font size=4>Creador de conjuntos de entrenamiento</font>](./tsm.md)

## Agradecimientos

_El servidor Photo-z utiliza cálculosl recursos del IDAC-Brasil en el Laboratório Interinstitucional de e-Astronomia (LIneA) con apoyo financiero del INCT do e-Universo (Proceso nº 465376/2014-2) y del proyecto FINEP: LIneA: Centro de e-Ciencia para la exploración de los misterios del Universo y apoyo a proyectos de Big Data (ref nº 0883/24)._