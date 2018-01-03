## Reutilizando o ambiente testado em um outro servidor

```bash
$ docker swarm init --advertise-addr 10.0.4.6
Swarm initialized: current node (4btqj3sjfwvzo9hw6keejbng0) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-5n6krdkd57vf4xa007e972jq61j5xbf334v4705a6387hb7hx0-0k4rjv7yhizwnh8ujft22kb9t \
    10.0.4.6:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

[node4] (local) root@10.0.4.6 ~
```

```bash
$ docker node ls
```

Executar o comando para adicionar um worker e executar o comando `docker node ls` novamente.

Baixando arquivo compose do github

```bash
$ apk update
$ apk add git
$ git clone https://github.com/treina-docker/docker-compose.git
$ cd docker-compose
```


##### Criando uma stack com docker swarm
```bash
$ docker stack deploy --compose-file docker-compose.yml treinadocker
```

Listando os serviços
```bash
$ docker service ls
```

O comando abaixo mostra o log de execução do serviço:
```bash
docker service logs treinadocker_jenkins
```

### Atualizando versão no jenkins

Para atualizar a versão do jenkins basta editarmos o arquivo `docker-compose.yml` e alterar a linha:

```yaml
    image: jenkinsci/jenkins:2.46.1-alpine
```

para
```yaml
    image: jenkinsci/jenkins:alpine
```

Executar novamente o comando de criação da stack e esperar o serviço subir para ver a versão atualizada.


#### Verificando qual nó o serviço foi criado
```bash
$ docker service ps treinadocker_mysql
```

#### Adicionando o container portainer ao docker swarm
```bash
$ docker service create \
      --name portainer \
      --publish 9000:9000 \
      --constraint 'node.role == manager' \
      --mount type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
      portainer/portainer \
      -H unix:///var/run/docker.sock
```

