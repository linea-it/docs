
O [DES Science Server](https://scienceserver.linea.org.br/) é um serviço de visualização de imagens e dados de catálogos que foi desenvolvido como parte da contribuição do LIneA para o levantamento [Dark Energy Survey](https://www.darkenergysurvey.org/) (DES). 

O DES Science Server é composto por microsserviços independentes para atender a diferentes demandas. Para cada um desses microsserviços, uma nova versão mais flexível e moderna está sendo desenvolvida para oferecer acesso aos dados privados do projeto LSST para membros e dados públicos de diversos levantamentos para toda a comunidade, tudo num só lugar (veja detalhes sobre os novos seviços de visualização Sky Viewer e Target Viewer na página [Plataformas Científicas](/sci-platforms/index.html). Sua versão inicial ainda está disponível e oferece acesso às imagens e dados tabulares do DES DR2.


### Sky Viewer
Oferece exibição panorâmica das imagens do DES, combinadas para formar uma imagem única limitada apenas pelas bordas do _footprint_, e de mapas em formato [HEALPix](https://healpix.sourceforge.io/).  

### Target Viewer
Ferramenta para visualizar e manipular imagens de uma lista de objetos previamente definida (_targets_). Oferece a possibilidade de _rankear_ imagens, aplicar filtros baseados em propriedades dos objetos e criar mosaicos com múltiplas imagens. 

### Tile Viewer
Oferece a visualização das imagens do DES com exibição baseada nas _Tiles_, a unidade de área adotada pelo levantamento. O Tile Viewer foi amplamente utilizado na inspeção e validação dos dados nos períodos que antecederam os _data releases_ internos e públicos.
