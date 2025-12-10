# Submit Job Scripts - Templates 

#### Simple Job Script 
```bash
  #!/bin/bash
  #SBATCH -p PARTITION                       #Name of the Partition to use
  #SBATCH --nodelist=NODENAME                #Name of the Node to be allocated
  #SBATCH -J JOB-NAME                                  #Job name
  #----------------------------------------------------------------------------#

  ##path to executable code
  EXEC=/scripts/YOUR.USER/EXECUTABLE.CODE

  srun $EXEC
```
Nesse script é preciso especificar o **nome da fila (Partition)** que será usada, o **nome do nó** que será alocado para a execução do Job, e o **caminho para o código/programa** a ser executado.

#### EUPS Load Script

```bash
  #!/bin/bash
  #SBATCH -p PARTITION                     #Name of the Partition to use
  #SBATCH --nodelist=NODENAME              #Name of the Node to be allocated
  #SBATCH -J JOB-NAME			           #Job name
  #----------------------------------------------------------------------------#

  #Carregar o EUPS
  export EUPS_USERDATA=/scratch/users/YOUR.USER
  . /opt/eups/bin/setups.sh
  
  #Carregar pacote 
  setup <PACKAGE> <VERSION>

  #Caminho para seu código
  EXEC=/scripts/YOUR.USER/EXECUTABLE.CODE

  srun $EXEC
```

## Parallel Submit Job Script

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

  #Exibe os nodes alocados
  echo "SLURM_NODELIST=$SLURM_NODELIST"
  echo "SLURM_NTASKS=$SLURM_NTASKS"
  echo "Running on nodes: $(scontrol show hostnames $SLURM_NODELIST)"

  #Carregar o EUPS
  export EUPS_USERDATA=/scratch/users/YOUR.USER
  . /opt/eups/bin/setups.sh
  
  setup openmpi 5.0.8+0

  #Executar via srun ou mpirun
  mpirun /path/to/code
```
