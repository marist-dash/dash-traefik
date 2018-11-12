# dash-traefik
Reverse proxy with SSL for Docker services. This is intended for deployments to a Marist VM. An instance of Portainer starts with Traefik.

[https://docs.traefik.io/](https://docs.traefik.io/)

## Instructions
1) Create the Dash network (if not created already) `docker network create dash-net`

2) Add the digital certificate and private key (named `traefik.crt` and `private.key`, respectively) to the `ssl/` directory

3) Start the services
    - `docker-compose up -d`
    
3) To hook a service running in Docker to Traefik
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
