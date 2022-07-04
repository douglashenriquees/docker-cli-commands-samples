# Volumes

* ```cd cli```
*  ```docker container run -it --name alp1 -v $(pwd)/exemplo-volume:/usr/share alpine```
  * ```$(pwd)``` é o diretório atual
* ```echo 'teste de volume no alpine' > /usr/share/teste.txt```
* ```cat /usr/share/teste.txt```

# Volume MySql DB

* ```docker volume create volumedb```
* ```docker container run -d --name mysqldb -v volumedb:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=strongpass --platform linux/x86_64 mysql:5.7```
  *  ```-d``` executa o container em **background**
  *  ```-e``` define a variável de ambiente exigida pela imagem do mysql para o **password**
  * ```--platform linux/x86_64``` especifica a arquitetura do **host** que está obtendo a imagem
* ```docker container exec -it mysqldb /bin/bash```
  * o comando acima executa o **bash** do container em modo **interativo** em um **pseudo-terminal**
  * ```mysql -u root -p``` loga no mysql dentro do container
  * ```create database exemplo_db;``` cria um nova base de dados
  * ```show databases;``` exibe as bases de dados

# Portas

* ```cd cli```
* ```docker container run -d --name ws1 -p 4400:80 -v$(pwd)/html:/usr/share/nginx/html nginx```

# Imagens - Build

* ```cd cli```
* ```cd images```
* ```docker image build -t sampleimage/img:1.0 . ```
* ```docker container run -d -p 4400:80 --name ws1 sampleimage/img:1.0```