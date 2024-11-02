## Demo project


3 Building BLocks
---

### Database -> Mongo DB
    - data must presist
    - access should be limited 

Docker App

Export the port 27017 that is the def port of mongo
With the 0000:0000 the first numbers is the localhost port and the next is the container port 
```docker
docker run --name mondodb --rm -d -p 27017:27017 mongo
```

---

### BackEnd -> NodeJS Rest API
    - data must persist
    - live source code update

Create the dockerfile in the backend folder
Create the image

```docker
docker build -t goals-node .
```

Publish the port with the -p command to make it visible
Create the container
```docker
docker run --name goals-backend --rm -d -p 80:80 goals-node 
```

----



### Front - React SPA
    - live source code update

Create the dockerfile in the backend folder
Create the image

```docker
docker build -t goals-react .
```

Publish the port with the -p command to make it visible
Create the container
```docker
docker run --name goals-frontend --rm -d -p 300:3000  -it goals-react
```



---


## Use common network

Create:

docker network create goals-net


Run the container usign the nerwork

There is no need to add the ports because they are in the same network

## Mongo
```
docker run --name mondodb --rm -d --network goals-net mongo
```

## Backend

Check for the connection string:
```javascript
    //  'mongodb://host.docker.internal:27017/course-goals',

      'mongodb://mongodb:27017/course-goals',
```

```
docker run --name goals-backend --rm -d --network goals-net goals-node 
```

```
docker run --name goals-backend --rm -d -p 80:80 --network goals-net goals-node 
```

## Frontent

Check where is used local host:
 
 Replace: with the container name
```
  const response = await fetch('http://localhost/goals');
```

With:
```
      const response = await fetch('http://goals-backend/goals/' + goalId, {
```
Command to re-run

```
docker run --name goals-frontend --rm -d --network goals-node  -it goals-react

```



# Data Persistance in Mongo DB


## MONGO
```
docker run --name mongodb -v data:/data/db--rm -d --network goals-net mongo
```

Security

```
docker run --name mongodb -v data:/data/db--rm -d --network goals-net -e MONGO_INITDB_ROOT_USERNAME=jsandinoDev -e MONGO_INITDB_ROOT_PASSWORD=secret mongo
```




Modify the connections string

```
    `mongodb://${process.env.MONGODB_USERNAME}:${process.env.MONGODB_PASSWORD}mongodb:27017/course-goals?authSource=admin'`,
```

## BACKEND

Add bind mode and volumes
```
docker run --name goals-backend -v multi-01-starting-setup/backend/app:/app -v logs:/app/logs -v /app/node_modules x--rm -d -p 80:80 --network goals-net goals-node 
```
Change the packagejson file && add nodemon

Change the dockerfile


Also add env variable and modify the connection string

```docker
ENV MONGODB_USERNAME root
ENV MONGODB_PASSWORD=secret

CMD [ "npm","start"]
```

With all the changes
```
docker run --name goals-backend -v multi-01-starting-setup/backend/app:/app -v logs:/app/logs -v /app/node_modules -e MONGODB_USERNAME=jsandino --rm -d -p 80:80 --network goals-net goals-node 
```

Add a .dockerignore

```
node_modules
dockerfile
.git

```

# Live source code updates

The 1. -v is to copy the src folder of my local pc to the container and check live updates

```
docker run -v /Users/josuesandino/Documents/Code/docker-learning/05-Multi-ContainerApps/multi-01-starting-setup/frontend/src:/app/src --name goals-frontend --rm -d --network goals-node  -it goals-react
```

Add the same .dockerignore file