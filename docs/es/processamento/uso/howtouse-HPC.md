# Cómo Utilizar

## Cómo acceder

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
    Los nodos de computación no tienen acceso a su directorio _(home)_ de usuario. Mueva o copie todos los archivos necesarios para el envío de su job a su directorio SCRATCH.

## Cómo usar el área SCRATCH

Su directorio SCRATCH es el lugar para enviar los archivos esenciales para el envío de su Job, así como para verificar los resultados después de la ejecución del código. Es crucial informar que **`todos los resultados y archivos generados deben transferirse de vuelta a su directorio de usuario (home)`**. De lo contrario, **`existe el riesgo de perder estos archivos almacenados en su SCRATCH`**.

- **Para acceder a su directorio SCRATCH:**

  ```bash
    ssh $SCRATCH
  ``` 

- **Para enviar archivos a su directorio SCRATCH:**

  ```bash
    cp <ARCHIVO> $SCRATCH
  ``` 

## Gestor de paquetes EUPS

[EUPS](https://github.com/RobertLuptonTheGood/eups) es un gestor de paquetes alternativo (y oficial de LSST) que permite cargar variables de entorno e incluir la ruta a programas y bibliotecas de forma modular.

- **Para cargar EUPS:**

  ```bash
    . /mnt/eups/linea_eups_setup.sh
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

## Cómo Enviar un Job

Un Job solicita recursos de computación y especifica las aplicaciones a iniciar en esos recursos, junto con cualquier dato/opción de entrada y directiva de salida. La gestión y programación de tareas y recursos del clúster se realiza a través de Slurm. Por lo tanto, para enviar un Job necesita usar un script como el siguiente:

```bash
  #!/bin/bash
  #SBATCH -p PARTICIÓN                       #Nombre de la Partición a usar
  #SBATCH --nodelist=NODO                    #Nombre del Nodo a asignar
  #SBATCH -J simple-job                      #Nombre del Job
  #----------------------------------------------------------------------------#
  ##ruta al código ejecutable
  EXEC=/lustre/t0/scratch/users/USUARIO/CODIGO.EJECUTABLE
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

## Comandos útiles de Slurm

Para aprender sobre todas las opciones disponibles para cada comando, ingrese `man <comando>` mientras esté conectado al entorno del Cluster.

| Comando  | Definición                                                                     |
| -------- | ----------------------------------------------------------------------------- |
| sbatch   | Envía scripts de trabajos a la cola de ejecución                              |
| squeue   | Muestra estado de los jobs                                                    |
| scontrol | Usado para mostrar estado de Slurm (varias opciones disponibles solo para root)|
| sinfo    | Muestra estado de particiones y nodos                                         |
| salloc   | Envía un job para ejecución o inicia un trabajo en tiempo real                |

## Videos tutoriales

* [How to login](https://youtu.be/3DHqWk7KGHw)
* [How to use EUPS](https://youtu.be/ifJqGEvqzdY)
* [How to access the Scratch](https://youtu.be/dnMzGYwICBw)
* [How to submit a Job](https://youtu.be/AbRCL_KsBVY)
