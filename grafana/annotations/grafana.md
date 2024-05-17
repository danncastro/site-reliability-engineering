Em quais casos devemos considerar montar um ambiente de monitoramento?
Somente quando o nosso sistema atingir uma certa maturidade, pois não devemos disponibilizar recursos de monitoramento no início do projeto
Alternativa errada! Uma entrega de software com qualidade inclui monitorar todos os estágios de maturidade de um software, inclusive no início do seu ciclo, podemos estudar o comportamento do sistema e da infraestrutura para provisionar recursos antecipadamente.

O monitoramento deve ser uma prática comum para garantir a qualidade, inclusive em estágios de pré-produção
Alternativa correta! É extremamente importante olhar para a saúde do nosso ecossistema de software. Todos os dados coletados e as análise feitas auxiliam na tomada de decisões, reação a problemas e prevenção de falhas.

Somente após a entrega do software, pois o objetivo é corrigir os erros assim que eles aparecerem
Alternativa errada! Um dos objetivos de se monitorar o ambiente é prever possíveis comportamentos anormais e ser pró-ativo.

Qual a infraestrutura básica para um ambiente de monitoramento?
Preferencialmente, devemos utilizar uma solução terceirizada para monitoramento
Alternativa errada! Existem várias ferramentas gratuitas e open source para monitoramento, como o Grafana, sendo assim, podemos utilizar a mesma infraestrutura da aplicação com ferramentas leves para coleta de dados.

Podemos utilizar a mesma infraestrutura da aplicação
Alternativa correta! Nos estágios iniciais, podemos utilizar a mesma infraestrutura, mas após certa maturidade, devemos separar os recursos, para não comprometer a saúde da aplicação.

Um servidor exclusivo para o monitoramento, pois, como vamos monitorar somente o ambiente de produção, vamos precisar de muito recurso
Alternativa errada! Como já foi dito, podemos e devemos monitorar vários ambientes, inclusive os estágios iniciais da aplicação, então podemos utilizar a mesma infraestrutura da aplicação.

Quais as principais características de dashboards, considerando que podemos visualizar os dados brutos da aplicação?
São complementares à análise tradicional do comportamento do sistema. Com uma precisão próxima da real, as métricas exibidas são importantes para insights do comportamento geral
Alternativa errada! Todos os coletores analisam os valores reais do sistema.

São complementares à análise tradicional do comportamento do sistema. Os valores são precisos, pois foram coletados em tempo de execução
Alternativa correta!

Preferencialmente devemos analisar os logs da aplicação e do sistema. Os dashboards são imprecisos e lentos.
Alternativa errada! Dependendo do tamanho da nossa aplicação e da aptidão técnica de quem a analisa, olhar para os logs é um trabalho impossível de ser feito na mesma velocidade que os coletores fazem. Nesse contexto, o dashboard facilita a compreensão do comportamento do sistema.

Sobre o conceito de coletores de métricas e bancos de dados temporais (time series databases), podemos afirmar, respectivamente:
São complementares, o primeiro faz uma "raspagem" de dados no sistema e injeta os valores no segundo, que mantém um histórico temporal dos dados, para análises futuras
Alternativa correta!

São independentes, o primeiro, sozinho, é capaz de analisar as métricas e exibir o comportamento do sistema ao passar do tempo. O segundo deve ser utilizado quando for necessária a análise de séries de tempo maiores que um dia
Alternativa errada! O Telegraf é somente um coletor, que faz uma "raspagem" de dados no sistema e injeta os valores no InfluxDB, que mantém um histórico temporal dos dados para análises futuras.

Eventualmente podemos utilizar somente o Grafana para armazenar os dados em conjunto com o Telegraf
Alternativa errada! O Grafana não armazena métricas. Precisamos utilizar o InfluxDB para guardar os dados coletados pelo Telegraf.

O que melhor define um data source no Grafana?
É a parte responsável por configurar onde os dados serão armazenados no Grafana
Alternativa errada! O Grafana não armazena métricas, para isso utilizamos o InfluxDB.

É nessa parte do Grafana que definimos o principal repositório de dados para análise do comportamento do sistema que eles aparecerem
Alternativa errada! O Grafana aceita múltiplos repositório de dados, não existindo um principal, pois tudo vai depender dos tipos de métricas e da complexidade do sistema que estamos analisando.

É nessa parte do sistema que definimos múltiplos repositórios de dados para montarmos nossos dashboards
Alternativa correta!

Sobre a análise de métricas de um sistema, deve-se considerar alguns aspectos na coleta e a afirmativa que valida esse conceito é:
Devemos coletar principalmente as métricas da aplicação. A infraestrutura é complementar e sua análise deve ser feita somente em produção
Alternativa errada! O comportamento da aplicação está fortemente vinculada à capacidade da infraestrutura, inclusive no escopo de desenvolvimento e teste.

A análise da infraestrutura deve ser feita em um evento chamado post mortem, que consiste na análise subsequente a um evento de falha
Alternativa errada! Uma entrega de software com qualidade inclui monitorar todos os estágios de maturidade de um software, inclusive no início do seu ciclo, podemos estudar o comportamento do sistema e da infraestrutura para provisionar recursos antecipadamente.

A coleta e análise de métricas de vários tipos contribui para a saúde de uma aplicação e é tão importante quanto qualquer outro passo no fluxo de entrega de um produto
Alternativa correta!

Considerando o monitoramento da infraestrutura, quais indicadores podem demonstrar a saúde da aplicação?

Processador. Com o histórico dessa métrica, pode-se avaliar o comportamento do sistema durante as horas de pico da aplicação e em conjunto com os logs gerados pelo sistema, pode-se prever comportamento futuro e provisionar mais recursos
Alternativa errada! A afirmação é correta mas não é a única.

As duas afirmativas citadas estão corretas, mas recomenda-se ainda adicionar mais métricas para aumentar a cobertura do monitoramento
Alternativa correta!

Memória. Com o histórico dessa métrica, pode-se avaliar o comportamento do sistema durante as horas de pico da aplicação e em conjunto com os logs gerados pelo sistema, pode-se prever comportamento futuro e provisionar mais recursos
Alternativa errada! A afirmação é correta mas não é a única.

Gerir a utilização do disco no servidor da aplicação pode prevenir alguns problemas como:

1) Não ter espaço para armazenar os logs da aplicação, podendo, em alguns casos extremos ou ainda características da arquitetura, interromper o funcionamento da instância

2) Mesmo que a utilização do disco não esteja em 100%, é válido coletar as métricas dos inodes do sistema, pois, mesmo com espaço, sem inodes disponíveis, não podemos criar novos arquivos e isso compromete o funcionamento da aplicação

Dos problemas citados acima, qual(is) realmente pode(m) ser prevenido(s) com a utilização do disco no servidor da aplicação?
Ambos os problemas

Sobre analisar o consumo de memória nos servidores de aplicação, analise as afirmativas abaixo:

1) A análise do consumo de memória é muito útil para prever períodos ou eventos onde a nossa aplicação pode precisar de novos recursos, como um evento de vendas importante

2) Em uma aplicação que sua inicialização depende da quantidade de memória alocada, como o JBoss, a análise da métrica, durante um período em conjunto com a análise dos logs, pode determinar uma alteração na capacidade do servidor

Qual(is) sentença(s) reflete(m) a(s) motivação(ões) para a coleta dessa métrica?
Ambas as sentenças

É possível analisar métricas de servidores Docker distintos? Como?
Podemos utilizar o plugin do Docker do Telegraf, e apontar para outras instâncias do Docker
Alternativa errada! A sentença é correta, mas não é a única verdadeira.

Ambas as alternativas
Alternativa correta!

Utilizar o Telegraf em outros servidores e apontar sua configuração para o servidor onde o InfluxDB está rodando, e no painel, podemos utilizar as variáveis do dashboard para mudar qual instância gostaríamos de visualizar
Alternativa errada! A sentença é correta, mas não é a única verdadeira.

O que é responsável por permitir métricas adicionais no Grafana, além das métricas padrão?
Variáveis de dashboards para separar as métricas que desejamos visualizar
Alternativa errada! As variáveis de ambiente somente consultam dados gravados no InfluxDB. Como a injeção foi feita pelo Telegraf em outro measurement, podemos ter múltiplas métricas.

Diferentes measurements no InfluxDB
Alternativa correta! Como o InfluxDB é um banco de dados independente, podemos apontar os plugins para gravar dados em measurements distintos, como por exemplo o Docker.

Múltiplos data sources
Alternativa errada! Não é necessário criar um novo data source, podemos utilizar a mesma instância para vários tipos de measurements distintos.

Sobre a saúde da aplicação, escolha a alternativa que representa uma boa prática de desenvolvimento e monitoramento:
Recomenda-se deixar um mecanismo para ligar os logs somente quando vamos fazer o tracking de alguma issue
Alternativa errada! Apesar de cada aplicação e domínio de negócio determine como devemos tratar os logs, recomenda-se que sempre exista algum log para ser observado, não somente quando uma issue é trabalhada.

Logs objetivos, concisos, com uma coleta regular de métricas
Alternativa correta!

Logs concisos e curtos, para evitar deixar o filesystem muito cheio, o que pode degradar o desempenho geral do sistema
Alternativa errada! Nem sempre logs curtos são recomendáveis, devemos achar um equilíbrio na quantidade de informações que colocamos nos logs, para que o rastreamento de exceções seja completo.

Dadas as seguintes afirmações:
São logs dos eventos dos motores das aplicação, como Apache, JBoss e Node.js
São os logs que resumem o comportamento da aplicação e eventualmente as regras de negócio implementadas
Tais afirmações se referem, respectivamente:

Logs de sistema e logs da aplicação
Alternativa correta!

São logs que devem ser gravados no mesmo arquivo/diretório, independente do tipo
Alternativa errada! Para facilitar a coleta de métrica e o entendimento do comportamento do sistema, deve-se separar os logs de motores de aplicação, como por exemplo o JBoss, dos logs gerados pelo Log4j, que é um framework utilizado para alterar o detalhamento e os níveis de logs.

Logs da aplicação e logs de sistema
Alternativa errada! O correto é o oposto.

Escolha, dentre das opções abaixo, a que reflete as maneiras de selecionar os dados no InfluxDB para gerar os painéis:
Ambas as alternativas citadas
Alternativa correta!

Podemos utilizar o mouse no formato Point And Click, onde selecionamos as métricas e definimos as características do painel
Alternativa errada! Apesar de correta, ainda existe a opção de selecionar utilizando queries do tipo SQL.

Podemos utilizar queries do tipo SQL para consultar no InfluxDB as métricas e definir as informações que serão exibidas
Alternativa errada! Apesar de correta, ainda existe a opção de selecionar as métricas com o mouse, em uma interface mais intuitiva.

Das alternativas abaixo, escolha a que lista somente as ferramentas de notificação presentes no Grafana.
Discord, Kafka, Slack, Telegram, Microsoft Teams e Google Hangouts

Como comentamos no início do curso, o conceito de monitoramento envolve, além de outras coisas, o conceito de NOC. Escolha a alternativa que explica a importância desse componente.

NOC - Network Operation Center, consiste em um conjunto de administradores de sistema reunidos para monitorar, gerenciar e controlar um sistema baseado em métricas e ferramentas
Alternativa errada! A afirmação é correta mas não é a única.

Ambas as alternativas estão corretas. O conceito de NOC é que, nesse centro, o time de suporte garanta um monitoramento ativo e eficaz
Alternativa correta!

É na NOC que colocamos em produção os nossos dashboards produzidos no Grafana, com as métricas que coletamos com o Telegraf e injetamos no InfluxDB, para garantir a estabilidade e saúde do nosso sistema
Alternativa errada! A afirmação é correta mas não é a única.