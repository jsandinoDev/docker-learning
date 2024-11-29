## Demo project


3 Building BLocks
---

- Database -> Mongo DB
    - data must presist
    - access should be limited 

Docker App

Export the port 27017 that is the def port of mongo
With the 0000:0000 the first numbers is the localhost port and the next is the container port 
```docker
docker run --name mondodb --rm -d -p 27017:27017 mongo
```

---

- BackEnd -> NodeJS Rest API
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



- Front - React SPA
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
Rebuild image:


```
docker run --name goals-frontend --network goals-net --rm -d -p 300:3000  -it goals-react

```