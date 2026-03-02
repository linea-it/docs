# Apptainer

## ¿Qué es?
El **[Apptainer](https://apptainer.org/docs/user/1.4/introduction.html)** es una plataforma de contenedores ampliamente utilizada en entornos científicos y de HPC. Fue diseñado para permitir la ejecución de aplicaciones complejas en clusters de forma **segura, portátil y reproducible**, garantizando que el entorno de ejecución sea idéntico independientemente del nodo donde se ejecute el job.

En entornos multiusuario, como los clusters, Apptainer se destaca por funcionar sin necesidad de privilegios administrativos, integrándose de manera natural con gestores de colas como Slurm.

Apptainer utiliza imágenes en el formato **SIF (_Singularity Image Format_)**, que consisten en un **archivo único e inmutable** que contiene todo el sistema necesario para ejecutar la aplicación, incluyendo sistema operativo, bibliotecas, dependencias y software adicional. Esto facilita el intercambio, el versionado y la reproducibilidad de los experimentos.

## Cómo utilizar Apptainer - Cluster Apollo
En el **Cluster Apollo**, el uso de Apptainer está permitido únicamente para la **ejecución de imágenes previamente construidas**.

Por motivos de seguridad y estandarización del entorno:
!!! WARNING "Atención:"
    **No está permitido realizar el build de imágenes (apptainer build) en los nodos del cluster.** 

Las imágenes deben ser construidas externamente (en una máquina local u otro entorno apropiado) y enviadas al cluster en formato `.sif`.

### Paso a paso de uso
1. **Subida de la imagen `.sif`**

    El usuario debe:

      - Acceder a la plataforma **[LIneA Open OnDemand](https://ondemand.linea.org.br)**
      - Subir el archivo `.sif` a su directorio de **scratch**
           <br>ruta: `/scratch/YOUR.USER/IMAGE_NAME.sif`</br> 

2. **Elegir la forma de ejecución**

El usuario puede ejecutar la aplicación de dos maneras principales:

   - **Opción A** – Envío mediante Slurm (Forma Recomendada):

Crear un script de envío, por ejemplo job_apptainer.sh:
```Shell
#!/bin/bash
#SBATCH -p <PARTITION>                       
#SBATCH --nodes=<QT-NODE>
#SBATCH --account=<ACCOUNT-NAME>	           
#SBATCH -J <JOB-NAME>           
#----------------------------------------------------------------------------#
echo "SLURM_NODELIST=$SLURM_NODELIST"

# Ruta de la imagen
IMAGE=/scratch/users/YOUR.USER/IMAGE_NAME.sif

# Ejecuta el comando dentro del contenedor
srun apptainer run --containall $IMAGE

echo "Finalizado"
```

   - **Opción B** – Asignación Interactiva:

Para pruebas o ejecución interactiva:

  1. Realice la asignación de un nodo de cómputo:
```Shell
salloc -p PARTITION-NAME --nodelist=NODE
ssh NODE
```

  2. Después de asignar el nodo y acceder a él vía SSH, ejecute:
```Shell
apptainer exec /scratch/YOUR.USER/IMAGE_NAME.sif YOUR_CODE
```

  3. O bien, si desea ingresar al entorno del contenedor:
```Shell
apptainer shell /scratch/YOUR.USER/IMAGE_NAME.sif
```
Esto abrirá una terminal dentro de la imagen. Dentro del contenedor, el usuario puede ejecutar comandos normalmente.

## Buenas Prácticas en el Cluster - Apptainer

✔ Ejecute siempre mediante Slurm  
✔ Utilice el `scratch` para los archivos de imágenes y datos temporales  
✔ Pruebe interactivamente antes de enviar jobs largos  
✔ Mantenga sus imágenes versionadas

Ante cualquier duda, póngase en contacto con el [Service Desk](http://localhost:8088/suporte.html).
