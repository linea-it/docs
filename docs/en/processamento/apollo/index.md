# HPE Apollo 2000

The **Apollo Cluster** has 28 compute nodes and offers a total of **1072 physical cores**. Its nodes are equipped with `Intel Xeon Skylake 5120 2.2GHz` processors (apl01-16) and `Intel Xeon Gold 5320 2.20GHz` processors (apl17-28). The system provides approximately 85 Tflops of computational capacity.
The 28 compute nodes of the Apollo Cluster are from the HPE ProLiant server family, with 16 being XL170r models and 12 being XL220n models. Currently, the number of available cores is *2144*, as HT is active on the compute nodes.

#### Server Specifications

| Model | # Cores (HT)<sup>[1]</sup> | RAM | OS | Hosts |
| ------- | ------| ------------ | -------- | -----------| 
| HPE Proliant XL170r  | 56    | 128 GB       | Rocky Linux 9.5 |  apl[01-16]  |
| HPE Proliant XL220n  | 104   | 256 GB       | Rocky Linux 9.5 |  apl[17-28]  |
<sup>[1]</sup> Hyper-Threading (HT) technology is enabled on all cluster nodes.

**Cluster Characteristics (Consolidated)**

| # Nodes | # Cores | Total RAM | Installed |
| ------- | ------| ------------ | -----------| 
| 16      | 896   | 1.5TB        |  Apr-2019  |
| 12      | 624   | 3TB          |  Jul-2023  |

The **Apollo Cluster** is managed by **Slurm v24.05.5**.

### Filesystem

The **Apollo Cluster** features a high-performance Lustre filesystem, available as a "Scratch" area. User "Home" directories are only accessible on the login node and are provided via NFS.

These storage areas should be used as follows:

**Scratch:** Mounted from `/scratch/<username>`. Used to store all files needed during job execution (submission scripts, executables, input data, output data, etc). Environment variable `$SCRATCH`.

**Home:** Mounted from `/home/<username>`. Used especially to store results that should be kept throughout the project duration. Environment variable `$HOME`.

**Scriptland:** Mounted from `/scriptland/<username>`. A storage area optimized for scripts and code. Environment variable `$SCRIPTLAND`.

[Click here for more details](../../armazenamento/index.html)

!!! warning "Attention"
    Remember to copy necessary files (executables, libraries, input data) to the SCRATCH area, as the HOMEDIR area is not accessible by compute nodes.

## Slurm

Slurm is an open-source cluster management and job scheduling system, fault-tolerant and highly scalable for both large and small Linux clusters. Slurm requires no kernel modifications for operation and is relatively independent. As a cluster workload manager, Slurm has three key functions:

 - Allocate exclusive and/or non-exclusive access to resources (compute nodes) to users for specified durations
 - Provide a framework for starting, executing, and monitoring work (typically parallel jobs) on allocated nodes
 - Manage the submission queue, arbitrating conflicts between computational resource requests

### Available Partitions

The Apollo cluster is organized into different partitions (machine subsets) to meet different needs, such as guaranteeing maximum priority for LSST project users on machines dedicated to IDAC-Brazil.

|PARTITION   |TIMELIMIT  |NODES  |NODELIST  |
|------------|-----------|-------|----------|
|cpu_dev     |30:00      |26     |apl[01-28]|
|cpu_small   |3-00:00:00 |26     |apl[01-28]|
|cpu         |5-00:00:00 |26     |apl[01-28]|
|cpu_long    |31-00:00:0 |26     |apl[01-28]|
|lsst_cpu_dev     |30:00      |12     |apl[17-28]|
|lsst_cpu_small   |3-00:00:00 |12     |apl[17-28]|
|lsst_cpu         |5-00:00:00 |12   |apl[17-28]|
|lsst_cpu_long    |10-00:00:0 |12     |apl[17-28]|

### Available Accounts

- **Workflow** – Interrupts any running job: **hpc-photoz** (photoz)
- **LSST** – Next in queue: **hpc-lsst** [only on new apollos apl[17-28]] (lsst)
- **Group A** - Highest Priority: **hpc-bpglsst** (itteam, bpg-lsst)
- **Group B** - Medium Priority: **hpc-collab** (des, desi, sdss, tno)
- **Group C** - Lowest Priority: **hpc-public** (linea-members)

The partitions (**cpu_dev**, **cpu_small**, **cpu** and **cpu_long**) include all apollos (*apl[01-28]*), while LSST group partitions only include *apl[17-28]*. Only the *hpc-lsst* account can submit jobs to "lsst" prefixed partitions, which have higher priority on nodes.

!!! warning "Attention"
    As part of the BRA-LIN in-kind contribution program, IDAC Brazil is committed to generating photometric redshifts annually for the LSST survey, always preceding official data releases. During these periods, the Apollo Cluster will be fully occupied for this purpose for an estimated duration of several hours, potentially extending to several days. Users will be notified in advance via email about cluster unavailability. [Click here](https://linea-it.github.io/pz-lsst-inkind-doc/) to learn more about photometric redshift production and the BRA-LIN in-kind contribution program.

### Anatomy of a Job

A Job requests computing resources and specifies the applications to be launched on those resources, along with any input data/options and output directives. The user submits the job, typically as a batch job script, to the batch scheduler.

The batch job script consists of four main components:

 - The interpreter used to execute the script
 - "#" directives that convey default submission options
 - Environment variable and/or script configuration (if needed)
 - The applications to be executed along with their input arguments and options

Here's an example batch script requesting 3 nodes in the "cpu" partition and launching 36 myApp tasks on the 3 allocated nodes:

```bash
#!/bin/bash
#SBATCH -N 3
#SBATCH -p cpu
#SBATCH --ntasks 36
srun myApp
```

When the job is scheduled for execution, the resource manager will execute the batch job script on the first node of the allocation.

#### Specifying Resources

Slurm has its own syntax for requesting computing resources. Below is a summary table of some frequently requested resources and the Slurm syntax to obtain them. For a complete syntax list, run the man sbatch command.

|Syntax  |Meaning|
|---------|-----------|
|#SBATCH -p partition  | Defines the partition where the job will run|
|#SBATCH -J job_name | Defines the Job name|
|#SBATCH -n quantity | Defines the total number of CPU tasks|
|#SBATCH -N quantity  | Defines the number of requested compute nodes|

#### Basic Slurm Commands

To learn about all available options for each command, enter man <command> while connected to the Cluster environment.

|Command	| Definition|
|-----------|----------|
|sbatch	| Submits job scripts to the execution queue|
|scancel	| Cancels a job|
|scontrol	| Used to display Slurm state (various options available only to root)|
|sinfo	| Displays partition and node states|
|squeue	| Displays job states|
|salloc	| Submits a job for execution or starts a real-time job|

#### Execution Environment

For each job type above, the user can define the execution environment. This includes environment variable settings as well as shell limits (`bash ulimit` or `csh limit`). **sbatch** and **salloc** provide the `--export` option to pass specific environment variables to the execution environment. They also have the `--propagate` option to pass specific shell limits to the execution environment.

#### Environment Variables

The first category of environment variables are those that Slurm inserts into the job execution environment. They convey to the job script and application information such as job ID `(SLURM_JOB_ID)` and task ID `(SLURM_PROCID)`. For the complete list, see the `"OUTPUT ENVIRONMENT VARIABLES"` section in the [sbatch](https://slurm.schedmd.com/sbatch.html), [salloc](https://slurm.schedmd.com/salloc.html) and [srun](https://slurm.schedmd.com/srun.html) man pages.

The next category are environment variables that users can define in their environment to pass default options to each submitted job. This includes options like the wall clock limit. For the complete list, see the `"INPUT ENVIRONMENT VARIABLES"` section in the [sbatch](https://slurm.schedmd.com/sbatch.html), [salloc](https://slurm.schedmd.com/salloc.html) and [srun](https://slurm.schedmd.com/srun.html) man pages.

!!! info "More Information?"
    Learn how to use the Apollo Cluster at [How to Use](../../processamento/uso/howtouse-HPC.html). <br> 
    For more information, contact the [Service Desk](../../suporte.html).
