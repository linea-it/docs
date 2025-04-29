

O **User Query** ([**userquery.linea.org.br**](https://userquery.linea.org.br/)) é uma interface amigável para consultas ao banco de dados que possibilita a criação de tabelas temporárias. As tabelas geradas pelos usuários são gerenciadas pelo MyDB e podem ser acessadas imediatamente no JupyterHub do LIneA ou de qualquer lugar por meio do TAP Service. Além disso, se uma tabela contiver coordenadas equatoriais e uma coluna com identificadores únicos, ela se torna automaticamente uma lista de alvos, o que permite a visualização das imagens correspondentes aos objetos na ferramenta [Target Viewer](/docs/sci-platforms/target_viewer.md) (imagens do LSST para membros e imagens do DES DR2 para o público geral).

### TAP Service

O TAP Service (Table Access Protocol) do [IVOA](https://ivoa.net/index.html) é um padrão que permite acesso e consulta a bancos de dados astronômicos por meio de linguagem SQL, de forma padronizada e interoperável entre diferentes serviços. Além do serviço User Query, os catálogos públicos hospedados no LIneA também podem ser acessados remotamente por meio do TAP Service no programa [TOPCAT](https://www.star.bris.ac.uk/~mbt/topcat/) ou programaticamente por meio da biblioteca Python [`pyvo`](https://pyvo.readthedocs.io/en/latest/#). Acesse a [documetatação do TAP Service](https://userquery.linea.org.br/cms/services/scripted-access/) no site do User Query para mais detalhes sobre como utilizar o serviço.


