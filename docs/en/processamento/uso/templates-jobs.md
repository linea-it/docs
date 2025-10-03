# Submit Job Scripts - Templates

#### Simple Job Script

```bash
  #!/bin/bash
  #SBATCH -p PARTITION                       #Name of the Partition to use
  #SBATCH --nodelist=NODENAME                #Name of the Node to be allocated
  #SBATCH -J JOB-NAME                        #Job name
  #----------------------------------------------------------------------------#

  ##path to executable code
  EXEC=/scripts/YOUR.USER/EXECUTABLE.CODE

  srun $EXEC
```

In this script you need to specify the **queue name (Partition)** to be used, the **node name** to be allocated for Job execution, and the **path to the code/program** to be executed.

#### EUPS Load Script

```bash
  #!/bin/bash
  #SBATCH -p PARTITION                     #Name of the Partition to use
  #SBATCH --nodelist=NODENAME              #Name of the Node to be allocated
  #SBATCH -J JOB-NAME                      #Job name
  #----------------------------------------------------------------------------#

  #Load EUPS
  . /mnt/eups/linea_eups_setup.sh

  #Load package
  setup <PACKAGE> <VERSION>

  ##path to executable code
  EXEC=/scripts/YOUR.USER/EXECUTABLE.CODE

  srun $EXEC
```

## Parallel Submit Job Script

#### OpenMP


#### MPI
