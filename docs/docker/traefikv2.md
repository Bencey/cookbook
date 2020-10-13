Traefik is an open source reverse proxy and load balancer that allows you to deploy services easy. 


In my docker swarm I use traefik (Version 2) as my reverse proxy for all of my services (Even homeassistant which isnt running on my swarm). In this guide I will be teaching you how to install traefik Version 2 so you can use it on your dockerswarm


### Requirements

* Docker Swarm Setup


### Guide

The first thing you will need to do is to create the traefik network, In this guide the Directory I have used is /var/data/config/traefik and in this directory I have a file called traefik.yml. The file contains the following



### Prepare traefik.yml

```
version: "3.2"

services:
  scratch:
    image: scratch
    deploy:
      replicas: 0
    networks:
      - public

networks:
  public:
    driver: overlay
    attachable: true
    ipam:
      config:
        - subnet: 172.16.200.0/24
```

This creates a network that allows traefik to attach itself to.


You will now need to start preparing your traefik files. The directory I have used is /var/data/config/traefikv2/ In this directory I have 2 files. `traefik.toml` and `traefikv2.yml`


### Prepare traefik.toml

```
[global]
  checkNewVersion = true

# Enable the Dashboard
[api]
  dashboard = true

# Write out Traefik logs
[log]
  level = "DEBUG"

[entryPoints.http]
  address = ":80"
  [entryPoints.http.http.redirections.entryPoint]
    scheme = "https"

[entryPoints.https]
  address = ":443"
  [entryPoints.https.http.tls]
    certResolver = "main"

# Docker Traefik provider
[providers.docker]
  endpoint = "unix:///var/run/docker.sock"
  swarmMode = true
  watch = true
  exposedbydefault = true
  network = "traefik_public"
```


### Prepare traefikv2.yml

```
version: "3.8"

services:
  traefik:
    image: traefik:latest
    ports:
      - "80:80"
      - "8080:8080" # traefik dashboard
      - "443:443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /var/data/config/traefikv2:/etc/traefik
    networks:
      - traefik_public
    deploy:
      labels:
        - "traefik.docker.network=traefik_public"
        - "traefik.http.routers.api.rule=Host(`traefik.example.com`)" 
        - "traefik.http.routers.api.service=api@internal"
        - "traefik.http.services.api.loadbalancer.server.port=9999"

networks:
  traefik_public:
    external: true
```

So now all you will need to edit is the Host so matches your domain. Then you will need to create an A record in your DNS host for traefik.domain.com and point it at the public IP of the network the device is running on. I must warn you that at this point you do not have any authentication setup so once you deploy the service anyone is able to access your website. In the future I will be publishing a guide on how to protect your services using authelia


### Deploying 

Now it is time to deploy your services. First you will need to deploy the network using `docker stack deploy -c traefik /var/data/config/traefik/traefik.yml`, wait until it has deployed then deploy traefikv2 using `docker stack deploy -c traefikv2 /var/data/config/traefikv2/traefikv2`. You will be able to view the status of the services by running `docker stack ps traefikv2`
