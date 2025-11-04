# Combine Redshift Catalogs

O **Combine Redshift Catalogs** é um pipeline que permite gerar uma única amostra unificada de redshifts combinando múltiplos catálogos individuais. Ele utiliza a biblioteca [LSDB](https://docs.lsdb.io/en/stable/index.html), desenvolvida pelo [LINCC Frameworks](https://lsstdiscoveryalliance.org/programs/lincc-frameworks/), para realizar o *crossmatch* espacial entre catálogos e identificar múltiplas medições de uma mesma galáxia. O pipeline oferece opções flexíveis para resolver ou manter duplicatas.

Esse processo é especialmente útil para preparar amostras de redshift limpas que podem ser usadas como *training sets*, *validation sets* ou entradas de calibração para estimativas de redshift fotométrico (*photo-z*).

## Como Executar o Pipeline

### Website do Photo-z Server

Ao executar o pipeline pela interface gráfica do [Photo-z Server](https://pzserver.linea.org.br/), o usuário deve fornecer as seguintes informações:

1. **Nome do catálogo combinado**  
    Um nome curto e descritivo que será usado para registrar o resultado no sistema e encontrá-lo na lista de produtos em buscas futuras.  
    Não é necessário escolher um nome único — o sistema adicionará automaticamente um número de ID interno ao nome do produto para garantir unicidade.

2. **Descrição** *(opcional)*  
   Qualquer observação ou nota sobre a amostra.

3. **Selecionar Catálogos de Redshift**  
   Escolha dois ou mais catálogos já disponíveis no sistema.

4. **Resolver duplicatas**
     - **Não**: Apenas concatena os catálogos, empilhando as colunas conforme o significado atribuído durante a associação de colunas no momento do upload ([etapa 3 da seção “Upload a new data product”](http://127.0.0.1:8000/en/sci-platforms/pz_server.html#upload-a-new-data-product)).  
       **Importante:** Mesmo nesse modo, a etapa de preparação ainda é executada e tenta produzir `z_flag_homogenized` e `instrument_type_homogenized` caso ainda não existam. Portanto, erros de validação ainda podem ocorrer (veja os avisos abaixo).
     - **Sim, mas manter todos**: Identifica duplicatas e adiciona a informação `tie_result`, mas mantém todas as entradas.
     - **Sim, e remover duplicatas**: Identifica e **mantém apenas a melhor medição** por galáxia (somente linhas com `tie_result == 1`).

5. **Formato de saída**  
   Escolha um entre: Parquet, HDF5, CSV ou FITS.

Após preencher os campos, clique em **Run** para executar o pipeline.

### API do Photo-z Server

Para instruções sobre como executar o pipeline via biblioteca Python (API) do Photo-z Server, veja:  
https://github.com/linea-it/pzserver/blob/main/docs/notebooks/pzserver_tutorial.ipynb

### Interpretando `tie_result`

Quando a resolução de duplicatas está ativada, o pipeline atribui um valor à coluna `tie_result`:

- `1`: Melhor entrada (objeto único ou vencedor do *tie-break*)  
- `0`: Entrada descartada (perdedora do *tie-break*)  
- `2`: *Hard tie* (empate não resolvido)  
- `3`: Estrela (objeto não extragaláctico; não participa do *tie-breaking*)

Essas informações são especialmente relevantes ao escolher as opções “Sim, mas manter todos” ou “Sim, e remover duplicatas”.

!!! warning
    Para habilitar a resolução de duplicatas, seus catálogos de entrada **devem** fornecer sinais de *tie-breaking* por pelo menos um dos caminhos suportados abaixo.  
    A etapa de preparação **sempre** é executada (mesmo ao escolher concatenação simples) e irá validar/calcular as colunas homogenizadas conforme necessário.

    **Caminho A — Colunas homogenizadas fornecidas pelo usuário**

    Forneça ambas:

    - `z_flag_homogenized` (numérica) com os valores permitidos:
        - `0`: Sem redshift  
        - `1`: Baixa confiança (<70%)  
        - `2`: Confiança média (70–90%)  
        - `3`: Alta confiança (90–99%)  
        - `4`: Confiança muito alta (>99%)  
        - `6`: Estrela (objeto não extragaláctico)
    - `instrument_type_homogenized` (string) com:
        - `s`: Espectroscópico  
        - `g`: Grism  
        - `p`: Fotométrico

    Observações:

    - Valores `NaN` são permitidos e serão ignorados pelo *tie-breaking*.  
    - Qualquer valor **fora** dos conjuntos acima fará o pipeline **falhar**.

    **Caminho B — Inferência rápida a partir de colunas de qualidade e tipo (estilo SITCOMTN-154)**

    - Se a coluna associada a `z_flag` contém valores em **[0, 1]** com **ao menos um valor estritamente entre 0 e 1** (ou seja, um escore de qualidade), o pipeline mapeia automaticamente para `z_flag_homogenized` usando limites internos.  
    - Se existir uma coluna chamada `type` contendo apenas os valores **`"s"`, `"p"`, ou `"g"`** (case-insensitive), ela será atribuída a `instrument_type_homogenized`.  
    - Esse é o caso de catálogos que seguem o padrão SITCOMTN-154.  
    - Se a coluna `z_flag` não for do tipo qualidade ou se `type` contiver valores inválidos (ex: `"m"`), essas colunas são **ignoradas**, e o pipeline recorre à tradução baseada em nomes de `survey`.

    **Caminho C — Tradução via nomes de `survey`**

    - Se existir uma coluna `survey`, o pipeline tenta inferir `z_flag_homogenized` e `instrument_type_homogenized` linha a linha usando regras internas definidas no arquivo de tradução:  
      [flags_translation.yaml](https://github.com/linea-it/pzserver_pipelines/blob/main/combine_redshift_dedup/flags_translation.yaml)
    - Diferentes nomes de `survey` podem coexistir no mesmo arquivo. Entretanto, **cada linha** deve referenciar um *survey* presente no arquivo de tradução.  
      Se pelo menos uma linha contiver um *survey* não suportado, o pipeline **gerará um erro**.
    - Casos especiais tratados com mapeamentos fixos:
        - `JADES_LETTER_TO_SCORE = {"A": 4.0, "B": 3.0, "C": 2.0, "D": 1.0, "E": 0.0}`
        - `VIMOS_FLAG_TO_SCORE = {"LRB_X": 0.0, "MR_X": 0.0, "LRB_B": 1.0, "LRB_C": 1.0, "MR_C": 1.0, "MR_B": 3.0, "LRB_A": 4.0, "MR_A": 4.0}`  

      Para esses dois *surveys*, o valor da flag de entrada **deve coincidir exatamente** com as strings mostradas acima para ser reconhecido.

    **Condições de falha (qualquer uma das seguintes):**

    - `z_flag_homogenized` ou `instrument_type_homogenized` existem mas contêm valores fora dos conjuntos permitidos.  
    - Nenhuma coluna homogenizada é fornecida **e**:
        - `z_flag` **não** é do tipo qualidade (sem valores estritamente entre 0 e 1), **ou** a coluna `type` está ausente ou contém valores fora de `{"s","p","g"}`; **e**
        - O pipeline recorre à tradução mas a coluna `survey` está ausente **ou** contém pelo menos um *survey* não suportado.  
    - Após a preparação/homogeneização, **todos os valores** em `z_flag_homogenized` ou em `instrument_type_homogenized` são `NaN` (essas colunas são obrigatórias pelo critério de *tie-breaking* e devem conter ao menos um valor válido).

    ---

    💡 **Melhoria futura**

    Em versões futuras, planejamos suportar arquivos de tradução e regras de *tie-breaking* definidas pelo usuário. Isso permitirá:

    - Personalizar a ordem e o conteúdo das colunas usadas no *tie-breaking* (`tiebreaking_priority`)
    - Utilizar colunas numéricas adicionais ou alternativas (ex: escores de qualidade ou métricas personalizadas) para resolver empates
    - Adicionar suporte a novos *surveys* fornecendo suas próprias regras de tradução para flags de qualidade e tipos de instrumento
