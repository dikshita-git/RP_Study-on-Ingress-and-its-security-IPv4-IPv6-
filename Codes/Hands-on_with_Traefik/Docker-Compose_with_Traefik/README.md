In this demonstration, we try to expose a simple "whoami" service using docker-compose file. The output basically prints the details of the current used machine from which the docker-compose was built alongwith the HTTP request which was used to define the container "simple-service" as mentioned in the docker compose file.

1. Here, we declare our "Entrypoint" alongwith the port match which will help us to open and accept the HTTP traffic. The entrypoint declared was in Line 16 of the docker-compose file:
    command:
        "--entrypoints.web.address=:80"         # Traefik will listen to incoming request on the port 80 (HTTP)
        ports:
          - "80:80"                             # Line 18
2. Then in Line 13, we expose  the Traefik API to be able to check if any configuration is needed:
    command:
        "--api.insecure=true"                   # Traefik will listen on port 8080 by default for API request.
    ports:
          - "8080:8080"                         # Line 19
       
3. Next,we allow Traefik to collect the configurations from Docker. This is mentioned between  Line 14-15, 20-21, 23-29:

  traefik:
  command:
    - "--providers.docker=true"                        # Enabling docker provider
    - "--providers.docker.exposedbydefault=false"      # Do not expose containers unless explicitly told so
    
  volumes:
    - "/var/run/docker.sock:/var/run/docker.sock:ro"

whoami:
  labels:
    - "traefik.enable=true"                                                     # Explicitly tell Traefik to expose this container
    - "traefik.http.routers.whoami.rule=Host(`whoami.localhost`)"               # The domain the service will respond to
    - "traefik.http.routers.whoami.entrypoints=web"                             # Allow request only from the predefined entry point named "web"
