# dash-traefik
Reverse proxy with SSL for Docker services. This is intended for deployments to a Marist VM (no SSL).

[https://docs.traefik.io/user-guide/docker-and-lets-encrypt/](https://docs.traefik.io/user-guide/docker-and-lets-encrypt/)

## Instructions
1) Create the Dash network (if not created already) `docker network create dash-net`

2) Checkout the `deploy-marist` branch: `git checkout deploy-marist`

3) Pull the Traefik Docker image and start the server
    - `docker-compose up -d`
    
4) To hook a service running in Docker to Traefik
    - Add a `labels` object in the service's `docker-compose.yml` or `production.yml`
    - Add the following options under `labels`
```
labels:
  - "traefik.backend=<your_service_name>"
  - "traefik.docker.network=dash-net"
  - "traefik.enable=true"
  - "traefik.frontend.rule=Host:<subdomain>.domain.com"
  - "traefik.port=<service_port>"
```
