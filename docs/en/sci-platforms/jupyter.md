[JupyterHub](https://jupyter.org/hub) is a multi-user development environment based on _iPython Notebooks_ that provides access to shared computing resources on a remote server, without requiring installation or maintenance by users. The only prerequisites to access JupyterHub are: having a LIneA user account (see [here](../primeiros_passos.md) how to create your account) and a web browser with internet access. So-called [Jupyter Notebooks](https://docs.jupyter.org/en/latest/) allow combining interactive code, execution results, explanatory text and multimedia resources in a single document.

As part of the [LIneA Science Platform](../lsp/index.md), the LIneA JupyterHub is integrated with other visualization tools ([Science Server](../lsp/sci_server.md)) and data access tools ([User Query](../lsp/user_query.md)). This way, all data analysis can be done _online_ within the platform, from reading, visualization, processing to result analysis, without needing to _download_ the data.

When clicking on the "JupyterHub" card within the LIneA Science Platform, you will be directed to the login page and then to the JupyterHub homepage showing your user profile. Click the **START** button to begin.

The standard JupyterHub installation uses the new [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) interface and is based on the [datascience-notebook](https://github.com/jupyter/docker-stacks) image, extended with the [Astropy](https://www.astropy.org/) and [dblinea](https://dblinea.readthedocs.io/en/latest/index.html) libraries (the library that connects to the database). This means several popular _Python_ libraries like [Numpy](https://numpy.org/) and [Matplotlib](https://matplotlib.org/) will be automatically available.

## User Support
### Tutorials in Jupyter Notebooks
In the [jupyterhub-tutorial](https://github.com/linea-it/jupyterhub-tutorial) repository you will find tutorials in _notebook_ format:

#### 1-primeiros-passos.ipynb
General instructions for using the JupyterLab platform, tips and shortcuts for writing notebooks for different cell types.
#### 2-acesso-a-dados.ipynb
Instructions for using the dblinea library to read data from the database directly from a notebook cell with usage examples (building a simple color-magnitude diagram for a star sample).
#### 3-conda-env.ipynb
Instructions for creating conda environments to manage libraries that persist and survive container destruction and recreation, allowing users to return in a new session and find the same environment as the previous session (not available for public bronze profile users).
***
To access the _notebooks_, simply open a Terminal in JupyterLab by clicking the "+" button in the top bar and then the "Terminal" icon in the "Other" section of the "Launcher" tab, and enter the command:

    git clone https://github.com/linea-it/jupyterhub-tutorial.git

## Computational Resources
**Available configurations for Jupyter over K8S**
After logging into the platform, a menu with up to three configuration options will be displayed. Simply select and click _Start My Server_.

| **Size** | **CPUs** |  **RAM**   |
|---------|------|--------|
| **Small**   | 1.0  |  4 GiB |
| **Medium**  | 2.0  |  8 GiB |
| **Large**   | 4.0  | 16 GiB |

**K8S environment server configuration**
The Jupyter platform runs on Kubernetes (K8S) and has 12 dedicated physical servers. Each machine is equipped with the following computational resources:

|   Kubernetes Node Configuration  ||
| ----------------------- | ------- |
| **RAM**                 | 64 GB |
| **Thread(s) per core**  | 2   |
| **Core(s) per socket**  | 6   |
| **Socket(s)**           | 2   | 

!!! info "Jupyter over K8S vs Jupyter over HPC"
     LIneA provides two separate Jupyter Notebook environments. The first runs in containers on the Kubernetes (K8S) platform. The second is available on the [Ondemand platform](../processamento/uso/openondemand.md) and directly accesses the HPC infrastructure.
