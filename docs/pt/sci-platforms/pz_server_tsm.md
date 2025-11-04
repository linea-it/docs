# Training Set Maker

O **Training Set Maker** é um *pipeline* que realiza o *crossmatch* espacial entre um Catálogo de Redshifts previamente registrado e o catálogo **LSST Object**, com o objetivo de criar *training sets* para algoritmos de *photo-z* baseados em aprendizado de máquina. Ele utiliza a biblioteca [LSDB](https://docs.lsdb.io/en/stable/index.html), desenvolvida pelo [LINCC Frameworks](https://lsstdiscoveryalliance.org/programs/lincc-frameworks/), para lidar de forma eficiente com dados espacialmente distribuídos e oferecer flexibilidade nos observáveis incluídos no *training set*.



### Execução via site do Photo-z Server

O *pipeline* pode ser executado pelo site do **Photo-z Server**, que fornece uma interface amigável para configurar os parâmetros de *crossmatch*, selecionar os conteúdos desejados e escolher o formato de saída.

Ao executar o *pipeline* pela interface do Photo-z Server, o usuário deve fornecer as seguintes informações:

1. **Nome do training set**  
    Um nome curto e descritivo que será usado para registrar o resultado no sistema e encontrá-lo na lista de produtos em buscas futuras.  
    Não é necessário escolher um nome único — o sistema adicionará automaticamente um número de ID interno ao nome do produto para garantir unicidade.

2. **Descrição (opcional)**  
    Qualquer observação ou nota sobre a amostra.

3. **Selecionar o Catálogo de Redshifts para o crossmatch**  
    Escolha entre os Catálogos de Redshift de Referência já registrados e listados no menu.  
    Ele pode ser o resultado de uma execução anterior do *pipeline* **Combine Redshift Catalogs** ou um catálogo personalizado previamente enviado pelo usuário.

4. **Selecionar o Catálogo de Objetos (dados fotométricos)**  
    Escolha entre os *releases* do LSST disponíveis, por exemplo, **DP0.2**, **DP1**, etc.  

    * **Tipo de fluxo (Flux type)**: selecione quais colunas de fluxo serão usadas durante o *crossmatch*. As opções são populadas dinamicamente de acordo com o catálogo selecionado, por exemplo, `cModel`, `gaap1p0`, `psf`, etc.
    * **Aplicar desavermelhamento a partir de mapas de poeira (dustmaps)**  
    Selecione qual mapa de poeira (se houver) será usado para desavermelhar os fluxos fotométricos.
    * **Converter fluxos em magnitudes**  
    Marque esta opção para converter os fluxos em magnitudes. A conversão é feita pela fórmula:

    $$
    mag = -2.5 * \log_{10}(flux) + 31.5
    $$

    onde $flux$ é o valor na coluna de fluxo selecionada.

5. **Selecionar as configurações do crossmatch**
      * **Distância limite (arcseg)**: distância máxima permitida entre as fontes correspondentes.
      * **Número de vizinhos**: número de correspondências mais próximas a serem recuperadas.  
        Selecione **1** para um *match* um-a-um ou um número maior para recuperar múltiplas correspondências.
      
6. **Selecionar galáxias únicas (em breve)**  
    Em casos de múltiplas correspondências, aplicar um critério de deduplicação (opção ainda não implementada).

7. **Formato de saída**  
    Escolha entre os formatos suportados: **Parquet**, **CSV**, **FITS** ou **HDF5**.


#### Exemplo 1: Training Set com galáxias simuladas do DP0.2

!!! warning
    seção em construção 
    
    
#### Exemplo 2: Training Set com galáxias observadas do DP1

!!! warning
    seção em construção 
    

### Execução via API do Photo-z Server

!!! warning
    seção em construção 
