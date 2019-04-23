This project comes from a need to understand Docker Swarm and the requirement
that 'stacks' be used. The most reproducible form of stack creation is the
Docker stack create command referencing docker-compose files, but there are few
examples today of such files that interoperate with the Traefik reverse-proxy
utility.

The project will procede through a number of iterations that will progressively
add improved Docker functionality and incorporate security, such as through
Docker secrets.

# Once-only environment setup

<<<<<<< HEAD
Your docker needs to be in swarm mode. Create a single node swarm with this command:
````
docker swarm init --advertise-addr 10.117.xxx.yyy
````

An overlay network called traefik-net needs to be present. You only need to do this once:
````
docker network create --driver=overlay traefik-net
````

# To run the stacks
=======
```
docker service create --name traefik --constraint=node.role==manager --publish 80:80 --publish 443:443 --publish 8080:8080 --mount type=bind,source=$PWD/traefik.key,target=/traefik.key --mount type=bind,source=$PWD/traefik.crt,target=/traefik.crt --mount type=bind,source=/var/run/docker.sock,target=/var/run/docker.sock --mount type=bind,source=$PWD/traefik.toml,target=/etc/traefik/traefik.toml --network traefik-net traefik --docker --docker.swarmMode --docker.domain=traefik --docker.watch --api
```
>>>>>>> 9d85152cbbcc7c3b8516c2696d7154806e6715d6

Create the traefik reverse-proxy service with the following command:
````
docker service create --name traefik --constraint=node.role==manager --publish 80:80 --publish 443:443 --publish 8080:8080 --mount type=bind,source=$PWD/traefik.key,target=/traefik.key --mount type=bind,source=$PWD/traefik.crt,target=/traefik.crt --mount type=bind,source=/var/run/docker.sock,target=/var/run/docker.sock --mount type=bind,source=$PWD/traefik.toml,target=/etc/traefik/traefik.toml --network traefik-net traefik --docker --docker.swarmMode --docker.domain=traefik --docker.watch --api
````
Traefik is now ready to direct traffic to any properly configured stack. Deploy
any combination of stack samples e.g.:
````
docker stack deploy -c whoami.yml whoami
docker stack deploy -c wordpress.yml wordpress
````
