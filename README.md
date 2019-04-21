This project comes from a need to understand Docker Swarm and the requirement
that 'stacks' be used. The most reproducible form of stack creation is the
Docker stack create command referencing docker-compose files, but there are few
examples today of such files that interoperate with the Traefik reverse-proxy
utility.

The project will procede through a number of iterations that will progressively
add improved Docker functionality and incorporate security, such as through
Docker secrets.

Create the traefik service with the following command:

"docker service create \
    --name traefik \
    --constraint=node.role==manager \
    --publish 80:80 --publish 8080:8080 \
    --mount type=bind,source=/var/run/docker.sock,target=/var/run/docker.sock \
    --network traefik-net \
    traefik \
    --docker \
    --docker.swarmMode \
    --docker.domain=traefik \
    --docker.watch \
    --api"

  and then launch a stack (yml file) with a regular docker stack command.
