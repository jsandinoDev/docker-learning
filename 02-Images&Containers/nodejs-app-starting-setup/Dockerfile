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

