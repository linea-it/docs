# Como Utilizar 

## Como acessar
O acesso ao nosso cluster pode ser feito, através do [**Open OnDemand**](./openondemand.md) ou pelo do Terminal do **JupyterLab (K8S)**. Em ambas opções, é imprescindível possuir uma conta válida no ambiente computacional do LIneA. Caso não possua uma conta, entre em contato com o Service Desk por email (helpdesk@linea.org.br) para mais informações.

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
Nesse script é preciso especificar: o **nome da fila (Partition)** que será usada; o **nome do nó** que será alocado para a execução do Job; e o **caminho para o código/programa** a ser executado. 

!!! danger "ATENÇÃO"
	 **É expressamente proibida a submissão de _jobs_ diretamente para máquina _loginapl01_. Qualquer código em execução nessa máquina será interrompido imediatamente, sem aviso prévio.**

- **Para submeter o Job:**

  ```bash
    cd $SCRATCH 
    sbatch $SCRIPTS/PATH/TO/script-submit-job.sh
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

## EUPS (gerenciador de pacotes)

O [EUPS](https://github.com/RobertLuptonTheGood/eups) é um gerenciador de pacotes alternativo (e oficial do LSST) que permite carregar variáveis de ambiente e incluir o caminho para programas e bibliotecas de forma modular.

- **Para carregar o EUPS:**

!!! info
    Atualmente o EUPS é carregado automaticamente após o usuário acessar qualquer máquina do cluster apollo.
  
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



## Conda no Ambiente HPC

### Visão Geral

O conda está disponível de forma centralizada em todos os nós do cluster em `/opt/conda`.

Os ambientes conda do sistema são mantidos pela equipe de operações e são somente leitura.

*(a partir daqui chamaremos de `env` ou `envs`)*

Cada usuário pode criar e gerenciar seus próprios `envs` pessoais.

---

### Ativando o Conda

O conda já está disponível assim que você faz login. Para verificar:

```bash
conda --version
```

---

### Envs Disponíveis

Para listar todos os `envs` disponíveis, do sistema e os seus pessoais:

```bash
conda env list
```

Exemplo de saída:

```
# conda environments:
base                     /opt/conda
base-science             /opt/conda/envs/base-science

meu-env                  /home/usuario/.conda/envs/meu-env
analise-xray             /scripts/usuario/.conda/envs/analise-xray
projeto-survey           /scripts/cl/prj/projeto-survey
```

Os `envs` em `/opt/conda/envs/` são mantidos pela equipe de suporte e não podem ser modificados.

---

### Ativando um `env`

```bash
conda activate base-science
```

Para voltar ao `env` base:

```bash
conda deactivate
```

---

### Criando `envs` pessoais

#### No home (recomendado para Jupyter e desenvolvimento)

```bash
conda create -n meu-env python=3.11
```

O `env` será criado em `~/.conda/envs/meu-env`, que é acessível tanto no cluster quanto no Jupyter.

#### No /scripts (recomendado para jobs no cluster)

```bash
conda create -p /scripts/$USER/.conda/envs/meu-env python=3.11
```

#### Para um projeto compartilhado

Caso não exista, solicite ao *helpdesk@linea.org.br* a criação do diretório do projeto em `/scripts/cl/prj/<projeto>`.

Após a criação:

```bash
conda create -p /scripts/cl/prj/<projeto> python=3.11
```

!!! warning "ATENÇÃO"
    **Atenção:** `/scripts` não possui garantia de backup. Mantenha seus arquivos de definição de `env` (`environment.yml`) versionados no git.

---

### Removendo um `env`

```bash
conda env remove -n meu-env
```

Ou pelo path completo:

```bash
conda env remove -p /scripts/$USER/.conda/envs/meu-env
```

---

### Instalando Pacotes

Sempre ative o `env` antes de instalar:

```bash
conda activate meu-env
conda install numpy scipy matplotlib
```

Para instalar de um canal específico:

```bash
conda install -c conda-forge astropy
```

Para instalar via pip dentro do `env`:

```bash
pip install meu-pacote
```

!!! warning "ATENÇÃO"
    **Atenção:** prefira sempre `conda install` antes de recorrer ao `pip`, para evitar conflitos de dependências.

---

### Removendo Pacotes

```bash
conda activate meu-env
conda remove numpy
```

---

### Exportando e Reproduzindo `envs`

Para exportar a definição do **env**:

```bash
conda activate meu-env
conda env export > environment.yml
```

Para recriar o `env` a partir do arquivo:

```bash
conda env create -f environment.yml
```

!!! info
    Recomendamos versionar o `environment.yml` no git para garantir a reprodutibilidade dos seus experimentos.

---

### Usando o `env` em Jobs

Em jobs submetidos ao scheduler, ative o conda explicitamente no script:

```bash
#!/bin/bash
#SBATCH --job-name=meu-job
#SBATCH --ntasks=1

source /etc/profile.d/conda.sh
conda activate meu-env

python meu_script.py
```

!!! info
    O `env` ativado no seu shell **não** é herdado automaticamente pelo job. O `source` e o `conda activate` são obrigatórios no script.

---

### Boas Práticas

- **Não instale conda no `/scratch`** — o scratch é para dados temporários de I/O, não para ambientes. Ambientes lá serão perdidos.
- **Não instale conda no `/scripts/<username>` diretamente** - use sempre o subdiretório `.conda/envs` dentro do seu diretório em `/scripts`.
- **Mantenha o `environment.yml` no git** - é a única garantia de reprodutibilidade do seu `env`.
- **Use `mamba` no lugar de `conda`** para instalar pacotes - é mais rápido na resolução de dependências:
  ```bash
  mamba install numpy scipy
  ```
- **Ambientes grandes** podem impactar a performance do filesystem. Se o seu job faz uso intensivo de muitos pacotes, considere falar com o *helpdesk* sobre o uso de `conda-pack`.

---

### Onde os `envs` são criados

| Contexto | Path padrão | Persistente |
|---|---|---|
| `conda create -n nome` | `~/.conda/envs/` | ✅ sim |
| Jupyter (k8s) | `~/.conda/envs/` | ✅ sim |
| Job no cluster | `/scripts/$USER/.conda/envs/` | ⚠️ sem backup |
| Projeto compartilhado | `/scripts/cl/prj/<projeto>` | ⚠️ sem backup |

---

### Usando conda-pack em Jobs de Larga Escala

O `conda-pack` empacota um `env` conda inteiro num arquivo `.tar.gz` que pode ser
descompactado e usado diretamente no nó de compute, sem acesso ao NFS durante a execução.

Isso é recomendado para jobs que rodam em muitos nós simultaneamente, evitando uma
rajada de acessos ao `/opt/conda` via NFS no momento da ativação.

Os `envs` do sistema já estão disponíveis empacotados em `/opt/conda/packed/`:

```
/opt/conda/packed/
└── base-science.tar.gz
```

#### Usando no job script

```bash
#!/bin/bash
#SBATCH --job-name=meu-job

# Descompacta o env no scratch local do nó
mkdir -p /scratch/$USER/envs/base-science
tar -xzf /opt/conda/packed/base-science.tar.gz -C /scratch/$USER/envs/base-science

# Ativa o env localmente, sem depender do NFS
source /scratch/$USER/envs/base-science/bin/activate

# Roda normalmente
python meu_script.py

# Limpa o env ao final do job
rm -rf /scratch/$USER/envs/base-science
```

#### Empacotando um `env` pessoal

Caso queira usar `conda-pack` com um `env` pessoal:

```bash
conda activate meu-env
conda pack -o /scratch/$USER/meu-env.tar.gz
```

!!! info
    **Atenção:** o arquivo `.tar.gz` pode ser grande dependendo do env.


#### Quando usar

| Situação | Recomendado |
|---|---|
| Desenvolvimento interativo no login | ❌ |
| Jupyter (k8s) | ❌ |
| Jobs curtos em poucos nós | ⚠️ opcional |
| Jobs longos ou com muitos nós simultâneos | ✅ |
| Ambientes com muitos pacotes | ✅ |

---

### Suporte

Em caso de dúvidas ou para solicitar a criação de `envs` do sistema, abra um chamado com o *helpdesk*.


---

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
