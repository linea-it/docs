# Open OnDemand

El *Open OnDemand* es una interfaz que facilita el uso de nuestro entorno HPC conformado por el *Cluster Apollo*. Para acceder es necesario tener una cuenta válida en LIneA ([más información](../../primeiros_passos.md)). 

El acceso al *Open OnDemand* es a través de https://ondemand.linea.org.br/.

En la [**pantalla inicial**](../img/OOD1.png) de la plataforma, en la parte superior, se puede visualizar un menú con los siguientes elementos:

* **Files** - proporciona una interfaz para su directorio de usuario (_Home Directory_).
* **Jobs** - proporciona interfaces para las pantallas "Active Jobs" y "Job Composer".
* **Clusters** - proporciona acceso al terminal en un navegador web.
* **Interactive Apps** - proporciona acceso a *Jupyter Notebook*.

## Home Directory

El [**Home Directory**](../img/OOD2.jpeg) permite visualizar el directorio de usuario donde se almacenan sus archivos, además de mostrar varios botones con diferentes funcionalidades.

#### Cambiando de Directorio

Al hacer clic en el botón [**Change Directory**](../img/OOD3.png) puede navegar por nuestra infraestructura. Solo escriba la ruta en el campo **Path** y luego presione "**ok**".

#### Accediendo al Terminal

Al hacer clic en el botón [**Open Terminal**](../img/OOD4.png) accederá al terminal Linux en la máquina de login (loginapl01) del *Cluster Apollo*.
En este terminal, se encuentra en su directorio "_Home_" de usuario y puede visualizar sus archivos. También puede ejecutar todas las operaciones habituales de usuario HPC, como asignar nodos de computación y verificar la cola de *Slurm* mediante comandos.

#### Creando, Transfiriendo y Moviendo Archivos

En el *Open OnDemand*, crear nuevos archivos y directorios es muy sencillo, así como transferirlos.

* Haga clic en los botones **"New File"** o **"New Directory"** y elija el nombre que desea para el nuevo archivo o directorio.
* El botón **"Upload"** le permite transferir archivos desde su computadora local a su directorio _home_ en nuestro entorno.
* El botón **"Download"** le permite enviar los archivos seleccionados a su computadora local.
Para visualizar, renombrar o **editar** un archivo creado, haga clic en los **_"tres puntos"_** que aparecen junto al archivo para ver un menú con estas opciones.
Para [**mover** o **copiar**](../img/OOD5.png) archivos, siga estos pasos:
1. Seleccione el archivo deseado
2. Haga clic en **"Copy/Move"**
3. Haga clic en **"Change Directory"** y escriba la ruta del directorio destino
4. Haga clic en "Copy" o "Move" en el cuadro que aparece en el lado izquierdo de la pantalla.

## Jobs

En la sección [**Jobs**](../img/OOD6.png) del menú principal, se encuentran las opciones "Job Composer" y "Active Jobs". La pantalla "Job Composer" facilita el proceso de envío de *jobs* y en [**"Active Jobs"**](../img/OOD9.png) puede monitorear la ejecución de su *Job* con detalles.

Para enviar un *job* es necesario usar un script de envío como este: ([más información](../../processamento/apollo/index.md#anatomia-de-un-job))

```bash
#!/bin/bash
#SBATCH -p PARTICIÓN                       #Nombre de la Partición a usar
#SBATCH --nodelist=NODO                    #Nombre del Nodo a asignar
#SBATCH -J simple-job                      #Nombre del Job
#----------------------------------------------------------------------------#

##ruta al código ejecutable

EXEC=/scripts/USUARIO/CODIGO.EJECUTABLE

srun $EXEC
```
[**Para ver más plantillas de scripts de envío de *Jobs*, haga clic aquí**](./templates-jobs.md)

### Job Composer

El *Open OnDemand* simplifica todo el proceso de envío de *jobs* mediante la herramienta [Job Composer](../img/OOD7.png). Solo siga estos pasos:

1. Haga clic en el botón [**"New Job"**](../img/OOD6.1.png)
2. Elija la opción **"Default Template"**
3. Edite las especificaciones del script de envío en **"Open Editor"**
4. Haga clic en **"Submit"** para que el *Job* comience su ejecución.

!!! warning "Aviso Importante"
    Los nodos de computación del cluster no tienen acceso a su directorio de usuario (Home Directory). Mueva o copie todos los archivos necesarios para el envío de su *job* a su directorio SCRATCH.

## JupyterLab

Con el *Open OnDemand*, es posible acceder al *Jupyter Notebook* en nuestro entorno HPC. A través de "Interactive Apps", *Jupyter Notebook* iniciará una sesión en uno de los nodos de computación del cluster. Para ello:

1. Haga clic en ["Jupyter Notebook"](../img/OOD8.png)
2. Complete los campos "Account", "Partition" y "Select node"
3. Presione el botón ["Launch"](../img/OOD10.png)
4. Conéctese a [*JupyterLab*](../img/OOD12.png) haciendo clic en [**"Connect to Jupyter"**](../img/OOD11.png).

!!! warning "Jupyter en Kubernetes VS Jupyter en HPC"
    Tenemos dos entornos Jupyter en infraestructuras distintas. Uno disponible en [https://jupyter.linea.org.br](https://jupyter.linea.org.br) que se ejecuta sobre [Kubernetes](https://kubernetes.io/). El otro, mencionado arriba, se ejecuta de forma interactiva mediante Open OnDemand directamente en los nodos de procesamiento.

Al abrir un [**terminal dentro de *JupyterLab***](../img/OOD14.png) vía *Open OnDemand*, podrá ver que está ubicado en un nodo del *Cluster Apollo* y tiene acceso a su área "_scratch_" en el almacenamiento *Lustre*.

### Creando Kernels Python

**Los siguientes comandos deben ejecutarse en el terminal en "LIneA Shell Access"**. <br> Para acceder:

* En la página inicial del *Open OnDemand*, haga clic en: **Clusters -> LIneA Shell Access**.

Siga estos comandos:

1. Vaya a su área SCRIPTS, cree y active un venv *conda* e instale _ipykernel_:
```bash

    cd $SCRIPTS

    conda create -p $SCRIPTS/kernelname
    conda activate kernelname/
    
    conda install -c anaconda ipykernel
```

2. Configure JUPYTER_PATH (debe ser exactamente esta ruta):
```bash
    JUPYTER_PATH=$SCRATCH/.local
    echo $JUPYTER_PATH
    
    python -m ipykernel install --prefix=$JUPYTER_PATH --name 'kernelname'
```

3. Abra una sesión de *Jupyter Notebook*.

???+ success "La salida del último comando debe ser:"

    ````yaml
    #[InstallIPythonKernelSpecApp] WARNING | Installing to /scratch/users/USUARIO/.local/share/jupyter/kernels, which is not in ['/scratch/users/USUARIO/kernelname/share/jupyter/kernels', '/home/USUARIO/.local/share/jupyter/kernels', '/usr/local/share/jupyter/kernels', '/usr/share/jupyter/kernels', '/home/USUARIO/.ipython/kernels']. The kernelspec may not be found.
    Installed kernelspec kernelname in /scratch/users/USUARIO/.local/share/jupyter/kernels/kernelname
    ````

Al finalizar estos comandos, podrá ver el botón del *kernel* de Python creado.

## Videos tutoriales

* [Cómo enviar un Job](https://youtu.be/6w1H3VS40Ew)
* [Cómo acceder a Jupyter Notebook](https://youtu.be/SemHNDr8vjg)
* [Cómo crear un kernel]()

Para cualquier duda, contacte al [Service Desk](../../suporte.md).
