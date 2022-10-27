

O [JupyterHub](https://jupyter.org/hub) é um ambiente de desenvolvimento multiusuário baseado em _iPython Notebooks_ que oferece acesso a recursos computacionais compartilhados em um servidor remoto, sem a necessidade de instalação e manutenção por parte dos usuários. Os únicos pré-requisitos para acessar o JupyterHub são: ter uma conta de usuário no LIneA (veja [aqui](../primeiros_passos.md) como criar sua conta) e um navegador com acesso à Internet. Os chamados [Jupyter Notebooks](https://docs.jupyter.org/en/latest/) permitem combinar código interativo, resultados de execução, texto explicativo e recursos de multimídia em um só documento.  

Como parte do [LIneA Science Platform](../lsp/index.md), o LIneA JupyterHub está integrado às demais ferramentas de visualização ([Science Server](../lsp/sci_server.md)) e acesso a dados ([User Query](../lsp/user_query.md)). Desse modo, toda a análise de dados pode ser feita _online_ dentro da plataforma, desde a leitura, visualização, processamento e análise de resultados, sem a necessidade de _download_ dos dados. 

Ao clicar no _card_ "JupyterHub" dentro do _LIneA Science Platform_, você será direcionado para a página de login e em seguida para a página inicial do JupyterHub que mostrará o seu perfil de usuário. Clique no botão **START** para iniciar.       

A instalação padrão do JupyterHub utiliza a nova interface [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) e é baseada na imagem [datascience-notebook](https://github.com/jupyter/docker-stacks). Isto significa que uma série de bibliotecas _Python_ de grande popularidade como [Numpy](https://numpy.org/) e [Matplotlib](https://matplotlib.org/) estarão automaticamente disponíveis.


No notebook jupyterhub-tutorial.ipynb você encontrará instruções para utilizar a plataforma, instalar bibliotecas adicionais, acessar o banco de dados, visualizar os dados de catálogos e utilizar a integração com o LIneA Science Server para a visualização das imagens astronômicas. Para acessar o notebook, basta abrir um Terminal no JupyterLab clicando no botão "+" na barra superior e em seguida no ícone "Terminal" da seção "Other" na aba "Launcher", e inserir o comando:

```shell
git clone https://github.com/linea-it/jupyterhub-tutorial.git
```

 
 
