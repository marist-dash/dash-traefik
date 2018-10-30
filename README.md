# dash-traefik
Reverse proxy with SSL for Docker services

[https://docs.traefik.io/user-guide/docker-and-lets-encrypt/](https://docs.traefik.io/user-guide/docker-and-lets-encrypt/)

## Instructions
1) Create the Dash network (if not created already) `docker network create dash-net`

2) Pull the Traefik Docker image and start the server
    - `docker-compose up -d`
    
3) To hook a service running in Docker to Traefik
    - Add a `labels` object in the service's `docker-compose.yml` or `production.yml`
    - Add the following options under `labels`
```
labels:
  - "traefik.backend=<your_service_name>"
  - "traefik.docker.network=dash-net"
  - "traefik.enable=true"
  - "traefik.frontend.rule=Host:<subdomain>.maristdash.tk"
  - "traefik.port=<service_port>"
```

Example: `production.yml` in dash-parse
```
labels:
  - "traefik.enable=true"
  - "traefik.port=8081"
  - "traefik.frontend.rule=Host:dash-parse.maristdash.tk"
  - "traefik.docker.network=dash-net"
  - "traefik.backend=dash-parse"
```

