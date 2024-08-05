**Install Docker**
```shell
sudo apt install docker-compose
```

**List [a]ll docker processes**
```shell
docker ps -a
```
## Basic commands
### Docker image
<small>Download and handle images to run</small>

**Download docker image**
<small>The tag is optional</small>
```shell
docker pull [image]:[tag]
```

**List docker images**
```shell
docker image ls
```

**Remove image**
```shell
docker image rm -f [image]
```

### Docker run
<small>Run images, the containerisation begins</small>

**Basic run**
```shell
docker run [image]
```

**Run docker image interactively [i] with terminal [t]**
<small>Allow us to interact with the container directly</small>
```shell
docker run -it [image] /bin/bash
```

**Run docker  detached images**
```shell
docker run -d [image]
```

**Open port on host system**
```shell
docker run -p 80:80 [image]
```

### Docker start
<small>(Re)Start a down docker</small> 
```shell
docker start [id]
```
### Docker exec
<small>Go in a container with shell</small>
```shell
docker exec -it [id] /bin/bash
```
### Docker stop
<small>Stop a running container</small>
```shell
docker stop [id]
```
### Docker rm
<small>Remove a stopped container, use -f to force</small>
```shell
docker rm [id]
```

## Dockerfile
### Build a webserver
Firstly, setup a Dockerfile in the root of the project

**1. Setup the Dockerfile**
```yml
# THIS IS A COMMENT
FROM ubuntu:22.04

# Update the APT repository to ensure we get the latest version of apache2
RUN apt-get update -y 

# Install apache2
RUN apt-get install apache2 -y

# Tell the container to expose port 80 to allow us to connect to the web server
EXPOSE 80 

# Tell the container to run the apache2 service
CMD ["apache2ctl", "-D","FOREGROUND"]
```

**2. Build the image**
```bash
docker build -t webserver .
```

**3. (Facultative) Show the builded image**
```shell
docker image ls
```

**4. Run the image**
```bash
docker run -d --name webserver -p 80:80 webserver
```

**5. (Facultative) Execute a shell in the container**
```shell
docker exec -it webserver /bin/bash
```

**Copy files from my system to the container**
```shell
docker cp [my-file] [docker-id]:[path]
```

## Docker Compose

### Example

#### docker-compose.yml
```yml
version: '3.3'
services:
  web:
    build: ./web
    networks:
      - ecommerce
    ports:
      - '80:80'


  database:
    image: mysql:latest
    networks:
      - ecommerce
    environment:
      - MYSQL_DATABASE=ecommerce
      - MYSQL_USERNAME=root
      - MYSQL_ROOT_PASSWORD=helloword

networks:
  ecommerce:
```

**In the ./web, make the Dockerfile**
```yml
FROM ubuntu:22.04

RUN apt-get update -y 

RUN apt-get install apache2 -y

EXPOSE 80 

CMD ["apache2ctl", "-D","FOREGROUND"]
```

**Then docker-compose up, add `-d` to detach**
```shell
docker-compose up 
```

**Down the container**
```shell
docker-compose down
```

