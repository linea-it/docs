# Enviar Scripts de Trabajo - Plantillas

#### Script Simple de Trabajo

```bash
  #!/bin/bash
  #SBATCH -p PARTICIÓN                       #Nombre de la Partición a usar
  #SBATCH --nodelist=NOMBREDELNODO           #Nombre del Nodo a asignar
  #SBATCH -J NOMBRE-DEL-TRABAJO              #Nombre del Trabajo
  #----------------------------------------------------------------------------#

  ##ruta al código ejecutable
  EXEC=/lustre/t0/scratch/users/USUARIO/CODIGO.EJECUTABLE

  srun $EXEC
```

En este script debe especificar el **nombre de la cola (Partition)** a usar, el **nombre del nodo** a asignar para la ejecución del Job, y la **ruta al código/programa** a ejecutar.

#### Script de Carga EUPS

```bash
  #!/bin/bash
  #SBATCH -p PARTICIÓN                     #Nombre de la Partición a usar
  #SBATCH --nodelist=NOMBREDELNODO         #Nombre del Nodo a asignar
  #SBATCH -J NOMBRE-DEL-TRABAJO            #Nombre del Trabajo
  #----------------------------------------------------------------------------#

  #Cargar EUPS
  . /mnt/eups/linea_eups_setup.sh

  #Cargar paquete
  setup <PAQUETE> <VERSIÓN>

  ##ruta al código ejecutable
  EXEC=/lustre/t0/scratch/users/USUARIO/CODIGO.EJECUTABLE

  srun $EXEC
```

## Script de Envío Paralelo

#### OpenMP


#### MPI
