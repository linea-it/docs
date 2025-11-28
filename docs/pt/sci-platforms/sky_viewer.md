O [*Sky Viewer*](https://skyviewer.linea.org.br) é uma plataforma avançada de visualização astronômica, baseada na ferramenta *Aladin Lite v3*. O sistema foi concebido para a exibição de imagens *HiPS* (*Hierarchical Progressive Surveys*) e a sobreposição de catálogos provenientes de múltiplas fontes de dados.

Analogamente ao *Target Viewer*, o *Sky Viewer* foi originalmente desenvolvido no âmbito do levantamento *DES* (*Dark Energy Survey*). A sua versão precursora permanece acessível através do [*DES Science Server*](https://scienceserver.linea.org.br/), provendo acesso legado aos dados do *DES* DR2. A iteração atual, arquitetada para integrar o portfólio de serviços do IDAC-Brasil, é uma solução flexível, oferecendo níveis de acesso diferenciados: dados públicos destinados à comunidade geral e dados embargados (a exemplo do LSST) restritos a membros devidamente autenticados.

![Sky Viewer landing page](../../images/skyviewer-landingpage.png)

## Painel de controle para dados locais

Localizado na lateral direita da interface.

Este painel atua como o gerenciador central dos dados hospedados no LIneA, filtrando o acesso em conformidade com as credenciais do usuário.

### 1.1 Seção de Imagens (*Background*)

Esta seção enumera os levantamentos *HiPS* disponíveis localmente para utilização como imagem de fundo.

**Acesso Restrito:** Usuários não autenticados visualizarão exclusivamente os levantamentos públicos. Após o procedimento de *login* (*Sign In*), membros de colaborações específicas (como o LSST) terão acesso a opções adicionais, restritas aos seus respectivos grupos de trabalho.

### 1.2 Seção de Catálogos (*Overlays*)

Apresenta a relação de catálogos tabulares (*HiPScat*) disponíveis localmente para sobreposição.

**Visualização:** A ativação de um catálogo é efetuada através da marcação da caixa de seleção correspondente.

**Personalização:** Ao clicar na seta (▼) adjacente ao nome do catálogo, expandem-se os controles de visualização:

- ***Color***: Define a coloração dos marcadores, facilitando a distinção entre múltiplos catálogos.
- ***Shape***: Altera a morfologia geométrica dos marcadores (círculo, quadrado, cruz, entre outros).
- ***Size***: Ajusta a dimensão dos pontos em *pixels*.

![Sky Viewer local catalogs](../../images/skyviewer-select-catalog.png)

### Dados Disponíveis no LIneA

| Nome do *Dataset* | Tipo | Acesso | Descrição Técnica |
|-------------------|------|--------|-------------------|
| *DES* DR2 | Imagem & Catálogo | Público | *Dark Energy Survey Data Release 2* (Espectro Óptico). |
| *Rubin First Look* | Imagem | Público | Imagens públicas iniciais do Observatório Rubin. |
| LSST DP0.2 | Imagem | Restrito | *Data Preview 0.2* (Simulação). Exclusivo para membros. |
| LSST DP1 | Imagem & Catálogo | Restrito | *Data Preview 1*. Restrito a ambientes de desenvolvimento e membros autorizados. |

## Interface padrão do *Aladin Lite*

Menus e barras de ferramentas nativas da aplicação. Estas ferramentas regem a modalidade de visualização dos dados, oferecem recursos de navegação e referência espacial e são fundamentais para a experiência do usuário.

### 2.1 Barra de busca (topo)

Possibilita a navegação para uma posição específica através de dois métodos:

- **Por Coordenadas:** Aceita formatos decimais ou sexagesimais (ex: 13 25 27.6 -43 01 09).
- **Por Nome de Objeto:** Permite a inserção do identificador (ex: NGC 4755). O sistema consulta o serviço *Sesame* (CDS) para resolver as coordenadas e centralizar o mapa no objeto.

### 2.2 Gerenciador de camadas (ícone de "pilha" - topo esquerdo)

Enquanto o painel direito destina-se ao gerenciamento mais direto da exibição de imagens *HiPS* e catálogos locais, este menu dá acesso a um grande conjunto de catálogos públicos remotos hospedados pelo CDS.

- ***Overlays***: permite a seleção de catálogos ou mesmo a busca de outros catálogos para exibição em camada sobre as imagens de *surveys*. Bases de catálogos locais também são exibidas nessa lista.


![Sky Viewer search catalogs](../../images/skyviewer-search-catalogs.png)


- ***Surveys***: destina-se à seleção e gestão de camadas de imagens disponíveis em servidores remotos. Imagens *HiPS* locais também são exibidas nessa lista.
    - ***Contrast***: controles para ajuste de brilho, saturação, contraste e gama da imagem exibida;
    - ***Opacity***: permite a criação de composições visuais, ajustando a transparência da imagem de fundo ou dos catálogos sobrepostos;
    - ***Colormap***: altera o mapa de cores da imagem (ex: *Rainbow*, *Heatmap*, *Grayscale*), recurso útil para evidenciar variações de intensidade em imagens monocromáticas;
    - ***Cutout***: controles para exportação de *snapshots* e tipo de *stretch* (modelo de realce de contraste) das imagens.

### 2.3 Configurações de visualização (ícone de engrenagem)

- ***Coordinate Grid***: Sobrepõe as linhas de Ascensão Reta e Declinação.
- ***Reticle***: Exibe uma mira central fixa, essencial para a identificação precisa da coordenada no centro do campo de visão.
- ***Healpix Grid***: Exibe a grade de tesselação dos dados, útil para a inspeção técnica da qualidade da imagem.

### 2.4 Sistemas de referência e projeções

Localizados na barra inferior ou no menu de configurações:

#### *Coordinate Frame* (Sistemas de Referência)

Permite selecionar o referencial astronômico para a orientação do mapa e a grade de coordenadas. As opções disponíveis são:

- **ICRS:** *International Celestial Reference System* (Sistema de Referência Celeste Internacional). Configuração padrão que utiliza coordenadas equatoriais (J2000) com representação sexagesimal (horas, minutos, segundos).
- **ICRSd:** variação do sistema ICRS que exibe as coordenadas equatoriais em formato de graus decimais, facilitando a leitura direta de valores numéricos para análise quantitativa.
- **GAL:** Sistema de Coordenadas Galácticas. Reorienta a visualização tendo como plano fundamental o disco da Via Láctea, ideal para estudos da estrutura galáctica.

#### Tipos de projeção

O usuário pode selecionar a geometria de projeção do mapa que melhor se adeque à sua exploração. As opções incluem:

- ***Tangential***: (projeção gnômica) projeta a esfera a partir do centro sobre um plano tangente. Esta projeção possui a propriedade única de representar todos os círculos máximos (como meridianos e o equador) como linhas retas. É ideal para astrometria em campos pequenos, porém apresenta distorção severa nas bordas em campos largos.
- ***Stereographic***: (projeção estereográfica) projeção conforme que preserva os ângulos locais. Embora a área não seja preservada (a escala aumenta longe do centro), as formas de objetos pequenos (como crateras ou galáxias) permanecem verdadeiras.
- ***Spheric***: a representação padrão de "globo virtual", simulando a visão de uma esfera tridimensional a partir de uma distância finita. É a projeção mais intuitiva para navegação geral.
- ***Zenital equal-area***: (projeção azimutal de Lambert) preserva rigorosamente a área dos objetos em detrimento da forma. É fundamental para análises estatísticas, como contagem de fontes por grau quadrado ou estudos de densidade estelar, garantindo que regiões de mesmo tamanho na tela correspondam a áreas iguais no céu.
- ***Mercator***: projeção cilíndrica conforme clássica. Preserva as direções e formas locais, mas distorce significativamente as áreas em altas latitudes (polos). É útil para visualizar regiões equatoriais.
- ***Hammer-Aitoff***: projeção de área igual que exibe a esfera celeste inteira (*All-Sky*) em uma elipse. Comparada à *Mollweide*, produz distorções angulares menos severas nas regiões externas, sendo excelente para mapas de distribuição galáctica completa.
- ***Mollweide***: projeção pseudocilíndrica de área igual, frequentemente utilizada para mapas globais da esfera celeste, como mapas de radiação cósmica de fundo ou densidade estelar da Via Láctea. Os paralelos são linhas retas, enquanto os meridianos são elipses.

## Menu contextual

Acessível via clique com o botão direito no mapa.

O menu contextual disponibiliza atalhos imediatos e ferramentas analíticas exclusivas. Algumas funcionalidades complementam os recursos supracitados, enquanto outras oferecem capacidades distintas.

### 3.1 Identificação e coordenadas

- ***What is this?***: Executa uma consulta radial no banco de dados SIMBAD, baseada na posição do clique, retornando a lista de objetos catalogados na região.
- ***Copy Coordinates***: Copia imediatamente as coordenadas (RA/Dec no sistema ICRS) do ponto selecionado para a área de transferência.

### 3.2 Exportação de dados

- ***Take a snapshot***: Gera uma imagem visual (*webp*) fidedigna à visualização em tela. Ideal para apresentações e ilustrações. A personalização visual da imagem deve ser feita no gerenciador de camadas do *survey* (*cutout*);
- ***HiPS2FITS cutout***: função indisponível no momento.

### 3.3 Ferramentas de seleção (para catálogos)

Permitem a filtragem visual dos dados de um catálogo carregado.

- ***Select sources*** > ***Rectangular*** / ***Circular*** / ***Polygon***: O usuário define uma região no mapa. O sistema selecionará todas as fontes do catálogo contidas na geometria delimitada e filtrará a tabela de dados correspondente no painel inferior. Este subconjunto selecionado pode ser baixado como um arquivo CSV.

![Sky Viewer context menu](../../images/skyviewer-context-menu.png)


### 3.4 Importação local

- ***Load a local file***: Permite o carregamento de dados armazenados localmente no computador do usuário para comparação com os dados do servidor.
  - **Catálogos:** Arquivos CSV ou *VOTable* (requerem colunas RA/Dec).
  - **Imagens:** Arquivos *FITS* com WCS válido.
