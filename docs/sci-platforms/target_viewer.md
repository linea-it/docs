O **Target Viewer** ([**targetviewer.linea.org.br**](https://targetviewer.linea.org.br)) é um serviço, em fase de desenvolvimento, personalizado para visualização imagens astronômicas com base em uma lista de objetos-alvo previamente definida pelo usuário.

**Passo a passo:**

1. Crie uma nova lista de alvos como uma tabela no espaço *MyDB* do usuário no **User Query**. Para que o Target Viewer consiga localizar as imagens dos alvos, a tabela deve conter as colunas `objectId`, `ra` e `dec`.
2. Clique no botão **"NEW CATALOG"** na página inicial do Target Viewer e preencha o formulário de 3 etapas para registrar a tabela como uma lista de alvos.
3. Após o envio do formulário, a lista de alvos aparecerá no menu da página inicial.

Assim como o Sky Viewer, o Target Viewer foi originalmente desenvolvido para o levantamento DES, e sua versão inicial ainda está disponível na página [DES Science Server](https://scienceserver.linea.org.br/), oferecendo acesso às imagens e dados tabulares do DES DR2. A nova versão, que está sendo desenvolvida para compor o portfólio de serviços do IDAC-Brasil, será flexível e oferecerá acesso aos dados do LSST para membros, além de dados públicos de outros levantamentos para o público geral.
