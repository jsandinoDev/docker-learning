# Docker Commands

Build an image base in the docker file
```bash
    docker build .
```

This generates the image:

96577f2d608d30661a896d4f0268ebfdc63f7119dfa630d15f96d216740786d0


To run 
```bash
    docker run -p 3000:3000 96577f2d608d30661a896d4f0268ebfdc63f7119dfa630d15f96d216740786d0
```

To see the running ports 
```bash
    docker ps
```

To stop a docker container
```bash
   docker stop admiring_khayyam
```
