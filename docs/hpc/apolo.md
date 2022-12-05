# Sistemas

![Image](../images/icex.jpg)

## HPE ICEX + Apollo 2000

O cluster SGI ICE X 4 blades foi adquirido pelo LIneA em março de 2017 e possui pico de performance em torno de 3.379 # 1. Características
## Informações de hardware

![Image](../images/icex.jpg)


### HPE Apollo 2000

O cluster HPE Apollo 2000 possui 16 nós computacionais com processadores `Intel Xeon Skylake 5120, 14-cores, 2.2GHz` e provê cerca de 15 Tflops de capacidade computacional. Todo o conjunto de máquinas Apollo 2000 oferece 448 cores e 2Tflops. Com processadores `Intel Xeon Haswell E5-2650V4 12-Core 2.2GHz`, é composto de 4 nós de computação interligados por conexão Infiniband F de memória RAM. Cada nó possui 56 cores e 125GB de RAM.


| # Nodes | # Cores | Total de ram | Tflops | Instalado em |
| ------- | ------| ------------ | ------ | -----------| 
| 16      | 448   | 2TB          | 15.769 |  Abr-2019  |

## Filesystem e Armazenamento
- O ambiente conta com sistema de arquivos de altaperformance Lustre. Este possui dois níveis (tiers) de armazenamento, sendo o primeiro T0 em SSDR de 56Gbits num total de 96 cores e 512GB de memória RAM. Cada nó do ICE X possui 24 cores (hyperthreading desativado) e 125GB de RAM.

OBS: Em abril de 2019 o ICE X foi expandido para 19.148 Tflops com chegada das novas máquinas HPE Apollo 2000. Nessa expansão toda a infraestrutura do ICE X foi aproveitada.

## HPE Apollo 2000

O cluster HPE ICEX-Apollo 2000 possui 20 nós computacionais com processadores `Intel Xeon Skylake 5120, 14-cores, 2.2GHz` e provê cerca de 20 Tflops de capacidade computacional. Todo o conjunto de máquinas ICEX-Apollo 2000 oferece 544 cores e 2TB de memória RAM. Cada nó possui 56 cores e 125GB de RAM.


cluster              | # nós | # cores | total de ram | Tflops | Instalado em |
-------------------- | ------- | ------| ------------ | ------ | -----------| 
SGI Altix XE1300     | 42      | 504   | 4TB          |  4.838 |  Ago-2012  |
SGI ICE X            | 4       | 96    | 512GB        |  3.379 |  Abr-2017  |
HPE Apollo 2000      | 16      | 448   | 2TB          | 15.769 |  Abr-2019  |
           **Total** |**62**   |**1048** |**6.5TB**   |**23.986**|**Abr-2019**|
 com 70TB e o segundo T1 em HDD com 500TB;
- O HOME do usuário é fornecido através de NFS;
- Essas áreas de armazenamento são acessíveis tanto no host de submissão quanto nos nodes do cluster;

# 3. Módulos de Ambiente (EUPS)

EUPS é um sistema similar ao [modules](http://modules.sourceforge.net/) que permite um gerenciamento simples de múltiplas versões de uma aplicação em ambientes *nix, bem como suas dependências.

## Configurar ambiente com eups

Antes de utilizar o eups é preciso configurá-lo em sua sessão/ambiente, para isso faça:

```bash
source /mnt/eups/linea_eups_setup.sh 
```

## Como listar as aplicações instaladas?

Para listar as aplicações/bibliotecas instaladas no eups basta executar os comandos abaixo:

```bash
eups list
```

!!! note
    Caso alguma dependência não esteja listada, é possível solititar a instalação enviando e-mail ao nosso suporte em `helpdesk@linea.org.br`. 
    No assunto coloque `Instalação <nome aplicação/biblioteca> versão n.n`, no corpo da mensagem descreva suas preferências para a instalação e informe a url/site de onde a equipe de suporte poderá baixar a aplicação/biblioteca.

Exemplo de email solicitando a instalação de uma dependência:

```text

Assunto: Instalação pyfits v3.4
Corpo:
    Solicito que na compilação seja utilizada a versão 3.8 do python
    A biblioteca pode ser encontrada em https://github.com/spacetelescope/PyFITS

```

## Como configurar uma aplicação e suas dependências?

```bash

source /mnt/eups/linea_eups_setup.sh     # Inicializa o eups na sua sessão
setup pyfits 3.4+0                       # Adiciona/configura o pacote pyfits, na versão 3.4, revisão 0 na sua sessão
unsetup pyfits                           # Remove o pacote pyfits da sua sessão

```

!!! tip
    Ao configurar um pacote que está no repositório EUPS, na sua sessão, todas as suas dependências também são adicionadas automaticamente.
    Caso queira visualizar as dependências durante a configuração basta adicionar o parâmetro `-v`, neste caso o comando do exemplo ficaria
    `setup -v pyfits 3.4+0`.

# 4. Submeter Jobs

O Condor (oficialmente conhecido como [HTCondor](https://research.cs.wisc.edu/htcondor/)) é o agendador de tarefas, ou escalonador de processos, utilizado no ambiente e possui atualmente uma única fila padrão de processos. Novas filas serão disponibilizadas em breve.

### Universos

O Condor possui diferentes universos cada qual para execução de aplicações específicas. No ambiente, temos disponíveis os universos: `vanilla, docker, parallel e DAGMan`. Para a maioria dos casos de uso o universo `vanilla` é o recomendado.


### Como utilizar o Condor?

#### Acesse o nó de submissão

O acesso ao host de submissão é feito por chave `ssh` e é necessário que você possua uma conta válida (mais informações podem ser encontradas [aqui](../accounts/account.md)). Caso não possua uma conta, leia a nossa política de acesso e preencha o formulário de registro em [http://www.linea.org.br > Serviços > Registro de Participantes](http://www.linea.org.br)

#### Configure as dependências

Caso sua aplicação possua dependências você pode configurá-las em seu ambiente/seção através do programa eups, que é um gerenciador de pacotes.

```bash

source /mnt/eups/linea_eups_setup.sh 
setup pyfits 3.4+0                   
             

```

Mais instruções sobre o eups neste [link](#3-módulos-de-ambiente-eups)

#### Crie o arquivo de submissão

O arquivo de submissão é apenas um arquivo texto simples com as instruções para o condor. O formato das instruções, ou diretivas, segue o padrão `chave = valor`, uma por linha. Para um melhor entendimento veja os exemplos abaixo.

!!! tip
    Os nomes das chaves(diretivas) e dos valores **NÃO** são *case-sensitive*, ou seja, tanto faz você informar `getenv = true`ou `Getenv = TRUE`, o Condor 
    entenderá ambas da mesma forma. No entanto, caminhos para arquivos e nomes de arquivos **SÃO** *case-sensitive* então `Executable = /mydir/app` é diferente
    de `Executable = /MYDIR/App`.

#### Submeta à fila de execução

Para submeter à fila de execução basta executar o comando abaixo:

```sh
condor_submit myapp.sub
```

Onde `myapp.sub` é o arquivo de submissão que você criou e que contém todas as intruções para o condor executar sua aplicação.

##### Exemplos 

Abaixo três exemplos de como você pode utilizar o Condor para executar suas aplicações.

Antes de começar faça o clone do repositório [htcondor-examples](https://github.com/linea-it/htcondor-examples) conforme abaixo. 

Com o terminal aberto:

```sh
git clone https://github.com/linea-it/htcondor-examples
```

Acesse o diretório recém criado `htcondor-examples`.

```sh
cd htcondor-examples/
```

O diretório htcondor-examples possui três subdiretórios, cada um corresponde a um exemplo de como você pode utilizar o Condor e, além das aplicações, inclui os arquivos de submissão do Condor e os inputs quando necessários.

Lista de sub-diretórios e arquivos.

```sh
$ ls
Example1  Example2  Example3  LICENSE  README.md
```

###### Exemplo 1

No primeiro exemplo, vamos executar um script python que efetua a contagem de objetos em arquivos .fits e que possui como dependência a biblioteca `pyfits 3.4+0`.

!!! tip
    - No eups o número após a versão identifica a revisão do pacote, por exemplo: `numpy 2.3.1+1`, significa que é a versão `2.3.1` na revisão `1`.
    - Se você omitir o nome do pacote na hora de listar então o eups listará todos os disponíveis.
    - Se você omitir a versão na hora do 'setup' então o eups irá carregar a que estiver marcada como `current`.


Abra um terminal e acesse o diretório `Example1`.

```sh
cd Example1
```

Dentro do diretório `Example1` você encontrará os seguintes conteúdo:

```sh
$ ls
count_objects_into_fits.py  data  example1.sub  setup.sh
```

- **count_objects_into_fits.py** -> script python que efetua a contagem de linhas(objetos) em arquivos .fits
- **example1.sub** -> arquivo de submissão de trabalhos para o Condor.
- [**setup.sh**]() -> arquivo com os comandos utilizados para carregar o [eups](eups.md) e configurar a dependência `pyfits 3.4+0`.

**data** -> contém 2 arquivos .fits de exemplo.

O primeiro passo é carregar o eups no ambiente e as dependências do script `count_objects_into_fits.py` já contidas no arquivo `setup.sh`, para isso faça:

```sh
source setup.sh
```

Agora que as dependências estão carregadas vamos dar uma olhada no arquivo de submissão `example1.sub`.


Agora vamos criar o arquivo de submissão, segue abaixo o arquivo de exemplo `exemplo1.sub` para executar o script python no Condor.

```text
####################################
# Exemplo de arquivo de submissão  #
# para execução do script contador #
# de objetos em arquivos fits      #
####################################
getenv          = true
universe        = vanilla
executable      = $ENV(HOME)/htcondor-examples/Example1/count_objects_into_fits.py
notification    = Complete
notify_user     = carlosadean@linea.gov.br
error           = example1-$(Process).err
output          = example1-$(Process).out
log             = example1-$(Process).log

queue

```

Explicando as linhas do arquivo acima `exemplo1.sub`.

`getenv = true` instrui o Condor a copiar o atual ambiente `shell` do usuário para o nó remoto onde o script será executado. Útil para quando você carregar as dependências configuradas com o eups ou variáves definidas manualmente.

`universe = vanilla` indica o universo em que a aplicação será executada. No ambiente do CAPDA os universos podem ser `vanilla, docker, parallel(mpi) e DAGMan`.

`executable  = $ENV(HOME)/htcondor-examples/Example1/count_objects_into_fits.py` indica o caminho para o script.

`notification = Complete` habilita notificação quando a execução terminar. Deve ser completada com a próxima diretiva.

`notify_user = user@domain.com` e-mail para onde o Condor enviará as notificações de término de execução. **OBS:** *Se você submeter 1000 execuções receberá 1000 emails diferentes.*

`error = example1-$(Process).err` arquivo onde o condor irá armazenar a saída de erro padrão da sua aplicação. Útil para *debugar* problemas na execução.

`output = example1-$(Process).out` arquivo onde será armazenada a saída padrão. Tudo o que a aplicação exibir na tela será enviado para o `'output'`.

`log = example1-$(Process).log` registro de log da execução da aplicação. Por exemplo, é neste arquivo que fica o endereço da máquina remota onde a aplicação foi executada.

`queue` instrução para adicionar na fila de execução.

A 'variável' $(Process) é uma macro interna que recebe automaticamente um número inteiro sequencial.

Para executar a aplicação no cluster basta utilizar o comando `condor_submit` seguido do arquivo de submissão.

```bash
condor_submit exemplo1.sub
```

Uma vez submetido o trabalho (job) para acompanhar sua execução utilize o comando `condor_q`.

```bash
$ condor_q

-- Schedd: hostname.local : <10.148.0.7:9618?... @ 04/09/19 20:26:15
OWNER       BATCH_NAME    SUBMITTED   DONE   RUN    IDLE  TOTAL JOB_IDS
carlosadean ID: 6339     4/9  20:26      _      1      _      1 6339.0

Total for query: 1 jobs; 0 completed, 0 removed, 0 idle, 1 running, 0 held, 0 suspended 
Total for carlosadean: 1 jobs; 0 completed, 0 removed, 0 idle, 1 running, 0 held, 0 suspended 
Total for all users: 1 jobs; 0 completed, 0 removed, 0 idle, 1 running, 0 held, 0 suspended
```

Ao final da execução você verá os três arquivos definidos no arquivo de submissão estarão.

```bash
$ ls -lths

4.0K -rw-r--r-- 1 carlosadean production 1.1K Apr  9 20:28 example1-0.log
4.0K -rw-r--r-- 1 carlosadean production   49 Apr  9 20:28 example1-0.out
   0 -rw-r--r-- 1 carlosadean production    0 Apr  9 20:28 example1-0.err
   0 drwxr-xr-x 2 carlosadean production   46 Apr  9 18:50 data
4.0K -rw-r--r-- 1 carlosadean production  440 Apr  9 17:04 example1.sub
4.0K -rwxr-xr-x 1 carlosadean production  848 Apr  9 17:03 count_objects_into_fits.py
4.0K -rw-r--r-- 1 carlosadean production   56 Apr  9 16:55 setup.sh
```

A saída do script python está no arquivo `example1-0.out`.

```bash
$ cat example1-0.out 
# files: 2
# objects: 3060
```


###### Exemplo 2 - calculapi em C 

Este segundo exemplo trata-se de uma aplicação compilada com gcc e que também recebe argumentos.

```text
###################################
# Exemplo de arquivo de submissão #
# para executar um binário que    #
# recebe argumentos               #
###################################
universe    = vanilla
executable  = /home/user/bin/calculapi
arguments = 1000000
notification = Complete
notify_user = user@domain.com
error = calculapi-$(Process).err
output = calculapi-$(Process).out
log = calculapi-$(Process).log
queue
```

Explicando as linhas do arquivo acima `calculapi.sub`.

`universe = vanilla` indica o universo em que a aplicação será executada. No ambiente do CAPDA os universos podem ser `vanilla, docker, parallel(mpi) e DAGMan`.

`executable  = /home/user/bin/calculapi` indica o caminho para a aplicação.

`arguments = 1000000` essa diretiva do Condor recebe os argumentos que serão passados para a aplicação. **NÃO** passe os argumentos através da diretiva `executable`, fazendo assim sua aplicação será executada incorretamente.


`notification = Complete` informa ao Condor que você quer ser notificado quando a execução terminar. Deve ser completada com a próxima diretiva

`notify_user = user@domain.com` e-mail para onde o condor enviará as notificações de término de execução. **OBS:** *Se você submeter 1000 execuções receberá 1000 emails diferentes*.

`error = calculapi-$(Process).err` arquivo onde o condor irá armazenar a saída de erro padrão da sua aplicação. Útil para *debugar* problemas na execução.

`output = calculapi-$(Process).out` arquivo onde será armazenada a saída padrão. Tudo o que a aplicação exibir na tela será enviado para o `'output'`.

`log = calculapi-$(Process).log` registro de log da execução da aplicação. Por exemplo, é neste arquivo que fica o endereço da máquina remota onde a aplicação foi executada.

`queue` instrução para o condor adicionar na fila de execução.

A 'variável' $(Process) é uma macro interna do condor e recebe automaticamente um número inteiro sequencial.

Para executar a aplicação no cluster basta utilizar o comando `condor_submit` seguido do arquivo de submissão.

```bash

condor_submit calculapi.sub

```

###### Exemplo 3 - hello world mpi

Em desenvolvimento

#### Monitore a execução

A execução da sua aplicação pelo condor pode ser monitorada com o comando `condor_q`.

#### Remove jobs da fila ou em execução
- por JOB_ID: `condor_rm <JOB_ID>`
- todos jobs do usuário: `condor_rm <USERNAME>`

#### Troubleshooting

Em breve
