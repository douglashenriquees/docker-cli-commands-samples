# Volumes

* ```cd cli```
*  ```docker container run -it --name alp1 -v $(pwd)/exemplo-volume:/usr/share alpine```
  * ```$(pwd)``` é o diretório atual
* ```echo 'teste de volume no alpine' > /usr/share/teste.txt```
* ```cat /usr/share/teste.txt```

# Portas

* ```cd cli```
* ```docker container run -d --name ws1 -p 4400:80 -v$(pwd)/html:/usr/share/nginx/html nginx```

# Imagens - Build

* ```cd cli```
* ```cd images```
* ```docker image build -t sampleimage/img:1.0 . ```
* ```docker container run -d -p 4400:80 --name ws1 sampleimage/img:1.0```