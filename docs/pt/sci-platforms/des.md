O DES Science Portal ([des-portal.linea.org.br](https://des-portal.linea.org.br/)) foi uma plataforma desenvolvida para o projeto [Dark Energy Survey](https://www.darkenergysurvey.org/) (DES) que hospedou uma variedade de _pipelines_ destinados a preparar catálogos customizados para diferentes aplicações na astronomia, bem como para realizar uma variedade de análises científicas.

A parte que envolve o processamento de dados do DES Science Portal foi descontinuada devido à obsolescência das tecnologias envolvidas, mas seu sistema de acesso aos resultados gerados e de controle de proveniência continua disponível para os membros da colaboração DES. As funcionalidades do DES Science Portal foram gradativamente substituídas por outros serviços mais modernos desenvolvidos pelo LIneA, preparados para lidar com Big Data em preparação para o LSST, mantendo sua filosofia e boas práticas.

Os _pipelines_ do DES Science Portal foram agrupados em etapas:

* **Data Preparation** – inclui a criação de mapas de efeitos sistemáticos, classificação de objetos (estrela/galáxia), cálculo de _redshifts_ fotométricos e outros valores agregados, dependendo da aplicação. Por exemplo, estimativa de idade, metalicidade ou massa estelar de galáxias.

* **Value-added Catalog** – combina os dados e resultados obtidos na etapa anterior, seleciona as colunas de interesse e aplica cortes de qualidade e limpeza dos dados, com critérios fortemente dependentes da aplicação científica para a qual o catálogo será destinado.

* **Science Workflows** – agrega diversos _pipelines_ de análise científica. Tem como dados de entrada os catálogos criados na etapa anterior.

Um dos principais pontos fortes do DES Science Portal foi a capacidade de fornecer informações completas de todo o histórico de processos executados, permitindo ao usuário rastrear a entrada, a configuração, a versão dos códigos utilizados e os resultados obtidos, na forma de um registro do produto com gráficos e tabelas informativas. O portal também forneceu acesso a uma série de ferramentas destinadas ao usuário, ao desenvolvedor e ao administrador.

Para se aprofundar nos detalhes sobre o DES Science Portal, leia os dois artigos publicados na revista [Astronomy and Computing](https://www.sciencedirect.com/journal/astronomy-and-computing):

* **Gschwend et al. 2018 – DES science portal: Computing photometric redshifts**  
([doi.org/10.1016/j.ascom.2018.08.008](https://doi.org/10.1016/j.ascom.2018.08.008)) ([arXiv:1708.05643](https://arxiv.org/abs/1708.05643))  

* **Fausti Neto et al. 2018 – DES science portal: Creating science-ready catalogs**  
([doi.org/10.1016/j.ascom.2018.01.002](https://doi.org/10.1016/j.ascom.2018.01.002)) ([arXiv:1708.05642](https://arxiv.org/abs/1708.05642))