# Apptainer

## What is it?
The **[Apptainer](https://apptainer.org/docs/user/1.4/introduction.html)** is a container platform widely used in scientific and HPC environments. It was designed to enable the execution of complex applications on clusters in a **secure, portable, and reproducible** way, ensuring that the execution environment is identical regardless of which node runs the job.

In multi-user environments, such as clusters, Apptainer stands out for operating without the need for administrative privileges, integrating naturally with workload managers such as Slurm.

Apptainer uses images in the **SIF (*Singularity Image Format*)** format, which consist of a **single and immutable file** containing the entire system required to run the application, including the operating system, libraries, dependencies, and additional software. This facilitates sharing, versioning, and reproducibility of experiments.

## How to Use Apptainer - Apollo Cluster
In **Cluster Apollo**, the use of Apptainer is only permitted for **running already built images**.

For security and environmental standardization reasons:
!!! WARNING "Attention:" 
	**Building images (`apptainer build`) on cluster nodes is not permitted.**

Images must be built externally (on a local machine or another appropriate environment) and uploaded to the cluster in `.sif` format.

### Step-by-step Usage
1.  **Upload the `.sif` image**

    The user must:

       - Access the **[LIneA Open OnDemand](https://ondemand.linea.org.br)**
       - Upload the `.sif` file to their **scratch** directory
              <br>path: `/scratch/YOUR.USER/IMAGE_NAME.sif`</br> 

2.  **Choose the execution method**

The user can execute the application in two main ways:

   - **Option A** -- Submission via Slurm (Recommended):

Create a submission script, for example `job_apptainer.sh`:
``` shell
#!/bin/bash
#SBATCH -p <PARTITION>                       
#SBATCH --nodes=<QT-NODE>
#SBATCH --account=<ACCOUNT-NAME>               
#SBATCH -J <JOB-NAME>           
#----------------------------------------------------------------------------#
echo "SLURM_NODELIST=$SLURM_NODELIST"

# Image path
IMAGE=/scratch/users/YOUR.USER/IMAGE_NAME.sif

# Execute command inside the container
srun apptainer run --containall $IMAGE

echo "Finished"
```

   - **Option B** -- Interactive Allocation:

For testing or interactive execution: 

  1. Allocate a compute node:
``` shell
salloc -p PARTITION-NAME --nodelist=NODE
ssh NODE
```

  2. After allocating the node and accessing it via SSH, execute:
``` shell
apptainer exec /scratch/YOUR.USER/IMAGE_NAME.sif YOUR_CODE
```

  3. Or, if you want to enter the container environment:
```Shell
    apptainer shell /scratch/YOUR.USER/IMAGE_NAME.sif
```
This will open a terminal inside the image. Inside the container, the
user can execute commands normally.

## Best Practices on the Cluster - Apptainer

✔ Always execute via Slurm  
✔ Use the `scratch` directory for image files and temporary data  
✔ Test interactively before submitting long-running jobs  
✔ Keep your images versioned

If you have any questions, please contact the [Service Desk](http://localhost:8088/suporte.html).
