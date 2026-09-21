# Proyectos

Esta página es un punto de partida para quienes comenzaron a utilizar el Cluster Apollo en el marco de un **proyecto** con asignación de recursos en el entorno, por ejemplo, proyectos seleccionados en la convocatoria pública como [SINCADA](https://www.linea.org.br/noticia/linea-lanca-o-programa-sincada), y otros casos apoyados por LIneA.

No sustituye las demás páginas de Procesamiento y Almacenamiento. El objetivo es orientar el primer uso y destacar lo que cambia cuando el trabajo está asociado a un proyecto, y no solo a una cuenta personal.

!!! info
    El hardware, las particiones, Slurm, Open OnDemand, Conda y el detalle de las áreas de almacenamiento siguen en sus páginas específicas. Use los enlaces de esta página para llegar a ellas.

## Antes de procesar

1. **Cuenta en LIneA.** El acceso a los servicios y recursos de LIneA comienza con el registro de una cuenta. El proceso debe seguir uno de los flujos descritos en [Primeros pasos](../../primeiros_passos.md), de acuerdo con el perfil del usuario.

2. **Acceso al HPC.** Tener una cuenta previamente aprobada **no** garantiza acceso automático al Cluster Apollo tras la aprobación de un proyecto. Si el acceso al HPC aún no está activo, abra un [ticket](../../suporte.md) indicando la sigla del proyecto al que está vinculado.

3. **Políticas.** El uso del entorno está sujeto a las [políticas](../../politicas.md) de LIneA.

Cada persona asociada a un proyecto debe utilizar su **propia cuenta**. No está permitido compartir cuentas. Para incluir o quitar a un colaborador de un proyecto, el responsable debe abrir un [ticket](../../suporte.md) indicando la sigla del proyecto y el usuario que debe ser agregado o quitado.

El camino más simple para el primer contacto es la plataforma [Open OnDemand](../uso/openondemand.md) (`https://ondemand.linea.org.br/`). Desde el navegador, puede abrir un terminal en el servidor de login, gestionar archivos, enviar jobs y, si es necesario, iniciar un JupyterLab **en el clúster**.

También existe JupyterHub, disponible en `https://jupyter.linea.org.br`, que se ejecuta en una infraestructura distinta, basada en Kubernetes. Aunque comparte dos áreas de almacenamiento con el clúster (`$HOME` y `/mnt/cl/prj/<sigla>`), los entornos son independientes. El procesamiento de jobs en Apollo debe prepararse y enviarse desde el propio clúster. Consulte [Cómo Utilizar](../uso/howtouse-HPC.md).

!!! danger
    Está prohibido ejecutar procesamiento en el servidor de login. Cualquier código en ejecución en este servidor podrá interrumpirse sin aviso.

## Dónde guardar los datos

| Área | Finalidad | Disponibilidad |
| --- | --- | --- |
| `/mnt/cl/prj/<sigla>` | Archivos de largo plazo del proyecto (NAS/NFS) | Login, Open OnDemand y Jupyter. **No** disponible en los nodos de cómputo |
| `$SCRATCH` | Datos de entrada, intermedios y de salida de los jobs (Lustre) | Todos los nodos del clúster. Temporal, con limpieza automática |
| `$SCRIPTS`<br>`/scripts/cl/prj/<sigla>` | Scripts de envío y entornos Conda | Todos los nodos del clúster |
| `/data` | Datos de trabajo que requieren I/O de alto rendimiento (Lustre) | Todos los nodos del clúster. **No** es el almacenamiento permanente del proyecto |
| `$HOME` | Configuraciones y archivos personales | Login y Jupyter. **No** disponible en los nodos de cómputo |

Sustituya `<sigla>` por el identificador del proyecto informado por LIneA. Las características de retención, backup y uso de Lustre se describen en [Almacenamiento](../../armazenamento/index.md).

### Archivo de largo plazo

Esta es el área que debe guardar los datos del proyecto, pensada para almacenamiento de largo plazo y no para I/O paralelo de los jobs.

En el terminal de Open OnDemand (servidor de login):

```bash
ls /mnt/cl/prj/<sigla>

cd /mnt/cl/prj/<sigla>
```

Cuota **personal**:

```bash
show_quota
```

Cuota del **proyecto**:

```bash
show_proj_quota <sigla>
```

Si el directorio no existe o no tiene permiso, abra un [ticket](../../suporte.md) indicando la sigla.

!!! warning
    `$SCRATCH` es temporal, sin backup y con limpieza automática. Lo que el proyecto deba conservar debe almacenarse en `/mnt/cl/prj/<sigla>`, no permanecer solo en `$SCRATCH` ni en `$HOME`.

### Datos de trabajo en Lustre

`/data` **no** es el área de almacenamiento permanente del proyecto. Es un área de trabajo en Lustre, destinada a datos que deben estar disponibles en los nodos de cómputo y que requieren I/O de alto rendimiento.

Use `/data` cuando el job necesite trabajar con grandes volúmenes de datos directamente en los nodos de cómputo. Para archivos temporales, intermedios y resultados de un job, utilice `$SCRATCH`.

No use `/data` como almacenamiento permanente del proyecto en lugar de `/mnt/cl/prj/<sigla>`.

### Ciclo de un job

El área de almacenamiento del proyecto no está disponible en los nodos de cómputo. Por eso, los datos necesarios para el procesamiento deben transferirse a un área disponible en el clúster antes de ejecutar el job.

Para datos temporales, intermedios y resultados de un job, utilice `$SCRATCH`. Cuando el procesamiento exija un área de trabajo en Lustre para datos que deban permanecer disponibles a los nodos de cómputo, utilice `/data`.

**1. En el servidor de login, copie lo necesario a `$SCRATCH`**

```bash
mkdir -p $SCRATCH/meu-job

cp -a /mnt/cl/prj/<sigla>/entrada $SCRATCH/meu-job/
```

**2. Ponga el script en `$SCRIPTS` y envíe el job**

```bash
cd $SCRATCH/meu-job

sbatch $SCRIPTS/submeter.sh
```

En el script, las rutas de entrada y salida deben apuntar a `$SCRATCH` (o `/data`, cuando corresponda), y no a `/mnt/cl/prj/<sigla>`. Detalles en [Cómo Utilizar](../uso/howtouse-HPC.md).

**3. Cuando el job termine y esté satisfecho con el resultado, todavía en el servidor de login, transfiera el resultado al área de almacenamiento del proyecto.**

```bash
cp -a $SCRATCH/meu-job/saida /mnt/cl/prj/<sigla>/
```

Después, elimine de `$SCRATCH` los datos que ya estén almacenados en el área del proyecto, para evitar la pérdida en la limpieza automática y el uso innecesario de la cuota.

!!! danger
    No apunte el job a `/mnt/cl/prj/<sigla>`. Esa área no está montada en los nodos de cómputo. Tampoco ejecute el procesamiento en `login`.

### Scripts y Conda del proyecto

Los entornos Conda compartidos quedan en `/scripts/cl/prj/<sigla>`. La creación del directorio se solicita al Service Desk; el paso a paso está en [Conda en el entorno HPC](../uso/howtouse-HPC.md#conda-en-el-entorno-hpc).

Instale paquetes desde el servidor de login: los nodos de cómputo **no tienen acceso a internet**.

## El clúster es compartido

El entorno Apollo también atiende otros proyectos. Para estos proyectos:

- Use las particiones generales (`cpu_dev`, `cpu_small`, `cpu`, `cpu_long`). La partición `cpu_bpglsst` es exclusiva de miembros del BPG LSST.

- Si el proyecto tiene cuenta Slurm, indíquela en el envío (`#SBATCH --account=...`) y en el Jupyter de Open OnDemand. El identificador lo informa automáticamente la aplicación.

- En algunos períodos el clúster queda indisponible para la producción de *redshifts* fotométricos del programa in-kind BRA-LIN. Los usuarios son avisados por correo. Vea el aviso en [Cluster Apollo](./index.md).

Los límites de tiempo, los nodos y los detalles de un job están en [Slurm](./index.md#slurm). Ejemplos de script: [Scripts de Trabajo](../uso/templates-jobs.md).

## Reconocimiento

Las publicaciones, tesis y presentaciones que usen el entorno deben reconocer a LIneA. El texto sugerido para la comunidad brasileña y para proyectos de convocatoria pública (por ejemplo, SINCADA) está en [Créditos](../../creditos.md#uso-de-los-recursos-del-linea).

## Dónde ir después

| Necesito… | Página |
| --- | --- |
| Primer acceso desde el navegador | [Open OnDemand](../uso/openondemand.md) |
| Enviar jobs y usar scratch/scripts | [Cómo Utilizar](../uso/howtouse-HPC.md) |
| Hardware, particiones y Slurm | [Cluster Apollo](./index.md) |
| Contenedores | [Apptainer](../uso/apptainer-uso.md) |
| Áreas, cuotas y Lustre | [Almacenamiento](../../armazenamento/index.md) |
| Tutoriales en video | [Cursos y Tutoriales](../../cursos.md) |
| Dudas | [Soporte](../../suporte.md) |
