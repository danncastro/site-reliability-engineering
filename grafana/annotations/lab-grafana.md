1) Copiar o arquivo do vagrant para um diretorio

2) Comando: vagrant up

3) Comando: vagrant ssh

4) Atualizar o sistema: sudo apt-get update && sudo apt-get upgrade -y

5) Instalar o Docker:
~~~bash
# Download do script
$ curl -fsSL https://get.docker.com -o get-docker.sh 

# Instalando
$ sh get-docker.sh 

# Dando permissão para o usuário
$ sudo usermod -aG docker vagrant 

# Recarregando as permissões
$ exit
$ vagrant ssh

# Testando a instalação
$ docker ps
~~~
6) Instalar o Grafana:
~~~bash
# Criando o diretorio para a instalação do Grafana
$ mkdir -p  $PWD/grafana-alura/volumes/grafana
$ cd $PWD/grafana-alura/

# Criando a rede do docker para o grafana
$ docker network create grafana-net

# Definindo a instalação do grafana para o user atual e instalando
$ ID=$(id -u)
$ docker run -d --user $ID                  \
    -v "$PWD/volumes/grafana:/var/lib/graf  \
    -p 3000:3000                            \
    --name=grafana                          \
    --network=grafana-net                   \
    grafana/grafana
$ docker ps
~~~
7) Acessar o Grafana: http://ip_servidor/:3000
Usuário: admin
Senha: admin
Nova senha: mestre
8) Criar o primeiro dashboard:
Create -> Dashboard -> Add Query
Visualization -> Mostrar os tipos de visualizações mais comuns
Criar um gráfico, gauge, bar gauge e table (Nome: Aula1 - Meu primeiro dashboard)

#### 

1) Instalar o InfluxDB:
~~~bash
# Criando o diretório para persistir o volume e as configurações
$ mkdir -p $PWD/volumes/influxdb

$ Instalando o INFLUXDB:
$ docker run -d -v "$PWD/volumes/influxdb:/var/lib/influxdb"    \
    -p 8083:8083                                                \
    -p 8086:8086                                                \
    -p 25826:25826/udp                                          \
    --name=influxdb                                             \
    --network=grafana-net                                       \
    influxdb:1.0
~~~
2) Instalar o coletor de métricas Telegraf:
~~~bash
$wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -
$source /etc/lsb-release
$echo "deb https://repos.influxdata.com/${DISTRIB_ID,,} \
    ${DISTRIB_CODENAME} stable" |                   \
    sudo tee /etc/apt/sources.list.d/influxdb.list
$sudo apt-get update && sudo apt-get install telegraf
$sudo service telegraf start
~~~
3) Visualizando as métricas coletadas pelo Telegraf no InfluxDB:
~~~bash
# Conectando no Influx
$ docker exec -ti influxdb bash
$ influx

# Acessando as métricas
> use telegraf
> show measurements;
> exit
$ exit
~~~
4) Configurar o data source no Grafana:

Configuration > Data Sources > Add data source > InfluxDB
Name: InfluxDB
URL: http://192.168.33.10:8086
Access: Browser
Database: telegraf
Save & Test
5) Ler a primeira métrica em um dashboard:

Create > Dashboard > Configurações
General > Name: Aula 2 - Como armazenar as métricas
Variables > Type: Query
Variables > Name: server
Variables > Type: Query
Variables > Data source: InfluxDB
Variables > Query: SHOW TAG VALUES FROM system WITH KEY=host
New Panel > Add Query

####

1) Montar o painel de monitoramento de infraestrutura

Create > Dashoard > Dashbard Settings
Name: Aula 3 - Monitorando o Sistema
2) Criar os painéis:

Utilização de CPU
~~~bash
# Instalando a ferramenta para estressar o servidor
$ sudo apt-get update
$ sudo apt-get install stress-ng
# Estressando a CPU do servidor
$ stress-ng -c 0 -l 95
~~~

Utilização de FS
~~~bash
# Criando um arquivo grande no servidor
$ dd if=/dev/zero of=arquivo.img bs=1M count=3000
~~~

Utilização da memória RAM
~~~bash
# Estressando a Memória do servidor
$ stress-ng --vm-bytes \
  $(awk \
    '/MemAvailable/{printf "%d\n", $2 * 0.9;}' \
    < /proc/meminfo\
  )k \
  --vm-keep -m 1
~~~

####

1) Modificar o arquivo /etc/telegraf/telegraf.conf e dar permissão para ler o socket do Docker:
~~~bash
$sudo usermod -aG docker telegraf
$cat telegraf.conf | grep -A 45 inputs.docker | egrep -v "^#"
[[inputs.docker]]
   endpoint = "unix:///var/run/docker.sock"
   container_names = []
   container_name_include = []
   container_name_exclude = []
   timeout = "5s"
   total = false
$ sudo service telegraf status
~~~
2) Criar os painéis:
Containers rodando no servidor $server
Utilização de CPU pelos containers

####

Para criar o painel Erros 404 siga os passos abaixo:

1) Clique no botão Add panel;

2) Em seguida clique em Add Query, logo na parte superior temos que deixar a opção server com valor ubuntu-bionic;

3) Agora no campo Query selecione a opção InfluxDB;

4) Logo abaixo no campo A (onde tem a consulta em si, com os campos FROM, WHERE, SELECT, GROUP BY), clique no botão com ícone lápis (nome do campo é: Toggle text edit mode) que fica no lado direito, para podermos ir para o modo de edição de texto;

5) No campo de texto que aparecer, devemos ver uma consulta padrão semelhante a essa:
~~~bash
SELECT mean("value") FROM "measurement" WHERE $timeFilter GROUP BY time($__interval) fill(null)
~~~

Altere essa consulta para:
~~~bash
SELECT count("request") FROM "apache_access_log" WHERE ("host" =~ /^$server$/)  AND "resp_code" = '404'  AND $timeFilter AND "agent" != 'Go-http-client/1.1' AND agent != 'worldping-api'
~~~

6) Agora clique na opção Visualization, e no seletor Visualization selecione a opção Gauge;

7) No campo Calc do painel Display selecione a opção Total;

8) No campo Unit no painel Field selecione a opção Misc e depois a opção none (dentro de Misc);

9) No campo Min que fica logo abaixo do campo Unit, digite 0 (zero);

10) Agora na opção General no campo Title coloque o nome Erros 404;

11) Pronto, agora é só voltar para o Dashboard e você vai poder visualizar o novo painel Erros 404.

####

1) Instalar o Apache:
~~~bash
$ sudo apt-get install -y apache2
~~~
2) Modificar o arquivo /etc/telegraf/telegraf.conf e dar permissão para ler os logs do Apache:
~~~bash
$ sudo usermod -a -G adm telegraf
$ cat telegraf.conf | grep -A 45 "\[\[inputs.logparser\]\]" | egrep -v "^#"
 [[inputs.logparser]]
   files = ["/var/log/apache2/access.log"]
   from_beginning = true
   [inputs.logparser.grok]
     patterns = ["%{COMBINED_LOG_FORMAT}"]
     measurement = "apache_access_log"
$ sudo service telegraf restart
~~~
3) Criar o dashboard:

Create > Dashboard > Configurações
4) Criar a variável host para o uso global do dashboard:

Create > Dasbhoard > Configurações
Variables > Type: Query
Variables > Name: code
Variables > Type: Query
Variables > Data source: InfluxDB
Refresh > On time range change
Variables > Query: SHOW TAG VALUES WITH KEY = "resp_code" where host =~ /^$server$/
Multi-Value e Include all options
5) Criar os painéis para monitorar o Apache:
Número de erros 404
~~~bash
SELECT count("request")
FROM "apache_access_log"
WHERE "host" =  'ubuntu-bionic'
  AND "resp_code" = '404'
  AND $timeFilter 
  AND "agent" != 'Go-http-client/1.1'
  AND agent != 'worldping-api'
~~~
Logs
~~~bash
SELECT  "request"
FROM "apache_access_log"
WHERE "host" =~ /^$server$/ 
  AND "resp_code" =~ /^$code$/ 
  AND $timeFilter  

SELECT  "client_ip"
FROM "apache_access_log"
WHERE "host" =~ /^$server$/ 
  AND "resp_code" =~ /^$code$/ 
  AND $timeFilter  
~~~
6) Estressar a aplicação para visualizar os logs e contagem de erros:
~~~bash
# Estressando a quantidade de requests
$ for i in {1..501}; do curl http://localhost/  > /dev/null 2>&1;done

# Estressando a quantidade de requests invalidos:
$ for i in {1..501}; do curl http://localhost/alura  > /dev/null 2>&1;done
~~~

####

1) Configurar o bot no canal do Slack

Adicionar uma APP no canal -> Incoming WebHooks > Add Configuration
Post to Channel > cursos-alura > Add Incoming WebHooks integration`
2) Adicionar a configuração no Grafana

Alerting > Notification channels > Add channel
Name: slack
Type: Slack
Default
Url: Slack Webhook URL
Send Test
3) Criar o alerta para memória: