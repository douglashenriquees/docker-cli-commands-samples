## Volumes

*  ```docker container run -it -v $(pwd)/volume-alpine:/usr/share alpine```
    * ```$(pwd)``` é o diretório atual
* ```echo 'teste de volume no alpine' > /usr/share/teste.txt```
* ```cat /usr/share/teste.txt```

## Volume MySql DB

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

## Imagens - Build

* ```cd images```
* ```docker image build -t sample-image-debian/deb:1.0 . ```
* ```docker container run -d -p 8800:80 sample-image-debian/deb:1.0```