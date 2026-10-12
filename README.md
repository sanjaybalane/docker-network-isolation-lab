# Docker Network Isolation Lab

A small lab showing how Docker networks separate containers.

## Idea

User -> frontend -> backend -> database

The backend container is the only one attached to both networks, so it is the bridge between them.

## Setup

frontend network: frontendapp + backendapp
backend network: backendapp + db

## Commands used

```bash
docker network create frontend
docker network create backend
docker run -d --name backendapp --network frontend nginx
docker run -d --name frontendapp --network frontend nginx
docker network connect backend backendapp
docker network ls
docker network inspect frontend
```

Then inside frontendapp:

```bash
apt update && apt install -y iputils-ping
ping -c 2 db
```

## Result

From frontendapp the name db does not resolve. See screenshots/.

## Limitations

No db container was created in this run, so the failed ping alone does not prove isolation.

Next steps:
1. Start the database: docker run -d --name db --network backend nginx
2. Retest ping from frontendapp (should still fail).
3. Connect frontendapp to the backend network and show the isolation breaks.

## Reference

https://docs.docker.com/reference/cli/docker/network/
