# How to Use

## How to access

Access to our cluster can be done through [**Open OnDemand**](../../processamento/uso/openondemand.html) or via the **JupyterLab (K8S) Terminal**. In both options, it is essential to have a valid account in LIneA's computational environment. If you don't have an account, contact the Service Desk by email (helpdesk@linea.org.br) for more information.

!!! warning "Attention"
    Even with an active LIneA account, access to the HPC processing environment is not automatic. For more information contact the Service Desk at helpdesk@linea.org.br.

**Accessing via JupyterLab terminal**

In the [**home screen**](../img/tela-jupyter.png) of your Jupyter Notebook, in the **_"Other"_** section, you will find the terminal button. When clicking it, you will be redirected to a Linux terminal, initially located in your _home_ directory. To access the Apollo Cluster, simply execute the following command:

  ```bash
    ssh loginapl01
  ```
The machine <font color="#172b4d">**_loginapl01_**</font> is where you can allocate compute nodes to submit your job.

!!! warning "$HOME and $SCRATCH"
    Compute nodes don't have access to your user _(home)_ directory. Move or copy all files needed for job submission to your SCRATCH directory.

## How to use the SCRATCH area

Your SCRATCH directory is the place to send essential files for job submission, as well as to check results after code execution. It's crucial to note that **`all results and generated files must be transferred back to your user directory (home)`**. Otherwise, **`there's a risk of losing these files stored in your SCRATCH`**.

- **To access your SCRATCH directory:**

  ```bash
    ssh $SCRATCH
  ``` 
  
- **To send files to your SCRATCH directory:**

  ```bash
    cp <FILE> $SCRATCH
  ``` 

## EUPS Package Manager

[EUPS](https://github.com/RobertLuptonTheGood/eups) is an alternative package manager (and official LSST one) that allows loading environment variables and including paths to programs and libraries in a modular way.

- **To load EUPS:**

  ```bash
    . /mnt/eups/linea_eups_setup.sh
  ```

- **To list all available packages:**

  ```bash
    eups list
  ```

- **To list a specific package:**

  ```bash
    eups list | grep <PACKAGE>
  ```

- **To load a package in current session:**

  ```bash
    setup <PACKAGE NAME> <PACKAGE VERSION>
  ```

- **To remove loaded package:**

  ```bash
    unsetup <PACKAGE NAME> <PACKAGE VERSION>
  ```

## How to Submit a Job

A Job requests computing resources and specifies applications to be launched on those resources, along with any input data/options and output directives. Cluster task and resource management is done through Slurm. Therefore, to submit a Job you need to use a script like below:

```bash
  #!/bin/bash
  #SBATCH -p PARTITION                       #Name of the Partition to use
  #SBATCH --nodelist=NODE                    #Name of the Node to be allocated
  #SBATCH -J simple-job                      #Job name
  #----------------------------------------------------------------------------#
  ##path to executable code
  EXEC=/lustre/t0/scratch/users/YOUR.USER/EXECUTABLE.CODE
  srun $EXEC
```

In this script you need to specify: the **queue name (Partition)** to be used; the **node name** to be allocated for Job execution; and the **path to the code/program** to be executed.

!!! danger "WARNING"
     **It is strictly prohibited to submit _jobs_ directly to the _loginapl01_ machine. Any code running on this machine will be immediately interrupted without prior notice.**

- **To submit the Job:**

  ```bash
    sbatch script-submit-job.sh
  ```

If the script is correct **there will be an output indicating the job ID**.

- **To check job progress and information:**

  ```bash
    scontrol show job <ID> 
  ```

- **To cancel the Job:**

  ```bash
    scancel <ID> 
  ```
  
!!! warning "Internet access"
    Compute nodes **do not** have internet access. Packages and libraries must be installed from _loginapl01_ in your scratch area.

## Useful Slurm Commands

To learn about all available options for each command, enter `man <command>` while connected to the Cluster environment.

| Command  | Definition                                                                     |
| -------- | ----------------------------------------------------------------------------- |
| sbatch   | Submits job scripts to execution queue                                        |
| squeue   | Displays job status                                                           |
| scontrol | Used to display Slurm state (various options available only to root)          |
| sinfo    | Displays partition and node status                                            |
| salloc   | Submits a job for execution or starts a real-time job                         |

## Tutorial videos

* [How to login](https://youtu.be/3DHqWk7KGHw)
* [How to use EUPS](https://youtu.be/ifJqGEvqzdY)
* [How to access the Scratch](https://youtu.be/dnMzGYwICBw)
* [How to submit a Job](https://youtu.be/AbRCL_KsBVY)
