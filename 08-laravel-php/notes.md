### Laravel && Php


To run only the desire services in a docker-compose file the command should be:


```
    docker-compose up -d service1 service2 service3
```

To run the same (add the service2 and 3 as a dependency of service1) and also build the images if it is needed 

```
    docker-compose up -d  --build service1 
```