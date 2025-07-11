# HPE Apollo 2000


O **Cluster Apollo** possui 28 nós computacionais e oferece um total de **1072 cores** físicos. Seus nós são equipados com processadores `Intel Xeon Skylake 5120 2.2GHz` (apl01-16) e `Intel Xeon Gold 5320 2.20GHz` (apl17-28). O conjunto de máquinas provê cerca de 85 Tflops de capacidade computacional. 

Os 28 nós computacionais do Cluster Apollo são da família de servidores HPE ProLiant, sendo 16 do modelo XL170r e 12 do modelo XL220n. Atualmente, o número de cores disponíveis é de *2144*, pois o HT estar ativo nos nós de computação.

#### Características de cada servidor

| Modelo | # Cores (HT)<sup>[1]</sup> | RAM | OS | Hosts |
| ------- | ------| ------------ | -------- | -----------| 
| HPE Proliant XL170r  | 56    | 128 GB       | Rocky Linux 9.5 |  apl[01-16]  |
| HPE Proliant XL220n  | 104   | 256 GB       | Rocky Linux 9.5 |  apl[17-28]  |

<sup>[1]</sup> A tecnologia Hyper-Threading (HT) está habilitada em todos os nós do cluster. 

**Características do cluster (consolidado)**

| # Nodes | # Cores | Total de ram | Instalado em |
| ------- | ------| ------------ | -----------| 
| 16      | 896   | 1.5TB        |  Abr-2019  |
| 12      | 624   | 3TB          |  Jul-2023  |

O **Cluster Apollo** é gerenciado pelo **Slurm v24.05.5**.

### Filesystem

O **Cluster Apollo** conta com um sistema de arquivos de alta performance Lustre, disponibilizado como área de _"Scratch"_. O "Home" dos usuários está acessível apenas no nó de login e é fornecido através de NFS.

Essas áreas de armazenamento devem ser utilizadas da seguinte forma:

**Scratch:** Estrutura montada a partir do diretório `/scratch/<username>`. Utilizado para armazenar todos os arquivos que serão utilizados durante a execução de um job (scripts de submissão, executáveis, dados de entrada, dados de saída etc). Variável de ambiente `$SCRATCH`.

**Home:** Estrutura montada a partir do diretório `/home/<username>`. Utilizado para armazenar especialmente os resultados que se queira manter durante toda a vigência do projeto. Variável de ambiente `$HOME`.

**Scriptland:** Estrutura montada a partir do diretório `/scriptland/<username>`. É uma área de armazenamento otimizada para armazenar scripts e códigos. Variável de ambiente `$SCRIPTLAND`.

[Clique aqui para mais detalhes](/armazenamento/index.html)

!!! warning "Atenção"
    Não esqueça de copiar os arquivos necessários (executável, bibliotecas, dados de entrada) para dentro da área de SCRATCH, pois a área de HOMEDIR não é acessível pelos nós computacionais.

## Slurm
Slurm é um sistema de gerenciamento de cluster e agendamento de tarefas de código aberto, tolerante a falhas e altamente escalonável para clusters Linux grandes e pequenos. Slurm não requer modificações no kernel para sua operação e é relativamente independente. Como gerenciador de carga de trabalho de cluster, o Slurm tem três funções principais: 

 - alocar acesso exclusivo e/ou não exclusivo aos recursos (nós de computação) aos usuários por um determinado período de tempo para que possam realizar o trabalho. 
 - oferecer uma estrutura para iniciar, executar e monitorar o trabalho (normalmente um trabalho paralelo) no conjunto de nós alocados.
 - gerenciar a fila de submissão, arbitrando conflitos entre os pedidos de recursos computacionais.

### Partições disponíveis

O cluster Apollo é organizado em diferentes partições (subconjunto de máquinas) para atender a diferentes necessidades, por exemplo, a garantia da prioridade máxima dos usuários do projeto LSST na utilização das máquinas dedicadas ao IDAC-Brasil. 

|PARTITION   |TIMELIMIT  |NODES  |NODELIST  |
|------------|-----------|-------|----------|
|cpu_dev     |30:00      |26     |apl[01-28]|
|cpu_small   |3-00:00:00 |26     |apl[01-28]|
|cpu         |5-00:00:00 |26     |apl[01-28]|
|cpu_long    |31-00:00:0 |26     |apl[01-28]|
|lsst_cpu_dev     |30:00      |12     |apl[17-28]|
|lsst_cpu_small   |3-00:00:00 |12     |apl[17-28]|
|lsst_cpu         |5-00:00:00 |12   |apl[17-28]|
|lsst_cpu_long    |10-00:00:0 |12     |apl[17-28]|


### Accounts disponíveis 

- **Workflow** – Interrompe qualquer job que esteja rodando: **hpc-photoz** (photoz)
- **LSST** – Próximo da fila: **hpc-lsst** [somente nas novas apollos apl[17-28]] (lsst)
- **Grupo A** - Prioridade Maior: **hpc-bpglsst** (itteam, bpg-lsst)
- **Grupo B** - Prioridade Intermediária: **hpc-collab** (des, desi, sdss, tno)
- **Grupo C** - Prioridade Menor: **hpc-public** (linea-members)

As partições (**cpu_dev**, **cpu_small**, **cpu** e **cpu_long**) possuem todas as apollos (*apl[01-28]*), enquanto as partições do grupo LSST possuem apenas as *apl[17-28]*. Somente a account *hpc-lsst* poderá submeter jobs nas partições de prefixo "lsst", que possuem maior prioridade nos nodes.

!!! warning "Atenção"
	Como parte do programa de contribuição in-kind BRA-LIN, o IDAC Brasil possui o compromisso de gerar _redshifts_ fotométricos anualmente para o levantamento LSST, sempre na época que antecede as liberações oficiais dos dados. Nestes períodos, o Cluster Apollo será totalmente ocupado para este propósito por um tempo estimado de algumas horas, mas podendo se estender a alguns dias. Na ocasião, os usuários serão informados com antecência sobre a indisponibilidade do cluster por e-mail. [Clique aqui](https://linea-it.github.io/pz-lsst-inkind-doc/) para saber mais sobre a produção de medidas de _redshift_ e o programa de conrtribuição in-kind BRA-LIN. 

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

#### Especificando Recursos
O Slurm tem sua própria sintaxe para solicitar recursos de computação. Abaixo está uma tabela de resumo de alguns recursos solicitados com frequência e a sintaxe de Slurm para obtê-los. Para obter uma lista completa da sintaxe, execute o comando man sbatch.

|Sintaxe  |Significado|
|---------|-----------|
|#SBATCH -p partition  | Define a partição em que o job será executado|
|#SBATCH -J job_name | Define o nome do Job|
|#SBATCH -n quantidade | Define o número total de tarefas da CPU.|
|#SBATCH -N quantidade  | Define o número de nós de computação solicitados.|

#### Comandos Básicos do Slurm
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


!!! info "Mais Informações ?"
    Saiba como utilizar o Cluster Apollo em [Como Utilizar](/processamento/uso/howtouse-HPC.html). <br> 
    Para maiores informações, entre em contato com o [Service Desk](/suporte.html).
