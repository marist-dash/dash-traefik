# dash-traefik
Reverse proxy with SSL for Docker services

[https://docs.traefik.io/user-guide/docker-and-lets-encrypt/](https://docs.traefik.io/user-guide/docker-and-lets-encrypt/)

## Instructions
1) Create the Dash network (if not created already) `docker network create dash-net`

2) Create the directory where Traefik will be configured
    - `mkdir -p /opt/traefik`
    
3) Create three files required for Traefik and Let's Encrypt
    - `touch /opt/traefik/docker-compose.yml`
    - `touch /opt/traefik/acme.json && chmod 600 /opt/traefik/acme.json`
    - `touch /opt/traefik/traefik.toml`

4) Configure `docker-compose.yml` according to [docker-compose.yml](https://github.com/marist-dash/dash-traefik/blob/master/docker-compose.yml)

5) Configure `traefik.toml` according to [traefik.toml](https://github.com/marist-dash/dash-traefik/blob/master/traefik.toml)

6) Pull the Traefik Docker image and start the server
    - `docker-compose up -d`
    
7) To hook a service running in Docker to Traefik
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

