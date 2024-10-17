
# Data

Application 
    - code + enviroment
    - provided by developer
    - added to image on build phase
    - can't be changed once image is build
    - Read only in images

Temporary App Data 
    - fetched/produced in running container
    - Stores in memory temp files
    - Dynamic and changing 
    - Read + write temporary in containers

Permanent App Data
    - ex user accounts
    - fetchet in running container
    - stored in files or DB
    - must bnot be lost if container restart /stops
    - Read + write - stored with container & **Volumes**



---


## Volumes

- Folders in my host machine that are made available into containers

- Connect a folder inside a container to a folder outside the container

- Values persist if the container shutdown

- A container can write data into a volume a read data from it

### Types

- Anonymoues Volumes
- Named Volumes

Docker sets up a folder on the host machine  - use docker volume commands

With name volumes -> the volumes with survive the container shutdown

Greate for data which should be persistent

See the active volumnes

```docker
docker volume ls
```

```docker
docker run -p 3000:80 -d --rm --name feedback-app -v feedback:/app/feedback feedback-node:volumes
```


## Bind Mounts

- Volumes are run by docker - we (developer) dont know where they are

- In bind mounts we define the path

- Great for persistend & editable data

```docker
docker run -p 3000:80 -d --rm --name feedback-app -v feedback:/app/feedback -v "/Users/josuesandino/Documents/Code/docker-learning/03-Data&&Volumnes/data-volumes-01-starting-setup:/app" -v /app/node_modules feedback-node:volumes
```

To add Read Only 

Add **ro** at the end
```docker
docker run -p 3000:80 -d --rm --name feedback-app -v feedback:/app/feedback -v "/Users/josuesandino/Documents/Code/docker-learning/03-Data&&Volumnes/data-volumes-01-starting-setup:/app:ro" -v /app/node_modules feedback-node:volumes
```
The thing with this if that if I want to use other folders to write and read I need an subcontainer that avoid to overwrite
Like the /node_modules

So it will be like this;
```docker
docker run -p 3000:80 -d --rm --name feedback-app -v feedback:/app/feedback -v "/Users/josuesandino/Documents/Code/docker-learning/03-Data&&Volumnes/data-volumes-01-starting-setup:/app:ro" -v /app/temp -v /app/node_modules feedback-node:volumes
```


First -v: -v feedback:/app/feedback
-	This mounts a Docker-managed volume named feedback to the /app/feedback directory inside the container. Docker volumes persist data even if the container is removed, so this is used to store data persistently.
-	Second -v: -v "/Users/josuesandino/Documents/Code/docker-learning/03-Data&&Volumnes/data-volumes-01-starting-setup:/app"
	-	This mounts a directory from your host machine (/Users/josuesandino/Documents/Code/docker-learning/03-Data&&Volumnes/data-volumes-01-starting-setup) to the /app directory inside the container. This allows your local project files to be accessible inside the container at /app.
- Third -v: -v /app/node_modules
	-	This is an anonymous volume. It tells Docker to create an unnamed volume that will store the /app/node_modules directory within the container. This prevents the local node_modules folder from being overwritten by your host machine’s /app directory. It ensures that any installed dependencies in the container don’t affect your local environment.

    Why Use an Anonymous Volume for node_modules?

Mounting /app/node_modules like this ensures that the container’s version of node_modules is isolated from your host machine, while still allowing the rest of the app files (/app) to sync with your local development environment.


### Add nodemon to autoupdate the js files

Add this in the package.json file to enable the autoupdate

Nodemon checks when a file in the server in chaged and update it.
```json
"scripts": {
    "start": "nodemon server.js"
  },
  "devDependencies": {
    "nodemon":"2.0.4"
  }
```

Also it is needed to change the dockerfile
```docker
CMD [ "npm","start  " ]
```

---

## Summary 

### Anonymous Volume
```docker
docker run -v /app/data ...
```
- Create specifically for a single container
- Survives container restart unless --rm is used
- Can not be shared accross containers
- can't be re-used

### Named Volume
```docker
docker run -v data:/app/data ...
```
- created in general - not tied to an especific container
- survives container shutdown
- cna be shared across containers
- can be re-used 


Bind Mount
```docker
docker run -v /path/to/code:/app/data ...
```

- localtion on host system - not tied to any container
- survives container shutdow/restart
- can be shared
- cna be re-used


## Managing Docker Volumes

Create volume manually 
```docker
docker volume create feedback-files
```
Inspect
```docker
docker volume inspect feedback-files
```
Remove
```docker
docker volume rm feedbaack-files
```

 
### Docker Ignore

file similar to .gitignore

---

# Argument & Env variables


### Env
- Available insidor docker file & app code
- Set via --env on **docker run**

```javascript
// In docker file
ENV PORT 80

EXPOSE $PORT

// In js file
app.listen(process.env.PORT)
```

Can be used with --env / -e / if multiple env use -e ... -e ...

When used with a file use
--env-file ./.env

```docker
docker run -p 3000:8000 --env PORT=8000 -d --rm --name feedback-app -v feedback:/app/feedback -v "/Users/josuesandino/Documents/Code/docker-learning/03-Data&&Volumnes/data-volumes-01-starting-setup:/app:ro" -v /app/temp -v /app/node_modules feedback-node:volumes
```

### Args

- Available inside docker file, not in CMD or any app
- Set on  image build via ** --build-arg**

```javascript
// In docker file
ARG DEFAULT_PORT=80
...
...

ENV PORT $DEFAULT_PORT

EXPOSE $PORT

// In js file
app.listen(process.env.PORT)
```
Build image
```docker
docker build -t feedback-node:dev --build-arg DEFAULT_PORT=8000 .
```