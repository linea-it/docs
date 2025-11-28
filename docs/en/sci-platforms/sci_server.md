
The [*DES Science Server*](https://scienceserver.linea.org.br/) is an image and catalogue data visualisation service developed as part of LIneA's contribution to the [*Dark Energy Survey*](https://www.darkenergysurvey.org/) (DES) survey.

The *DES Science Server* consists of independent microservices to meet different demands. For each of these microservices, a more flexible and modern version is being developed to provide access to LSST project private data for members and public data from various surveys for the entire community, all in one place (see details about the new visualisation services *Sky Viewer* and *Target Viewer* on the [Scientific Platforms](./index.md) page). Its initial version remains available and provides access to *DES* DR2 images and tabular data.


### Sky Viewer
Offers panoramic display of DES images, combined to form a single image limited only by the *footprint* edges, as well as maps in [HEALPix](https://healpix.sourceforge.io/) format.

### Target Viewer
Tool to visualise and manipulate images from a previously defined list of objects (*targets*). Allows ranking images, applying filters based on object properties, and creating mosaics with multiple images.

### Tile Viewer
Provides visualisation of DES images with display based on *Tiles*, the area unit adopted by the survey. The *Tile Viewer* was widely used for inspection and validation of data during periods preceding internal and public *data releases*.
