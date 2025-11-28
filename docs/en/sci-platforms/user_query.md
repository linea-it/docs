

The *User Query* ([**userquery.linea.org.br**](https://userquery.linea.org.br/)) is an intuitive interface for database queries that enables the creation of temporary tables. User-generated tables are managed by *MyDB* and can be immediately accessed in LIneA's *JupyterHub* or from anywhere via *TAP Service*. Additionally, if a table contains equatorial coordinates and a column with unique identifiers, it automatically becomes a target list, allowing visualisation of corresponding object images in the [*Target Viewer*](../sci-platforms/target_viewer.md) tool (LSST images for members and *DES* DR2 images for the general public).

### TAP Service

*TAP Service* (*Table Access Protocol*) from [IVOA](https://ivoa.net/index.html) is a standard that enables access and querying of astronomical databases through SQL language, in a standardised and interoperable way across different services. In addition to the *User Query* service, public catalogues hosted at LIneA can also be accessed remotely via *TAP Service* through the [TOPCAT](https://www.star.bris.ac.uk/~mbt/topcat/) program or programmatically through the Python library [`pyvo`](https://pyvo.readthedocs.io/en/latest/#). Consult the [*TAP Service* documentation](https://userquery.linea.org.br/cms/services/scripted-access/) on the *User Query* website for further details on how to use the service.

