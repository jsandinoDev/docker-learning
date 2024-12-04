

## Create Utility containers


Containers that dont incluide an app just the enviroment

Run node and create a project using npm init in a containers
### Docker exec 
Excecute commands in a running containers without interrupting the default command

Can be use to read files from a container
```
    docker exec -it container_name npm init
```

Override the initial or default command

```
    docker run -it node npm init
```


---

### Create Dockerfile
```
FROM node:14-alphine

WORKDIR /app
```

Run container and create bind mount to share data wiht host machine

```
docker run -it /Users/josuesandino/Documents/Code/docker-learning/07-utility-containers npm init
```

---

### Using entrypoint


This entry point allows to add some commands after, with cmd with command would be override by the command send in the creation command
```
FROM node:14-alphine

WORKDIR /app

ENTRYPOINT [ "npm" ]

```

Build the image
 
Run it
```
docker run -it /Users/josuesandino/Documents/Code/docker-learning/07-utility-containers mynpm init
```

To upate or run something else just:
```
docker run -it /Users/josuesandino/Documents/Code/docker-learning/07-utility-containers mynpm **HERE** 
```

Instead of **Here** just add ```install``` ```install express --save ```, etc 


---

### Using docker compose

Create the docker compose file

```
version: '3.8'
services:
  npm:
    build: ./
    stdin_open: true
    tty: true
    volumes:
      - ./:/app
```

Run:

Allows to run a single service by the service name, in this case **npm** and the append command **init**
```
    docker-compose run --rm npm init 
```