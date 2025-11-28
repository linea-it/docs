El *DES Science Portal* ([des-portal.linea.org.br](https://des-portal.linea.org.br/)) fue una plataforma desarrollada para el proyecto [*Dark Energy Survey*](https://www.darkenergysurvey.org/) (DES) que albergó una variedad de *pipelines* destinados a preparar catálogos personalizados para diferentes aplicaciones en astronomía, así como para realizar diversos análisis científicos.

La sección correspondiente al procesamiento de datos del *DES Science Portal* fue descontinuada debido a la obsolescencia de las tecnologías involucradas; sin embargo, su sistema de acceso a los resultados generados y de control de procedencia continúa disponible para los miembros de la colaboración DES. Las funcionalidades del *DES Science Portal* fueron gradualmente sustituidas por otros servicios más modernos desarrollados por LIneA, preparados para gestionar *Big Data* en preparación para el LSST, manteniendo su filosofía y buenas prácticas.

Los *pipelines* del *DES Science Portal* fueron agrupados en etapas:

* ***Data Preparation*** – incluye la creación de mapas de efectos sistemáticos, clasificación de objetos (estrella/galaxia), cálculo de *redshifts* fotométricos y otros valores agregados, dependiendo de la aplicación. Por ejemplo, estimación de edad, metalicidad o masa estelar de galaxias.

* ***Value-added Catalog*** – combina los datos y resultados obtenidos en la etapa anterior, selecciona las columnas de interés y aplica cortes de calidad y limpieza de datos, con criterios fuertemente dependientes de la aplicación científica para la cual será destinado el catálogo.

* ***Science Workflows*** – agrega diversos *pipelines* de análisis científico. Tiene como datos de entrada los catálogos creados en la etapa anterior.

Uno de los principales puntos fuertes del *DES Science Portal* fue la capacidad de proporcionar información completa de todo el historial de procesos ejecutados, permitiendo al usuario rastrear la entrada, la configuración, la versión de los códigos utilizados y los resultados obtenidos, en forma de un registro del producto con gráficos y tablas informativas. El portal también proporcionó acceso a una serie de herramientas destinadas al usuario, al desarrollador y al administrador.

Para profundizar en los detalles sobre el *DES Science Portal*, consulte los dos artículos publicados en la revista [*Astronomy and Computing*](https://www.sciencedirect.com/journal/astronomy-and-computing):

* **Gschwend et al. 2018 – DES science portal: Computing photometric redshifts**  
([doi.org/10.1016/j.ascom.2018.08.008](https://doi.org/10.1016/j.ascom.2018.08.008)) ([arXiv:1708.05643](https://arxiv.org/abs/1708.05643))

* **Fausti Neto et al. 2018 – DES science portal: Creating science-ready catalogs**  
([doi.org/10.1016/j.ascom.2018.01.002](https://doi.org/10.1016/j.ascom.2018.01.002)) ([arXiv:1708.05642](https://arxiv.org/abs/1708.05642))
