## LustreFS (HPC) 

El entorno del clúster Apollo cuenta con un sistema de archivos de alto rendimiento [Lustre](https://www.lustre.org/) con dos niveles (tiers) de almacenamiento: uno en SSD con ~70 TB (T0) y otro en HDD con ~500 TB (T1), ambos conectados a una red Infiniband EDR de 100 Gb/s. Los dos niveles de almacenamiento están disponibles en `/lustre/t0` y `/lustre/t1`.

Los usuarios podrán acceder a su directorio de scratch mediante la variable de entorno `$SCRATCH`, o accediendo al directorio ubicado en `/lustre/t0/scratch/users/<username>`.

### Buenas prácticas

Los sistemas de archivos distribuidos como Lustre son ideales para entornos HPC y HTC. En estos entornos, la carga de trabajo típica consiste en archivos grandes que necesitan ser accedidos desde muchos nodos de computación con gran ancho de banda y/o baja latencia. Por lo tanto, estos sistemas de archivos son muy diferentes a los usados en computadoras de escritorio o servidores aislados. Aunque son excelentes para manejar archivos grandes, también presentan fuertes limitaciones al tratar con archivos pequeños y patrones de acceso más comunes en entornos corporativos y de escritorio. Las operaciones que pueden ser extremadamente rápidas en un disco local de estación de trabajo pueden ser dolorosamente lentas y costosas en Lustre, afectando tanto a los usuarios que realizan estas operaciones como, eventualmente, a todos los demás usuarios. Estas mejores prácticas tienen como objetivo permitir un uso tranquilo de Lustre, minimizando operaciones innecesarias o muy costosas.

**Evite acceder a atributos de archivos y directorios**

Acceder a metadatos como atributos de archivo (tipo, propiedad, protección, tamaño, fechas, etc.) en Lustre consume muchos recursos y puede degradar el rendimiento, especialmente cuando se realiza con frecuencia o en directorios con muchos archivos. Minimice el uso de llamadas al sistema que accedan o modifiquen estos atributos, como `stat()`, `statx()`, `open()`, `openat()`, etc.

Lo mismo aplica para comandos como `ls -l` o `ls --color` que usan estas llamadas. En su lugar, use un simple `ls` o `ls -l nombrearchivo`.

**Evite comandos que accedan masivamente a metadatos**

Evite comandos como `ls -R`, `find`, `locate`, `du`, `df` y similares. Estos comandos recorren recursivamente el sistema de archivos y/o realizan operaciones pesadas de metadatos. Son muy intensivos en acceso a metadatos y pueden degradar gravemente el rendimiento. Si es absolutamente necesario recorrer recursivamente el sistema, use el comando `lfs find` de Lustre en lugar de `find`.

**Use el comando lfs de Lustre**

Para minimizar llamadas RPC a Lustre, siempre que sea posible use los comandos `lfs` en lugar de los del sistema:

* `lfs df` => en lugar de `df`
* `lfs find` => en lugar de `find`

**Evite usar comodines**

La expansión de comodines requiere muchos recursos. Ejecutar comandos con comodines en muchos archivos puede tomar mucho tiempo y afectar gravemente el rendimiento. En lugar de usar comodines, cree una lista de archivos objetivo y aplique el comando a cada uno.

**Acceso de solo lectura**

Cuando sea posible, abra archivos como solo lectura con `O_RDONLY`, y si no necesita actualizar el tiempo de acceso, use `O_RDONLY | O_NOATIME`. Si se necesita información de tiempo de acceso durante E/S paralela, haga que el proceso padre abra los archivos como `O_RDONLY` y los demás como `O_RDONLY|O_NOATIME`.

**Evite muchos archivos en un solo directorio**

Cuando se accede a un archivo, Lustre bloquea su directorio padre. Muchos archivos en el mismo directorio crean contención. Escribir miles de archivos en un directorio sobrecarga los servidores de metadatos, a menudo resultando en indisponibilidad del sistema. Acceder a un directorio con miles de archivos puede causar gran contención.

La alternativa es organizar los datos en múltiples subdirectorios. Un enfoque común es usar la raíz cuadrada del número de archivos - para 90,000 archivos crear 300 directorios con 300 archivos cada uno.

**Evite archivos pequeños**

Acceder a archivos pequeños en Lustre es muy ineficiente. El tamaño recomendado es mayor a 1 GB. Reorganice datos en archivos grandes o use formatos como **HDF5**. Alternativamente, si el tamaño total es pequeño (pocos GB), copie los archivos a `/tmp` o a un directorio temporal local al inicio del trabajo (no olvide transferir/eliminar al final). Esto puede combinarse con herramientas como `tar` para almacenar archivos pequeños en tarballs grandes.

Al leer/escribir archivos, Lustre funciona mucho mejor con buffers grandes (>= 1 MB). Se recomienda agregar pequeñas operaciones de E/S en operaciones mayores. MPI-IO permite E/S agregada.

**Evite pequeñas operaciones repetitivas**

Evite realizar pequeñas operaciones de E/S repetitivas como abrir frecuentemente archivos en modo append, escribir poco y cerrar. En su lugar, abra el archivo una vez, realice todas las operaciones y luego cierre.

**Evite múltiples procesos abriendo los mismos archivos simultáneamente**

Esto puede crear contención y errores. En su lugar, realice la apertura desde un proceso padre, o abra como solo lectura para evitar bloqueos, o implemente un enfoque de reintento con espera ante errores.

Evite múltiples procesos accediendo a la misma región de archivo
Si múltiples procesos acceden a la misma región simultáneamente, el administrador de bloqueos de Lustre forzará coherencia, lo que puede degradar el rendimiento.
En este caso, puede ser preferible: replicar el archivo, dividirlo, realizar E/S desde un solo proceso, o asegurar que no habrá acceso simultáneo. Se recomienda minimizar operaciones paralelas de apertura/bloqueo.

Si múltiples procesos intentan anexar al mismo archivo, esto causará contención. Idealmente, solo un proceso debe anexar a cada archivo.

**Operaciones de archivo a través del proceso padre**

Al acceder a archivos pequeños compartidos en tareas paralelas, suele ser más eficiente que el proceso padre realice todas las operaciones necesarias y, si es necesario, transmita los datos a otros procesos. Similarmente, si múltiples procesos necesitan información sobre un archivo, es mejor que el proceso padre realice las llamadas necesarias (ej. `stat()`, `fstat()`) y luego transmita la información.

**Distribución de archivos (striping)**

En Lustre, archivos grandes pueden dividirse en segmentos distribuidos automáticamente entre múltiples dispositivos de almacenamiento. La distribución es útil para E/S paralela en archivos grandes. Para que funcione, el punto de montaje debe apuntar a múltiples dispositivos (OSTs). Use `lfs df` para verificar esto. Para obtener información de distribución de un archivo:

`lfs getstripe nombrearchivo`

La distribución puede configurarse con `lfs setstripe`. Si se aplica a un directorio, establece la configuración por defecto para archivos creados allí. Un subdirectorio hereda la configuración de su padre. Si se aplica a un archivo, distribuye ese archivo según la configuración.

`lfs setstripe -s 128m -c 8 nombrearchivo` => divide el archivo en segmentos de 128 MB distribuidos en 8 OSTs

Si un archivo grande es compartido en paralelo por múltiples procesos, puede ser útil dividirlo en un número de segmentos igual al número de procesos.

Para máximo rendimiento, las solicitudes de E/S deben alinearse con los segmentos, accediendo en offsets que coincidan con los límites de los segmentos.
Para archivos pequeños, desactive la distribución estableciendo un conteo de 1. Lo mismo aplica si un archivo grande es accedido por un solo proceso.

`lfs setstripe -s 1m -c 1 midir/archivospequenos/`

**Evite instalar software en Lustre**

El software generalmente consiste en muchos archivos pequeños, y como se mencionó, acceder a muchos archivos pequeños puede sobrecargar los servidores de metadatos. Las compilaciones en particular se realizan mejor localmente copiando/descomprimiendo el software en `/tmp/$USER/` o en el `homedir`.

Además, bajo alta carga, el acceso a Lustre puede bloquearse. Si los ejecutables están en Lustre y el acceso falla, pueden colapsar. Por lo tanto, es mejor copiar los ejecutables al `/tmp` de los nodos.

### Cuota

|area|TB  |bsoft|bhard|isoft|ihard|grace period|
|----|----|-------------|-------------|-------------|-------------|------------|
|T0 | 70 |     200 GB    |   250 GB   | 200000 | 250000  |  7 days    |

### Área de scratch

Los archivos no modificados en los últimos 60 días serán eliminados automáticamente.

!!! critical
    ¡Esta área NO tiene backup y NO se enviarán avisos de eliminación!

!!! warning
    El script de limpieza se ejecuta semanalmente los fines de semana.


### Comandos útiles

a) ¿Cómo acceder a mi área de scratch?
   
    cd $SCRATCH 
    
b) ¿Cómo consultar mi cuota disponible?

    lfs quota -u $USER /lustre/t0
    
c) ¿Cómo consultar mis archivos creados hace más de 60 días?

    lfs find $SCRATCH --uid $UID -mtime +60 --print

d) ¿Cómo consultar mis archivos creados hace menos de 60 días?

    lfs find $SCRATCH --uid $UID -mtime -60 --print
    
e) ¿Cómo listar los OSTs de Lustre?

    lfs osts /lustre/t0

f) ¿Cómo listar archivos mayores a 60 días en un OST específico?

    lfs find $SCRATCH -mtime +60 --print --obd t0-OST0002_UUID
    
g) ¿Cómo configurar striping en un directorio para "dividir" archivos y distribuir "trozos" en 10 OSTs?

    lfs setstripe -c 10 $SCRATCH/mis_archivos_grandes
    
h) ¿Cómo consultar el striping de archivos/directorios?

    lfs getstripe $SCRATCH/mis_archivos_grandes

!!! tip
    El Lustre de LIneA está diseñado para trabajar a 100Gbps - para máximo rendimiento use striping y siempre con archivos grandes (+1GB).

## NAS (NFS)

Los sistemas NAS se usan para almacenamiento a largo plazo y no son accesibles desde los nodos de cómputo (HPC).

Características actuales:

| Fabricante | Modelo | Capacidad | Instalado |
| ------- | ------ | ------------ | -----------| 
| SGI     | IS5500<sup>[1]</sup> | 540TB        |  Dic-2011  |
| SGI     | IS5600 | 240TB        |  Jul-2014  |

<sup>[1]</sup> _este equipo fue desactivado en Jun/2023 por problemas físicos._

### /home

El directorio `home` es para que los usuarios almacenen archivos personales y es accesible desde los nodos de login del clúster y la plataforma [jupyter](.).

### /archive

Área para almacenar datos crudos de catálogos astronómicos transferidos desde otros centros de datos o producidos internamente por las plataformas de LIneA.

### /process

Área para almacenar datos provenientes del procesamiento del DES realizados por el [Portal del DES](https://des-portal.linea.org.br).

### Cuota

|area| bsoft|bhard|isoft|ihard|grace period|
|----| -------------|-------------|-------------|-------------|------------|
|/home  |   40 GB    |   50 GB   | 4000000  | 5000000   |  7 days    |

!!! tip
    Para verificar las cuotas configuradas use: `quota -s -u <usuario> /home`.

## Backup

| áreas | frecuencia | tipo | retención |
| ----- | ---------- | ---- | ---------------- |
| /home | diario | incremental | 30 días |
| /home | semanal | diferencial | 30 días |
| /home | mensal | completo | 90 días |
| /archive | - | - | - |
| /scratch | - | - | - |

## Referencias

Estas mejores prácticas fueron compiladas de la experiencia del equipo de LIneA y las siguientes fuentes:

1. https://www.nas.nasa.gov/hecc/support/kb/lustre-best-practices_226.html
1. https://hpcf.umbc.edu/general-productivity/lustre-best-practices/
1. https://wiki.gsi.de/foswiki/bin/view/Linux/LustreFs
1. https://doc.lustre.org/lustre_manual.pdf
