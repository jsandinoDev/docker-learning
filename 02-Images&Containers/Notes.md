
----

### Containers

The running "unit of software"

Can be:
- named  / --name
- configured / --help
- listed / docker ps
- removed / docker rm
----


### Images

Templates/Blueprint for containers

Contains code + required tools/runtimes

We can use an image to create different containers

Are layer base
- every instructions is a layer
- this layers area cache

Can be
- tagged (named) / -t docker tag ..
- listed  / docker images 
- analyzed / docker image inspect
- removed / docker rmi, docker prune


--- 