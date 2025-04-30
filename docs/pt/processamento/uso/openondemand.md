# Open OnDemand
O Open Ondemand é uma interface que facilita a utilização do nosso ambiente de HPC constituído pelo Cluster Apollo. Para acessar é necessário possuir uma conta válida no LIneA ([saiba mais](/primeiros_passos.html)). 
O acesso ao Open Ondemand é através de https://ondemand.linea.org.br/ .

Na [**tela inicial**](../img/OOD1.png) da plataforma, na parte superior, é possível visualizar um menu com os seguinte itens:

* **Files** - fornece uma interface para o seu diretório de usuário (_Home Directory_).
* **Jobs** - fornece uma interface para as telas “Active Jobs” e “Job Composer”.
* **Clusters** - fornece um acesso para o terminal em um navegador da web.
* **Interactive Apps** - fornece acesso ao Jupyter Notebook.

## Home Directory
O [**Home Directory**](../img/OOD2.jpeg) possibilita a visualização do diretório de usuário, onde estão armazenados seus arquivos, além de exibir uma variedade de botões com diferentes funcionalidades. 

#### Mudando de Diretório
Clicar no botão [**Change Directory**](../img/OOD3.png) permite que você mude de diretório dentro da nossa infraestrutura. Para isso, basta escrever o caminho no campo **Path** e em seguida apertar "**ok**".

#### Acessando o Terminal
Clicar no botão [**Open Terminal**](../img/OOD4.png) o levará ao terminal linux dentro da máquina de login (loginapl01) do Cluster Apollo. 
Neste terminal, você se encontra no seu diretório "_Home_" de usuário e tem a capacidade de visualizar seus arquivos. No terminal também é possível executar todas as operações usuais de usuário HPC, como por exemplo, alocar um nó de computação e verificar a fila Slurm usando comandos.

#### Criando, Transferindo e Movendo Arquivos 
No Open OnDemand a criação de novos arquivos e diretórios é bem simples, bem como a transferência deles.

* Clique nos botões **"New File"** ou **"New Directory"** e escolha o nome que deseja para o novo arquivo ou diretório. 
* O botão **"Upload"** permite que você transfira arquivos da sua máquina para o seu diretório _home_ em nosso ambiente; 
* O botão **"Download"** possibilita enviar os arquivos que foram selecionados para a sua máquina local. 

Para visualizar, renomear ou **editar** o novo arquivo criado, clique nos **_"três pontinhos"_** que aparecem ao lado direito do arquivo e você verá um menu com essas opções.

Para [**mover** ou **copiar**](../img/OOD5.png) arquivos é preciso seguir os passos: 

1. Selecionar o arquivo que deseja;
2. Clicar no botão **"Copy/Move"**;
3. Clicar em **"Change Directory"** e escrever o caminho do diretório para onde deseja copiar ou mover o arquivo;
4. Clicar em "Copy" ou "Move" na caixa que aparece no canto esquerdo da tela.

## Jobs
Na seção [**Jobs**](../img/OOD6.png) do menu inicial, encontram-se as opções "Job Composer" e "Active Jobs". A tela "Job Composer" facilita o processo de submissão de jobs e em [**"Active Jobs"**](../img/OOD9.png) você pode acompanhar a execução do seu Job com detalhes.

Para submeter um job é necessário utilizar um script de submissão como este descrito abaixo: ([saiba mais](/processamento/apollo/index.html#anatomia-de-um-job)) 

```bash
#!/bin/bash
#SBATCH -p PARTITION                       #Name of the Partition to use
#SBATCH --nodelist=NODE                    #Name of the Node to be allocated
#SBATCH -J simple-job			                 #Job name
#----------------------------------------------------------------------------#

##path to executable code
EXEC=/lustre/t0/scratch/users/YOUR.USER/ondemand/projects/EXECUTABLE.CODE

srun $EXEC
```
[**Para visualizar mais templates de script de submissão de Jobs, clique aqui**](/processamento/uso/templates-jobs.html)

### Job Composer
O Open OnDemand facilita todo o processo de submissão de jobs através da ferramenta [Job Composer](../img/OOD7.png). Para isto basta seguir os seguintes passos:

1. Clicar no botão [**"New Job"**](../img/OOD6.1.png);
2. Escolher a opção **"Default Template"**;
3. Editar as especificações do script de submissão em **"Open Editor"**;
4. Clicar em **"Submit"** para que o Job entre em execução.

!!! warning "Aviso Importante"
    Os nós de computação do cluster não possuem acesso ao seu diretório de usuário (Home Directory). Mova ou copie, para seu diretório SCRATCH, todos os arquivos necessários para a submissão do seu job. 

## JupyterLab
Com o Open OnDemand, é viável acessar o Jupyter Notebook em nosso ambiente de HPC. Por meio de "Interactive Apps", o Jupyter Notebook iniciará uma sessão em um dos nós de computação do cluster, bastando para isso:

1. Clicar em ["Jupyter Notebook"](../img/OOD8.png);
2. Em seguida preencher os campos "Account", "Partition" e "Select node"; 
3. Depois pressionar o botão ["Launch"](../img/OOD10.png).
4. Para conectar-se ao [JupyterLab](../img/OOD12.png), clicando em [**"Connect to Jupyter"**](../img/OOD11.png).  

!!! warning "Jupyter no Kubernetes VS Jupyter no HPC"
    Nós possuímos dois ambientes Jupyter em duas infraestruturas distintas. Um deles está disponível em [https://jupyter.linea.org.br](https://jupyter.linea.org.br) e é executado sobre [Kubernetes](https://kubernetes.io/). O outro, mencionado acima, é executado de forma interativa pela plataforma Open OnDemand diretamente nos nós de processamento.
Ao abrir um [**terminal dentro do JupyterLab**](../img/OOD14.png) via Open OnDemand, você poderá verificar que está localizado em um nó do Cluster Apollo e tem acesso à sua área "_scratch_" no storage de armazenamento Lustre.

### Criando Kernels Python
**Os comandos a  seguir devem ser executados no terminal em "LIneA Shell Access"**. <br> Para acessar: 

* Na pagina inicial do Open OnDemand, clique em: **Clusters -> LIneA Shell Access**.

Siga os comandos abaixo:

1. Vá para sua área SCRATCH, Instale e carregue o Miniconda:
``` bash
	cd $SCRATCH

    curl -L -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    chmod +x Miniconda3-latest-Linux-x86_64.sh
    ./Miniconda3-latest-Linux-x86_64.sh -p $SCRATCH/miniconda
    
    source miniconda/bin/activate 

    conda deactivate #Necessário para desativar o env "base"
```
2. Crie, ative um venv conda e instale o _ipykernel_:
``` bash
	conda create -p $SCRATCH/kernelname
    conda activate kernelname/
    
    conda install -c anaconda ipykernel
```
3. Configure o JUPYTER_PATH (obrigatório ser esse caminho abaixo):
```bash
	JUPYTER_PATH=$SCRATCH/.local
	echo $JUPYTER_PATH
    
    python -m ipykernel install --prefix=$JUPYTER_PATH --name 'kernelname'
```
4. Abra uma sessão do Jupyter Notebook.

???+ success "O output do último comando deve ser:"

    ```` yaml
    
    #[InstallIPythonKernelSpecApp] WARNING | Installing to /lustre/t0/scratch/users/YOUR.USER/.local/share/jupyter/kernels, which is not in ['/lustre/t0/scratch/users/YOUR.USER/kernelname/share/jupyter/kernels', '/home/YOUR.USER/.local/share/jupyter/kernels', '/usr/local/share/jupyter/kernels', '/usr/share/jupyter/kernels', '/home/YOUR.USER/.ipython/kernels']. The kernelspec may not be found.
    Installed kernelspec kernelname in /lustre/t0/scratch/users/YOUR.USER/.local/share/jupyter/kernels/kernelname
    
    ```` 

Ao final dessa execução de comandos, será possível ver o botão do kernel python criado.



## Vídeos tutoriais
* [Como submeter um Job](https://youtu.be/6w1H3VS40Ew)
* [Como acessar o Jupyter Notebook](https://youtu.be/SemHNDr8vjg) 

Qualquer dúvida, entre em contato com o [Service Desk](https://docs.linea.org.br/suporte.html).






