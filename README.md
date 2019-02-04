* Introduction
* Requirements
* Installation & Configuration
* Usage

INTRODUCTION
------------
Traefik is a modern HTTP reverse proxy and load balancer for microservices. Traefik makes all microservices deployment easy, integrated with existing infrastructure components such as Docker, Swarm Mode, Kubernetes, Amazon ECS, Rancher, Etcd, Consul etc.

Traefik serves as a router for all your microservices applications, routing all client requests to correct microservices destination.


REQUIREMENTS
------------
**Docker:**
- [Install Docker on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04)
- [Install Docker on MacOS](https://docs.docker.com/docker-for-mac/install/)

**Docker Compose:**
- [Install Docker Compose on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-16-04)
- Docker compose on Macos comes with the Docker Desktop app.

INSTALLATION & CONFIGURATION
----------------------------
1. Create docker network

`docker network create proxy`

2. Install 'apache2-utils' using the apt command below.

`sudo apt install apache2-utils -y`

3. Run the htpasswd command below to generate a new password for traefik dashboard authentication.

`htpasswd -nb admin password`

4. Keep the result in your note.

***The password will depend on the password you have set***

`admin:$apr1$hEgpZUN2$OYG3KwpzI3T1FqIg9LIbi.`

5. Move to `/opt` directory
6. Clone the Traefik repo inside the `/opt` directory
7. `git clone git@github.com:SpecbeeLabs/traefik.git`
8. Move to the cloned traefik directory
9. Update #15 of `traefik.toml` file to add the generated password in Step 4.
10. Run `docker-compose up -d`

This will create a docker container for Traefik to route applications

Goto http://monito.specbeelabs.io and enter the credentials generated at Step 4 which will show you the Traefik dashboard.


USAGE
-----
Sample docker-compose file to route an application via Traefik

```
version: '3'

services:
  whoami:
    image: emilevauge/whoami
    volumes:
    labels:
      - "traefik.docker.network=proxy"
      - "traefik.enable=true"
      - "traefik.basic.frontend.rule=Host:whoami.specbeelabs.io"
    networks:
      - proxy

networks:
  proxy:
    external:
      name: traefik_webgateway
```

