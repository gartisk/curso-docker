https://www.udemy.com/course/curso-docker

No console rode:
5|37:
```sh
# na pasta nginx_ex1:
docker image build -t ex-simple-build .
```

5|38:
```sh
# na pasta build-com-arg:
docker image build -t ex-build-arg
# para visualizar variavel do sistema:
docker container run ex-build-arg bash -c 'echo $S3_BUCKET'

# para enviar parametro para imagem?:
docker image build --build-arg S3_BUCKET=myapp -t ex-build-arg .
resultado 

# apos alteração acima tente visualizar a variavel do sistema
docker container run ex-build-arg bash -c 'echo $S3_BUCKET'
# resultado: myapp

# para visualizar o mantenedor da imagem, é possivel obter informação rapidamente utilizando filtro abaixo:
docker image inspect --format="{{index .Config.Labels \"maintainer\"}}" ex-build-arg
```

5|39
```sh
    docker image build -t ex-build-copy .
    docker container run -p 80:80 ex-build-copy
```

5|41 - Uso de instruções para execução do container 
```sh
    docker image build -t ex-build-dev .

    # -v: volume
    # -it: modo interativo
    # -p: porta
    docker container run -it -v $(pwd):/app -p 80:8000 --name python-server ex-build-dev

    # novo container debian irá ler o log gerado no container anterior
    docker container run -it --volumes-from=python-server debian cat /log/http-server.log
```
*É necessário mapear o volume para que o diretorio /app aponte para a pasta build-dev que está na máquina host*

6|44
Apresentados 3 tipos de rede: none, bridge e host.

6|45 Rede Tipo Bridge
```sh
docker container run --rm alpine ash -c "ifconfig"
docker container run --rm --net none alpine ash -c "ifconfig"

docker network inspect bridge

# teste comunicação entre containers em modo bridge
docker container run -d name container1 alpine sleep 1000
docker container exec -it container1 ifconfig

docker container run -d name container1 alpine sleep 1000
docker container exec -it container2 ifconfig

# veja que é possivel pingar o outro container
docker container exec -it container1 ping 172.17.0.3

#assim como é possivel pingar um site qualquer
docker container exec -it container1 ping www.google.com

# ----------------------

# criação de uma nova rede
docker network create --driver bridge rede_nova

docker container run -d --name container3 --net rede_nova alpine sleep 1000
docker container exec -it container3 ifconfig

# veja que nesse momento não há conectividade entre as diferentes redes.
docker container exec -it container3 ping 172.17.0.2

# para permitir que o container3 se conecte tambéma a rede bridge
docker network connect bridge container3

# como pode observar uma nova interface eth1 com a rede bridge é criada.
docker container exec -it container3 ifconfig

# veja que é possivel agora pingar a rede bridge
docker container exec -it container3 ping 172.17.0.2

# para desativar a conexao do container com a rede bridge
docker network disconnect bridge container3

# veja que agora só é mantida a interface eth0
docker container exec -it container3 ifconfig
```

6|46
```sh
docker container run -d --name container4 --net host alpine sleep 1000

# veja o container fica mais exposto e todas as interfaces do host são compartilhadas com a maquina.
docker container exec -it container4 ifconfig
```

