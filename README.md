# Volumes

* ```cd cli```
*  ```docker container run -it --name alp1 -v $(pwd)/exemplo-volume:/usr/share alpine```
  * ```$(pwd)``` é o diretório atual
* ```echo 'teste de volume no alpine' > /usr/share/teste.txt```
* ```cat /usr/share/teste.txt```

# Volume MySql DB

* ```docker container run -d --name mysqldb -v $(pwd)/exemplo-volume-mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=strongpass --platform linux/x86_64 mysql:5.7```
  *  ```-d``` executa o container em **background**
  *  ```-e``` define a variável de ambiente exigida pela imagem do mysql para o **password**
  * ```--platform linux/x86_64``` que precede o nome da imagem, é para especificar a arquitetura da **host** que está obtendo a imagem

# Portas

* ```cd cli```
* ```docker container run -d --name ws1 -p 4400:80 -v$(pwd)/html:/usr/share/nginx/html nginx```

# Imagens - Build

* ```cd cli```
* ```cd images```
* ```docker image build -t sampleimage/img:1.0 . ```
* ```docker container run -d -p 4400:80 --name ws1 sampleimage/img:1.0```