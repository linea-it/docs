# Open OnDemand
O Open Ondemand é uma interface que facilita a utilização do nosso ambiente de HPC constituído pelo Cluster Apollo. Para acessar é necessário possuir uma conta válida no LIneA ([registre-se aqui](https://register.linea.org.br/registry/co_petitions/start/coef:155)). 
O acesso ao Open Ondemand é através de https://ondemand.linea.org.br/ .

Na [tela inicial](https://github.com/noobiagarcia/teste/blob/main/img/OOD1.png) da plataforma, na parte superior, é possível visualizar um menu com os seguinte itens:
* **Files** - fornece uma interface para o seu diretório de usuário (_Home Directory_).
* **Jobs** - fornece uma interface para as telas “Active Jobs” e “Job Composer”.
* **Clusters** - fornece um acesso para o terminal em um navegador da web.
* **Interactive Apps** - fornece acesso ao Jupyter Notebook.


## Home Directory
O [_Home Directory_](https://github.com/noobiagarcia/teste/blob/main/img/OOD2.jpeg) possibilita a visualização do diretório de usuário, onde estão armazenados seus arquivos, além de exibir uma variedade de botões com diferentes funcionalidades. 

#### Mudando de Diretório
Clicar no botão [**Change Directory**](img/OOD3.png) permite que você mude de diretório dentro da nossa infraestrutura. Para isso, basta escrever no campo **_Path_** o destino que deseja ir e apertar em "**ok**".

#### Acessando o Terminal
Clicar no botão [**Open Terminal**](img/OOD4.png) o levará ao terminal linux dentro da máquina de login (loginapl01) do Cluster Apollo. 
Neste terminal, você se encontra no diretório do usuário e tem a capacidade de visualizar seus arquivos. Você pode também executar todas as operações usuais de usuário HPC, como, por exemplo, alocar um nó de computação e verificar a fila Slurm usando comandos.

#### Criando, Transferindo e Movendo Arquivos 
No Open OnDemand a criação de novos arquivos e diretórios é bem simples, basta clicar nos botões **"New File"** e **"New Directory"** e escolher o nome que deseja. A transferência de arquivos é igualmente fácil, o botão **"Upload"** permite que você tranfira arquivos da sua máquina para o seu diretório de usuário em nosso ambiente, bem como o botão **"Download"** possibilita enviar os arquivos que foram selecionados para a sua máquina local. 

Para visualizar, renomear ou **editar** o novo arquivo criado, clique no **_"três pontinhos"_** que aparece ao lado direito do arquivo.

Para [**mover** ou **copiar**](img/OOD5.png) arquivos é preciso seguir os passos: 
1. Selecionar o arquivo que deseja;
2. Clicar no botão **"Copy/Move"**;
3. Clicar em **"Change Directory"** e escrever o caminho do diretório para onde deseja copiar ou mover o arquivo;
4. Clicar em "Copy" ou "Move" na caixa que aparece no canto esquerdo da tela.


## Jobs
Na seção [**Jobs**](img/OOD6.png) do menu inicial, encontram-se as opções "Job Composer" e "Active Jobs". A tela "Job Composer" facilita o processo de submissão de jobs e em [**"Active Jobs"**](img/OOD9.png) você pode acompanhar a execução do seu Job com detalhes.

Para submeter um job é necessário utilizar um script de submissão como este descrito abaixo: ([saiba mais](docs/processamento/apollo.md))

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
................................................[**_Para visualizar mais templates de script de submissão de Jobs, clique aqui_**]()...................................................
### Submetendo um Job - [Job Composer](img/OOD7.png)
O Open OnDemand facilita todo o processo de submissão de jobs, para isto basta seguir os seguintes passos:
1. Clicar no botão [**"New Job"**](img/OOD6.1.png);
2. Escolher a opção **"Default Template"**;
3. Editar as especificações do script de submissão em **"Open Editor"**;
4. Clicar em **"Submit"** para que o Job entre em execução.

!!!ATENÇÃO: Os nós de computação do cluster não possuem acesso ao seu diretório de usuário (Home Directory). Mova ou copie, para seu diretório SCRATCH, todos os arquivos necessários para a submissão do seu job. 

## Interactive Apps - [Jupyter Notebook](img/OOD12.png)
Com o Open OnDemand, é viável acessar o Jupyter Notebook em nosso ambiente de HPC. Por meio de "Interactive Apps", o Jupyter Notebook iniciará uma sessão em um dos nós de computação do cluster, bastando para isso:
* Clicar em ["Jupyter Notebook"](img/OOD8.png);
* Em seguida preencher os campos "Account", "Partition" e "Select node" que aparecem na tela; 
* Depois pressionar o botão ["Launch"](img/OOD10.png).  

Depois de iniciar a sessão, você poderá conectar-se ao Jupyter Notebook - [**"Connect to Jupyter"**](img/OOD11.png). Ao abrir um [**terminal dentro do Jupyter**](img/OOD14.png), é possível verificar que você está localizado em um nó do Cluster Apollo e tem acesso à sua área "_scratch_" no storage de armazenamento Lustre.


#### Para mais informações, assita os vídeos:
* [Usando o Open OnDemand]()
* [Como submeter um Job]()
* [Como acessar o Jupyter Notebook]() 
