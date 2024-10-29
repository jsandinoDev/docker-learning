
# Networking 


### Case 1
Container to www communication 
- Request from a container to www using http
- Works without any special changes

### Case 2
Container host to a localmachine
- Example: Data base running in local machine

Replace  **localhost** in the connection string with **host.docker.internal**
```js
'mongodb://localhost:27017/swfavorites',
 'mongodb://host.docker.internal:27017/swfavorites',
```


### Case 3 
Container to Container

Ex  1 container web API
    1 container with mongoDB 

Create a new container with mongo db image
```
docker run mongo -d --name mongodb
```

Inspect the docker container just created
```
    docker container inspect mongodb
```

Search for the Network settings -> IPAddress 
```js
 'mongodb://172.17.0.2:27017/swfavorites',
```
---

### Docker Networks
In this network all the containers are able to talk to each other

```js
docker run --network my_network
```
Ex

Create a network

```
    docker network create favorites-net
```

```
docker run  -d --name mongodb --network favorites-net  mongo
```
Change the connection string (localhost,etc) for the name of the network
```js
 'mongodb://mongodb:27017/swfavorites',
```

Run the node container with the same network