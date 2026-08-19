# Almacenamiento

Hay diferentes áreas de almacenamiento disponibles, cada una con una finalidad específica. Las áreas tienen distintas características de acceso, retención y backup.

## Visión general de las áreas de almacenamiento

<div style="text-align: center;">

  <img src="../../images/storages_diagram.png"

       style="width: 100%; max-width: 1200px; height: auto; box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2); border-radius: 6px;">

</div>

| Área       | Uso principal                                          | Limpieza automática                   | Backup | Acceso                                                     |
| ---------- | ------------------------------------------------------ | ------------------------------------- | ------ | ---------------------------------------------------------- |
| `/home`    | Archivos personales, configuraciones y entornos Python | No                                    | Sí     | Nodo de inicio de sesión del clúster y entorno Jupyter     |
| `/scratch` | Datos temporales de procesamiento                      | Después de 30 días sin modificaciones | No     | Todos los nodos del clúster                                |
| `/scripts` | Scripts de envío, entornos Python y kernels            | No                                    | No     | Todos los nodos del clúster                                |
| `/data`    | Almacenamiento de largo plazo                          | No                                    | No     | Todos los nodos del clúster (uso restringido bajo demanda) |


!!! info
    El acceso a `/data` se proporciona bajo demanda.

## `/scratch`

`/scratch` está destinado al almacenamiento temporal de datos utilizados durante el procesamiento en el HPC. Puede utilizarse para archivos de entrada, resultados intermedios y otros datos necesarios durante la ejecución de los jobs.

Los usuarios pueden acceder a su directorio de scratch mediante la variable de entorno o utilizando la ruta completa.

```Bash

cd $SCRATCH

```

Ou 

```Bash

cd /scratch/users/<username> 

```

!!! danger
    ¡Esta área NO tendrá backup!

Los archivos que no hayan sido modificados en los últimos 30 días se eliminarán automáticamente, por lo que esta área constituye un almacenamiento temporal.

Se recomienda que los usuarios transfieran los archivos importantes de `$SCRATCH` a su `homedir`.


!!! warning
    El script de limpieza se ejecuta una vez por semana, siempre durante los fines de semana.  

**La cuota predeterminada de `/scratch` disponible para los usuarios autorizados a utilizar el Cluster es:**

| area     | bsoft  | bhard  | isoft  | ihard  | grace period |
| -------- | ------ | ------ | ------ | ------ | ------------ |
| /scratch | 35 GB  | 40 GB  | 100000 | 120000 | 7 days       |

## `/data`

`/data` está destinado al almacenamiento de largo plazo. El acceso a esta área se proporciona bajo demanda.



## `/home`

`/home` está destinado a los archivos personales y las configuraciones del usuario. También puede utilizarse para almacenar entornos Python que se utilizarán en la plataforma Jupyter.
 [Jupyter Notebook](https://jupyter.linea.org.br).

**La cuota predeterminada del directorio home de cada usuario, según su perfil, se muestra a continuación:**

| perfil                 | bsoft  | bhard  | isoft   | ihard    | grace period |
| ---------------------- | ------ | ------ | ------- | -------- | ------------ |
| público general          | 5 GB   | 7 GB    | 7000    | 10000    | 7 dias       |
| público institucional  | 25 GB  | 30 GB   | 40000   | 50000    | 7 dias       |
| colaboración LSST       | 35 GB  | 40 GB   | 1000000 | 1200000  | 7 dias       |

!!! tip
    Para verificar los valores de cuota configurados, basta con utilizar el comando: `show_quota`.


Nota: El directorio `/home` del usuario **no** se ve afectado por el proceso de limpieza automática.

## `/scripts`

`/scripts` está destinado a scripts y entornos utilizados para ejecutar trabajos en el HPC. Es la ubicación recomendada para scripts de envío de jobs, entornos Python y kernels utilizados durante el procesamiento.

Los usuarios pueden acceder a su directorio de scripts mediante la variable de entorno o utilizando la ruta completa.

```Bash

cd $SCRIPTS

```

Ou 

```Bash

cd /scripts/<username> 

```

Esta área está destinada al almacenamiento de scripts de envío de jobs al clúster y otros. También se recomienda utilizar esta ruta para crear entornos (envs) Python y kernels.

**La cuota predeterminada de `/scripts` disponible para los usuarios es:**

| area     | bsoft | bhard | isoft | ihard | grace period |
| -------- | ----- | ----- | ----- | ----- | ------------ |
| /scripts | 10 GB | 12 GB | 100k  | 120k  | 7 days       |

Nota: El directorio `/scripts` **no** se ve afectado por el proceso de limpieza automática.



!!! info
    Aunque `/home` y `/scripts` pueden almacenar entornos Python, tienen finalidades diferentes. Los entornos almacenados en `/home` pueden utilizarse en Jupyter, mientras que `/scripts` es la ubicación recomendada para los entornos que se utilizarán en los nodos de procesamiento del HPC.


## NAS (NFS)

Los sistemas de almacenamiento NAS se utilizan para almacenamiento de largo plazo y no son accesibles a través de los nodos de procesamiento (HPC).

Características actuales: 

| Fabricante | Modelo | Capacidade | Instalado em | Disponibilidade |
| ---------- | -------------------- | ---------- | ------------ | --------------- |
| SGI | IS5600 | 240TB | Jul-2014 | En uso |
| HPE | APOLO 4510 | 1.2 PB | Apr-2025 | En uso |

## Backup

| áreas | backup incremental (diario) | backup completo (mensual) | retención |
| -------- | :-------------------------: | :-----------------------: | :------: |
| /home | :heavy_check_mark: | :heavy_check_mark: | 90 dias |
| /data | :x: | :x: | - |
| /scratch | :x: | :x: | - |
| /scripts | :x: | :x: | - |

!!! info
    Aunque no cuenta con una programación de backup, el volumen `/data` utiliza un sistema robusto de redundancia de discos que preserva la integridad de sus datos.

## Uso de Lustre

El entorno del clúster Apollo cuenta con un sistema de archivos de alto rendimiento [Lustre](https://www.lustre.org/) con dos niveles (*tiers*) de almacenamiento, uno en SSD con ~70 TB (*T0*) y otro en HDD con ~500 TB (*T1*), ambos conectados a una red InfiniBand EDR de 100 Gb/s. Los dos niveles de almacenamiento están disponibles en `/scratch` y `/data`.

### Buenas prácticas

Los sistemas de archivos distribuidos como Lustre son ideales para entornos HPC y HTC. En estos entornos, la carga de trabajo típica consiste en archivos grandes que deben ser accesibles desde muchos nodos de cómputo con un ancho de banda muy alto y/o baja latencia. Por lo tanto, estos sistemas de archivos son muy diferentes de los utilizados en computadoras de escritorio o servidores independientes. Aunque son excelentes para manejar archivos grandes, también presentan fuertes limitaciones al trabajar con archivos pequeños y patrones de acceso más comunes en entornos corporativos y de escritorio. Las operaciones que pueden ser extremadamente rápidas en el disco local de una estación de trabajo pueden ser dolorosamente lentas y costosas en un sistema de archivos Lustre, afectando tanto a los usuarios que realizan estas operaciones como, eventualmente, a todos los demás usuarios. Estas mejores prácticas y recomendaciones tienen como objetivo permitir un uso fluido de Lustre, minimizando o evitando operaciones innecesarias o muy costosas del sistema de archivos.

**Evite acceder a los atributos de archivos y directorios**

Acceder a información de metadatos, como atributos de archivos (por ejemplo, tipo, propietario, protección, tamaño, fechas, etc.) en Lustre consume muchos recursos y puede degradar el rendimiento del sistema de archivos, especialmente cuando se realiza con frecuencia o en directorios con una gran cantidad de archivos.

Minimice el uso de llamadas al sistema que acceden o modifican estos atributos, como `stat()`, `statx()`, `open()`, `openat()`, etc.

Lo mismo se aplica a comandos como `ls -l` (para todo el directorio) o `ls --color`, que utilizan las llamadas mencionadas anteriormente. En su lugar, utilice un `ls` simple o `ls -l filename`.

**Evite utilizar comandos que acceden masivamente a los metadatos**

Evite utilizar comandos como `ls -R`, `find`, `locate`, `du`, `df` y similares.

Estos comandos recorren el sistema de archivos de forma recursiva y/o ejecutan operaciones pesadas de metadatos. Son muy intensivos en el acceso a los metadatos del sistema de archivos y pueden degradar gravemente el rendimiento general del sistema. Si es absolutamente necesario recorrer el sistema de archivos de forma recursiva, utilice el comando proporcionado por Lustre `lfs find` en lugar de `find`, por ejemplo.

**Utilice el comando `lfs` de Lustre**

Para minimizar el número de llamadas RPC de Lustre, utilice los comandos `lfs` en lugar de los comandos proporcionados por el sistema siempre que sea posible:

* `lfs df` => en lugar de `df`

* `lfs find` => en lugar de `find`

**Evite utilizar comodines**

La expansión de comodines requiere muchos recursos. Ejecutar comandos con comodines sobre un gran número de archivos puede tardar mucho tiempo y afectar gravemente al rendimiento del sistema de archivos. En lugar de utilizar comodines, cree una lista de los archivos de destino y aplique el comando a cada uno de ellos.

**Acceso de solo lectura**

Siempre que sea posible, abra los archivos como solo lectura utilizando `O_RDONLY`. Además, si no necesita actualizar el tiempo de acceso al archivo, abra los archivos como `O_RDONLY | O_NOATIME`. Si la información del tiempo de acceso es necesaria durante la E/S paralela, deje que el proceso padre abra los archivos como `O_RDONLY` y que las demás clasificaciones abran los mismos archivos como `O_RDONLY|O_NOATIME`.

**Evite tener un gran número de archivos en un único directorio**

Cuando se accede a un archivo, Lustre bloquea el directorio padre. Cuando es necesario abrir muchos archivos del mismo directorio, esto genera contención. Escribir miles de archivos en un único directorio produce una carga masiva en los servidores de metadatos de Lustre, lo que generalmente provoca la desactivación de los sistemas de archivos. Acceder a un único directorio que contiene miles de archivos puede causar una gran contención de recursos y degradar el rendimiento del sistema de archivos.

La alternativa es organizar los datos en varios subdirectorios y dividir los archivos entre ellos. Un enfoque común es utilizar la raíz cuadrada del número de archivos; por ejemplo, para 90.000 archivos la raíz cuadrada sería 300, por lo que deberían crearse 300 directorios con 300 archivos cada uno.

**Evite los archivos pequeños**

Acceder a archivos pequeños en el sistema de archivos Lustre es muy ineficiente. El tamaño de archivo recomendado es superior a 1 GB. Reorganice los datos en archivos grandes o utilice formatos de archivo como **HDF5**. Alternativamente, si el tamaño total de los archivos es pequeño, como algunos gigabytes, copie los archivos pequeños a `/tmp` o a un directorio temporal local de cada nodo de cómputo al inicio del trabajo (no olvide transferir y/o eliminar los archivos al final). Este enfoque puede combinarse con herramientas de archivado como `tar`; los archivos pequeños almacenados en uno o más tarballs grandes pueden mantenerse en Lustre de manera más eficiente.

Al leer o escribir archivos, Lustre tiene un rendimiento mucho mejor con tamaños de búfer grandes (>= 1 MB). Se recomienda encarecidamente agrupar pequeñas operaciones de lectura y escritura en operaciones mayores. El búfer colectivo MPI-IO permite E/S agregada.

**Evite pequeñas operaciones repetitivas de archivos**

Evite realizar pequeñas operaciones de E/S repetitivas, como abrir archivos frecuentemente en modo de adición, escribir pequeñas cantidades de datos y cerrar el archivo. En su lugar, abra el archivo una vez, realice todas las operaciones de E/S y ciérrelo.

**Evite que varios procesos abran los mismos archivos al mismo tiempo**

Varios procesos que abren los mismos archivos al mismo tiempo pueden crear contención y errores al abrir archivos. En su lugar, realice la apertura desde un único proceso (padre), o abra el archivo en modo de solo lectura para evitar bloqueos, o implemente la apertura con un enfoque de reintento y espera en caso de error.

**Evite acceder a la misma región de un archivo desde muchos procesos**

Si varios procesos acceden a la misma región de un archivo al mismo tiempo, el gestor de bloqueos distribuido de Lustre impondrá la coherencia para que todos los clientes vean resultados consistentes. Tener muchos procesos intentando acceder a la misma región simultáneamente puede causar una degradación del rendimiento.

En este caso, puede ser preferible replicar el archivo, dividirlo, realizar las operaciones de E/S desde una única clasificación de proceso o garantizar que no se produzca acceso simultáneo. En cualquier caso, se recomienda mantener lo más pequeña posible la cantidad de operaciones de apertura y bloqueo de archivos en paralelo para reducir la contención.

Si varios procesos intentan añadir datos al mismo archivo, esto activará el bloqueo y puede causar una gran contención. Idealmente, solo un proceso debería añadir datos a cada archivo.

**Operaciones de archivo a través del proceso padre**

Al acceder a archivos pequeños compartidos en una tarea paralela, a menudo es más eficiente realizar todas las operaciones necesarias mediante el proceso padre y, si es necesario, transmitir los datos a otras clasificaciones, en lugar de que todas las clasificaciones accedan a los mismos archivos. Del mismo modo, si varias clasificaciones de un trabajo paralelo necesitan información sobre un archivo determinado, el enfoque más eficiente es que el proceso padre realice las llamadas necesarias (por ejemplo, `stat()`, `fstat()`, etc.) y luego transmita la información a las demás clasificaciones.

**Distribución de archivos (striping)**

En Lustre, los archivos grandes pueden dividirse en segmentos que, a su vez, pueden distribuirse automáticamente entre varios dispositivos de almacenamiento. La distribución de archivos es útil para E/S paralela en archivos grandes. Para que esto funcione, el punto de montaje en cuestión debe apuntar a varios dispositivos de almacenamiento (OSTs). El comando `lfs df` puede utilizarse para comprobar si un determinado punto de montaje apunta a varios OSTs. Para obtener información sobre la distribución de un archivo determinado, utilice:

`lfs getstripe filename`

La distribución de archivos puede configurarse mediante el comando `lfs setstripe`. Si el comando se aplica a un directorio, establecerá la configuración de distribución predeterminada para los archivos creados en ese directorio. Un subdirectorio hereda todas las configuraciones de distribución de su directorio padre. Si el comando se aplica a un archivo, distribuirá ese archivo entre los OSTs según la configuración especificada.

`lfs setstripe -s 128m -c 8 filename` => divide el archivo en segmentos de 128 MB y los distribuye en 8 OSTs

Si un archivo grande es compartido en paralelo por varios procesos, y cada proceso trabaja en su propia parte del archivo, puede ser útil dividir el archivo en un número de segmentos igual al número de procesos, o un múltiplo del número de procesos.

Para obtener el máximo rendimiento, las solicitudes de E/S deben estar alineadas con las franjas, lo que significa que los procesos que acceden al archivo deben hacerlo en desplazamientos que correspondan a los límites de las franjas. Esto minimiza las posibilidades de que un proceso tenga que acceder a más de un segmento (y más de un OST) para obtener los datos necesarios.

Para archivos pequeños, la distribución (striping) debe deshabilitarse; esto puede lograrse estableciendo un recuento de distribución de 1. Lo mismo se aplica si un archivo grande es accedido por un único proceso.

`lfs setstripe -s 1m -c 1 meudiretorio/arquivospequenos/`

**Evite instalar software en Lustre**

El software generalmente está compuesto por muchos archivos pequeños y, como se mencionó anteriormente, acceder a muchos archivos pequeños en Lustre puede sobrecargar los servidores de metadatos. Las compilaciones de software, en particular, pueden ejecutarse mejor localmente copiando o descomprimiendo el software en `/tmp/$USER/` o en su `homedir`.

Además, bajo una carga elevada, el acceso de E/S a los sistemas de archivos Lustre puede bloquearse. Si los ejecutables se almacenan en Lustre y el acceso al sistema de archivos falla, los ejecutables pueden bloquearse. Por lo tanto, siempre que sea posible, es mejor copiar los ejecutables a `/tmp` de los nodos del clúster.

### Comandos útiles

a) ¿Cómo puedo comprobar mi cuota disponible?

`    show_quota`

b) ¿Cómo puedo comprobar la cuota de un proyecto?

`    show_proj_quota <projeto>`

c) ¿Cómo puedo consultar mis archivos creados hace *más* de 30 días?

`    lfs find $SCRATCH --uid $UID -mtime +30 --print`

d) ¿Cómo puedo consultar mis archivos creados hace *menos* de 30 días?

`    lfs find $SCRATCH --uid $UID -mtime -30 --print`

e) ¿Cómo puedo listar los OSTs de Lustre?

`    lfs osts $SCRATCH`

f) ¿Cómo puedo listar los archivos almacenados durante más de 30 días en un OST específico de Lustre?

`    lfs find $SCRATCH -mtime +30 --print --obd t0-OST0002_UUID`

g) ¿Cómo puedo configurar el striping en un directorio para "romper" los archivos y distribuir esas "piezas" en 10 OSTs?

`    lfs setstripe -c 10 $SCRATCH/meus_arquivos_grandes`

h) ¿Cómo puedo consultar el striping de archivos/directorios?

`    lfs setstripe -c $SCRATCH/meus_arquivos_grandes`

!!! tip
    El Lustre de LIneA fue diseñado para trabajar a 100 Gbps. Para alcanzar el máximo rendimiento, utilice striping y siempre archivos grandes (+1GB).

## Referencias

Estas mejores prácticas fueron compiladas a partir de la experiencia del equipo de LIneA y de las siguientes fuentes:

1. https://www.nas.nasa.gov/hecc/support/kb/lustre-best-practices_226.html

1. https://hpcf.umbc.edu/general-productivity/lustre-best-practices/

1. https://wiki.gsi.de/foswiki/bin/view/Linux/LustreFs

1. https://doc.lustre.org/lustre_manual.pdf
