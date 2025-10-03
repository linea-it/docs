# Open OnDemand

Open OnDemand is an interface that facilitates the use of our HPC environment consisting of the Apollo Cluster. To access it, you need a valid LIneA account ([learn more](../../primeiros_passos.html)).

Access to Open OnDemand is through https://ondemand.linea.org.br/.

On the [**home screen**](../img/OOD1.png) of the platform, at the top, you can see a menu with the following items:

* **Files** - provides an interface to your user directory (_Home Directory_).
* **Jobs** - provides interfaces for "Active Jobs" and "Job Composer".
* **Clusters** - provides web browser terminal access.
* **Interactive Apps** - provides access to Jupyter Notebook.

## Home Directory

The [**Home Directory**](../img/OOD2.jpeg) allows you to view your user directory where your files are stored, along with various buttons offering different functionalities.

#### Changing Directory

Clicking the [**Change Directory**](../img/OOD3.png) button lets you navigate through our infrastructure. Simply enter the path in the **Path** field and click "**ok**".

#### Accessing Terminal

Clicking the [**Open Terminal**](../img/OOD4.png) button takes you to a Linux terminal on the login machine (loginapl01) of the Apollo Cluster.
In this terminal, you're in your user "_Home_" directory and can view your files. You can also perform all standard HPC user operations, such as allocating compute nodes and checking the Slurm queue using commands.

#### Creating, Transferring and Moving Files

In Open OnDemand, creating new files and directories is simple, as is transferring them.

* Click the **"New File"** or **"New Directory"** buttons and choose names for your new files/directories.
* The **"Upload"** button lets you transfer files from your local machine to your _home_ directory in our environment.
* The **"Download"** button lets you send selected files to your local machine.

To view, rename or **edit** created files, click the **_"three dots"_** next to the file to see a menu with these options.

To [**move** or **copy**](../img/OOD5.png) files:

1. Select the desired file
2. Click **"Copy/Move"**
3. Click **"Change Directory"** and enter the destination path
4. Click "Copy" or "Move" in the box that appears on the left side of the screen.

## Jobs

In the [**Jobs**](../img/OOD6.png) section of the main menu, you'll find "Job Composer" and "Active Jobs". The "Job Composer" screen simplifies job submission, while [**"Active Jobs"**](../img/OOD9.png) lets you monitor your job execution in detail.

To submit a job, you need a submission script like this: ([learn more](../../processamento/apollo/index.html#anatomia-de-um-job))

```bash
#!/bin/bash
#SBATCH -p PARTITION                       #Name of the Partition to use
#SBATCH --nodelist=NODE                    #Name of the Node to be allocated
#SBATCH -J simple-job                      #Job name
#----------------------------------------------------------------------------#
##path to executable code
EXEC=/scratch/users/YOUR.USER/ondemand/projects/EXECUTABLE.CODE
srun $EXEC
```

[**To view more job submission script templates, click here**](/processamento/uso/templates-jobs.html)

### Job Composer

Open OnDemand simplifies the entire job submission process through the [Job Composer](../img/OOD7.png) tool. Just follow these steps:

1. Click the [**"New Job"**](../img/OOD6.1.png) button
2. Choose **"Default Template"**
3. Edit submission script specifications in **"Open Editor"**
4. Click **"Submit"** to start job execution.

!!! warning "Important Notice"
    The cluster's compute nodes don't have access to your user directory (Home Directory). Move or copy all files needed for job submission to your SCRATCH directory.

## JupyterLab

With Open OnDemand, you can access Jupyter Notebook in our HPC environment. Through "Interactive Apps", Jupyter Notebook will start a session on one of the cluster's compute nodes by:

1. Clicking ["Jupyter Notebook"](../img/OOD8.png)
2. Filling in the "Account", "Partition" and "Select node" fields
3. Pressing the ["Launch"](../img/OOD10.png) button
4. Connecting to [JupyterLab](../img/OOD12.png) by clicking [**"Connect to Jupyter"**](../img/OOD11.png).

!!! warning "Jupyter on Kubernetes VS Jupyter on HPC"
    We have two Jupyter environments on different infrastructures. One is available at [https://jupyter.linea.org.br](https://jupyter.linea.org.br) running on [Kubernetes](https://kubernetes.io/). The other, mentioned above, runs interactively through Open OnDemand directly on compute nodes.

When opening a [**terminal within JupyterLab**](../img/OOD14.png) via Open OnDemand, you'll see you're on an Apollo Cluster node with access to your "_scratch_" area on Lustre storage.

### Creating Python Kernels

**The following commands must be executed in the terminal in "LIneA Shell Access"**. <br> To access:

* On the Open OnDemand home page, click: **Clusters -> LIneA Shell Access**.

Follow these commands:

1. Go to your SCRATCH area, install and load Miniconda:

```bash
    cd $SCRATCH
    curl -L -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    chmod +x Miniconda3-latest-Linux-x86_64.sh
    ./Miniconda3-latest-Linux-x86_64.sh -p $SCRATCH/miniconda
    
    source miniconda/bin/activate 
    conda deactivate #Required to deactivate "base" env
```

2. Create, activate a conda venv and install _ipykernel_:

```bash
    conda create -p $SCRATCH/kernelname
    conda activate kernelname/
    
    conda install -c anaconda ipykernel
```

3. Configure JUPYTER_PATH (must be this exact path):

```bash
    JUPYTER_PATH=$SCRATCH/.local
    echo $JUPYTER_PATH
    
    python -m ipykernel install --prefix=$JUPYTER_PATH --name 'kernelname'
```

4. Open a Jupyter Notebook session.

???+ success "The last command's output should be:"

    ````yaml
    #[InstallIPythonKernelSpecApp] WARNING | Installing to /lustre/t0/scratch/users/YOUR.USER/.local/share/jupyter/kernels, which is not in ['/lustre/t0/scratch/users/YOUR.USER/kernelname/share/jupyter/kernels', '/home/YOUR.USER/.local/share/jupyter/kernels', '/usr/local/share/jupyter/kernels', '/usr/share/jupyter/kernels', '/home/YOUR.USER/.ipython/kernels']. The kernelspec may not be found.
    Installed kernelspec kernelname in /lustre/t0/scratch/users/YOUR.USER/.local/share/jupyter/kernels/kernelname
    ````

After executing these commands, you'll see the created Python kernel button.

## Tutorial videos

* [How to submit a Job](https://youtu.be/6w1H3VS40Ew)
* [How to access Jupyter Notebook](https://youtu.be/SemHNDr8vjg)

For any questions, contact the [Service Desk](../../suporte.html).
