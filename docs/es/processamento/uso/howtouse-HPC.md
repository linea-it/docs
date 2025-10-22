# Cómo Utilizar

## Cómo Acceder

El acceso a nuestro clúster puede realizarse a través de [**Open OnDemand**](../../processamento/uso/openondemand.html) o mediante el Terminal de **JupyterLab (K8S)**. En ambas opciones, es imprescindible poseer una cuenta válida en el entorno computacional de LIneA. Si no tiene una cuenta, contacte al Service Desk por correo (helpdesk@linea.org.br) para más información.

!!! warning "Atención"
    Aún teniendo una cuenta activa en LIneA, el acceso al entorno de procesamiento HPC no es automático. Para más información contacte al Service Desk en helpdesk@linea.org.br.

**Accediendo por terminal de JupyterLab**

En la [**pantalla inicial**](../img/tela-jupyter.png) de su Jupyter Notebook, en la sección **_"Other"_**, encontrará el botón del terminal. Al hacer clic en él, será redirigido a un terminal Linux, inicialmente ubicado en su directorio _home_. Para acceder al Cluster Apollo, simplemente ejecute el siguiente comando:

  ```bash
    ssh loginapl01
  ```
La máquina <font color="#172b4d">**_loginapl01_**</font> es donde podrá asignar nodos de computación para enviar su job.

!!! warning "$HOME y $SCRATCH"
    Los nodos de computación no tienen acceso a su directorio _home_ de usuario. Mueva o copie todos los archivos necesarios para el envío de su job a su directorio SCRATCH.

## Cómo usar el área SCRATCH

Su directorio de `SCRATCH` es el lugar donde puede dirigir los archivos de resultados de su trabajo, así como almacenar datos **temporalmente** que el código utiliza en el momento del procesamiento.

- **Para acceder a su directorio SCRATCH:**

```bash
   cd $SCRATCH
``` 

- **Para enviar archivos a su directorio SCRATCH:**

```bash
   cp <ARCHIVO> $SCRATCH
```

#### Limpieza automática del Scratch
**Scratch** es un área de almacenamiento temporal para los archivos de salida y procesamiento de trabajos realizados en el clúster. Para mantener el medio ambiente organizado y garantizar el espacio disponible para todos, está vigente un **script de limpieza automático**, que funciona **una vez por semana**.

Este proceso elimina los **archivos a los que no se ha accedido dentro del período establecido de retención**, actualmente **45 días**.

Los archivos de configuración esenciales (por ejemplo, `.bashrc`, `.bash_profile`, `.ssh`, etc.) se conservan automáticamente y **no ingresan el proceso de exclusión**.

!!! danger "Atención"
	El scratch no debe usarse para el almacenamiento permanente. Recomendamos mover datos importantes a su directorio **home**.

## Cómo usar el área de script
Su directorio de `SCRIPTS` es el lugar donde puede almacenar scripts, códigos para ejecutarse en el clúster. También se recomienda utilizar esta área para crear un entorno de conda.

**Para acceder a su directorio de SCRIPTS:**

```bash
  cd $SCRIPTS
```

!!! warning "Ser Consciente"
	El área de script no está incluida en la rutina de backups. Por lo tanto, no debe usarse como almacenamiento de datos permanente.

## Cómo Enviar un Job

Un Job solicita recursos de computación y especifica las aplicaciones a iniciar en esos recursos, junto con cualquier dato/opción de entrada y directiva de salida. La gestión y programación de tareas y recursos del clúster se realiza a través de Slurm. Por lo tanto, para enviar un Job necesita usar un script como el siguiente:

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

En este script debe especificar: el **nombre de la cola (Partition)** a usar; el **nombre del nodo** a asignar para la ejecución del Job; y la **ruta al código/programa** a ejecutar.

!!! danger "ADVERTENCIA"
     **Está estrictamente prohibido enviar _jobs_ directamente a la máquina _loginapl01_. Cualquier código ejecutándose en esta máquina será interrumpido inmediatamente sin previo aviso.**

- **Para enviar el Job:**

```bash
 sbatch script-submit-job.sh
```

Si el script es correcto **habrá una salida que indica el ID del job**.

- **Para verificar el progreso e información del Job:**

```bash
   scontrol show job <ID> 
```

- **Para cancelar el Job:**

```bash
   scancel <ID> 
```

!!! warning "Acceso a internet"
    Los nodos de computación **no** tienen acceso a internet. Paquetes y bibliotecas deben instalarse desde _loginapl01_ en su área scratch.

## Gestor de paquetes EUPS

[EUPS](https://github.com/RobertLuptonTheGood/eups) es un gestor de paquetes alternativo (y oficial de LSST) que permite cargar variables de entorno e incluir la ruta a programas y bibliotecas de forma modular.

- **Para cargar EUPS:**

!!! info
    Actualmente, EUPS se carga automáticamente después de que el usuario accede a cualquier máquina del clúster Apollo.

  ```bash
    source /opt/eups/bin/setups.sh
  ```

- **Para listar todos los paquetes disponibles:**

  ```bash
    eups list
  ```

- **Para listar un paquete específico:**

  ```bash
    eups list | grep <PAQUETE>
  ```

- **Para cargar un paquete en la sesión actual:**

  ```bash
    setup <NOMBRE DEL PAQUETE> <VERSIÓN DEL PAQUETE>
  ```

- **Para eliminar el paquete cargado:**

  ```bash
    unsetup <NOMBRE DEL PAQUETE> <VERSIÓN DEL PAQUETE>
  ```

## Comandos útiles de Slurm

Para aprender sobre todas las opciones disponibles para cada comando, ingrese `man <comando>` mientras esté conectado al entorno del Cluster.

| Comando  | Definición                                                                      |
| -------- | ------------------------------------------------------------------------------- |
| sbatch   | Envía scripts de trabajos a la cola de ejecución                                |
| squeue   | Muestra estado de los jobs                                                      |
| scontrol | Usado para mostrar estado de Slurm (varias opciones disponibles solo para root) |
| sinfo    | Muestra estado de particiones y nodos                                           |
| salloc   | Envía un job para ejecución o inicia un trabajo en tiempo real                  |

## Videos tutoriales

* [How to login](https://youtu.be/3DHqWk7KGHw)
* [How to use EUPS](https://youtu.be/ifJqGEvqzdY)
* [How to access the Scratch](https://youtu.be/dnMzGYwICBw)
* [How to submit a Job](https://youtu.be/AbRCL_KsBVY)
