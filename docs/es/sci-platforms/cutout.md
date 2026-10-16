El [*Cutout Service*](https://cutout.linea.org.br) entrega recortes de imagen de los relevamientos alojados en LIneA. *DES* DR2 es público. Cada usuario solo accede a los demás relevamientos para los que tiene derechos de acceso (*data rights*).

La interfaz web y la API REST son formas independientes de acceder al servicio. El formulario no se utiliza en los ejemplos de código, y los ejemplos de código no dependen de él.

---

## Interfaz web

Abra [https://cutout.linea.org.br](https://cutout.linea.org.br). Para *DES* DR2, el formulario queda a la izquierda y el mapa *HiPS* (*Aladin Lite*) a la derecha.

1. En **Survey**, seleccione `DES DR2`.
2. Indique **RA (deg)** y **Dec (deg)** en grados decimales ICRS, o centre el mapa y use el botón de búsqueda para rellenar el centro del campo.
3. **Radius (arcmin)** es el radio en minutos de arco. El máximo es 30′.
4. En **Format**, elija `FITS` o `PNG`. En el PNG, **RGB color composite** arma el color; en *DES* DR2 los presets son `gri`, `riz` e `izY`. Sin el color compuesto, **Band** elige un filtro (`g`, `r`, `i`, `z`, `Y`).
5. Para radios de hasta 5′, **Download** devuelve el archivo inmediatamente. Para radios mayores, el botón pasa a ser **Queue job**.
6. **Add region** acumula posiciones y **Queue jobs** envía cada una por separado. En **Bulk Coordinates** puede pegar pares `RA Dec` o un CSV con encabezado `ra,dec` y columnas opcionales `band,format,radius,color,rgb_bands`. El `radius` de esa lista también se indica en minutos de arco.
7. **Queued Jobs** muestra el progreso y descarga los archivos, también en un archivo ZIP.

!!! warning "La lista queda en el navegador en el que abrió el Cutout Service, y el archivo se elimina del servidor después de 7 días"
    **Queued Jobs** se guarda en el navegador en el que abrió [cutout.linea.org.br](https://cutout.linea.org.br). Puede cerrar esa pestaña y volver a ella. La lista no aparece en otro navegador ni en otra computadora.
    El job asíncrono y el archivo se eliminan del servidor **7 días** después de su creación. Descárguelo antes. Después de ese plazo, la lista todavía puede aparecer en ese mismo navegador, pero la descarga ya no encontrará el archivo.

En **Survey**, después del inicio de sesión, aparecen los relevamientos públicos y aquellos para los que la cuenta tiene derechos de acceso.

---

## API REST

Los ejemplos de abajo acceden a

```text
https://cutout.linea.org.br
```

desde Jupyter. *DES* DR2 (`ID=des_dr2`) no requiere cuenta. Todos los recortes síncronos de esta página usan un radio de **0,5′**, inferior a 1′.

| Uso | Método y ruta | Respuesta |
|-----|---------------|------------|
| Recorte inmediato | `GET` o `POST /api/sync` | El archivo, `application/fits` o `image/x-png` |
| Crear un job | `POST /api/async` | `303 See Other`, encabezado `Location` y un JSON del job |
| Consultar un job | `GET /api/async/{job_id}` | JSON con fase, parámetros, tareas y resultados |
| Fase | `GET /api/async/{job_id}/phase` | Texto plano: `QUEUED`, `EXECUTING`, `COMPLETED`, `ERROR` o `ABORTED` |
| Interrumpir | `POST /api/async/{job_id}/phase` con `PHASE=ABORT` | La fase nueva, en texto |
| Descargar | `GET` de la `download_url` del resultado | El archivo |

`GET /api/async` lista los jobs de la cuenta o, cuando no hay token, los jobs de la sesión anónima.

En el Jupyter de LIneA, `requests`, `astropy` y `matplotlib` ya están en el entorno. Fuera de él: `pip install requests astropy matplotlib`.

### Token

El token identifica su cuenta ante la API. Con él, el servicio permite acceder a los relevamientos para los que usted tiene derechos de acceso y recuperar un job desde otra máquina. *DES* DR2 no lo necesita.

1. Entre en [https://cutout.linea.org.br](https://cutout.linea.org.br).
2. Haga clic en su nombre de usuario, en la esquina superior derecha.
3. Seleccione **API token**. La página también es [https://cutout.linea.org.br/users/~token/](https://cutout.linea.org.br/users/~token/).
4. Copie la clave que aparezca.

Guarde la clave en la variable de entorno `CUTOUT_TOKEN`. El valor de abajo es falso: sustitúyalo por la clave de su página. **Regenerate token** invalida la clave anterior y crea otra.

```bash
export CUTOUT_TOKEN="0123456789abcdef0123456789abcdef01234567"
```

Si el notebook ya está abierto, el `export` no estará disponible en él. En ese caso, defina la variable en la primera celda usando su clave:

```python
import os
os.environ["CUTOUT_TOKEN"] = "0123456789abcdef0123456789abcdef01234567"
```

El pedido lee esa variable y añade `Token` y un espacio delante de la clave. El ejemplo descarga una imagen de LSST DP1, que es un relevamiento con acceso restringido. La cuenta necesita tener derechos de acceso a él. Para *DES* DR2, esta autenticación no es necesaria.

```python
import os
import requests
from IPython.display import Image, display

session = requests.Session()
session.headers["Authorization"] = f"Token {os.environ['CUTOUT_TOKEN']}"

response = session.get(
    "https://cutout.linea.org.br/api/sync",
    params={
        "ID": "lsst_dp1",
        "POS": "CIRCLE 53.096 -28.024 0.008333",  # 0,5 arcmin
        "BAND": "g",
        "format": "png",
    },
    timeout=60,
)
response.raise_for_status()
display(Image(data=response.content))
```

!!! warning "Mantenga su clave segura"
Quien tenga la clave podrá utilizar su cuenta. No la envíe por mensaje ni deje la clave real en un notebook que vaya a compartir.

Sin token, un job creado en `/api/async` queda asociado a la cookie `sessionid` enviada en la respuesta. Las llamadas siguientes tienen que reutilizar esa cookie; una `requests.Session()` lo hace automáticamente. Un `requests.get` independiente sobre la URL del resultado responde `403`.

### Subconjunto SODA implementado por el servicio

Las solicitudes usan los parámetros de [*SODA*](https://www.ivoa.net/documents/SODA/){:target="_blank"}. El ciclo de vida del job asíncrono sigue [*UWS*](https://www.ivoa.net/documents/UWS/20161024/REC-UWS-1.1-20161024.html){:target="_blank"}. Los nombres no distinguen entre mayúsculas y minúsculas: `ID` e `id` son el mismo parámetro, igual que `POS`, `BAND` y `PHASE`.

`ID` identifica el *release*. En *DES* DR2, el valor es `des_dr2`. `BAND` identifica el filtro de ese *release* (`g`, `r`, `i`, `z`, `Y`).

`POS` describe la región en grados ICRS:

| Forma | Sintaxis | Recorte |
|-------|----------|---------|
| `CIRCLE` | `CIRCLE ra dec radio` | Recorte cuadrado cuyo lado mide el doble del radio. El radio es un ángulo en el cielo, en grados. |
| `RANGE` | `RANGE ra_min ra_max dec_min dec_max` | Rectángulo definido por los límites de ascensión recta y declinación. |
| `POLYGON` | `POLYGON ra1 dec1 ra2 dec2 ...` | Caja que contiene los vértices. Se necesitan al menos tres vértices, proporcionados en sentido antihorario. |

Cada pedido acepta un `ID` y un `POS`. Repetir cualquiera de los dos responde `422`.

!!! warning "`RANGE` es rechazado por la comprobación del radio"
    Actualmente, un `POS=RANGE` pequeño recibe `422` con un mensaje que indica que el radio supera 30′. La comprobación lee los números de la caja en el orden equivocado. Por eso, los ejemplos usan `CIRCLE` y `POLYGON`.

El modo síncrono acepta una banda. El modo asíncrono puede recibir más de una `BAND` y crea una tarea por filtro. Al crear el job, `PHASE=RUN` inicia su ejecución. La respuesta es `303`, con `Location` apuntando a `/api/async/{job_id}`. El documento del job es JSON. La fase, en `/phase`, es texto plano.

Este subconjunto no implementa, a propósito:

- Documento XML del job, capabilities VOSI y un endpoint de ejemplos DALI. El cliente recomendado es `requests`.
- `DESTRUCTION` y `EXECUTIONDURATION` definidos por el cliente. El job asíncrono y el archivo se eliminan **7 días** después de su creación.
- Códigos de error en texto plano. Los errores se devuelven como JSON `{"detail": "..."}`.
- Parámetros adicionales, fuera de SODA: `format` (`fits` o `png`), `color` y `rgb_bands`.

### Parámetros

| Parámetro | Obligatorio | Valor |
|-----------|-------------|-------|
| `ID` | sí | Identificador del *release*. En los ejemplos públicos, `des_dr2`. Un *release* con acceso restringido solo responde si la cuenta tiene los derechos correspondientes. |
| `POS` | sí | `CIRCLE` o `POLYGON`, como arriba. |
| `BAND` | sí | Filtro. En *DES* DR2: `g`, `r`, `i`, `z`, `Y`. |
| `format` | no | `fits` (predeterminado) o `png`. |
| `color` | no | `true` con `format=png` arma el PNG en color a partir de `rgb_bands`. |
| `rgb_bands` | no | Tres filtros, como `gri`, `g,r,i` o `g r i`. El predeterminado es `gri`. |
| `PHASE` | en asíncrono | `RUN` al crear. `ABORT` interrumpe un job que sigue en curso. |
| `RUNID` | no | Etiqueta opaca, devuelta en el campo `run_id` del job. |

El radio de `CIRCLE` está en **grados**. Un minuto de arco es `1/60` de grado.

```text
0,5′ = 0,008333°
1′   = 0,016667°
5′   = 0,083333°
```

Por encima de 5′ `/api/sync` responde `422` y pide `/api/async`. El radio tiene que ser menor que 30′.

### Por qué algunos recortes tardan

Cuatro causas cubren casi todo cutout lento.

- **Área.** El lado crece con el radio y el número de píxeles crece con el cuadrado de ese radio. 0,5′ es un cuadrado de 228 píxeles, de unos 0,2 MB en FITS. 5′ supera los 2 000 píxeles y llega a unos 20 MB.
- **Mosaico.** El coadd de *DES* DR2 está dividido en una cuadrícula de tiles. Si el campo atraviesa más de un tile, el servicio abre cada imagen y combina los datos. Los tiles que no están en la misma cuadrícula también deben reproyectarse.
- **Color.** `color=true` lee tres filtros. Si cada filtro cruza varios tiles, son tres mosaicos antes de la composición RGB.
- **Borde del *footprint*.** Fuera del área cubierta por el *release*, la respuesta es `422` (`No files found for band ...`). En el borde, el archivo todavía se devuelve y los píxeles sin tile tienen valor cero.

Un campo de 0,5′ en un solo tile se procesa en cerca de 1 segundo. El mismo centro con un radio de 6′, en la banda `g`, tardó unos 30 segundos y generó un archivo de 30 MB, porque el cuadrado cubre varios tiles.

### Ejemplos en Jupyter

La posición es RA = 41,22° y Dec = −35,56°, dentro de *DES* DR2. `display` muestra la imagen en el notebook. Las figuras de esta página se generaron con esas mismas llamadas.

#### PNG de una banda

```python
import requests
from IPython.display import Image, display

response = requests.get(
    "https://cutout.linea.org.br/api/sync",
    params={
        "ID": "des_dr2",
        "POS": "CIRCLE 41.22 -35.56 0.008333",  # 0,5 arcmin
        "BAND": "g",
        "format": "png",
    },
    timeout=60,
)
response.raise_for_status()
display(Image(data=response.content))
```

![DES DR2, banda g, radio de 0,5 minuto de arco](../../images/cutout-desdr2-g.png){ width="320" }

La banda `i` del mismo campo cambia solo el filtro. El PNG monocromático ya trae el stretch del servicio, listo para mirar.

```python
response = requests.get(
    "https://cutout.linea.org.br/api/sync",
    params={
        "ID": "des_dr2",
        "POS": "CIRCLE 41.22 -35.56 0.008333",
        "BAND": "i",
        "format": "png",
    },
    timeout=60,
)
response.raise_for_status()
display(Image(data=response.content))
```

![DES DR2, banda i, radio de 0,5 minuto de arco](../../images/cutout-desdr2-i.png){ width="320" }

#### PNG en color

`color=true` lee `g`, `r` e `i` y produce una imagen RGB. `BAND` sigue presente en la solicitud.

```python
response = requests.get(
    "https://cutout.linea.org.br/api/sync",
    params={
        "ID": "des_dr2",
        "POS": "CIRCLE 41.22 -35.56 0.008333",
        "BAND": "g",
        "format": "png",
        "color": "true",
        "rgb_bands": "gri",
    },
    timeout=60,
)
response.raise_for_status()
display(Image(data=response.content))
```

![DES DR2, composición gri, radio de 0,5 minuto de arco](../../images/cutout-desdr2-gri.png){ width="320" }

`riz` es la otra composición habitual. La llamada es la misma, con `rgb_bands` cambiado.

```python
response = requests.get(
    "https://cutout.linea.org.br/api/sync",
    params={
        "ID": "des_dr2",
        "POS": "CIRCLE 41.22 -35.56 0.008333",
        "BAND": "r",
        "format": "png",
        "color": "true",
        "rgb_bands": "riz",
    },
    timeout=60,
)
response.raise_for_status()
display(Image(data=response.content))
```

![DES DR2, composición riz, radio de 0,5 minuto de arco](../../images/cutout-desdr2-riz.png){ width="320" }

#### Polígono

`POLYGON` produce el recorte de la caja que contiene los vértices. Las cuatro esquinas de abajo abarcan cerca de 0,5′ en declinación, todavía en una llamada síncrona.

```python
response = requests.get(
    "https://cutout.linea.org.br/api/sync",
    params={
        "ID": "des_dr2",
        "POS": "POLYGON 41.216 -35.564 41.224 -35.564 41.224 -35.556 41.216 -35.556",
        "BAND": "r",
        "format": "png",
    },
    timeout=60,
)
response.raise_for_status()
display(Image(data=response.content))
```

![DES DR2, banda r, recorte poligonal](../../images/cutout-desdr2-polygon-r.png){ width="280" }

#### FITS para medir

El FITS almacena cuentas por segundo, en `float32`, sin el *stretch* aplicado al PNG. Para visualizar las galaxias en el notebook, ajuste la escala de brillo. Este archivo tiene 228×228 píxeles y cerca de 0,2 MB.

```python
import requests
from io import BytesIO
import matplotlib.pyplot as plt
from astropy.io import fits
from astropy.visualization import AsinhStretch, ImageNormalize, ZScaleInterval

response = requests.get(
    "https://cutout.linea.org.br/api/sync",
    params={
        "ID": "des_dr2",
        "POS": "CIRCLE 41.22 -35.56 0.008333",  # 0,5 arcmin
        "BAND": "g",
        "format": "fits",
    },
    timeout=60,
)
response.raise_for_status()

data = fits.open(BytesIO(response.content))[0].data
norm = ImageNormalize(data, interval=ZScaleInterval(), stretch=AsinhStretch())
plt.figure(figsize=(4, 4))
plt.imshow(data, origin="lower", cmap="gray", norm=norm)
plt.title("DES DR2 g")
plt.tight_layout()
plt.show()
```

`origin="lower"` coloca el norte arriba en esta imagen FITS. El PNG de la banda `g`, mostrado arriba, representa la misma región del cielo, ya con el *stretch* aplicado por el servicio.

#### Por encima de 5′

El endpoint síncrono rechaza los radios grandes. Un `CIRCLE` de 6′ en `/api/sync` devuelve:

```json
{"detail":"Cutout radius 6.0 arcmin exceeds the synchronous limit of 5 arcmin. Please use POST /api/async instead. Expected processing time is 36s."}
```

El job asíncrono usa la misma `Session` del `POST` hasta la descarga. Sin token, ella lleva la cookie. Con token, descomente el encabezado y la cookie deja de hacer falta. `allow_redirects=False` conserva el `Location`. La fase es texto, no JSON.

Descargue el archivo en un plazo de **7 días**. Después de eso el servidor borra el job y el resultado.

Este ejemplo es deliberadamente mayor: atraviesa varios tiles y genera cerca de 30 MB. La visualización con `imshow` es la misma que se usa para el FITS pequeño.

```python
import time
import requests
from io import BytesIO
import matplotlib.pyplot as plt
from astropy.io import fits
from astropy.visualization import AsinhStretch, ImageNormalize, ZScaleInterval

session = requests.Session()
# session.headers["Authorization"] = f"Token {os.environ['CUTOUT_TOKEN']}"

response = session.post(
    "https://cutout.linea.org.br/api/async",
    data={
        "ID": "des_dr2",
        "POS": "CIRCLE 41.22 -35.56 0.100000",  # 6 arcmin
        "BAND": "g",
        "format": "fits",
        "PHASE": "RUN",
    },
    allow_redirects=False,
    timeout=60,
)
response.raise_for_status()
job_url = response.headers["Location"]

phase = ""
for _ in range(40):
    phase = session.get(f"{job_url}/phase", timeout=60).text.strip()
    if phase in {"COMPLETED", "ERROR", "ABORTED"}:
        break
    time.sleep(3)
else:
    raise TimeoutError(phase)

detail = session.get(job_url, timeout=60).json()
if phase != "COMPLETED":
    message = detail["tasks"][0].get("error_message")
    raise RuntimeError(f"{phase}: {message}")

fits_response = session.get(detail["results"][0]["download_url"], timeout=300)
fits_response.raise_for_status()
data = fits.open(BytesIO(fits_response.content))[0].data

norm = ImageNormalize(data, interval=ZScaleInterval(), stretch=AsinhStretch())
plt.figure(figsize=(6, 6))
plt.imshow(data, origin="lower", cmap="gray", norm=norm)
plt.title("DES DR2 g, 6 arcmin")
plt.tight_layout()
plt.show()
```
