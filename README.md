## Criando um ambiente com Docker Compose

Para isso criamos um arquivo `docker-compose.yml` com a descrição do ambiente que queremos e executamos o comando abaixo:
```bash
$ docker-compose up -d
```

Para configurar o jenkins precisamos pegar a chave de instalação, para isso precisamos olhar o log do container com o seguinte comando:
```bash
$ docker logs -f 01criandostackswarm_jenkins_1
```