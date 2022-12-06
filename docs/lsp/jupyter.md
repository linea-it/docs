

O [JupyterHub](https://jupyter.org/hub) é um ambiente de desenvolvimento multiusuário baseado em _iPython Notebooks_ que oferece acesso a recursos computacionais compartilhados em um servidor remoto, sem a necessidade de instalação e manutenção por parte dos usuários. Os únicos pré-requisitos para acessar o JupyterHub são: ter uma conta de usuário no LIneA (veja [aqui](../primeiros_passos.md) como criar sua conta) e um navegador com acesso à Internet. Os chamados [Jupyter Notebooks](https://docs.jupyter.org/en/latest/) permitem combinar código interativo, resultados de execução, texto explicativo e recursos de multimídia em um só documento.  

Como parte do [LIneA Science Platform](../lsp/index.md), o LIneA JupyterHub está integrado às demais ferramentas de visualização ([Science Server](../lsp/sci_server.md)) e acesso a dados ([User Query](../lsp/user_query.md)). Desse modo, toda a análise de dados pode ser feita _online_ dentro da plataforma, desde a leitura, visualização, processamento e análise de resultados, sem a necessidade de _download_ dos dados. 

Ao clicar no _card_ "JupyterHub" dentro do LIneA Science Platform, você será direcionado para a página de _login_ e em seguida para a página inicial do JupyterHub que mostrará o seu perfil de usuário. Clique no botão **START** para iniciar.       

A instalação padrão do JupyterHub utiliza a nova interface [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) e é baseada na imagem [datascience-notebook](https://github.com/jupyter/docker-stacks), estendida com as bibliotecas [Astropy](https://www.astropy.org/) e [dblinea](https://dblinea.readthedocs.io/en/latest/index.html) (a biblioteca que faz a conexão com o banco de dados). Isto significa que uma série de bibliotecas _Python_ de grande popularidade como [Numpy](https://numpy.org/) e [Matplotlib](https://matplotlib.org/) estarão automaticamente disponíveis.



## Apoio ao usuário

### Tutoriais em Jupyter Notebooks

No repositório [jupyterhub-tutorial](https://github.com/linea-it/jupyterhub-tutorial) você encontrará os tutoriais em formato _notebook_:


#### 1-primeiros-passos.ipynb 
Instruções gerais de uso da plataforma JupyterLab, dicas e atalhos na escrita de notebooks para diferentes tipos de células. 
#### 2-acesso-a-dados.ipynb
Instruções para uso da biblioteca dblinea para leitura de dados a partir do banco de dados diretamente de dentro de uma célula no notebook com exemplo de uso (construção de um diagrama cor-magnitude simples para uma amostra de estrelas). 
#### 3-conda-env.ipynb
Instruções para criação de ambientes no conda para gerenciamento de bibliotecas que sejam persistentes e sobrevivam a destruição e recriação dos containers para que os usuários possam retornar em uma nova sessão e encontrar o mesmo ambiente da sessão anterior (não disponível para usuários de perfil público bronze).  

***  

Para acessar os _notebooks_, basta abrir um Terminal no JupyterLab clicando no botão "+" na barra superior e em seguida no ícone "Terminal" da seção "Other" na aba "Launcher", e inserir o comando:

    git clone https://github.com/linea-it/jupyterhub-tutorial.git

### Tutoriais em vídeo

Para ter acesso a tutoriais em vídeo, visite a [página de tutoriais do LIneA Science Platform](https://lsp.linea.org.br/tutorials).  
 
### Minicurso
Como parte das atividades do programa de Iniciação Científica (IC), em 2022 o LIneA ofereceu uma série de minicursos para os estudantes e demais interessados com aulas remotas e atividades práticas propostas. Os vídeos das aulas estão disponíveis na página do [Minicurso Jupyter Notebook](https://classroom.google.com/c/NDkzMTA0MzEyODA1/m/NDcyNjUyMTg5Mjc1/details) no Google Classroom.  
