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