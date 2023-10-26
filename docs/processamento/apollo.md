# HPE Apollo 2000


O **Cluster Apollo** possui 28 nós computacionais e oferece um total de **1072 cores** físicos. Seus nós são equipados com processadores `Intel Xeon Skylake 5120, 14-cores, 2.2GHz` (apl01-14) e `Intel(R) Xeon(R) Gold 5320 CPU @ 2.20GHz` (apl15-26) com o sistema de _hyperthreading_ ativado. O conjunto de máquinas provê cerca de 80 Tflops de capacidade computacional. 

!!! note
    Os 28 nós computacionais do Cluster Apollo são da família de servidores HPE ProLiant, sendo 16 do modelo XL170r e 12 do modelo XL220n.
    

!!! attention
    Atualmente o número de cores disponíveis é de 2144 devido o _hyper-threading_ estar ativo nos nós de computação.


#### Características atuais: 

| # Nodes | # Cores | Total de ram | Instalado em |
| ------- | ------| ------------ | -----------| 
| 16<sup>[1]</sup>      | 448   | 2TB          |  Abr-2019  |
| 12      | 624   | 3TB          |  Jul-2023  |


<sup>[1]</sup> _atualmente 2 nós estão sendo usados como máquinas de serviço do cluster_.

O **Cluster Apollo** é gerenciado pelo **Slurm version 18.08.8**.


## Slurm
Slurm é um sistema de gerenciamento de cluster e agendamento de tarefas de código aberto, tolerante a falhas e altamente escalonável para clusters Linux grandes e pequenos. Slurm não requer modificações no kernel para sua operação e é relativamente independente. Como gerenciador de carga de trabalho de cluster, o Slurm tem três funções principais: 
 - alocar acesso exclusivo e/ou não exclusivo aos recursos (nós de computação) aos usuários por um determinado período de tempo para que possam realizar o trabalho. 
 - oferecer uma estrutura para iniciar, executar e monitorar o trabalho (normalmente um trabalho paralelo) no conjunto de nós alocados.
 - gerenciar a fila de submissão, arbitrando conflitos entre os pedidos de recursos computacionais.

### Acesse o nó de submissão

O acesso ao host de submissão é feito por chave `ssh` e é necessário que você possua uma conta válida no LIneA. Depois de se registrar como usuário do LIneA ([detalhes aqui](https://docs.linea.org.br/primeiros_passos.html#registro-de-usuarios)) e receber o e-mail de confirmação, entre em contato com o Service Desk através do e-mail [helpdesk@linea.org.br](mailto:helpdesk@linea.org.br) para receber orientações de como acessar o nó de submissão.

### Informações sobre partições disponíveis

|PARTITION |AVAIL  |TIMELIMIT  |NODES  |STATE |NODELIST|
|----------|-------|-----------|-------|------|--------|
|cpu_dev      |up      |30:00     |23   |idle |apl[01-26]|
|cpu_small    |up |3-00:00:00     |23   |idle |apl[01-26]|
|cpu          |up |5-00:00:00     |23   |idle |apl[01-26]|
|cpu_long     |up |31-00:00:0     |23   |idle |apl[01-26]|
|LSST         |up   |infinite      |9   |idle |apl[18-26]|

### Accounts disponíveis e suas prioridades
- **Workflow** – Interrompe qualquer job que esteja rodando: **hpc-photoz** (photoz)
- **LSST** – Próximo da fila: **hpc-lsst** [somente nas novas apollos apl[15-26]] (lsst)
- **Grupo A** - Prioridade Maior: **hpc-bpglsst** (itteam, bpg-lsst)
- **Grupo B** - Prioridade Intermediária: **hpc-collab** (des, desi, sdss, tno)
- **Grupo C** - Prioridade Menor: **hpc-public** (linea-members)

As partições (**cpu_dev**, **cpu_small**, **cpu** e **cpu_long**) possuem todas as apollos (*apl[01-26]*), enquanto a partição LSST possui apenas as *apl[15-26]*. Porém, somente o account *hpc-lsst* poderá submeter jobs nessa partição (**LSST**), que possui prioridade maior nesses nodes.


### Anatomia de um Job

Um Job solicita recursos de computação e especifica os aplicativos a serem iniciados nesses recursos, juntamente com quaisquer dados/opções de entrada e diretivas de saída. O usuário envia a tarefa, geralmente na forma de um script de tarefa em lote, ao agendador em lote.
O script de tarefa em lote é composto por quatro componentes principais:

 - O intérprete usado para executar o script
 - Diretivas “#” que transmitem opções de envio padrão.
 - A configuração de variáveis de ambiente e/ou script (se necessário)
 - Os aplicativos a serem executados junto com seus argumentos e opções de entrada.

Aqui está um exemplo de um script em lote que solicita 3 nós na partição "cpu" e inicia 36 tarefas do myApp nos 3 nós alocados:

```bash
#!/bin/bash
#SBATCH -N 3
#SBATCH -p cpu
#SBATCH --ntasks 36

srun myApp
```

Quando a tarefa estiver agendada para execução, o gerenciador de recursos executará o script da tarefa em lote no primeiro nó da alocação.


#### Executando um Job
Após ter construído o script de execução com todas basta chama-lo utilizando o comando sbatch como mostra o exemplo abaixo:

```bash
[usuario@loginapl01]$ sbatch myscript.slurm
```

Se o script estiver correto haverá uma saída que indica o ID do job. Por exemplo:

```bash
[usuario@loginapl01]$ sbatch myscript.slurm
Submitted batch job 510
```

#### Andamento do Job
A maioria das especificações de um job pode ser vista através do comando `scontrol show job <JobID>`. Mais detalhes sobre o trabalho, incluindo o script do trabalho, podem ser vistos adicionando o sinalizador `-d`. Um usuário não consegue ver o script do trabalho de outro usuário.

```bash
[usuario@loginapl01]$ scontrol show job 510  
```

O Slurm captura e relata o código de saída do script do job (tarefas sbatch), bem como o sinal que causou o término da execução.


#### Cancelando um Job

Se seu Job estiver em execução ou aguardando na fila, você poderá cancelar o trabalho usando o comando `scancel <JobId>`. Use o comando squeue se você não souber o ID do Job. Por exemplo:


```bash
[usuario@loginapl01]$ scancel 510
```

#### Especificando Recursos
O Slurm tem sua própria sintaxe para solicitar recursos de computação. Abaixo está uma tabela de resumo de alguns recursos solicitados com frequência e a sintaxe de Slurm para obtê-los. Para obter uma lista completa da sintaxe, execute o comando man sbatch.

|Sintaxe  |Significado|
|---------|-----------|
|#SBATCH -p partition  | Define a partição em que o job será executado|
|#SBATCH -J job_name | Define o nome do Job|
|#SBATCH -n quantidade | Define o número total de tarefas da CPU.|
|#SBATCH -N quantidade  | Define o número de nós de computação solicitados.|


#### Comandos Básico do Slurm
Para aprender sobre todas as opções disponíveis para cada comando, insira man <comando> enquanto estiver conectado ao ambiente do Cluster.

|Comando	| Definição|
|-----------|----------|
|sbatch	| Envia scripts de tarefas para a fila de execução|
|scancel	| Cancela um job|
|scontrol	| Usado para exibir o estado Slurm (várias opções disponíveis apenas para root)|
|sinfo	| Exibir estado de partições e nós|
|squeue	| Exibir estado dos jobs|
|salloc	| Envia um job para execução ou inicia um trabalho em tempo real|

#### Ambiente de Execução
Para cada tipo de trabalho acima, o usuário tem a capacidade de definir o ambiente de execução. Isso inclui definições de variáveis de ambiente, bem como limites de shell (`bash ulimit` ou `csh limit`). **sbatch** e **salloc** fornecem a opção `--export` para transmitir variáveis de ambiente específicas para o ambiente de execução. E também tem a opção `--propagate` para transmitir limites específicos do shell ao ambiente de execução.

#### Variáveis de ​​ambiente
A primeira categoria de variáveis de ambiente são aquelas que o Slurm insere no ambiente de execução do trabalho. Eles transmitem ao script da tarefa e informações do aplicativo, como ID da tarefa `(SLURM_JOB_ID)` e ID da tarefa `(SLURM_PROCID)`. Para obter a lista completa, consulte a seção `"OUTPUT ENVIRONMENT VARIABLES"` nas páginas [sbatch](https://slurm.schedmd.com/sbatch.html), [salloc](https://slurm.schedmd.com/salloc.html) e [srun](https://slurm.schedmd.com/srun.html).

A próxima categoria de variáveis de ambiente são aquelas que o usuário pode definir em seu ambiente para transmitir opções padrão para cada trabalho enviado. Isso inclui opções como o limite do relógio de parede. Para obter a lista completa, consulte a seção `"INPUT ENVIRONMENT VARIABLES"` nas páginas [sbatch](https://slurm.schedmd.com/sbatch.html), [salloc](https://slurm.schedmd.com/salloc.html) e [srun](https://slurm.schedmd.com/srun.html).


### Gerenciamento de pacotes (EUPS)
O objetivo do EUPS é carregar as variáveis de ambiente e a inclusão no path dos programas e bibliotecas de forma modular:

!!! attention
    Esses comandos deverão ser executados dentro do ambiente LIneA.

- Para carregar o EUPS:
```bash
. /mnt/eups/linea_eups_setup.sh
```

- Listar todos os pacotes disponíveis:
```bash
eups list 
```

- Instalar um pacote:
```bash
setup scipy 0.11.0+2
```

- Desinstalar um pacote:
```bash
unsetup scipy 0.11.0+2
```
