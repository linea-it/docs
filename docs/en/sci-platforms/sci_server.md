The [DES Science Server](https://scienceserver.linea.org.br/) is an image and catalog data visualization service developed as part of LIneA's contribution to the [Dark Energy Survey](https://www.darkenergysurvey.org/) (DES).

The DES Science Server consists of independent microservices to meet different demands. For each of these microservices, a newer, more flexible and modern version is being developed to provide access to LSST project private data for members and public data from various surveys for the entire community, all in one place (see details about the new visualization services Sky Viewer and Target Viewer on the [Scientific Platforms](/sci-platforms/index.html) page). Its initial version is still available and provides access to DES DR2 images and tabular data.

### Sky Viewer
Offers panoramic display of DES images, combined to form a single image limited only by the footprint edges, and maps in [HEALPix](https://healpix.sourceforge.io/) format.

### Target Viewer
Tool to visualize and manipulate images from a predefined list of objects (targets). Allows ranking images, applying filters based on object properties, and creating mosaics with multiple images.

### Tile Viewer
Provides visualization of DES images displayed based on Tiles, the area unit adopted by the survey. The Tile Viewer was widely used for inspection and validation of data during periods preceding internal and public data releases.
