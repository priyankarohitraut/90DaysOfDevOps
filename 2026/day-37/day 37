1. Run a container (interactive + detached)
Interactive: you go inside the container (-it)
Detached: runs in background (-d)
2. List, stop, remove containers and images

Basic Docker management:

docker ps, docker stop, docker rm, docker images, docker rmi
3. Explain image layers & caching
Docker images are built in layers
Each step in Dockerfile = one layer
Caching makes builds faster if nothing changed

👉 This is core concept, often asked in interviews

4. Write a Dockerfile from scratch

You should know:

FROM → base image
RUN → install stuff
COPY → copy files
WORKDIR → set directory
CMD → default command
5. CMD vs ENTRYPOINT
CMD → default command (can be overridden)
ENTRYPOINT → fixed command (harder to override)
6. Build and tag image
docker build -t myapp:1.0 .
7. Named volumes
Docker-managed storage
Survives container deletion
8. Bind mounts
Directly link your local folder to container
Used in development
9. Custom networks
Allow containers to talk to each other
Useful for multi-container apps
10. docker-compose.yml
Define multiple services (app + DB)
Run everything with one command
11. Environment variables & .env
Pass config like DB passwords
Keep secrets outside code
12. Multi-stage Dockerfile
Use multiple FROM
Reduce image size (important for production)
13. Push image to Docker Hub
docker push username/image
14. Healthchecks & depends_on
healthcheck → checks if container is working
depends_on → controls startup order
