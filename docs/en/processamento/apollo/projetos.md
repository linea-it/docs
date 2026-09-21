# Projects

This page is a starting point for people who began using the Apollo Cluster as part of a **project** with allocated resources in the environment, for example projects selected in a public call such as [SINCADA](https://www.linea.org.br/noticia/linea-lanca-o-programa-sincada), and other cases supported by LIneA.

It does not replace the other Processing and Storage pages. The goal is to guide first-time use and highlight what changes when the work is tied to a project, not only to a personal account.

!!! info
    Hardware, partitions, Slurm, Open OnDemand, Conda, and the detailed storage areas remain on their specific pages. Use the links on this page to reach them.

## Before you start processing

1. **LIneA account.** Access to LIneA services and resources starts with registering an account. Follow one of the flows described in [Getting Started](../../primeiros_passos.md), according to the user profile.

2. **HPC access.** Having a previously approved account does **not** grant automatic access to the Apollo Cluster after a project is approved. If HPC access is not yet active, open a [ticket](../../suporte.md) and include the acronym of the project you are linked to.

3. **Policies.** Use of the environment is subject to LIneA [policies](../../politicas.md).

Each person associated with a project must use their **own account**. Sharing accounts is not allowed. To add or remove a collaborator from a project, the principal investigator should open a [ticket](../../suporte.md) with the project acronym and the user to be added or removed.

The simplest first contact is through the [Open OnDemand](../uso/openondemand.md) platform (`https://ondemand.linea.org.br/`). From the browser, you can open a terminal on the login server, manage files, submit jobs, and, if needed, start a JupyterLab session **on the cluster**.

There is also JupyterHub, available at `https://jupyter.linea.org.br`, which runs on a different infrastructure based on Kubernetes. Although it shares two storage areas with the cluster (`$HOME` and `/mnt/cl/prj/<sigla>`), the environments are independent. Apollo jobs must be prepared and submitted from the cluster itself. See [How to Use](../uso/howtouse-HPC.md).

!!! danger
    Running processing on the login server is forbidden. Any code executing on this server may be stopped without notice.

## Where to store data

| Area | Purpose | Availability |
| --- | --- | --- |
| `/mnt/cl/prj/<sigla>` | Long-term project files (NAS/NFS) | Login, Open OnDemand, and Jupyter. **Not** available on compute nodes |
| `$SCRATCH` | Job input, intermediate, and output data (Lustre) | All cluster nodes. Temporary, with automatic cleanup |
| `$SCRIPTS`<br>`/scripts/cl/prj/<sigla>` | Submission scripts and Conda environments | All cluster nodes |
| `/data` | Working data that requires high-performance I/O (Lustre) | All cluster nodes. **Not** the project's permanent storage |
| `$HOME` | Personal configuration and files | Login and Jupyter. **Not** available on compute nodes |

Replace `<sigla>` with the project identifier provided by LIneA. Retention, backup, and Lustre usage are described in [Storage](../../armazenamento/index.md).

### Long-term archive

This is the area that should hold the project's data. It is designed for long-term storage, not for parallel job I/O.

On the Open OnDemand terminal (login server):

```bash
ls /mnt/cl/prj/<sigla>

cd /mnt/cl/prj/<sigla>
```

**Personal** quota:

```bash
show_quota
```

**Project** quota:

```bash
show_proj_quota <sigla>
```

If the directory does not exist or you do not have permission, open a [ticket](../../suporte.md) with the acronym.

!!! warning
    `$SCRATCH` is temporary, has no backup, and is cleaned automatically. Anything the project must keep should be stored in `/mnt/cl/prj/<sigla>`, not remain only in `$SCRATCH` or `$HOME`.

### Working data on Lustre

`/data` is **not** the project's permanent storage area. It is a Lustre working area for data that must be available on compute nodes and that requires high-performance I/O.

Use `/data` when the job needs to work with large volumes of data directly on the compute nodes. For temporary files, intermediates, and results of a job, use `$SCRATCH`.

Do not use `/data` as the project's permanent storage instead of `/mnt/cl/prj/<sigla>`.

### Job cycle

The project's storage area is not available on compute nodes. Therefore, the data required for processing must be transferred to an area available on the cluster before the job runs.

For temporary files, intermediates, and results of a job, use `$SCRATCH`. When processing requires a Lustre working area for data that must remain available to the compute nodes, use `/data`.

**1. On the login server, copy what you need to `$SCRATCH`**

```bash
mkdir -p $SCRATCH/meu-job

cp -a /mnt/cl/prj/<sigla>/entrada $SCRATCH/meu-job/
```

**2. Put the script in `$SCRIPTS` and submit the job**

```bash
cd $SCRATCH/meu-job

sbatch $SCRIPTS/submeter.sh
```

In the script, input and output paths must point to `$SCRATCH` (or `/data`, when appropriate), not to `/mnt/cl/prj/<sigla>`. Details in [How to Use](../uso/howtouse-HPC.md).

**3. When the job finishes and you are satisfied with the result, still on the login server, transfer the result to the project's storage area.**

```bash
cp -a $SCRATCH/meu-job/saida /mnt/cl/prj/<sigla>/
```

Then remove from `$SCRATCH` the data already stored in the project area, to avoid loss during automatic cleanup and unnecessary quota use.

!!! danger
    Do not point the job at `/mnt/cl/prj/<sigla>`. That area is not mounted on compute nodes. Do not run processing on `login` either.

### Project scripts and Conda

Shared Conda environments live in `/scripts/cl/prj/<sigla>`. Directory creation is requested from the Service Desk; the step-by-step is in [Conda in the HPC environment](../uso/howtouse-HPC.md#conda-in-the-hpc-environment).

Install packages from the login server: compute nodes **do not have internet access**.

## The cluster is shared

The Apollo environment also serves other projects. For these projects:

- Use the general partitions (`cpu_dev`, `cpu_small`, `cpu`, `cpu_long`). The `cpu_bpglsst` partition is exclusive to BPG LSST members.

- If the project has a Slurm account, include it in the submission (`#SBATCH --account=...`) and in Open OnDemand Jupyter. The identifier is filled in automatically by the application.

- At times the cluster becomes unavailable for photometric *redshift* production under the BRA-LIN in-kind program. Users are notified by email. See the notice on [Apollo Cluster](./index.md).

Time limits, nodes, and job details are in [Slurm](./index.md#slurm). Script examples: [Job Script](../uso/templates-jobs.md).

## Acknowledgement

Publications, theses, and presentations that use the environment must acknowledge LIneA. The suggested text for the Brazilian community and for public-call projects (for example, SINCADA) is in [Credits](../../creditos.md#use-of-linea-resources).

## Where to go next

| I need… | Page |
| --- | --- |
| First access from the browser | [Open OnDemand](../uso/openondemand.md) |
| Submit jobs and use scratch/scripts | [How to Use](../uso/howtouse-HPC.md) |
| Hardware, partitions, and Slurm | [Apollo Cluster](./index.md) |
| Containers | [Apptainer](../uso/apptainer-uso.md) |
| Areas, quotas, and Lustre | [Storage](../../armazenamento/index.md) |
| Video tutorials | [Courses and Tutorials](../../cursos.md) |
| Questions | [Support](../../suporte.md) |
