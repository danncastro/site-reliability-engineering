1) Configure o seu ambiente inicial. Você pode se guiar pelo script que o instrutor seguiu durante a aula:
~~~bash
# Configurando o ambiente e conectando máquina virtual:
    # Máquina virtual com o vagrant
    # Requisitos: http://vagrantup.com e https://www.virtualbox.org/
        # Baixar e descompactar o arquivo 1110-aula-inicial.zip
        # Entendendo o Vagranfile
        # Subindo o ambiente virtualizado
            vagrant plugin install vagrant-disksize
            vagrant up
            vagrant ssh
                ps -ef | grep -i mysql # Verificando se o MySQL esta rodando
                mysql -u devops -p # Senha mestre; show databases
                mysql -u devops_dev -p # Senha mestre; show databases
                # Instalando o Jenkins
                    cd /vagrant/scripts
                # Visualizar o conteudo do arquivo de instalacao do jenkins
                    sudo ./jenkins.sh

                # Acessar:  192.168.33.10:8080
                    sudo cat /var/lib/jenkins/secrets/initialAdminPassword

                # Credenciais
                    Nome de usuário: alura
                    Senha: mestre123
                    Nome completo: Jenkins Alura
                    Email: aluno@alura.com.br

                # Reload nas permissoes do docker
                    sudo usermod -aG docker $USER
                    sudo usermod -aG docker jenkins
                    exit
            vagrant reload
~~~

2) Versione o seu código, que você baixou no início da aula. Você pode se guiar pelo script que o instrutor seguiu durante a aula:
~~~bash
# Passos para a configuracao do git e versionamento do codigo
# Criar uma conta em github.com
    ssh-keygen -t rsa -b 4096 -C "<seu-usuario>@gmail.com"
    cat ~/.ssh/id_rsa.pub
# Configurar a chave no github
    git config --global user.name "<seu-usuario>"
    git config --global user.email <seu-usuario>@<seu-providor>
    ssh -T git@github.com
# Fazer download do código nos anexos e copiar para o diretório compartilhado app
    cd /vagrant/jenkins-todo-list
    git init
    git add .
    git commit -m "Meu primeiro commit"
    git log
# Criar um repositório no github: jenkins-todo-list
    git remote add origin git@github.com:<seu-usuario>/jenkins-todo-list.git
    git push -u origin master
~~~

3) Crie o seu primeiro job, que vai monitorar o seu repositório. Você pode se guiar pelo script que o instrutor seguiu durante a aula:
~~~bash
# Configurando a chave privada criada no ambiente da VM no Jenkins
    cat ~/.ssh/id_rsa
    Credentials -> Jenkins -> Global Credentials -> Add Crendentials -> SSH Username with private key [ github-ssh ]
# Criando o primeiro  job que vai monitorar o repositorio
    Novo job -> jenkins-todo-list-principal -> Freestyle project:
    Esse job vai fazer o build do projeto e registrar a imagem no repositório.
    # Gerenciamento de código fonte:
        Git: git@github.com:rafaelvzago/treinamento-devops-alura.git [SSH]
        Credentials: git (github-ssh)
        Branch: master
    # Trigger de builds
        Pool SCM: * * * * *
    # Ambiente de build
        Delete workspace before build starts
    # Salvar
    # Validar o log em: Git Log de consulta periódica
~~~
# 
1) Configure a sua aplicação e suba-a manualmente. Você pode se guiar pelo script que o instrutor seguiu durante a aula:
~~~bash
# Passos para configurar a app e subir manualmente
    # Criando o arquivo .env (temporário)
        cd /vagrant/jenkins-todo-list/to_do/
        vi .env
            [config]
            # Secret configuration
            SECRET_KEY = 'r*5ltfzw-61ksdm41fuul8+hxs$86yo9%k1%k=(!@=-wv4qtyv'

            # conf
            DEBUG=True

            # Database
            DB_NAME = "todo_dev"
            DB_USER = "devops_dev"
            DB_PASSWORD = "mestre"
            DB_HOST = "localhost"
            DB_PORT = "3306"

    # Instalando o venv
        sudo pip3 install virtualenv nose coverage nosexcover pylint
    # Criando e ativando o venv (dev)
        cd ../    
        virtualenv  --always-copy  venv-django-todolist
        source venv-django-todolist/bin/activate
        pip install -r requirements.txt
    # Fazendo a migracao inicial dos dados
        python manage.py makemigrations
        python manage.py migrate
    # Criando o superuser para acessar a app
        python manage.py createsuperuser
    # Repetir o processo de migracaoção para o ambiente de produção:
        vi to_do/.env
            [config]
            # Secret configuration
            SECRET_KEY = 'r*5ltfzw-61ksdm41fuul8+hxs$86yo9%k1%k=(!@=-wv4qtyv'

            # conf
            DEBUG=True

            # Database
            DB_NAME = "todo"
            DB_USER = "devops"
            DB_PASSWORD = "mestre"
            DB_HOST = "localhost"
            DB_PORT = "3306"

    # Fazendo a migracao inicial dos dados
        python manage.py makemigrations
        python manage.py migrate
    # Criando o superuser para acessar a app
        python manage.py createsuperuser

    # Verificar o ip do servidor
        ip addr
    # Rodando a app
        python manage.py runserver 0:8000
        http://192.168.33.10:8000
~~~

2) Configure o daemon do Docker. Você pode se guiar pelo script que o instrutor seguiu durante a aula:
~~~bash
# Expor o deamon do docker
    sudo mkdir -p /etc/systemd/system/docker.service.d/
    sudo vi /etc/systemd/system/docker.service.d/override.conf
        [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2376
    sudo systemctl daemon-reload
    sudo systemctl restart docker.service
~~~

3) Faça o build da sua imagem. Você pode se guiar pelo script que o instrutor seguiu durante a aula:
~~~bash
# Sugested Plugins
# Instalando os plugins
    Gerenciar Jenkins -> Gerenciar Plugins -> Disponíveis
        # docker
    Install without restart -> Depois reiniciar o jenkins
Gerenciar Jenkins -> Configurar o sistema -> Nuvem
    # Name: docker
    # URI: tcp://127.0.0.1:2376
    # Enabled
# This project is parameterized: 
    DOCKER_HOST
    tcp://127.0.0.1:2376
# Voltar no job criado na aula anterior
    # Manter a mesma configuracao do GIT para desenvolvimento
    # Build step 1: Executar Shell
# Validando a sintaxe do Dockerfile
docker run --rm -i hadolint/hadolint < Dockerfile
    # Build step 2: Build/Publish Docker Image
        Directory for Dockerfile: ./
        Cloud: docker
        Image: rafaelvzago/django_todolist_image_build
~~~
#
1) Prepare os seus ambientes. Você pode se guiar pelo script que o instrutor seguiu durante a aula:
~~~bash
# Instalar o plugin Config File Provider

# Configurar o Managed Files para Dev
    # Name : .env-dev
        [config]
        # Secret configuration
        SECRET_KEY = 'r*5ltfzw-61ksdm41fuul8+hxs$86yo9%k1%k=(!@=-wv4qtyv'
        # conf
        DEBUG=True
        # Database
        DB_NAME = "todo_dev"
        DB_USER = "devops_dev"
        DB_PASSWORD = "mestre"
        DB_HOST = "localhost"
        DB_PORT = "3306"

# Configurar o Managed Files para Prod
    # Name: .env.-prod
        [config]
        # Secret configuration
        SECRET_KEY = 'r*5ltfzw-61ksdm41fuul8+hxs$86yo9%k1%k=(!@=-wv4qtyv'
        # conf
        DEBUG=False
        # Database
        DB_NAME = "todo"
        DB_USER = "devops"
        DB_PASSWORD = "mestre"
        DB_HOST = "localhost"
        DB_PORT = "3306"

# No job: jenkins-todo-list-principal importar o env de dev para teste:

    Adicionar passo no build: Provide configuration Files
    File: .env-dev
    Target: ./to_do/.env

    Adicionar passo no build: Executar Shell

# Criando o Script para Subir o container com o arquivo de env e testar a app:
    #!/bin/sh

    # Subindo o container de teste
    docker run -d -p 82:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/jenkins-todo-list-principal/to_do/.env:/usr/src/app/to_do/.env --name=todo-list-teste django_todolist_image_build

    # Testando a imagem
    docker exec -i todo-list-teste python manage.py test --keep
    exit_code=$?

    # Derrubando o container velho
    docker rm -f todo-list-teste

    if [ $exit_code -ne 0 ]; then
        exit 1
    fi
~~~
2) Faça o push da sua imagem para o Docker Hub. Você pode se guiar pelo script que o instrutor seguiu durante a aula:
~~~bash
# Instalar o plugin: Parameterized Trigger 

# Modificar o Job para startar com 2 parametros:
    # Geral:
    Este build é parametrizado com 2 parametros de string
        Nome: image
        Valor padrão: <seu-usuario-no-dockerhub>/django_todolist_image_build

        Nome: DOCKER_HOST
        Valor padrão: tcp://127.0.0.1:2376

# No build step: Build / Publish Docker Image
    # Mudar o nome da imagem para: <seu-usuario-no-dockerhub>/django_todolist_image_build
    # Marcar: Push Image e configurar **suas credenciais** no dockerhub

# Mudar no job de teste a imagem para: ${image}
    docker run -d -p 82:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/jenkins-todo-list-principal/to_do/.env:/usr/src/app/to_do/.env --name=todo-list-teste ${image}
~~~
#
1) Faça a integração com o Slack. Você pode se guiar pelo script que o instrutor seguiu durante a aula:
~~~bash
# Criar app no slack: alura-jenkins.slack.com
    URL básico: <Url do Jenkins app no seu canal do Slack>
    Token de integração: <Token do Jenkins app no seu canal do Slack>

# Instalar o plugin do slack: Gerenciar Jenkins > Gerenciar Plugins > Disponíveis: Slack Notification
        # Configurar no jenkins: Gerenciar Jenkins > Configuraçao o sistema > Global Slack Notifier Settings
            # Slack compatible app URL (optional): <Url do Jenkins app no seu canal do Slack>
            # Integration Token Credential ID : ADD > Jenkins > Secret Text
                # Secret: <Token do Jenkins app no seu canal do Slack>
                # ID: slack-token
            # Channel or Slack ID: pipeline-todolist

# As notificações vão funcionar da seguinte maneira:
Job: todo-list-desenvolvimento será feito pelo Jenkinsfile (Próximas aulas)
Job: todo-list-producao: Ações de pós-build > Slack Notifications: Notify Success e Notify Every Failure
~~~
2) Crie o job para o ambiente de desenvolvimento. Você pode se guiar pelo script que o instrutor seguiu durante a aula:
~~~bash
# Novo Job: todo-list-desenvolvimento:
    # Tipo: Pipeline
    # Este build é parametrizado com 2 Builds de Strings:
        Nome: image
        Valor padrão: - Vazio, pois o valor sera recebido do job anterior.

        Nome: DOCKER_HOST
        Valor padrão: tcp://127.0.0.1:2376

    pipeline {

        agent any    

        stages {
            stage('Oi Mundo Pipeline como Codigo') {
                steps {
                    sh 'echo "Oi Mundo"'
                }
            }
        }
    }

    pipeline {
        environment {
            dockerImage = "${image}"
        }
        agent any

        stages {
            stage('Carregando o ENV de desenvolvimento') {
                steps {
                    configFileProvider([configFile(fileId: '<id do seu arquivo de desenvolvimento>', variable: 'env')]) {
                        sh 'cat $env > .env'
                    }
                }
            }
            stage('Derrubando o container antigo') {
                steps {
                    script {
                        try {
                            sh 'docker rm -f django-todolist-dev'
                        } catch (Exception e) {
                            sh "echo $e"
                        }
                    }
                }
            }        
            stage('Subindo o container novo') {
                steps {
                    script {
                        try {
                            sh 'docker run -d -p 81:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/jenkins-todo-list-desenvolvimento/.env:/usr/src/app/to_do/.env --name=django-todolist-dev ' + dockerImage + ':latest'
                        } catch (Exception e) {
                            slackSend (color: 'error', message: "[ FALHA ] Não foi possivel subir o container - ${BUILD_URL} em ${currentBuild.duration}s", tokenCredentialId: 'slack-token')
                            sh "echo $e"
                            currentBuild.result = 'ABORTED'
                            error('Erro')
                        }
                    }
                }
            }
            stage('Notificando o usuario') {
                steps {
                    slackSend (color: 'good', message: '[ Sucesso ] O novo build esta disponivel em: http://192.168.33.10:81/ ', tokenCredentialId: 'slack-token')
                }
            }
        }
    }

# todo-list-principal
    # Definir post build: image=$image
~~~
#
1) Crie o job para colocar a aplicação em produção. Você pode se guiar pelo script que o instrutor seguiu durante a aula:
~~~bash
# Criar o job para colocar a app em producao:
    Nome: todo-list-producao
    Tipo: Freestyle
    # Este build é parametrizado com 2 Builds de Strings:
        Nome: image
        Valor padrão: - Vazio, pois o valor sera recebido do job anterior.

        Nome: DOCKER_HOST
        Valor padrão: tcp://127.0.0.1:2376

    # Ambiente de build > Provide configuration files
        File: .env-prod
        Target: .env

    # Build > Executar shell
        #Execute shell
        #!/bin/sh
        { 
            docker run -d -p 80:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/todo-list-producao/.env:/usr/src/app/to_do/.env --name=django-todolist-prod $image:latest

        } || { # catch
            docker rm -f django-todolist-prod
            docker run -d -p 80:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/todo-list-producao/.env:/usr/src/app/to_do/.env --name=django-todolist-prod $image:latest
        }    

Ações de pós-build > Slack Notifications: Notify Success e Notify Every Failure
~~~
2) Faça a integração do jobs. Você pode se guiar pelo script que o instrutor seguiu durante a aula:
~~~bash
# Post build actions para os 3 jobs

Job: jenkins-todo-list-principal > Ações de pós-build > Trigger parameterized buld on other projects
    Projects to build: todo-list-desenvolvimento
    # Add parameters > Predefined parameters
        image=${image}

Job: todo-list-desenvolvimento

    pipeline {
        environment {
            dockerImage = "${image}"
        }
        agent any

        stages {
            stage('Carregando o ENV de desenvolvimento') {
                steps {
                    configFileProvider([configFile(fileId: '2ed9697c-45fc-4713-a131-53bdbeea2ae6', variable: 'env')]) {
                        sh 'cat $env > .env'
                    }
                }
            }
            stage('Derrubando o container antigo') {
                steps {
                    script {
                        try {
                            sh 'docker rm -f django-todolist-dev'
                        } catch (Exception e) {
                            sh "echo $e"
                        }
                    }
                }
            }        
            stage('Subindo o container novo') {
                steps {
                    script {
                        try {
                            sh 'docker run -d -p 81:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/todo-list-desenvolvimento/.env:/usr/src/app/to_do/.env --name=django-todolist-dev ' + dockerImage + ':latest'
                        } catch (Exception e) {
                            slackSend (color: 'error', message: "[ FALHA ] Não foi possivel subir o container - ${BUILD_URL} em ${currentBuild.duration}s", tokenCredentialId: 'slack-token')
                            sh "echo $e"
                            currentBuild.result = 'ABORTED'
                            error('Erro')
                        }
                    }
                }
            }
            stage('Notificando o usuario') {
                steps {
                    slackSend (color: 'good', message: '[ Sucesso ] O novo build esta disponivel em: http://192.168.33.10:81/ ', tokenCredentialId: 'slack-token')
                }
            }
            stage ('Fazer o deploy em producao?') {
                steps {
                    script {
                        slackSend (color: 'warning', message: "Para aplicar a mudança em produção, acesse [Janela de 10 minutos]: ${JOB_URL}", tokenCredentialId: 'slack-token')
                        timeout(time: 10, unit: 'MINUTES') {
                            input(id: "Deploy Gate", message: "Deploy em produção?", ok: 'Deploy')
                        }
                    }
                }
            }
            stage (deploy) {
                steps {
                    script {
                        try {
                            build job: 'todo-list-producao', parameters: [[$class: 'StringParameterValue', name: 'image', value: dockerImage]]
                        } catch (Exception e) {
                            slackSend (color: 'error', message: "[ FALHA ] Não foi possivel subir o container em producao - ${BUILD_URL}", tokenCredentialId: 'slack-token')
                            sh "echo $e"
                            currentBuild.result = 'ABORTED'
                            error('Erro')
                        }
                    }
                }
            }
        }
    }
~~~
