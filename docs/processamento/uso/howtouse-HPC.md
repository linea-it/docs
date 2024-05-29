# Como Utilizar 

##Como acessar
O acesso ao nosso cluster pode ser feito, através do [**Open OnDemand**](http://127.0.0.1:8088/processamento/uso/openondemand.html) ou pelo do Terminal do **Jupyter Notebook (K8S)**. Em ambas opções, é imprescindível possuir uma conta válida no LIneA, registrando-se como usuário. Entre em contato com o Service Desk por email (helpdesk@linea.org.br) para mais informações.

####Acessando pelo terminal JupyterLab
<font color=\"#172b4d\">**_COMPLETAR INFOS_**</font>



A máquina <font color=\"#172b4d\">**_loginapl_**</font> é onde você poderá fazer a alocação do nó de computação para submeter o seu job. 

!!! warning 
    **Os nós de computação não possuem acesso ao seu diretório _(home)_ de usuário. Mova ou copie, para seu diretório SCRATCH, todos os arquivos necessários para a submissão do seu job.**

## Como usar a area de SCRATCH
Seu diretório SCRATCH é o local para enviar os arquivos essenciais à submissão do seu Job, assim como verificar os resultados após a execução do código. É crucial informar que **`todos os resultados e arquivos gerados devem ser transferidos de volta para o seu diretório de usuário (home)`**. Caso contrário, **`há o risco de perder esses arquivos armazenados no seu SCRATCH`**.

- **Para acessar o seu diretório SCRATCH:**
  ```bash
    ssh $SCRATCH
  ``` 
- **Para enviar arquivos para seu diretório SCRATCH:**
  ```bash
    cp <ARQUIVO> $SCRATCH
  ``` 

## Como Usar o Gerenciador de Pacotes EUPS
O [EUPS](https://github.com/RobertLuptonTheGood/eups) é um gerenciador de pacotes alternativo (e oficial do LSST) que permite carregar variáveis de ambiente e incluir o caminho para programas e bibliotecas de forma modular.

- **Para carregar o EUPS:**
  ```bash
    . /mnt/eups/linea_eups_setup.sh
  ```
- **Para listar todos os pacotes disponíveis:**
  ```bash
    eups list
  ```
- **Para listar um pacotes específico:**
  ```bash
    eups list | grep <PACOTE>
  ```
- **Para carregar um pacotes na sessão atual:**
  ```bash
    setup <NOME DO PACOTE> <VERSÃO DO PACOTE>
  ```
- **Para remover o pacote carregado:**
  ```bash
    unsetup <NOME DO PACOTE> <VERSÃO DO PACOTE>
  ```
  
## Como Submeter um Job
Um Job solicita recursos de computação e especifica os aplicativos a serem iniciados nesses recursos, juntamente com quaisquer dados/opções de entrada e diretivas de saída. O gerenciamento e agendamento das tarefas e recursos do cluster é feito através do Slurm. Logo, para submeter um Job é necessário utilizar um script como abaixo:

```bash
  #!/bin/bash
  #SBATCH -p PARTITION                       #Name of the Partition to use
  #SBATCH --nodelist=NODE                    #Name of the Node to be allocated
  #SBATCH -J simple-job			                 #Job name
  #----------------------------------------------------------------------------#

  ##path to executable code
  EXEC=/lustre/t0/scratch/users/YOUR.USER/EXECUTABLE.CODE

  srun $EXEC
```
Nesse script é preciso especificar o **nome da fila (Partition)** que será usada, o **nome do nó** que será alocado para a excecução do Job, e o **caminho para o código/programa** a ser executado.             
.............. [**Para mais templates de script de submissão de Jobs, clique aqui**](/processamento/uso/templates-jobs.html) ..............

- **Para submeter o Job:**
  ```bash
    sbatch script-submit-job.sh
  ```
Se o script estiver correto **haverá uma saída que indica o ID do job**.

- **Para verificar o andamento e informações do Job:**
  ```bash
    scontrol show job <ID> 
  ```
- **Para cancelar o Job:**
  ```bash
    scancel <ID> 
  ```

#### Assista também os vídeos:
* [How to login]()
* [How to use EUPS]()
* [How to use storage area]()
* [How to submit a Job]()

#### Alguns Comandos Slurm  
Para aprender sobre todas as opções disponíveis para cada comando, insira `man <comando>` enquanto estiver conectado ao ambiente do Cluster.

|Comando	| Definição|
|-----------|----------|
|sbatch	| Envia scripts de tarefas para a fila de execução|
|squeue	| Exibir estado dos jobs|
|scontrol	| Usado para exibir o estado Slurm (várias opções disponíveis apenas para root)|
|sinfo	| Exibir estado de partições e nós|
|salloc	| Envia um job para execução ou inicia um trabalho em tempo real|
