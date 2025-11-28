O [*Target Viewer*](https://target.linea.org.br) é uma ferramenta personalizada para a visualização de imagens astronômicas baseada em listas de objetos definidos previamente pelo usuário. O sistema utiliza o *Aladin Lite v3* para proporcionar uma experiência integrada de exploração de catálogos armazenados no espaço pessoal de banco de dados do usuário.

Analogamente ao *Sky Viewer*, o *Target Viewer* foi originalmente desenvolvido no âmbito do *survey* *DES* (*Dark Energy Survey*). A iteração atual, projetada para integrar o portfólio de serviços do IDAC-Brasil, é uma solução flexível que oferece níveis de acesso diferenciados: dados públicos destinados à comunidade geral e dados embargados (como os do LSST) restritos a membros autenticados.

## Visão geral

O *Target Viewer* permite aos astrônomos:

- Registrar tabelas personalizadas como catálogos de objetos
- Visualizar imagens astronômicas centradas em cada objeto
- Navegar por catálogos extensos com funções avançadas de filtragem e ordenação
- Personalizar a configuração de visualização por catálogo

---

## Primeiros passos

### Passo 1: Criar a lista de objetos no User Query

Antes de utilizar o *Target Viewer*, é necessário criar uma tabela contendo os objetos de interesse através do [*User Query*](https://userquery.linea.org.br/query).

A tabela deve conter pelo menos três colunas:

- Um **identificador de objeto** (ex.: `objectId`, `id`)
- **Ascensão Reta** em graus decimais (ex.: `ra`)
- **Declinação** em graus decimais (ex.: `dec`)

A tabela pode ser criada através de:

- Execução de uma consulta SQL e armazenamento dos resultados
- União de dados de catálogos existentes com critérios de seleção próprios

!!! note
    Para obter instruções detalhadas sobre a criação e o armazenamento de tabelas, consulte a [documentação do User Query](https://docs.linea.org.br/userquery).

### Passo 2: Registrar a tabela no Target Viewer

1. Acesse o [*Target Viewer*](https://targetviewer.linea.org.br) e faça login
2. Clique em **"Add Catalog"** na página principal
3. Siga o assistente de registro de 3 passos (descrito a seguir)

### Passo 3: Visualizar os objetos

Após o registro, o catálogo aparecerá na página principal. Clique nele para começar a explorar os objetos com imagens astronômicas.



## Página principal

A página principal exibe todos os catálogos registrados. Cada entrada apresenta o nome do catálogo, o nome da tabela original, a data de criação e o número de linhas.

Clique no nome de um catálogo para abrir a interface de visualização, ou utilize o botão **"Add Catalog"** para registrar uma nova tabela.

![Página principal do Target Viewer](../../images/target-home.png)

---

## Registro de um novo catálogo

O processo de registro consiste em três passos. Se interrompido, o sistema solicitará que você continue ou descarte o registro pendente na próxima visita.

### Passo 1: Informações básicas

Selecione a tabela na lista suspensa e forneça um título descritivo. Apenas tabelas ainda não registradas aparecerão na lista.

![Target Viewer - Passo 1 do registro](../../images/target-register-step1.png)

### Passo 2: Associação de colunas

Este passo estabelece a correspondência entre as colunas da tabela e os campos requeridos. O sistema precisa identificar quais colunas contêm o ID do objeto, a AR e a Dec.

A interface exibe cada campo requerido com um indicador de cor:

- 🔴 **Vermelho** = Ainda não atribuído (requerido)
- 🟢 **Verde** = Atribuído corretamente

Selecione a coluna apropriada da sua tabela para cada campo requerido. O botão "Complete Registration" ficará disponível apenas quando todos os campos requeridos estiverem atribuídos.

**Atribuições requeridas:**

- **ID**: Coluna do identificador de objeto
- **RA**: Coluna de Ascensão Reta (deve estar em graus decimais)
- **Dec**: Coluna de Declinação (deve estar em graus decimais)

![Target Viewer - Passo 2 do registro](../../images/target-register-step2.png)

### Passo 3: Confirmação

Revise a configuração e clique em **"Complete Registration"** para finalizar. O catálogo aparecerá imediatamente na página principal.

---

## Visualização de catálogos

A interface principal de visualização é dividida em dois painéis:

- **Painel esquerdo**: Tabela de dados com os registros do catálogo
- **Painel direito**: Visualizador de imagens astronômicas *Aladin Lite*

![Visualização de catálogo no Target Viewer](../../images/target-catalog-view.png)

### Uso da tabela de dados

A tabela de dados exibe todos os registros do catálogo com funcionalidades completas de ordenação e filtragem.

**Navegação:**

- Clique nos cabeçalhos de coluna para ordenar
- Utilize os ícones de filtro para aplicar condições (igual a, contém, maior que, etc.)
- Navegue entre páginas através dos controles na parte inferior

**Seleção:**

- Clique em qualquer linha para selecioná-la
- O visualizador *Aladin Lite* centraliza automaticamente no objeto selecionado
- Um círculo marcador indica a posição do objeto

!!! note
    A seleção é mantida enquanto a aba do navegador permanecer aberta. Ao atualizar a página a seleção é preservada, mas ao fechar a aba ela é perdida.

### Ações da barra de ferramentas

Acima do visualizador *Aladin Lite*, uma barra de ferramentas disponibiliza ações rápidas para o objeto selecionado:

- **Object Detail**: Abre uma visualização detalhada em uma nova aba com todas as propriedades do objeto
- **Center on Target**: Recentraliza o visualizador no objeto selecionado
- **Show/Hide Marker**: Alterna a visibilidade do círculo marcador
- **Take Snapshot**: Faz o download da visualização atual como imagem



## Object Detail

Clique em **"Object Detail"** para abrir uma página dedicada ao objeto selecionado. Esta visualização apresenta:

- **Painel esquerdo**: Todas as propriedades do catálogo em formato de tabela
- **Painel direito**: Visualizador *Aladin Lite* completo centralizado no objeto

Esta funcionalidade é útil para inspeções detalhadas ou para compartilhar um link para um objeto específico.



## Configurações do catálogo

Os proprietários do catálogo podem acessar as configurações clicando no ícone de engrenagem (⚙️) no cabeçalho do catálogo.

### Opções disponíveis

**Metadados:**

- Atualizar o título e a descrição do catálogo

**Visualização:**

- **Default Survey**: Selecionar a imagem de fundo a ser exibida
- **Default FOV**: Definir o campo de visão inicial (em minutos de arco)
- **Marker Radius**: Definir o tamanho do marcador (em segundos de arco)

**Associação de colunas:**

- Modificar as atribuições de colunas ID/RA/Dec se necessário

**Remover catálogo:**

- Excluir o registro do catálogo (não afeta a tabela original)


## Image Surveys disponíveis no LIneA

Os seguintes *surveys* estão disponíveis como imagens de fundo:

| *Survey* | Acesso | Descrição |
|----------|--------|-----------|
| *DES DR2 IRG* | Público | *Dark Energy Survey* Data Release 2 (composição em cores) |
| *Rubin First Look* | Público | Imagens públicas iniciais do Observatório Rubin |
| *LSST DP0.2* | Restrito | *Data Preview 0.2* (Simulação). Exclusivo para membros. |
| *LSST DP1* | Restrito | *Data Preview 1*. Exclusivo para membros. |

Os *surveys* restritos aparecem na lista suspensa apenas se o usuário pertencer ao grupo de colaboração correspondente. A autenticação é gerenciada automaticamente através da *LIneA Science Platform*.

!!! note
    Além dos *surveys* hospedados no LIneA, é possível acessar qualquer *survey* público ou sobreposição de catálogos do CDS através dos menus nativos do *Aladin Lite* descritos a seguir.



## Referência do visualizador Aladin Lite

O *Target Viewer* utiliza o *Aladin Lite v3* para a visualização de imagens astronômicas. Esta seção descreve as funcionalidades nativas do visualizador, as quais são independentes da aplicação *Target Viewer*.

### Barra de busca

Localizada na parte superior do visualizador. Permite navegar para qualquer posição através de:

- **Coordenadas**: Insira o formato decimal ou sexagesimal (ex.: `13 25 27.6 -43 01 09`)
- **Nome de objeto**: Insira um identificador (ex.: `NGC 4755`) para resolver através do serviço *Sesame*

### Gerenciador de camadas (ícone de pilha)

Acesso a catálogos e *surveys* remotos do CDS:

- **Overlays**: Adicionar camadas de catálogos do SIMBAD, Gaia, 2MASS e outros
- **Surveys**: Alternar entre diferentes *surveys* de imagens de servidores remotos

### Controles de imagem

Quando um *survey* é selecionado no gerenciador de camadas:

- **Contrast**: Ajustar brilho, saturação e gama
- **Opacity**: Controlar a transparência da camada
- **Colormap**: Alterar a representação de cores (Rainbow, Heatmap, Grayscale, etc.)

### Configurações (ícone de engrenagem)

Alternar auxílios de visualização:

- **Coordinate Grid**: Exibir linhas de AR/Dec
- **Reticle**: Exibir mira central
- **HEALPix Grid**: Exibir grade de tesselação

### Sistemas de coordenadas

Alternar entre referenciais:

- **ICRS**: Coordenadas equatoriais (sexagesimal)
- **ICRSd**: Coordenadas equatoriais (graus decimais)
- **GAL**: Coordenadas galácticas

### Projeções

Projeções de mapa disponíveis:

- **Tangential**: Ideal para campos pequenos, círculos máximos retos
- **Stereographic**: Preserva formas, adequada para campos moderados
- **Spheric**: Visualização de globo virtual
- **Zenital equal-area**: Preserva áreas, adequada para estudos de densidade
- **Mercator**: Cilíndrica, adequada para regiões equatoriais
- **Hammer-Aitoff**: Elipse de área igual para todo o céu
- **Mollweide**: Pseudocilíndrica para todo o céu

### Menu contextual (clique com botão direito)

- **What is this?**: Consultar SIMBAD para objetos na posição do clique
- **Copy Coordinates**: Copiar AR/Dec para a área de transferência
- **Take a snapshot**: Fazer download da visualização atual como imagem
- **Select sources**: Desenhar regiões para filtrar pontos do catálogo

### Reconhecimentos

Esta aplicação utiliza o *Aladin sky atlas* desenvolvido no CDS, Observatório de Estrasburgo, França:

- [2000A&AS..143...33B](https://ui.adsabs.harvard.edu/abs/2000A%26AS..143...33B/abstract){:target="_blank"} (Aladin Desktop)
- [2014ASPC..485..277B](https://ui.adsabs.harvard.edu/abs/2014ASPC..485..277B/abstract){:target="_blank"} (Aladin Lite v2)
- [2022ASPC..532....7B](https://ui.adsabs.harvard.edu/abs/2022ASPC..532....7B/abstract){:target="_blank"} (Aladin Lite v3)


## Suporte

Em caso de dúvidas ou problemas, entre em contato através da [*LIneA Science Platform*](https://scienceplatform.linea.org.br/contact) ou visite nosso [Portal de Documentação](https://docs.linea.org.br).
