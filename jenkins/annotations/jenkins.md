Quais das afirmações abaixo representam uma vantagem da adoção de integração contínua no desenvolvimento de software?

Agregar todo o processo de construção de artefatos, controle de qualidade e entrega de software em um fluxo contínuo e transparente para todos os envolvidos
Alternativa correta! Esta afirmação reflete o resultado de uma boa integração contínua.

Melhora na cobertura de qualidade do código do projeto, visto que é possível analisar o produto durante o processo de construção
Alternativa correta! Esta afirmação reflete o resultado de uma boa integração contínua.


Os times vão trabalhar em silos, focando exclusivamente na qualidade de entrega das suas tasks diárias, reduzindo assim o re-trabalho
Alternativa errada! Isso é exatamente o que a integração contínua e a prática de DevOps evita. Para ser ágil na entrega e na qualidade, é importante quebrar as barreiras entre os times.

Diminuição do time to market na entrega do produto para o mercado
Alternativa correta! Esta afirmação reflete o resultado de uma boa integração contínua.

Baixe [aqui](https://caelum-online-public.s3.amazonaws.com/1110-jenkins/01/1110-aula-inicial.zip) o ZIP do projeto inicial deste treinamento, necessário para a continuidade do mesmo.

Além disso, o código-fonte de aplicação de exemplo pode ser baixado [aqui](https://caelum-online-public.s3.amazonaws.com/1110-jenkins/01/jenkins-todo-list.zip).

Durante o curso usaremos o Jenkins como ferramenta principal, mas o que é Jenkins?

Um servidor de integração continua.
Alternativa correta! O Jenkins é um servidor de Integração Contínua open-source escrito em Java. Ele é o mais popular mas não a única opção. Outros servidores de *Integração Contínua * são TeamCity, Bamboo, Travis CI ou Gitlab CI entre vários outros.

Uma ferramenta que permite o monitoramento de tarefas de maneira ágil.
Alternativa errada! Monitorar tarefas e projetos é o papel de ferramentas como Trello, Jira ou PivotalTracker entre vários outros.

Uma ferramenta de build (construção de pacote)
Alternativa errada! Ferramentas de build clássicos são Maven, Gradle, npm, Grunt ou make entre outros.

O ponto inicial da nossa Pipeline de integração continua é o repositório GIT que hospedamos no Github.
Das alternativas abaixo, assinale aquela que aponta o fluxo correto de um versionamento inicial de um código-fonte com o Git:

git init --> git add . --> git commit -m "Meu primeiro commit" --> git remote add origin git@github.com:<seu-usuario>/jenkins-todo-list.git --> git push origin master
init
add e commit
remote add
push to master

Qual a diferença entre o Git Log de consulta periódica e o log da Saída do console, em um job no Jenkins?
Selecione uma alternativa
O primeiro mostra a consulta ao repositório do código-fonte, já o segundo mostra o log da execução do build do job
Alternativa correta! O primeiro log mostra as consultas periódicas, configuradas no job, ao repositório, e somente após alguma alteração ele vai prosseguir com o build, onde seus logs podem ser acessados pela Saída do console.

Aprendemos que o Jenkins verifica periodicamente o repositório para encontrar mudanças no código e iniciar o build do Job.
Qual o repositório o Jenkins está verificando?

O repositório remoto no Github.
Alternativa correta, pois configuramos o Github como repositório remoto que o Jenkins verifica periodicamente se há alterações no código.

Sejam os seguintes comandos no build manual:
python createsuperuser
python migrate 
Qual a função deles, respectivamente?
Criar o principal usuário para acessar a aplicação; migrar as alterações dos seus modelos
Alternativa correta!

No servidor onde o Docker está instalado, qual o arquivo que criamos para possibilitar o controle remoto do Docker?
/etc/systemd/system/docker.service.d/override.conf
Alternativa correta!

Na construção de imagens para o Docker, recomenda-se o uso de um linter. Por quê?
Para garantir a utilização das melhores práticas na criação de um Dockerfile

Considerando a abordagem utilizada para definir a configuração da aplicação nos ambientes de desenvolvimento e produção, escolha a alternativa que justifica tal escolha:
Nada justifica tal escolha
Alternativa errada! Recomenda-se separar os ambientes de desenvolvimento, testes e produção. Na aplicação do curso, isso acontece por um arquivo .env para cada situação.

Apesar da escolha de separar os ambientes por arquivo de configuração, recomenda-se rodar os testes sistêmicos no banco de dados de produção
Alternativa errada! Recomenda-se separar os ambientes de desenvolvimento, testes e produção. Na aplicação do curso, isso acontece por um arquivo .env para cada situação.

A abordagem escolhida permite que a mesma imagem construída no processo seja utilizada para testes, validações e também para rodar em produção, visto que o arquivo .env de cada ambiente é passado na execução do container
Alternativa correta!

Seria mais interessante construir a imagem com o arquivo de configuração de ambiente
Alternativa errada! Recomenda-se separar os ambientes de desenvolvimento, testes e produção. Na aplicação do curso, isso acontece por um arquivo .env para cada situação.

Qual a vantagem de registrar a imagem no Docker Hub no início do pipeline?
A principal vantagem é que a imagem com a aplicação 100% funcional estará sempre disponível
Alternativa errada! Apesar da imagem estar registrada, não podemos garantir que ela está funcional. Seu registro se dá pela necessidade de deixar o artefato disponível, inclusive para debug.

Que, após registrar a imagem, qualquer um com acesso ao domínio pode baixar e rodar a aplicação
Alternativa correta! Devemos construir somente uma vez a imagem no pipeline.

Para garantir que o time de desenvolvimento não acesse o artefato construído para produção
Alternativa errada! Pelo contrário, com a adoção dessa prática, garantimos que o mesmo artefato é utilizado até o final do ciclo de vida do build.

Qual o caminho no Jenkins para instalação de plugins?
Gerenciar Jenkins --> Gerenciar Plugins --> Disponíveis

Escolha a opção que melhor reflete o que é a linguagem Groovy:
É uma sintaxe que utilizamos para criar os nossos pipelines com código

Qual é a sintaxe correta para colocar uma decisão no pipeline, utilizando a linguagem Groovy?
timeout(time: 10, unit: 'MINUTES') {
    input(id: "Deploy Gate", message: "Deploy em produção?", ok: 'Deploy')
}

Qual é uma das vantagens de passar parâmetros de um job para outro no Jenkins?
Com essa integração, alguns parâmetros podem ser passados para o primeiro job e os demais, quando configurados, podem receber automaticamente esses valores, como por exemplo o nome da imagem