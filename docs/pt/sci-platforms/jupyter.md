O [JupyterHub](https://jupyter.org/hub) é um ambiente de desenvolvimento multiusuário baseado em _iPython Notebooks_ que oferece acesso a recursos computacionais compartilhados em um servidor remoto, sem a necessidade de instalação e manutenção por parte dos usuários. Os únicos pré-requisitos para acessar o JupyterHub são: ter uma conta de usuário no LIneA (veja [aqui](../primeiros_passos.md) como criar sua conta) e um navegador com acesso à Internet. Os chamados [Jupyter Notebooks](https://docs.jupyter.org/en/latest/) permitem combinar código interativo, resultados de execução, texto explicativo e recursos de multimídia em um só documento.  

Como parte do [LIneA Science Platform](./index.md), o LIneA JupyterHub está integrado às demais ferramentas de visualização e acesso a dados. Desse modo, toda a análise de dados pode ser feita _online_ dentro da plataforma, desde a leitura, visualização, processamento e análise de resultados, sem a necessidade de _download_ dos dados para o computador pessoal do usuário.   


## Tela inicial 

Ao clicar no _card_ "JupyterHub" dentro do LIneA Science Platform (ou acessando diretamente no endereço [jupyter.linea.org.br](https://jupyter.linea.org.br)), você será direcionado para a página de _login_ e em seguida para a página inicial que mostra as diferentes opções de configuração para o servidor Jupyter.

<div style="text-align: center;">
  <img src="../../images/jupyterhub-launcher.png"
       width="500"
       style="box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2); border-radius: 6px;">
</div>


### Configurações disponíveis 

#### Imagens _Docker_ pré-configuradas 

A instalação padrão do LIneA JupyterHub é baseada em _Docker containers_ com ambientes previamente configurados para atender a maioria das demandas dos usuários. São quatro opções de imagens _Docker_ disponíveis:  


* **DataScience** – o stack [Jupyter Data Science Notebook](https://jupyter-docker-stacks.readthedocs.io/), incluindo bibliotecas populares de *data science* como Pandas, NumPy, Matplotlib, SciPy e Scikit-learn.  
* **Astronomy** – o stack genérico de astronomia, que inclui as principais bibliotecas de *data science* mais as bibliotecas mais utilizadas na área, como Astropy, Astroquery, Healpy, Photutils, PyVO, Dustmaps, LSDB, AstroML, entre outras.
* **SolarSystem** – o stack específico para a área de estudos do Sistema Solar, que inclui as principais bibliotecas das imagens **DataScience** e **Astronomy** mais as bibliotecas mais utilizadas na área, como sbpy, spiceypy, rebound, sora-astro, reboundx, entre outras.
* **IRAF Data Reduction** – uma imagem customizada para auxiliar tarefas que dependem de redução e manipulação de imagens astronômicas com ferramentas tradicionais como IRAF e DS9 (ferramentas mais modernas como o Astropy CCDProc, Photutils e Reproject estão disponíveis na imagem **Astronomy**).    
 

??? info "Confira aqui a lista com as principais bibliotecas incluídas nos ambientes e links para as suas respectivas documentações."  
    <table>
    <thead>
    <tr>
      <th>Categoria</th>
      <th>Biblioteca</th>
      <th>Para que serve</th>
      <th>Doc</th>
    </tr>
    </thead>
    <tbody>
    <!-- Data Science -->
    <tr>
      <td rowspan="9"><b>Ciência<br>de Dados<br>e ML</b></td>
      <td><a href="https://corner.readthedocs.io">corner</a></td>
      <td>Visualização de distribuições posteriores.</td>
      <td><a href="https://corner.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://dynesty.readthedocs.io">dynesty</a></td>
      <td>Nested sampling bayesiano.</td>
      <td><a href="https://dynesty.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://emcee.readthedocs.io">emcee</a></td>
      <td>Amostragem MCMC.</td>
      <td><a href="https://emcee.readthedocs.io/en/stable/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://lmfit.github.io/lmfit-py/">lmfit</a></td>
      <td>Ajuste de modelos não lineares.</td>
      <td><a href="https://lmfit.github.io/lmfit-py/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://numpy.org">NumPy</a></td>
      <td>Operações numéricas e arrays multidimensionais.</td>
      <td><a href="https://numpy.org/doc">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://pandas.pydata.org">Pandas</a></td>
      <td>Análise de tabelas e manipulação de dados.</td>
      <td><a href="https://pandas.pydata.org/docs/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://scikit-image.org">scikit-image</a></td>
      <td>Processamento de imagens.</td>
      <td><a href="https://scikit-image.org/docs/stable/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://scikit-learn.org">scikit-learn</a></td>
      <td>Machine learning e modelos preditivos.</td>
      <td><a href="https://scikit-learn.org/stable/documentation.html">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://scipy.org">SciPy</a></td>
      <td>Funções científicas avançadas e métodos numéricos.</td>
      <td><a href="https://docs.scipy.org/doc/scipy/">doc</a></td>
    </tr>  
    <!-- Visualization -->
    <tr>
      <td rowspan="7"><b>Visualização</b></td>
      <td><a href="https://bokeh.org">Bokeh</a></td>
      <td>Visualizações interativas para web.</td>
      <td><a href="https://docs.bokeh.org/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://datashader.org">Datashader</a></td>
      <td>Renderização de grandes volumes de dados.</td>
      <td><a href="https://datashader.org">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://holoviews.org">HoloViews</a></td>
      <td>Visualização declarativa de dados.</td>
      <td><a href="https://holoviews.org/reference/index.html">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://hvplot.holoviz.org">hvPlot</a></td>
      <td>API de alto nível para visualização.</td>
      <td><a href="https://hvplot.holoviz.org">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://matplotlib.org">Matplotlib</a></td>
      <td>Visualização científica 2D/3D.</td>
      <td><a href="https://matplotlib.org/stable/contents.html">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://plotly.com/python/">Plotly</a></td>
      <td>Gráficos interativos e dashboards.</td>
      <td><a href="https://plotly.com/python/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://seaborn.pydata.org">Seaborn</a></td>
      <td>Visualizações estatísticas de alto nível.</td>
      <td><a href="https://seaborn.pydata.org/tutorial.html">doc</a></td>
    </tr>
    <!-- Others -->
    <tr>
      <td rowspan="5"><b>Banco de Dados, <br>Jupyter e<br>Utilitários</b></td>
      <td><a href="https://www.psycopg.org/">psycopg2</a></td>
      <td>Conector PostgreSQL para Python.</td>
      <td><a href="https://www.psycopg.org/docs/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://www.sqlalchemy.org/">SQLAlchemy</a></td>
      <td>ORM e toolkit SQL para manipulação de bancos de dados.</td>
      <td><a href="https://docs.sqlalchemy.org/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://www.fatiando.org/pooch/latest/">pooch</a></td>
      <td>Gerenciamento de download de datasets.</td>
      <td><a href="https://www.fatiando.org/pooch/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://dask.org">Dask</a></td>
      <td>Paralelização e computação distribuída.</td>
      <td><a href="https://docs.dask.org">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://ipykernel.readthedocs.io">IPykernel</a></td>
      <td>Kernel Python para notebooks.</td>
      <td><a href="https://ipykernel.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <!-- Astronomy -->
    <tr>
      <td rowspan="21"><b>Astronomia</b></td>
      <td><a href="https://astrocut.readthedocs.io">astrocut</a></td>
      <td>Recorte de imagens astronômicas.</td>
      <td><a href="https://astrocut.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://www.astroml.org">astroML</a></td>
      <td>Machine learning aplicado à astronomia.</td>
      <td><a href="https://www.astroml.org/user_guide.html">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://www.astropy.org">Astropy</a></td>
      <td>Núcleo de ferramentas astronômicas.</td>
      <td><a href="https://docs.astropy.org">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://astroquery.readthedocs.io">Astroquery</a></td>
      <td>Consultas a arquivos e catálogos astronômicos online.</td>
      <td><a href="https://astroquery.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://scitools.org.uk/cartopy/docs/latest/">Cartopy</a></td>
      <td>Mapas e projeções geoespaciais.</td>
      <td><a href="https://scitools.org.uk/cartopy/docs/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://dustmaps.readthedocs.io">Dustmaps</a></td>
      <td>Mapas de extinção da Via Láctea.</td>
      <td><a href="https://dustmaps.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://hats-import.readthedocs.io">HATS Import</a></td>
      <td>Conversão de catálogos para formato HATS.</td>
      <td><a href="https://hats-import.readthedocs.io/en/stable/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://healpy.readthedocs.io">Healpy</a></td>
      <td>Manipulação de mapas HEALPix.</td>
      <td><a href="https://healpy.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://ipyaladin.readthedocs.io">ipyaladin</a></td>
      <td>Visualização interativa do céu (Aladin Lite no Jupyter).</td>
      <td><a href="https://ipyaladin.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://pypi.org/project/lineaSSP/">lineassp</a></td>
      <td>API do LIneA Solar System Portal.</td>
      <td><a href="https://pypi.org/project/lineaSSP/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://docs.lightkurve.org">lightkurve</a></td>
      <td>Análise de curvas de luz (Kepler/TESS).</td>
      <td><a href="https://docs.lightkurve.org">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://lsdb.readthedocs.io">LSDB</a></td>
      <td>Análise escalável de catálogos.</td>
      <td><a href="https://lsdb.readthedocs.io/en/stable/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://photutils.readthedocs.io">Photutils</a></td>
      <td>Fotometria e detecção de fontes.</td>
      <td><a href="https://photutils.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://pyspeckit.readthedocs.io">pyspeckit</a></td>
      <td>Ajuste e análise de espectros.</td>
      <td><a href="https://pyspeckit.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://github.com/rcboufleur/pySPAC">pyspac</a></td>
      <td>Análise e Caracterização da Curva de Fase Solar</td>
      <td><a href="https://rcboufleur.github.io/pySPAC/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://pyvo.readthedocs.io">PyVO</a></td>
      <td>Acesso a serviços do Virtual Observatory.</td>
      <td><a href="https://pyvo.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://github.com/linea-it/pzserver">pzserver</a></td>
      <td>Acesso a dados e execução de pipelines do PZ Server de forma remota.</td>
      <td><a href="https://linea-it.github.io/pzserver/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://pypi.org/project/pz-rail/">rail</a></td>
      <td>Pipeline de redshift fotométrico.</td>
      <td><a href="https://rail-hub.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://astropy-regions.readthedocs.io">Regions</a></td>
      <td>Manipulação de regiões no céu.</td>
      <td><a href="https://astropy-regions.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://reproject.readthedocs.io">Reproject</a></td>
      <td>Reprojeção de imagens astronômicas.</td>
      <td><a href="https://reproject.readthedocs.io/en/stable/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://specutils.readthedocs.io">specutils</a></td>
      <td>Análise espectral astronômica.</td>
      <td><a href="https://specutils.readthedocs.io/en/stable/">doc</a></td>
    </tr>
    <tr>
      <td rowspan="5"><b>Sistema<br>Solar</b></td>
      <td><a href="https://rebound.readthedocs.io">rebound</a></td>
      <td>Simulações N-corpos.</td>
      <td><a href="https://rebound.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://reboundx.readthedocs.io">reboundx</a></td>
      <td>Extensões para REBOUND.</td>
      <td><a href="https://reboundx.readthedocs.io/en/latest/">doc</a></td>
    </tr>

    <tr>
      <td><a href="https://sbpy.readthedocs.io">sbpy</a></td>
      <td>Ferramentas para ciência de pequenos corpos.</td>
      <td><a href="https://sbpy.readthedocs.io/en/latest/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://spiceypy.readthedocs.io">spiceypy</a></td>
      <td>Interface Python para SPICE (NASA).</td>
      <td><a href="https://spiceypy.readthedocs.io/en/main/">doc</a></td>
    </tr>
    <tr>
      <td><a href="https://pypi.org/project/sora-astro/">sora-astro</a></td>
      <td>Análise de ocultações estelares.</td>
      <td><a href="https://sora.readthedocs.io/latest/">doc</a></td>
    </tr>
    </tbody>
    </table>


### Recursos computacionais

**Configuração dos servidores do ambiente K8S**

A plataforma JupyterHub é executada sobre um cluster Kubernetes (K8S) e possui 12 servidores físicos dedicados. Cada máquina é equipada com os seguintes recursos computacionais:

|   Kubernetes Node Configuration  ||
| ----------------------- | ------- |
| **RAM**                 | 64 GB |
| **Threads per core**  | 2   |
| **Cores per socket**  | 6   |
| **Sockets**           | 2   | 


**Configurações disponíveis para os usuários**

Ainda na página inicial, do lado direito será exibido um menu com até três opções de recursos computacionais que serão reservados para o servidor Jupyter de cada usuário simultaneamente. As opções disponíveis são as seguintes:  


| **Tamanho** | **CPUs** |  **RAM**   |
|---------|------|--------|
| **Small**   | 1.0  |  4 GiB |
| **Medium**  | 2.0  |  8 GiB |
| **Large**   | 4.0  | 16 GiB |


As opções que oferecem mais de uma CPU permitem a execução de código paralelo, o que pode ser útil para tarefas que se beneficiam de múltiplos núcleos, como simulações numéricas, ou treinamento de modelos de machine learning. No entanto, o JupyterHub baseado em K8S é projetado para ser um ambiente de desenvolvimento leve e interativo, e não é otimizado para cargas de trabalho intensivas em CPU ou memória. Para tarefas mais exigentes, que envolvem processamento de dados em larga escala ou treinamento de modelos complexos, recomenda-se a utilização do outro serviço de Jupyter disponível na plataforma Ondemand, que tem acesso direto à infraestrutura de HPC do LIneA. 


!!! info "Jupyter over K8S vs Jupyter over HPC"
	 O LIneA disponibiliza dois ambientes separados de Jupyter Notebook. O primeiro é executado em containers na plataforma Kubernetes (K8S) e está integrado ao banco de dados Postgres e ao sistema de arquivos. O segundo está disponível na [plataforma Ondemand](../processamento/uso/openondemand.md) e acessa diretamente a infraestrutura de HPC.  


## Gerenciamento de ambientes 

Caso seu trabalho necessite de outras bibliotecas ou da fixação de versões específicas de alguma delas, recomendamos a utilização do gerenciador de pacotes [Conda](https://docs.conda.io/projects/conda/en/stable/index.html). Para saber mais, acesse o [tutorial oficial](https://bit.ly/tryconda) do Conda e a [*Cheat Sheet*](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf) disponibilizada pelo projeto com a lista de comandos mais úteis. 

### Como criar um ambiente customizado 

Por exemplo, vamos criar um ambiente com Python 3.12 chamado `myenv`. Para abrir um Terminal, no menu superior do Jupyter Lab, clique em:

`File > New > Terminal`

Vamos começar verificando os ambientes disponíveis.

``` bash
conda env list
``` 

Se está acessando pela primeira vez, deve ver apenas o ambiente _base_ disponível. 

Criando o novo ambiente: 

```bash
conda create -n myenv python=3.12
```


<font size=4>Ativar o ambiente</font>

Para ativar o ambiente:

```bash
conda activate myenv
``` 

Depois disso, você pode instalar pacotes normalmente com o próprio Conda, ou com Pip. Por exemplo:

```bash
conda install numpy=1.26
pip install astropy=7.1
```

Para que o novo ambiente esteja disponível para ser utilizado em um notebook, precisamos registrá-lo como um _kernel_. 


### Como registrar o ambiente como kernel do Jupyter

<font size=4>Criar novo kernel</font>

A criação de um novo _kernel_ a partir de um ambiente conda é feita pela biblioteca `ipykernel`. Ela já está disponível no ambiente _base_. 

```bash 
python -m ipykernel install --user --name myenv --display-name "MyEnv"
```
Depois de criados, os kernels ficam disponíveis para a inicialização de novos notebooks na aba "_Launcher_", ou podem ser selecionados no menu do canto superior direito do notebook. Para listar os kernels existentes:  

<font size=4>Remover o kernel</font>

Para remover um _kernel_:

```bash
jupyter kernelspec remove myenv
```

<font size=4>Remover ambiente</font>

Se desejar remover o ambiente: 

```bash
conda env remove --name myenv 
```



## Acesso a dados 

### Banco de dados 

O LIneA possui um banco de dados dedicado ao armazenamento de dados astronômicos tabulares gerenciado pelo sistema Postgres. Este banco armazena catálogos, mapas e tabelas auxiliares liberados por levantamentos astronômicos e tabelas criadas pelos usuários como resultados de consultas ou uploads. O acesso aos dados hospedados no banco a partir de um notebook no JupyterHub é feito com a biblioteca `pyvo` através do serviço [TAP](https://www.ivoa.net/documents/TAP/), com consultas programáticas nas linguagens **SQL** ou **ADQL**. 

**Setup** 

Para conectar na API do serviço TAP do LIneA, utilizaremos as bibliotecas [requests](https://docs.python-requests.org/en/latest/) e [PyVO](https://pyvo.readthedocs.io/en/latest/), um pacote afiliado ao Astropy que permite consultas remotas de dados astronômicos de repositórios que seguem o padrão de protocolos de serviço IVOA. 

```python
import pyvo
import requests
```

O primeiro passo é abrir uma conexão com o serviço TAP do LIneA: 

```python
tap_session = requests.Session()
tap_url = "https://userquery.linea.org.br/tap"
tap_service = pyvo.dal.TAPService(tap_url, session=tap_session)
```

Em seguida, podemos realizar consultas utilizando a linguagem SQL ou ADQL. Por exemplo, para consultar a tabela `gaia_dr3.gaia_source` e obter as 10 primeiras linhas: 

```python
query = "SELECT TOP 10 * FROM gaia_dr3.gaia_source"
result = tap_service.search(query)    
```
O resultado da consulta é retornado como um objeto do tipo `pyvo.dal.TAPResults`, que pode ser convertido para um `astropy.table.Table`, que por sua vez, pode ser convertito em Pandas DataFrame (opcional) para facilitar a manipulação e análise dos dados: 

```python
from astropy.table import Table
import pandas as pd
table = Table(result.to_table())
df = table.to_pandas()
```   


Acesse aqui a documentação completa sobre o acesso ao banco de dados Postgres do LIneA com a biblioteca `pyvo`: [TAP Service](../sci-platforms/user_query.html#tap-service)  



### LSDB 

O [Large Scale Database (LSDB)](https://docs.lsdb.io/en/stable/) é uma biblioteca Python desenvolvida pelo LSST Interdisciplinary Network for Collaboration and Computing (LINCC) no âmbito do Legacy Survey of Space and Time (LSST), projetada para facilitar o acesso e a análise de grandes conjuntos de dados astronômicos. Um amplo acervo de dados públicos é disponibilizado pelo projeto no site LSDB.io. 

O LIneA hospeda uma cópia local desses dados, integrando-se ao ecossistema internacional do LSDB.io. Essa infraestrutura oferece maior eficiência na transferência de dados para usuários geograficamente próximos ao Brasil e permite que a comunidade astronômica brasileira acesse e analise esses conjuntos de dados de forma otimizada, utilizando seus recursos de computação de alto desempenho (HPC).

O acesso aos dados do LSDB a partir do JupyterHub é feito com a biblioteca `lsdb`, que oferece uma interface Python para realizar consultas e manipular os dados armazenados no LSDB. Apesar de o LSDB ser otimizado para manipulação de grandes volumes, ele também pode ser útil para pequenas consultas. Visite a [lista de tutoriais da página oficial do LSDB](https://docs.lsdb.io/en/latest/tutorials.html) para saber mais sobre como aproveitar esta grande ferramenta em todo o seu potencial. 

Para fazer uma consulta simples, basta importar a biblioteca e abrir um catálogo específico. Por exemplo, para acessar o catálogo de objetos do DES DR2 hospedado no LSDB do LIneA: 

```python
import lsdb
import pandas as pd 
```

O LSDB utiliza a metodologia de computação "preguiçosa", onde os comandos são planejados a priori e as operações são realizadas somente no final do processo.

O comando abaixo ainda não traz os dados para a memória local, apenas planeja que vai trazer estas colunas do catálogo localizado pela URL informada.

```python
cat = lsdb.open_catalog("https://linea.data.lsdb.io/hats/des/des_dr2", 
                        columns=["COADD_OBJECT_ID", "RA", "DEC", "MAG_AUTO_G_DERED", "MAG_AUTO_R_DERED", 
                                 "EXTENDED_CLASS_COADD", "FLAGS_G", "FLAGS_R"])
``` 

A variável cat guarda um objeto do tipo Catalog que tem métodos e atributos a serem utilizados, também de forma preguiçosa, sem efetuar os cálculos. Por exemplo, vamos fazer uma busca espacial na região do aglomerado de estrelas de Sculptor, com um raio de 0.5 graus: 

```python
sculptor_stars = cat.cone_search(ra=15.03875, dec=-33.70916667, radius_arcsec=0.5 * 3600) 
```

Após definir as colunas e a região de interesse, podemos executar a consulta com o método `compute()` e finalmente trazer os dados para um Pandas DataFrame: 

```python
df = sculptor_stars.compute()
```


## Tutoriais em Jupyter Notebooks


No menu `Tutorials` na barra superior do Jupyter lab você encontra modelos de notebooks contendo exemplos de uso para aprender como:  

* Utilizar um Jupyter Notebook (caso seja iniciante)
* Customizar o seu ambiente com bibliotecas e versões específicas  
* Acessar os dados hospedados no LIneA diretamente a partir de um notebook  


<font color="red"> [PLACEHOLDER PARA FIGURA COM SCREENSHOT DO MENU TUTORIALS] </font>



Os tutorias estão disponíveis no repositório público do LIneA no GitHub [jupyterhub-tutorial](https://github.com/linea-it/jupyterhub-tutorial), e são atualizados regularmente para incluir novos exemplos de uso e cobrir as dúvidas mais frequentes dos usuários. Ao clicar em um tutorial específico, o sistema faz uma cópia do notebook para o diretório atual na área do usuário, garantindo que cada pessoa possa executar e fazer alterações na(s) sua(s) cópia(s). 

Ao logar no JupyterHub, o sistema cria uma cópia de todos os notebooks de tutoriais para um diretório `$HOME/notebooks/tutorials/` na área do usuário. Não recomenda-se a alteração dos arquivos originais, para evitar que as atualizações futuras sejam perdidas. Caso queira manter uma cópia personalizada, recomendamos usar sempre o menu `Tutorials` para criar uma nova cópia atualizada no diretório atual de trabalho.  



## Assistente de IA (experimental)

As imagens pré-configuradas possuem um assistente de IA integrado ao JupyterLab treinado com modelos de linguagem específicos para auxiliar na produção de código científico. Esta implementação ainda está em fase experimental e todo feedback dos usuários é bem-vindo. Para acessar o serviço, clique no ícone de chat no menu lateral.


<font color="red"> [PLACEHOLDER PARA FIGURA COM SCREENSHOT DO MENU LATERAL E ÍCONE DO ASSISTENTE DE IA] </font>



<font color="red">[PLACEHOLDER PARA EXPLICAÇÃO SOBRE OS LLMs TREINADOS E COMO UTILIZAR] </font>


