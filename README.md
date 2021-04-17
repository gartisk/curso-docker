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