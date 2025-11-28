

[JupyterHub](https://jupyter.org/hub) es un entorno de desarrollo multiusuario basado en *iPython Notebooks* que ofrece acceso a recursos computacionales compartidos en un servidor remoto, sin necesidad de instalación ni mantenimiento por parte de los usuarios. Los únicos prerrequisitos para acceder a JupyterHub son: disponer de una cuenta de usuario en LIneA (consulte [aquí](../primeiros_passos.md) cómo crear su cuenta) y un navegador con acceso a Internet. Los denominados [Jupyter Notebooks](https://docs.jupyter.org/en/latest/) permiten combinar código interactivo, resultados de ejecución, texto explicativo y recursos multimedia en un único documento.

Como parte de *LIneA Science Platform*, el LIneA JupyterHub está integrado con las demás herramientas de visualización ([*Science Server*](./sci_server.md)) y acceso a datos ([*User Query*](./user_query.md)). De este modo, todo el análisis de datos puede realizarse *en línea* dentro de la plataforma, desde la lectura, visualización, procesamiento hasta el análisis de resultados, sin necesidad de *descargar* los datos.

Al hacer clic en la tarjeta «JupyterHub» dentro de la LIneA Science Platform, será dirigido a la página de inicio de sesión y posteriormente a la página principal del JupyterHub, que mostrará su perfil de usuario. Haga clic en el botón **START** para iniciar.

La instalación estándar de JupyterHub utiliza la nueva interfaz [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) y se basa en la imagen [datascience-notebook](https://github.com/jupyter/docker-stacks). Esto significa que diversas bibliotecas *Python* de gran popularidad, como [Astropy](https://www.astropy.org/), [Numpy](https://numpy.org/) y [Matplotlib](https://matplotlib.org/), estarán automáticamente disponibles.


## Apoyo al usuario

### Tutoriales en Jupyter Notebooks

En el repositorio [jupyterhub-tutorial](https://github.com/linea-it/jupyterhub-tutorial) encontrará los tutoriales en formato *notebook*:

#### 1-primeiros-passos.ipynb
Instrucciones generales de uso de la plataforma JupyterLab, consejos y atajos para la redacción de *notebooks* para diferentes tipos de celdas.
#### 2-acesso-a-dados.ipynb
Instrucciones para el uso de la biblioteca dblinea para la lectura de datos a partir de la base de datos directamente desde una celda del *notebook*, con ejemplo de uso (construcción de un diagrama color-magnitud simple para una muestra de estrellas).
#### 3-conda-env.ipynb
Instrucciones para la creación de entornos en conda para la gestión de bibliotecas que sean persistentes y sobrevivan a la destrucción y recreación de los contenedores, de modo que los usuarios puedan regresar en una nueva sesión y encontrar el mismo entorno de la sesión anterior (no disponible para usuarios de perfil público bronce).

Para acceder a los *notebooks*, basta con abrir un Terminal en JupyterLab haciendo clic en el botón «+» en la barra superior y posteriormente en el icono «Terminal» de la sección «Other» en la pestaña «Launcher», e introducir el comando:

    git clone https://github.com/linea-it/jupyterhub-tutorial.git

<!-- 
### Minicursos

  1. Curso 1Los videos de las clases están disponibles en la página del [Minicurso Jupyter Notebook](https://classroom.google.com/c/NDkzMTA0MzEyODA1/m/NDcyNjUyMTg5Mjc1/details) en Google Classroom. -->

## Recursos computacionales

**Configuraciones disponibles para Jupyter over K8S**

Tras iniciar sesión en la plataforma, se mostrará un menú con hasta tres opciones de configuración. Basta con seleccionar y hacer clic en *Start My Server*.

| **Tamaño** | **CPUs** |  **RAM**   |
|---------|------|--------|
| **Small**   | 1.0  |  4 GiB |
| **Medium**  | 2.0  |  8 GiB |
| **Large**   | 4.0  | 16 GiB |


**Configuración de los servidores del entorno K8S**

La plataforma Jupyter se ejecuta sobre Kubernetes (K8S) y dispone de 12 servidores físicos dedicados. Cada máquina está equipada con los siguientes recursos computacionales:

|   Kubernetes Node Configuration  ||
| ----------------------- | ------- |
| **RAM**                 | 64 GB |
| **Thread(s) per core**  | 2   |
| **Core(s) per socket**  | 6   |
| **Socket(s)**           | 2   | 


!!! info "Jupyter over K8S vs Jupyter over HPC"
	 LIneA ofrece dos entornos separados de Jupyter Notebook. El primero se ejecuta en contenedores en la plataforma Kubernetes (K8S). El segundo está disponible en la [plataforma Ondemand](../processamento/uso/openondemand.md) y accede directamente a la infraestructura de HPC.
