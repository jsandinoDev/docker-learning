# Docker Commands

https://hub.docker.com/

Build an image base in the docker file
```bash
    docker build .
```

This generates the image:

96577f2d608d30661a896d4f0268ebfdc63f7119dfa630d15f96d216740786d0


To run 
```bash
    docker run -p 3000:3000 96577f2d608d30661a896d4f0268ebfdc63f7119dfa630d15f96d216740786d0


    docker run -p 3000:80 68bc04e187db0a74387173dc4d6c3fc54416ad3962223dc9e20b3689c61c457c
```
-p to publish (expose)
3000:80 -> first is to localport to access the app
          -> docker container expose port

Re-start a container (docker run - creates a new container )
```bash
    docker start **name**
```

### Attacth/Dettach mode
(blocking que console)

In run is attacth

In start is detach


To run dettacht mode **-d**
```bash
    docker run -p 3000:3000 -d 96577f2d608d30661a896d4f0268ebfdc63f7119dfa630d15f96d216740786d0
```

To attach -> docker attach **name**

To see the log -> docker logs **name**

To attacg the logs -> docker logs -f **name**

To use start but attacth -> docker start -a **name**

----

To see the running ports 
```bash
    docker ps
```

To stop a docker container
```bash
   docker stop admiring_khayyam
```


Run images 
```bash
    docker run node
```

Run images (expose and interact with new container)
```bash
    docker run  -it node
```





Summary 
```dockerfile

FROM node  

WORKDIR /app 
# sets docker that all the subsequence commands have to run inside /app folder

COPY package.json /app

RUN npm install
# Here because of the layers (optimization)
# Every time there is change in the src code there will be no need of run again npm install
# Docker will use the cache of this command

# COPY . ./  -> a way to do the line 7
COPY . /app
# 1 (.) path outside the dockerfile -> copy into the image
# 2(.) path inside the image  -> where to copy in the image
# create a snapshot of the source code


EXPOSE 80
# a docker container is ISOLATED 
# have its own network
# expose a port to the local machine

CMD  ["node",  "server.js"]
# not executed when an image is create but when a container is created



```

## List container
```bash
docker ps  / list all containers 

docker ps -a  // all containers of the past including past containers that are not runnig any more

```

## Run python

To interact with the console need

```bash
docker run -i -t 358e4c08920cc3fe644e716a96a575806317d5efafbb69639bc67b5d4a1bb917
```

- -i, --interactive                      Keep STDIN open even if not attached

- -t, --tty                              Allocate a pseudo-TTY

After running this the container will stop, so we need to restart

```bash
docker run -a -i 358e4c08920cc3fe644e716a96a575806317d5efafbb69639bc67b5d4a1bb917
```

- -i, --attacht

- -t, --- -i, --interactive                      Keep STDIN open even if not attached


## Delete images/container


### Container 

Remove ->

* can't remove a running container

docker rm **name** **name1** **name2**


### Images

List -> docker images

remove -> docker **rmi** **image_id**

remove all unused images -> docker image prune


---
### -rm

Aumatically remove the container 

```bash
docker run -p 3000:80 --rm 358e4c08920cc3fe644e716a96a575806317d5efafbb69639bc67b5d4a1bb917
```

## Inspect Images

```bash
docker image inspect **name**
```

---
### Copying files into & from container


Copy into
```bash
    docker cp dummy/. **container_name**:/test 
```


Copy from container
```bash
    docker cp **container_name**:/test  dummy/. 
```

---

### Naming & Tagging

Containers

```bash
docker run -p 3000:80 -d --rm --name goalsApp 68bc04e187db0a74387173dc4d6c3fc54416ad3962223dc9e20b3689c61c457c
```
Images

name : tag

FROM node : 12

```bash
docker build . -t goals:latest . 
```

---
