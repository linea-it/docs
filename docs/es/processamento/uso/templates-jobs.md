# Enviar Scripts de Trabajo - Plantillas

#### Script Simple de Trabajo

```bash
  #!/bin/bash
  #SBATCH -p PARTICIÓN                       #Nombre de la Partición a usar
  #SBATCH --nodelist=NOMBREDELNODO           #Nombre del Nodo a asignar
  #SBATCH -J NOMBRE-DEL-TRABAJO              #Nombre del Trabajo
  #----------------------------------------------------------------------------#

  ##ruta al código ejecutable
  EXEC=/scripts/USUARIO/CODIGO.EJECUTABLE

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
  EXEC=/scripts/USUARIO/CODIGO.EJECUTABLE

  srun $EXEC
```

## Script de Envío Paralelo

#### OpenMP


#### MPI
```bash
  #!/bin/bash
  #SBATCH -p PARTITION
  #SBATCH --nodes=QT-NODES   
  #SBATCH --account=ACCOUNT
  #SBATCH --ntasks-per-node=QT-TASKS-PER-NODE
  #SBATCH -J JOB-NAME            
  #----------------------------------------------------------#

  echo "SLURM_NODELIST=$SLURM_NODELIST"
  echo "SLURM_NTASKS=$SLURM_NTASKS"
  echo "Running on nodes: $(scontrol show hostnames $SLURM_NODELIST)"

  #Subir EUPS
  export EUPS_USERDATA=/scratch/users/YOUR.USER
  . /opt/eups/bin/setups.sh

  setup openmpi 5.0.8+0

  #Ejecutar mediante srun o mpirun
  mpirun /path/to/code
```
