## LustreFS (HPC)

O ambiente do cluster Apollo conta com sistema de arquivos de alta performance [Lustre](https://www.lustre.org/) com dois níveis (_tiers_) de armazenamento, um em SSD com ~70 TB (_T0_) e outro em HDD com ~500 TB (_T1_), ambos conectados a uma rede infiniband EDR de 100 Gb/s. Os dois níveis de armazenamento estão disponíveis em `/lustre/t0` e `/lustre/t1`. 

Os usuários poderão acessar seu diretório de scratch através da variável de ambiente `$SCRATCH`, ou acessando o diretório localizado em `/lustre/t0/scratch/users/<username>`. 

    
### Características

Os sistemas de arquivos Lustre têm um desempenho bastante diferente dos discos locais comuns em outras máquinas. O Lustre foi desenvolvido para fornecer acesso rápido a grandes arquivos de dados necessários para aplicações paralelas. Ele é particularmente ineficiente em lidar com arquivos pequenos e em fazer muitas pequenas operações nesses arquivos. _**Esses casos devem ser evitados tanto quanto possível**_.

**Boa prática de uso em um sistema Lustre**

Para obter o melhor desempenho de um sistema Lustre, você deve usar o menor número possível de arquivos e, sempre que acessar um arquivo, deve ler/gravar o máximo de dados possível de cada vez. Um programa ideal usando Lustre leria um único arquivo de dados usando I/O paralelo (por exemplo, MPI IO), processaria os dados e, no final, gravaria um único arquivo novamente usando IO paralelo, sem uso intermediário do disco.

**Má prática de uso em um sistema Lustre**

Como o Lustre foi projetado para ler rapidamente um pequeno número de arquivos grandes, certos padrões de E/S que são perfeitamente adequados em outros sistemas causam uma carga muito alta em um sistema Lustre, por exemplo:

 1. leituras pequenas
 2. Abrindo muitos arquivos
 3. Procurando dentro de um arquivo para ler um pequeno pedaço de dados

Essas práticas são muito comuns em aplicativos que foram projetados para serem executados em sistemas onde cada nó possui seu próprio disco de trabalho local. Em outras palavras, comandos como `ls -l` ou `stat`devem ser evitados sempre que possível.


### Quotas

|area|TB  |blocks (soft)|blocks (hard)|grace period|inodes (soft)|inodes (hard)|grace period|
|----|----|-------------|-------------|------------|-------------|-------------|------------|
|T0 (scratch)  | 70 |     1 TB    |   1.25 TB   |  7 days    | 10000 files |11000 files  |  7 days    |


### Área de scratch

Os arquivos que não foram modificados nos últimos 60 dias serão automaticamente removidos.

!!! warning
    Essa área NÃO sofrerá backup e também NÃO será enviado aviso de remoção de arquivos!

!!! warning
    O script de limpeza é executado uma vez por semana, aos fins de semana.  


### Comandos úteis

a) Como acessar a minha área de scratch?
   
    cd $SCRATCH 
    
b) Como consultar a minha quota disponível?

    lfs quota -u $USER /lustre/t0
    
c) Como consultar os meus arquivos criados há _mais_ de 60 dias? 

    lfs find $SCRATCH --uid $UID -mtime +60 --print

c) Como consultar os meus arquivos criados há _menos_ de 60 dias? 

    lfs find $SCRATCH --uid $UID -mtime -60 --print
    
d) Como listar os OSTs do Lustre?

    lfs osts /lustre/t0

e) Como listar os arquivos armazenados há mais de 60 dias em um determinado OST do Lustre?

    lfs find $SCRATCH -mtime +60 --print --obd t0-OST0002_UUID
    
f) Como configurar o striping em diretório de modo a "quebrar" os arquivos e distribuir esses "pedaços" em 10 OSTs?

    lfs setstripe -c 10 $SCRATCH/meus_arquivos_grandes
    
g) Como consultar o striping de arquivos/diretórios?

    lfs setstripe -c $SCRATCH/meus_arquivos_grandes


!!! tip
    O Lustre do LIneA foi projetado para trabalhar a 100Gbps, para alcançar o máximo de performance faça uso do striping e sempre com arquivos grandes (+100MB).    


## NAS (NFS)

Os sistemas de armazenamento NAS são utilizados para armazenamento de longo prazo e não estão acessíveis através dos nós de processamento (HPC).

Características atuais: 

| Fabricante | Modelo | Capacidade | Instalado em |
| ------- | ------ | ------------ | -----------| 
| SGI<sup>[1]</sup>     | IS5500 | 540TB        |  Dez-2011  |
| SGI     | IS5600 | 240TB        |  Jul-2014  |


<sup>[1]</sup> _este equipamento foi desativado devido a problemas físicos em Jun/2023._


### /home

O diretório `home` é uma área para os usuários armazenarem seus arquivos pessoais e é acessível nos nós de login e também no 

### /archive

Área de armazenamento de dados brutos de catálogos astronômicos transferidos a partir de outros centros de dados ou produzidos internamente pelas diversas plataformas desenvolvidas pelo LIneA.

### /process

Área de armazenamento de dados provenientes do processamento de dados do DES realizados pelo [Portal do DES](https://des-portal.linea.org.br).


## Backup

| áreas | frequência | tipo | retenção |
| ----- | ---------- | ---- | ---------------- |
| /home | diário | incremental | 90 dias |
| /home | mensal | completo | 90 dias |
| /archive | - | - | - |
| /process | - | - | - |
| /scratch | - | - | - |

