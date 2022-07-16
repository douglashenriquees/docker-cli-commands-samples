## Volumes

*  ```docker container run -it -v $(pwd)/volume-alpine:/usr/share alpine```
   * ```$(pwd)``` é o diretório atual
* ```echo 'teste de volume no alpine' > /usr/share/teste.txt```
* ```cat /usr/share/teste.txt```

## Volume MySql

* ```docker container run --name mysqldb -d -p 3306:3306 -v $(pwd)/volume-mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=strongpass --platform linux/x86_64 mysql:5.7```
  * ```-d``` executa o container em **background**
  * ```-e``` define a variável de ambiente exigida pela imagem do mysql para o **password**
  * ```--platform linux/x86_64``` especifica a arquitetura do **host** que está obtendo a imagem
* ```docker container exec -it mysqldb /bin/bash```
  * o comando acima executa o **bash** do container no modo **interativo** em um **pseudo-terminal**
  * ```mysql -u root -p``` faz o login no mysql dentro do container
  * ```create database exemplo_db;``` cria uma nova base de dados
  * ```show databases;``` exibe as bases de dados

## Portas

* ```docker container run -d -p 4400:80 -v $(pwd)/html:/usr/share/nginx/html nginx```

## Redes

* ```docker network create --driver bridge network-alpine```
  * o **driver** padrão é o **bridge**. O parâmetro ```--driver bridge``` pode ser suprimido
* ```docker container run -it --name alp1 --net network-alpine alpine```
  * definindo a rede interna para o container com o parâmetro ```--net```
  * ```hostname -i```dentro do container **alp1** retorna o **IP** **172.19.0.2**
* ```docker container run -it --name alp2 --net network-alpine alpine```
  * ```hostname -i```dentro do container **alp2** retorna o **IP** **172.19.0.3**
  * ```ping alp1``` demonstra que em redes customizadas é possível encontrar os containers pelo nome
* ```docker container run -it --name alp3 alpine```
  * o container **alp3** foi criado na rede padrão **bridge**, por não ter sido informado o parâmetro ```--net```
  * ```docker container inspect alp3```
  * ```docker network connect network-alpine alp3``` coloca o container na rede informada

## Redes - Balanceamento de Carga

* ```docker network create app```
* ```docker network create database```
* ```docker container run -d --name mysqldb -v $(pwd)/volume-mysql-redes:/var/lib/mysql --net database -e MYSQL_ROOT_PASSWORD=strongpass -e bind-address=0.0.0.0 --platform linux/x86_64 mysql:5.7```
  * a sentença ```-e bind-address=0.0.0.0``` faz o container atender somente as requisições recebidas via rede de software do docker. Não é possível chegar ao container através da máquina **host**
* ```docker container run --name app-1 -e DBHOST=mysqldb --net database username/mvc-produtos:2.0```
* ```docker container run --name app-2 -e DBHOST=mysqldb --net database username/mvc-produtos:2.0```
* ```docker container run --name app-3 -e DBHOST=mysqldb --net database username/mvc-produtos:2.0```
  * os containers de aplicação criados acima chegam ao container da base de dados pelo seu nome que está definido na variável de ambiente **DBHOST**. Eles foram criados na mesma rede
* ```docker network connect app app-1```
* ```docker network connect app app-2```
* ```docker network connect app app-3```
* ```docker container run -d --name load-balancer --net app -v $(pwd)/haproxy.cfg:/usr/local/etc/haproxy/haproxy.cfg -p 8800:80 --platform linux/x86_64 haproxy:1.7.0```

## Imagens - Build

* ```cd images```
* ```docker image build -t sample-image-debian/deb:1.0 . ```
* ```docker container run -d -p 8800:80 sample-image-debian/deb:1.0```