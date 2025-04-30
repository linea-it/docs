# Open OnDemand

Open OnDemand es una interfaz que facilita el uso de nuestro entorno HPC conformado por el Cluster Apollo. Para acceder es necesario tener una cuenta válida en LIneA ([más información](../../primeiros_passos.html)). 

El acceso a Open OnDemand es a través de https://ondemand.linea.org.br/.

En la [**pantalla inicial**](../img/OOD1.png) de la plataforma, en la parte superior, se puede visualizar un menú con los siguientes elementos:

* **Files** - proporciona una interfaz para tu directorio de usuario (_Home Directory_).
* **Jobs** - proporciona interfaces para las pantallas "Active Jobs" y "Job Composer".
* **Clusters** - proporciona acceso al terminal en un navegador web.
* **Interactive Apps** - proporciona acceso a Jupyter Notebook.

## Home Directory

El [**Home Directory**](../img/OOD2.jpeg) permite visualizar el directorio de usuario donde se almacenan tus archivos, además de mostrar varios botones con diferentes funcionalidades.

#### Cambiando de Directorio

Al hacer clic en el botón [**Change Directory**](../img/OOD3.png) puedes navegar por nuestra infraestructura. Solo escribe la ruta en el campo **Path** y luego presiona "**ok**".

#### Accediendo al Terminal

Al hacer clic en el botón [**Open Terminal**](../img/OOD4.png) accederás al terminal Linux en la máquina de login (loginapl01) del Cluster Apollo.
En este terminal, te encuentras en tu directorio "_Home_" de usuario y puedes visualizar tus archivos. También puedes ejecutar todas las operaciones habituales de usuario HPC, como asignar nodos de computación y verificar la cola Slurm usando comandos.

#### Creando, Transfiriendo y Moviendo Archivos

En Open OnDemand, crear nuevos archivos y directorios es muy sencillo, así como transferirlos.

* Haz clic en los botones **"New File"** o **"New Directory"** y elige el nombre que deseas para el nuevo archivo o directorio.
* El botón **"Upload"** te permite transferir archivos desde tu máquina local a tu directorio _home_ en nuestro entorno.
* El botón **"Download"** te permite enviar los archivos seleccionados a tu máquina local.
Para visualizar, renombrar o **editar** un archivo creado, haz clic en los **_"tres puntos"_** que aparecen junto al archivo para ver un menú con estas opciones.
Para [**mover** o **copiar**](../img/OOD5.png) archivos sigue estos pasos:
1. Selecciona el archivo deseado
2. Haz clic en **"Copy/Move"**
3. Haz clic en **"Change Directory"** y escribe la ruta del directorio destino
4. Haz clic en "Copy" o "Move" en el cuadro que aparece en el lado izquierdo de la pantalla.

## Jobs

En la sección [**Jobs**](../img/OOD6.png) del menú principal, se encuentran las opciones "Job Composer" y "Active Jobs". La pantalla "Job Composer" facilita el proceso de envío de jobs y en [**"Active Jobs"**](../img/OOD9.png)] puedes monitorear la ejecución de tu Job con detalles.

Para enviar un job necesitas usar un script de envío como este: ([más información](../../processamento/apollo/index.html#anatomia-de-um-job))

```bash
#!/bin/bash
#SBATCH -p PARTICIÓN                       #Nombre de la Partición a usar
#SBATCH --nodelist=NODO                    #Nombre del Nodo a asignar
#SBATCH -J simple-job                      #Nombre del Job
#----------------------------------------------------------------------------#
##ruta al código ejecutable
EXEC=/lustre/t0/scratch/users/USUARIO/ondemand/projects/CODIGO.EJECUTABLE
srun $EXEC
```
[**Para ver más plantillas de scripts de envío de Jobs, haz clic aquí**](../../processamento/uso/templates-jobs.html)

### Job Composer

Open OnDemand simplifica todo el proceso de envío de jobs mediante la herramienta [Job Composer](../img/OOD7.png). Solo sigue estos pasos:

1. Haz clic en el botón [**"New Job"**](../img/OOD6.1.png)
2. Elige la opción **"Default Template"**
3. Edita las especificaciones del script de envío en **"Open Editor"**
4. Haz clic en **"Submit"** para que el Job comience su ejecución.

!!! warning "Aviso Importante"
    Los nodos de computación del cluster no tienen acceso a tu directorio de usuario (Home Directory). Mueve o copia todos los archivos necesarios para el envío de tu job a tu directorio SCRATCH.

## JupyterLab

Con Open OnDemand, es posible acceder a Jupyter Notebook en nuestro entorno HPC. A través de "Interactive Apps", Jupyter Notebook iniciará una sesión en uno de los nodos de computación del cluster, para ello:

1. Haz clic en ["Jupyter Notebook"](../img/OOD8.png)
2. Completa los campos "Account", "Partition" y "Select node"
3. Presiona el botón ["Launch"](../img/OOD10.png)
4. Conéctate a [JupyterLab](../img/OOD12.png) haciendo clic en [**"Connect to Jupyter"**](../img/OOD11.png).

!!! warning "Jupyter en Kubernetes VS Jupyter en HPC"
    Tenemos dos entornos Jupyter en infraestructuras distintas. Uno disponible en [https://jupyter.linea.org.br](https://jupyter.linea.org.br) que se ejecuta sobre [Kubernetes](https://kubernetes.io/). El otro, mencionado arriba, se ejecuta de forma interactiva mediante Open OnDemand directamente en los nodos de procesamiento.

Al abrir un [**terminal dentro de JupyterLab**](../img/OOD14.png) vía Open OnDemand, podrás ver que estás ubicado en un nodo del Cluster Apollo y tienes acceso a tu área "_scratch_" en el almacenamiento Lustre.

### Creando Kernels Python

**Los siguientes comandos deben ejecutarse en el terminal en "LIneA Shell Access"**. <br> Para acceder:

* En la página inicial de Open OnDemand, haz clic en: **Clusters -> LIneA Shell Access**.

Sigue estos comandos:

1. Ve a tu área SCRATCH, instala y carga Miniconda:

```bash
    cd $SCRATCH
    curl -L -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    chmod +x Miniconda3-latest-Linux-x86_64.sh
    ./Miniconda3-latest-Linux-x86_64.sh -p $SCRATCH/miniconda
    
    source miniconda/bin/activate 
    conda deactivate #Necesario para desactivar el env "base"
```

2. Crea, activa un venv conda e instala _ipykernel_:

```bash
    conda create -p $SCRATCH/kernelname
    conda activate kernelname/
    
    conda install -c anaconda ipykernel
```

3. Configura JUPYTER_PATH (debe ser exactamente esta ruta):

```bash
    JUPYTER_PATH=$SCRATCH/.local
    echo $JUPYTER_PATH
    
    python -m ipykernel install --prefix=$JUPYTER_PATH --name 'kernelname'
```

4. Abre una sesión de Jupyter Notebook.

???+ success "La salida del último comando debe ser:"

    ````yaml
    #[InstallIPythonKernelSpecApp] WARNING | Installing to /lustre/t0/scratch/users/USUARIO/.local/share/jupyter/kernels, which is not in ['/lustre/t0/scratch/users/USUARIO/kernelname/share/jupyter/kernels', '/home/USUARIO/.local/share/jupyter/kernels', '/usr/local/share/jupyter/kernels', '/usr/share/jupyter/kernels', '/home/USUARIO/.ipython/kernels']. The kernelspec may not be found.
    Installed kernelspec kernelname in /lustre/t0/scratch/users/USUARIO/.local/share/jupyter/kernels/kernelname
    ````

Al finalizar estos comandos, podrás ver el botón del kernel python creado.

## Videos tutoriales

* [Cómo enviar un Job](https://youtu.be/6w1H3VS40Ew)
* [Cómo acceder a Jupyter Notebook](https://youtu.be/SemHNDr8vjg)

Para cualquier duda, contacta al [Service Desk](../../suporte.html).
