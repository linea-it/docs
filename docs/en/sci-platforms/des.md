The DES Science Portal ([des-portal.linea.org.br](https://des-portal.linea.org.br/)) was a platform developed for the [Dark Energy Survey](https://www.darkenergysurvey.org/) (DES) project that hosted a variety of pipelines designed to prepare customized catalogs for different astronomy applications, as well as to conduct various scientific analyses.

The data processing component of the DES Science Portal has been discontinued due to the obsolescence of the technologies involved, but its system for accessing generated results and provenance control remains available to DES collaboration members. The functionalities of the DES Science Portal have been gradually replaced by more modern services developed by LIneA, designed to handle Big Data in preparation for LSST, while maintaining its philosophy and best practices.

The DES Science Portal pipelines were grouped into stages:
* **Data Preparation** - includes creating systematic effect maps, object classification (star/galaxy), calculating photometric redshifts and other aggregated values, depending on the application. For example, estimating age, metallicity or stellar mass of galaxies.
* **Value-added Catalog** - combines data and results obtained in the previous stage, selects columns of interest and applies quality cuts and data cleaning, with criteria strongly dependent on the scientific application for which the catalog will be used.
* **Science Workflows** - aggregates various scientific analysis pipelines. Takes as input the catalogs created in the previous stage.

One of the main strengths of the DES Science Portal was its ability to provide complete information about the entire history of executed processes, allowing users to track inputs, configurations, code versions used and results obtained, in the form of a product record with informative graphs and tables. The portal also provided access to a series of tools designed for users, developers and administrators.

To learn more details about the DES Science Portal, read the two articles published in [Astronomy and Computing](https://www.sciencedirect.com/journal/astronomy-and-computing):
* **Gschwend et al. 2018 - DES science portal: Computing photometric redshifts**  
([doi.org/10.1016/j.ascom.2018.08.008](https://doi.org/10.1016/j.ascom.2018.08.008)) ([arXiv:1708.05643](https://arxiv.org/abs/1708.05643))  
* **Fausti Neto et al. 2018 - DES science portal: Creating science-ready catalogs**  
([doi.org/10.1016/j.ascom.2018.01.002](https://doi.org/10.1016/j.ascom.2018.01.002)) ([arXiv:1708.05642](https://arxiv.org/abs/1708.05642))
