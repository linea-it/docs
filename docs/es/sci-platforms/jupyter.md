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

## Recursos Computacionales
**Configuraciones disponibles para Jupyter over K8S**
Después de iniciar sesión en la plataforma, se mostrará un menú con hasta tres opciones de configuración. Solo seleccione y haga clic en _Start My Server_.

| **Tamaño** | **CPUs** |  **RAM**   |
|---------|------|--------|
| **Small**   | 1.0  |  4 GiB |
| **Medium**  | 2.0  |  8 GiB |
| **Large**   | 4.0  | 16 GiB |

**Configuración de servidores del entorno K8S**
La plataforma Jupyter se ejecuta sobre Kubernetes (K8S) y cuenta con 12 servidores físicos dedicados. Cada máquina está equipada con los siguientes recursos computacionales:

|   Kubernetes Node Configuration  ||
| ----------------------- | ------- |
| **RAM**                 | 64 GB |
| **Thread(s) per core**  | 2   |
| **Core(s) per socket**  | 6   |
| **Socket(s)**           | 2   | 

!!! info "Jupyter over K8S vs Jupyter over HPC"
     LIneA ofrece dos entornos separados de Jupyter Notebook. El primero se ejecuta en contenedores en la plataforma Kubernetes (K8S). El segundo está disponible en la [plataforma Ondemand](../processamento/uso/openondemand.md) y accede directamente a la infraestructura HPC.
