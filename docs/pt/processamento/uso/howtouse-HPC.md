# Como Utilizar 

## Como acessar
O acesso ao nosso cluster pode ser feito, através do [**Open OnDemand**](/processamento/uso/openondemand.html) ou pelo do Terminal do **JupyterLab (K8S)**. Em ambas opções, é imprescindível possuir uma conta válida no ambiente computacional do LIneA. Caso não possua uma conta, entre em contato com o Service Desk por email (helpdesk@linea.org.br) para mais informações.

!!! warning "Atenção"
    Mesmo possuindo uma conta ativa no LIneA, o acesso ao ambiente de processamento HPC não é automático. Para mais informações entre em contato com o Service Desk pelo email helpdesk@linea.org.br.

**Acessando pelo terminal do JupyterLab**

Na [**tela inicial**](../img/tela-jupyter.png) do seu Jupyter Notebook, na seção **_"Other"_**, você encontrará o botão do terminal. Ao clicar nele, você será redirecionado para um terminal Linux, inicialmente localizado em seu diretório _home_. Para acessar o Cluster Apollo, basta executar o seguinte comando:
  ```bash
    ssh loginapl01
  ```

A máquina <font color=\"#172b4d\">**_loginapl01_**</font> é onde você poderá fazer a alocação do nó de computação para submeter o seu job. 

!!! warning "$HOME e $SCRATCH"
    Os nós de computação não possuem acesso ao seu diretório _home_ de usuário. Mova ou copie, para seu diretório _SCRATCH_, todos os arquivos necessários para a submissão do seu job.

## Como usar a área de Scratch
Seu diretório `SCRATCH` é o local para onde você pode direcionar os arquivos resultados do seu job, assim como armazenar **temporariamente** dados que são utilizados pelo código no momento do processamento.

- **Para acessar o seu diretório SCRATCH:**

  ```bash
    cd $SCRATCH
  ``` 

- **Para enviar arquivos para seu diretório SCRATCH:**

  ```bash
    cp <ARQUIVO> $SCRATCH
  ``` 

#### Limpeza Automática do Scratch
O **scratch** é uma área de armazenamento temporária destinada à arquivos de saída e processamento dos jobs executados no cluster. Para manter o ambiente organizado e garantir espaço disponível para todos, está em vigor um **script de limpeza automática**, que é executado **uma vez por semana**.

Esse processo remove **arquivos que não foram acessados dentro do período de retenção definido** - atualmente, **45 dias**.

Arquivos de configuração essenciais (ex.: `.bashrc`, `.bash_profile`, `.ssh`, etc.) são preservados automaticamente e **não entram no processo de exclusão**.

!!! danger "ATENÇÃO"
	O scratch não deve ser usado para armazenamento permanente. Recomendamos mover dados importantes para seu diretório **home**.
## Como usar a área de Scripts
Seu diretório `SCRIPTS` é o local para onde você pode armazenar scripts, códigos para serem executados no cluster. Recomenda-se também utilizar essa área para criação de _environments_ Conda.

- **Para acessar o seu diretório SCRIPTS:**

  ```bash
    cd $SCRIPTS
  ``` 

!!! warning "Fique Atento"
	A área de scripts não está incluída na rotina de backups. Por isso, não deve ser utilizada como armazenamento permanente de dados.
## Como Submeter um Job
Um Job solicita recursos de computação e especifica os aplicativos a serem iniciados nesses recursos, juntamente com quaisquer dados/opções de entrada e diretivas de saída. O gerenciamento e agendamento das tarefas e recursos do cluster é feito através do Slurm. Logo, para submeter um Job é necessário utilizar um script como abaixo:

```bash
  #!/bin/bash
  #SBATCH -p PARTITION                       #Name of the Partition to use
  #SBATCH --nodelist=NODE                    #Name of the Node to be allocated
  #SBATCH -J simple-job			                 #Job name
  #----------------------------------------------------------------------------#

  ##path to executable code
  EXEC=/scripts/YOUR.USER/EXECUTABLE.CODE

  srun $EXEC
```
Nesse script é preciso especificar: o **nome da fila (Partition)** que será usada; o **nome do nó** que será alocado para a excecução do Job; e o **caminho para o código/programa** a ser executado. 

!!! danger "ATENÇÃO"
	 **É expressamente proibida a submissão de _jobs_ diretamente para máquina _loginapl01_. Qualquer código em execução nessa máquina será interrompido imediatamente, sem aviso prévio.**

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

!!! warning "Acesso à internet"
    Os nós de computação **não** têm acesso à internet. Pacotes e bibliotecas devem ser instalados a partir da _loginapl01_ em sua área de scripts .

## Gerenciador de pacotes EUPS

O [EUPS](https://github.com/RobertLuptonTheGood/eups) é um gerenciador de pacotes alternativo (e oficial do LSST) que permite carregar variáveis de ambiente e incluir o caminho para programas e bibliotecas de forma modular.

- **Para carregar o EUPS:**

!!! info
    Atualmente o EUPS é carregado automaticamente após o usuário acessar qualquer máquina do cluster apollo.

  ```bash
    source /opt/eups/bin/setups.sh
  ```
  
  
- **Para listar todos os pacotes disponíveis:**

  ```bash
    eups list
  ```
  
- **Para listar um pacotes específico:**

  ```bash
    eups list <PACOTE>
  ```
  
- **Para carregar um pacotes na sessão atual:**

  ```bash
    setup <NOME DO PACOTE> <VERSÃO DO PACOTE>
  ```
  
- **Para remover o pacote carregado:**

  ```bash
    unsetup <NOME DO PACOTE> <VERSÃO DO PACOTE>
  ```

## Comandos úteis do Slurm  
Para aprender sobre todas as opções disponíveis para cada comando, insira `man <comando>` enquanto estiver conectado ao ambiente do Cluster.

| Comando  | Definição                                                                     |
| -------- | ----------------------------------------------------------------------------- |
| sbatch   | Envia scripts de tarefas para a fila de execução                              |
| squeue   | Exibir estado dos jobs                                                        |
| scontrol | Usado para exibir o estado Slurm (várias opções disponíveis apenas para root) |
| sinfo    | Exibir estado de partições e nós                                              |
| salloc   | Envia um job para execução ou inicia um trabalho em tempo real                |

## Vídeos tutoriais
* [How to login](https://youtu.be/3DHqWk7KGHw)
* [How to use EUPS](https://youtu.be/ifJqGEvqzdY)
* [How to access the Scratch](https://youtu.be/dnMzGYwICBw)
* [How to submit a Job](https://youtu.be/AbRCL_KsBVY)
