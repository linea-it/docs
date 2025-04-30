# HPE Apollo 2000

El **Cluster Apollo** posee 28 nodos computacionales y ofrece un total de **1072 núcleos** físicos. Sus nodos están equipados con procesadores `Intel Xeon Skylake 5120 2.2GHz` (apl01-16) y `Intel Xeon Gold 5320 2.20GHz` (apl17-28). El conjunto de máquinas provee cerca de 85 Tflops de capacidad computacional.

Los 28 nodos computacionales del Cluster Apollo pertenecen a la familia de servidores HPE ProLiant, siendo 16 del modelo XL170r y 12 del modelo XL220n. Actualmente, el número de núcleos disponibles es de *2144*, ya que el HT está activo en los nodos de computación.

#### Características de cada servidor

| Modelo | # Núcleos (HT)<sup>[1]</sup> | RAM | SO | Hosts |
| ------- | ------| ------------ | -------- | -----------| 
| HPE Proliant XL170r  | 56    | 128 GB       | Rocky Linux 9.5 |  apl[01-16]  |
| HPE Proliant XL220n  | 104   | 256 GB       | Rocky Linux 9.5 |  apl[17-28]  |
<sup>[1]</sup> La tecnología Hyper-Threading (HT) está habilitada en todos los nodos del cluster.

**Características del cluster (consolidado)**

| # Nodos | # Núcleos | RAM total | Instalado en |
| ------- | ------| ------------ | -----------| 
| 16      | 896   | 1.5TB        |  Abr-2019  |
| 12      | 624   | 3TB          |  Jul-2023  |

El **Cluster Apollo** es gestionado por **Slurm v24.05.5**.

### Sistema de Archivos

El **Cluster Apollo** cuenta con un sistema de archivos de alto rendimiento Lustre, disponible como área de "Scratch". El "Home" de los usuarios está accesible únicamente en el nodo de login y se provee mediante NFS.
Estas áreas de almacenamiento deben utilizarse de la siguiente forma:

**Scratch:** Estructura montada desde `/scratch/<usuario>`. Utilizada para almacenar todos los archivos que serán usados durante la ejecución de un job (scripts de envío, ejecutables, datos de entrada, datos de salida, etc). Variable de entorno `$SCRATCH`.

**Home:** Estructura montada desde `/home/<usuario>`. Utilizada especialmente para almacenar resultados que se desee mantener durante toda la vigencia del proyecto. Variable de entorno `$HOME`.

**Scriptland:** Estructura montada desde `/scriptland/<usuario>`. Es un área de almacenamiento optimizada para scripts y códigos. Variable de entorno `$SCRIPTLAND`.

[Haga clic aquí para más detalles](../../armazenamento/index.html)

!!! warning "Atención"
    No olvide copiar los archivos necesarios (ejecutable, bibliotecas, datos de entrada) al área de SCRATCH, pues el área de HOMEDIR no es accesible por los nodos computacionales.

## Slurm

Slurm es un sistema de gestión de cluster y planificación de trabajos de código abierto, tolerante a fallos y altamente escalable para clusters Linux grandes y pequeños. Slurm no requiere modificaciones en el kernel para su operación y es relativamente independiente. Como gestor de carga de trabajo de cluster, Slurm tiene tres funciones principales:

 - Asignar acceso exclusivo y/o no exclusivo a recursos (nodos de computación) a usuarios por un período determinado
 - Ofrecer un marco para iniciar, ejecutar y monitorear trabajos (normalmente trabajos paralelos) en el conjunto de nodos asignados
 - Gestionar la cola de envío, arbitrando conflictos entre solicitudes de recursos computacionales

### Particiones disponibles

El cluster Apollo está organizado en diferentes particiones (subconjuntos de máquinas) para atender diversas necesidades, por ejemplo, garantizar la prioridad máxima de los usuarios del proyecto LSST en el uso de las máquinas dedicadas a IDAC-Brasil.

|PARTICIÓN   |LÍMITE TIEMPO  |NODES  |LISTA NODOS  |
|------------|-----------|-------|----------|
|cpu_dev     |30:00      |26     |apl[01-28]|
|cpu_small   |3-00:00:00 |26     |apl[01-28]|
|cpu         |5-00:00:00 |26     |apl[01-28]|
|cpu_long    |31-00:00:0 |26     |apl[01-28]|
|lsst_cpu_dev     |30:00      |12     |apl[17-28]|
|lsst_cpu_small   |3-00:00:00 |12     |apl[17-28]|
|lsst_cpu         |5-00:00:00 |12   |apl[17-28]|
|lsst_cpu_long    |10-00:00:0 |12     |apl[17-28]|

### Cuentas disponibles

- **Workflow** – Interrumpe cualquier job en ejecución: **hpc-photoz** (photoz)
- **LSST** – Próximo en la cola: **hpc-lsst** [solo en nuevas apollos apl[17-28]] (lsst)
- **Grupo A** - Prioridad Mayor: **hpc-bpglsst** (itteam, bpg-lsst)
- **Grupo B** - Prioridad Intermedia: **hpc-collab** (des, desi, sdss, tno)
- **Grupo C** - Prioridad Menor: **hpc-public** (linea-members)

Las particiones (**cpu_dev**, **cpu_small**, **cpu** y **cpu_long**) incluyen todas las apollos (*apl[01-28]*), mientras que las particiones del grupo LSST solo incluyen *apl[17-28]*. Solo la cuenta *hpc-lsst* puede enviar jobs a particiones con prefijo "lsst", que tienen mayor prioridad en los nodos.

!!! warning "Atención"
    Como parte del programa de contribución in-kind BRA-LIN, IDAC Brasil tiene el compromiso de generar redshifts fotométricos anualmente para el relevamiento LSST, siempre en el período previo a las liberaciones oficiales de datos. En estos períodos, el Cluster Apollo estará completamente ocupado para este propósito por un tiempo estimado de varias horas, pudiendo extenderse a varios días. Los usuarios serán informados con anticipación por correo sobre la indisponibilidad del cluster. [Haga clic aquí](https://linea-it.github.io/pz-lsst-inkind-doc/) para conocer más sobre la producción de redshifts fotométricos y el programa de contribución in-kind BRA-LIN.

### Anatomía de un Job

Un Job solicita recursos de computación y especifica las aplicaciones a iniciar en esos recursos, junto con cualquier dato/opción de entrada y directiva de salida. El usuario envía el trabajo, generalmente como un script de trabajo por lotes, al planificador por lotes.

El script de trabajo por lotes consta de cuatro componentes principales:

 - El intérprete usado para ejecutar el script
 - Directivas "#" que transmiten opciones de envío predeterminadas
 - Configuración de variables de entorno y/o script (si es necesario)
 - Las aplicaciones a ejecutar junto con sus argumentos y opciones de entrada

Aquí un ejemplo de script por lotes que solicita 3 nodos en la partición "cpu" e inicia 36 tareas de myApp en los 3 nodos asignados:

```bash
#!/bin/bash
#SBATCH -N 3
#SBATCH -p cpu
#SBATCH --ntasks 36
srun myApp
```

Cuando el trabajo esté programado para ejecución, el gestor de recursos ejecutará el script de trabajo por lotes en el primer nodo de la asignación.

#### Especificando Recursos

Slurm tiene su propia sintaxis para solicitar recursos computacionales. A continuación, una tabla resumen de algunos recursos solicitados frecuentemente y la sintaxis de Slurm para obtenerlos. Para la lista completa de sintaxis, ejecute el comando man sbatch.

|Sintaxis  |Significado|
|---------|-----------|
|#SBATCH -p partición  | Define la partición donde se ejecutará el job|
|#SBATCH -J nombre_job | Define el nombre del Job|
|#SBATCH -n cantidad | Define el número total de tareas de CPU|
|#SBATCH -N cantidad  | Define el número de nodos de computación solicitados|

#### Comandos Básicos de Slurm

Para aprender sobre todas las opciones disponibles para cada comando, ingrese man <comando> mientras esté conectado al entorno del Cluster.

|Comando	| Definición|
|-----------|----------|
|sbatch	| Envía scripts de trabajos a la cola de ejecución|
|scancel	| Cancela un job|
|scontrol	| Usado para mostrar el estado de Slurm (varias opciones disponibles solo para root)|
|sinfo	| Muestra estado de particiones y nodos|
|squeue	| Muestra estado de los jobs|
|salloc	| Envía un job para ejecución o inicia un trabajo en tiempo real|

#### Entorno de Ejecución

Para cada tipo de trabajo, el usuario puede definir el entorno de ejecución. Esto incluye configuraciones de variables de entorno así como límites de shell (`bash ulimit` o `csh limit`). **sbatch** y **salloc** proveen la opción `--export` para pasar variables de entorno específicas al entorno de ejecución. También tienen la opción `--propagate` para pasar límites específicos de shell al entorno de ejecución.

#### Variables de Entorno

La primera categoría son variables que Slurm inserta en el entorno de ejecución del trabajo. Transmiten al script del trabajo información como ID del job `(SLURM_JOB_ID)` e ID de la tarea `(SLURM_PROCID)`. Para la lista completa, consulte la sección `"OUTPUT ENVIRONMENT VARIABLES"` en las páginas [sbatch](https://slurm.schedmd.com/sbatch.html), [salloc](https://slurm.schedmd.com/salloc.html) y [srun](https://slurm.schedmd.com/srun.html).

La siguiente categoría son variables que el usuario puede definir en su entorno para pasar opciones predeterminadas a cada job enviado. Esto incluye opciones como el límite de tiempo. Para la lista completa, consulte la sección `"INPUT ENVIRONMENT VARIABLES"` en las páginas [sbatch](https://slurm.schedmd.com/sbatch.html), [salloc](https://slurm.schedmd.com/salloc.html) y [srun](https://slurm.schedmd.com/srun.html).

!!! info "¿Más Información?"
    Aprenda a utilizar el Cluster Apollo en [Cómo Utilizar](../../processamento/uso/howtouse-HPC.html). <br> 
    Para mayor información, contacte al [Service Desk](../../suporte.html).
