El *Target Viewer* ([**targetviewer.linea.org.br**](https://targetviewer.linea.org.br)) es un servicio, en fase de desarrollo, personalizado para la visualización de imágenes astronómicas con base en una lista de objetos objetivo previamente definida por el usuario.

**Paso a paso:**

1. Cree una nueva lista de objetivos como una tabla en el espacio *MyDB* del usuario en *User Query*. Para que el *Target Viewer* pueda localizar las imágenes de los objetivos, la tabla debe contener las columnas `objectId`, `ra` y `dec`.
2. Haga clic en el botón **«NEW CATALOG»** en la página principal del *Target Viewer* y complete el formulario de 3 pasos para registrar la tabla como una lista de objetivos.
3. Tras el envío del formulario, la lista de objetivos aparecerá en el menú de la página principal.

De manera análoga al *Sky Viewer*, el *Target Viewer* fue originalmente desarrollado para el relevamiento DES, y su versión inicial continúa disponible en la página de [*DES Science Server*](https://scienceserver.linea.org.br/), ofreciendo acceso a imágenes y datos tabulares de *DES* DR2. La nueva versión, que se está desarrollando para componer el portafolio de servicios de IDAC-Brasil, será flexible y ofrecerá acceso a los datos de LSST para miembros, además de datos públicos de otros relevamientos para el público en general.
