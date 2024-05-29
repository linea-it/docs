# Submit Job Scripts - Templates 

### Simple Job Script 
```bash
  #!/bin/bash
  #SBATCH -p PARTITION                       #Name of the Partition to use
  #SBATCH --nodelist=NODENAME                #Name of the Node to be allocated
  #SBATCH -J JOB-NAME                                  #Job name
  #----------------------------------------------------------------------------#

  ##path to executable code
  EXEC=/lustre/t0/scratch/users/YOUR.USER/EXECUTABLE.CODE

  srun $EXEC
```
Nesse script é preciso especificar o **nome da fila (Partition)** que será usada, o **nome do nó** que será alocado para a excecução do Job, e o **caminho para o código/programa** a ser executado.

### EUPS Load Script

```bash
  #!/bin/bash
  #SBATCH -p PARTITION                     #Name of the Partition to use
  #SBATCH --nodelist=NODENAME              #Name of the Node to be allocated
  #SBATCH -J JOB-NAME			   #Job name
  #----------------------------------------------------------------------------#

  #Carregar o EUPS
  . /mnt/eups/linea_eups_setup.sh

  #Carregar pacote 
  setup <PACKAGE> <VERSION>

  ##path to executable code
  EXEC=/lustre/t0/scratch/users/YOUR.USER/EXECUTABLE.CODE

  srun $EXEC
```

###

### Parallel Submit Job Script

#### OpenMP 


#### MPI
