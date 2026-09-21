# Projetos

Esta página é um ponto de partida para quem passou a utilizar o Cluster Apollo no âmbito de um **projeto** com alocação de recursos no ambiente, por exemplo, projetos selecionados na chamada pública como o [SINCADA](https://www.linea.org.br/noticia/linea-lanca-o-programa-sincada), e outros casos apoiados pelo LIneA.

Ela não substitui as demais páginas de Processamento e Armazenamento. O objetivo é orientar o primeiro uso e destacar o que muda quando o trabalho está associado a um projeto, e não apenas a uma conta pessoal.

!!! info
    Hardware, partições, Slurm, Open OnDemand, Conda e o detalhamento das áreas de armazenamento continuam nas páginas específicas. Use os links desta página para chegar a elas.

## Antes de processar

1. **Conta no LIneA.** O acesso aos serviços e recursos do LIneA começa pelo registro de uma conta. O processo deve seguir um dos fluxos descritos em [Primeiros passos](../../primeiros_passos.md), de acordo com o perfil do usuário.

2. **Acesso ao HPC.** Possuir uma conta previamente aprovada **não** garante acesso automático ao Cluster Apollo após a aprovação de um projeto. Caso o acesso ao HPC ainda não esteja ativo, abra um [ticket](../../suporte.md), informando a sigla do projeto ao qual está vinculado.

3. **Políticas.** O uso do ambiente está sujeito às [políticas](../../politicas.md) do LIneA.

Cada pessoa associada a um projeto deve utilizar sua **própria conta**. O compartilhamento de contas não é permitido. Para incluir ou remover um colaborador de um projeto, o responsável deve abrir um [ticket](../../suporte.md), informando a sigla do projeto e o usuário que deve ser adicionado ou removido.

O caminho mais simples para o primeiro contato é pela plataforma [Open OnDemand](../uso/openondemand.md) (`https://ondemand.linea.org.br/`). A partir do navegador, você pode abrir um terminal no servidor de login, gerenciar arquivos, submeter jobs e, se necessário, iniciar um JupyterLab **no cluster**.

Há também o JupyterHub, disponível em `https://jupyter.linea.org.br`, que roda em uma infraestrutura distinta, baseada em Kubernetes. Embora compartilhe duas áreas de armazenamento com o cluster (`$HOME` e `/mnt/cl/prj/<sigla>`), os ambientes são independentes. O processamento de jobs no Apollo deve ser preparado e submetido a partir do próprio cluster. Veja [Como utilizar](../uso/howtouse-HPC.md).

!!! danger
    É proibido executar processamento no servidor de login. Qualquer código em execução neste servidor poderá ser interrompido sem aviso.

## Onde guardar os dados

| Área | Finalidade | Disponibilidade |
| --- | --- | --- |
| `/mnt/cl/prj/<sigla>` | Arquivos de longo prazo do projeto (NAS/NFS) | Login, Open OnDemand e Jupyter. **Não** disponível nos nós de computação |
| `$SCRATCH` | Dados de entrada, intermediários e saída dos jobs (Lustre) | Todos os nós do cluster. Temporário, com limpeza automática |
| `$SCRIPTS`<br>`/scripts/cl/prj/<sigla>` | Scripts de submissão e ambientes Conda | Todos os nós do cluster |
| `/data` | Dados de trabalho que exigem I/O de alto desempenho (Lustre) | Todos os nós do cluster. **Não** é o armazenamento permanente do projeto |
| `$HOME` | Configurações e arquivos pessoais | Login e Jupyter. **Não** disponível nos nós de computação |

Substitua `<sigla>` pelo identificador do projeto informado pelo LIneA. As características de retenção, backup e uso do Lustre estão descritas em [Armazenamento](../../armazenamento/index.md).

### Arquivo de longo prazo

Essa é a área que deve guardar os dados do projeto, projetada para armazenamento de longo prazo, e não para I/O paralelo dos jobs.

No terminal do Open OnDemand (servidor de login):

```bash
ls /mnt/cl/prj/<sigla>

cd /mnt/cl/prj/<sigla>
```

Quota **pessoal**:

```bash
show_quota
```

Quota do **projeto**:

```bash
show_proj_quota <sigla>
```

Se o diretório não existir ou você não tiver permissão, abra um [ticket](../../suporte.md) informando a sigla.

!!! warning
    `$SCRATCH` é temporário, sem backup e com limpeza automática. O que o projeto precisa conservar deve ser armazenado em `/mnt/cl/prj/<sigla>`, não permanecer apenas no `$SCRATCH` nem no `$HOME`.

### Dados de trabalho no Lustre

`/data` **não** é a área de armazenamento permanente do projeto. É uma área de trabalho em Lustre, destinada a dados que precisam estar disponíveis nos nós de computação e que exigem I/O de alto desempenho.

Use `/data` quando o job precisar trabalhar com grandes volumes de dados diretamente nos nós de computação. Para arquivos temporários, intermediários e resultados de um job, utilize `$SCRATCH`.

Não use `/data` como armazenamento permanente do projeto no lugar de `/mnt/cl/prj/<sigla>`.

### Ciclo de um job

A área de armazenamento do projeto não está disponível nos nós de computação. Por isso, os dados necessários ao processamento devem ser transferidos para uma área disponível no cluster antes da execução do job.

Para dados temporários, intermediários e resultados de um job, utilize `$SCRATCH`. Quando o processamento exigir uma área de trabalho em Lustre para dados que precisam permanecer disponíveis aos nós de computação, utilize `/data`.

**1. No servidor de login, copie o necessário para o `$SCRATCH`**

```bash
mkdir -p $SCRATCH/meu-job

cp -a /mnt/cl/prj/<sigla>/entrada $SCRATCH/meu-job/
```

**2. Coloque o script em `$SCRIPTS` e submeta o job**

```bash
cd $SCRATCH/meu-job

sbatch $SCRIPTS/submeter.sh
```

No script, os caminhos de entrada e saída devem apontar para `$SCRATCH` (ou `/data`, quando apropriado), e não para `/mnt/cl/prj/<sigla>`. Detalhes em [Como utilizar](../uso/howtouse-HPC.md).

**3. Quando o job terminar e você estiver satisfeito com o resultado, ainda no servidor de login, transfira o resultado para a área de armazenamento do projeto.**

```bash
cp -a $SCRATCH/meu-job/saida /mnt/cl/prj/<sigla>/
```

Depois, remova do `$SCRATCH` os dados que já estiverem armazenados na área do projeto, para evitar a perda na limpeza automática e o uso desnecessário da quota.

!!! danger
    Não aponte o job para `/mnt/cl/prj/<sigla>`. Essa área não está montada nos nós de computação. Também não rode o processamento em `login`.

### Scripts e Conda do projeto

Ambientes Conda compartilhados ficam em `/scripts/cl/prj/<sigla>`. A criação do diretório é solicitada ao Service Desk; o passo a passo está em [Conda no ambiente HPC](../uso/howtouse-HPC.md#conda-no-ambiente-hpc).

Instale pacotes a partir do servidor de login: os nós de computação **não possuem acesso à internet**.

## O cluster é compartilhado

O ambiente Apollo atende também outros projetos. Para esses projetos:

- Use as partições gerais (`cpu_dev`, `cpu_small`, `cpu`, `cpu_long`). A partição `cpu_bpglsst` é exclusiva de membros do BPG LSST.

- Em alguns períodos o cluster fica indisponível para a produção de *redshifts* fotométricos do programa in-kind BRA-LIN. Os usuários são avisados por e-mail. 

Limites de tempo, nós e os detalhes de um job estão em [Slurm](./index.md#slurm). Exemplos de script: [Job Script](../uso/templates-jobs.md).

## Reconhecimento

Publicações, teses e apresentações que usem o ambiente devem reconhecer o LIneA. O texto sugerido para a comunidade brasileira e para projetos de chamada pública (por exemplo, SINCADA) está em [Créditos](../../creditos.md#uso-dos-recursos-do-linea).

## Onde ir em seguida

| Preciso de… | Página |
| --- | --- |
| Primeiro acesso pelo navegador | [Open OnDemand](../uso/openondemand.md) |
| Submeter jobs e usar scratch/scripts | [Como utilizar](../uso/howtouse-HPC.md) |
| Hardware, partições e Slurm | [Cluster Apollo](./index.md) |
| Containers | [Apptainer](../uso/apptainer-uso.md) |
| Áreas, quotas e Lustre | [Armazenamento](../../armazenamento/index.md) |
| Tutoriais em vídeo | [Cursos e Tutoriais](../../cursos.md) |
| Dúvidas | [Suporte](../../suporte.md) |
