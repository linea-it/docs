[JupyterHub](https://jupyter.org/hub) es un entorno de desarrollo multiusuario basado en _iPython Notebooks_ que ofrece acceso a recursos computacionales compartidos en un servidor remoto, sin necesidad de instalación ni mantenimiento por parte de los usuarios. Los únicos requisitos para acceder a JupyterHub son: tener una cuenta de usuario en LIneA (consulte [aquí](../primeiros_passos.md) cómo crear su cuenta) y un navegador con acceso a internet. Los llamados [Jupyter Notebooks](https://docs.jupyter.org/en/latest/) permiten combinar código interactivo, resultados de ejecución, texto explicativo y recursos multimedia en un solo documento.

Como parte de la [Plataforma Científica LIneA](../lsp/index.md), el JupyterHub de LIneA está integrado con otras herramientas de visualización ([Science Server](../lsp/sci_server.md)) y acceso a datos ([User Query](../lsp/user_query.md)). De esta manera, todo el análisis de datos puede realizarse _en línea_ dentro de la plataforma, desde la lectura, visualización, procesamiento hasta el análisis de resultados, sin necesidad de _descargar_ los datos.

Al hacer clic en la tarjeta "JupyterHub" dentro de la Plataforma Científica LIneA, será dirigido a la página de inicio de sesión y luego a la página principal de JupyterHub que mostrará su perfil de usuario. Haga clic en el botón **START** para comenzar.

La instalación estándar de JupyterHub utiliza la nueva interfaz [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) y se basa en la imagen [datascience-notebook](https://github.com/jupyter/docker-stacks), extendida con las bibliotecas [Astropy](https://www.astropy.org/) y [dblinea](https://dblinea.readthedocs.io/en/latest/index.html) (la biblioteca que conecta con la base de datos). Esto significa que varias bibliotecas populares de _Python_ como [Numpy](https://numpy.org/) y [Matplotlib](https://matplotlib.org/) estarán automáticamente disponibles.

## Soporte al usuario
### Tutoriales en Jupyter Notebooks
En el repositorio [jupyterhub-tutorial](https://github.com/linea-it/jupyterhub-tutorial) encontrará tutoriales en formato _notebook_:

#### 1-primeiros-passos.ipynb
Instrucciones generales para usar la plataforma JupyterLab, consejos y atajos para escribir notebooks para diferentes tipos de celdas.
#### 2-acesso-a-dados.ipynb
Instrucciones para usar la biblioteca dblinea para leer datos directamente desde la base de datos dentro de una celda del notebook con ejemplos de uso (construcción de un diagrama color-magnitud simple para una muestra de estrellas).
#### 3-conda-env.ipynb
Instrucciones para crear entornos conda para gestionar bibliotecas que persistan y sobrevivan a la destrucción y recreación de contenedores, permitiendo a los usuarios volver en una nueva sesión y encontrar el mismo entorno que en la sesión anterior (no disponible para usuarios de perfil público bronce).
***
Para acceder a los _notebooks_, simplemente abra una Terminal en JupyterLab haciendo clic en el botón "+" en la barra superior y luego en el ícono "Terminal" en la sección "Other" de la pestaña "Launcher", e ingrese el comando:
    git clone https://github.com/linea-it/jupyterhub-tutorial.git

### Solicitud de Recursos
Inicialmente cada servidor Jupyter Notebook tiene un conjunto básico de recursos computacionales. Si necesita más recursos, puede solicitarlos contactando a nuestro [Service Desk](https://docs.linea.org.br/suporte.html) y enviando la siguiente información:

```
Núcleos (CPU):
Memoria (RAM):
Volumen de datos (entrada):
Volumen de datos (salida):
Resumen de su proyecto de investigación:
Breve justificación para los recursos solicitados:
```

Los valores proporcionados pueden ser estimaciones o aproximaciones.

Su solicitud será enviada al Comité de Gestión para evaluación, y le responderemos por correo electrónico. Si su solicitud es aprobada y los recursos son añadidos a su cuenta, tendrá 90 días para utilizarlos. Por favor, tenga en cuenta nuestra [política de uso](https://docs.linea.org.br/politicas.html#reconhecimento-de-uso-dos-recursos-computacionales-do-linea).

**Información de los Servidores del entorno K8S**

Un servidor Jupyter Notebook K8S puede utilizar como máximo una máquina. Nuestro entorno Kubernetes (K8S) tiene 14 máquinas, cada una equipada con los siguientes recursos, con hyper-threading habilitado:
| CPU(s):                 | 24      |
| ----------------------- | ------- |
| **Thread(s) per core:** | **2**   |
| **Core(s) per socket:** | **6**   |
| **Socket(s):**          | **2**   |
| **Memoria (RAM):**      | **94G** |
