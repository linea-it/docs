El **Target Viewer** ([**targetviewer.linea.org.br**](https://targetviewer.linea.org.br)) es un servicio en desarrollo personalizado para visualizar imágenes astronómicas basadas en una lista de objetivos predefinida por el usuario.

**Paso a paso:**
1. Cree una nueva lista de objetivos como tabla en el espacio *MyDB* del usuario en **User Query**. Para que el Target Viewer pueda localizar las imágenes de los objetivos, la tabla debe contener las columnas `objectId`, `ra` y `dec`.
2. Haga clic en el botón **"NEW CATALOG"** en la página principal del Target Viewer y complete el formulario de 3 pasos para registrar la tabla como una lista de objetivos.
3. Después de enviar el formulario, la lista de objetivos aparecerá en el menú de la página principal.

Al igual que el Sky Viewer, el Target Viewer fue desarrollado originalmente para el relevamiento DES, y su versión inicial sigue disponible en la página del [DES Science Server](https://scienceserver.linea.org.br/), ofreciendo acceso a imágenes y datos tabulares del DES DR2. La nueva versión, que se está desarrollando como parte del portafolio de servicios de IDAC-Brasil, será flexible y proporcionará acceso a datos del LSST para miembros, además de datos públicos de otros relevamientos para el público en general.
