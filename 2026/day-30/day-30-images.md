
Challenge Tasks
Task 1: Docker Images

 
Pull the nginx, ubuntu, and alpine images from Docker Hub
docker pull nginx
docker pull ubuntu
docker pull alpine
REPOSITORY   TAG       IMAGE ID       SIZE
nginx        latest    abc123         180MB
ubuntu       latest    def456         72MB
alpine       latest    ghi789         7MB


List all images on your machine — note the sizes
🔹 2. List All Images + Check Size
docker images

Example output:

REPOSITORY   TAG       IMAGE ID       SIZE
nginx        latest    abc123         180MB
ubuntu       latest    def456         72MB
alpine       latest    ghi789         7MB


Compare ubuntu vs alpine — why is one much smaller?
🔹 3. Compare Ubuntu vs Alpine (Important Interview Question)
🟢 Ubuntu Image

Full OS (many packages included)

Uses glibc

Bigger size (~70MB+)

Easy for beginners

🟢 Alpine Image

Minimal OS (only essential tools)

Uses musl libc

Very small (~5–7MB)

Faster to download & deploy 


Inspect an image — what information can you see?
docker inspect nginx

Remove an image you no longer need
docker rmi alpine

Task 2: Image Layers

Run docker image history nginx — what do you see?
Each line is a layer. Note how some layers show sizes and some show 0B
Write in your notes: What are layers and why does Docker use them?
Task 2: Image Layers
🔹 1. Run the Command
docker image history nginx
🔹 2. What Do You See?

You’ll get output like:

IMAGE          CREATED        CREATED BY                     SIZE
abc123         2 weeks ago    /bin/sh -c #(nop) CMD ...      0B
def456         2 weeks ago    /bin/sh -c apt-get install     25MB
ghi789         2 weeks ago    /bin/sh -c #(nop) ADD file     0B
jkl012         2 weeks ago    base image                     70MB
Understanding the Output

Each row = one layer of the image

 Columns meaning:

CREATED BY → command used to create layer

SIZE → how much data added in that layer

Why some layers show 0B?

 Because:

Some instructions don’t add files

Example:

CMD

ENV

EXPOSE
These only store metadata → no actual size

 What Are Layers? (Write This in Notes)

Definition:

Docker image layers are read-only building blocks created from each instruction in a Dockerfile.

Example (Simple

Task 3: Container Lifecycle

Practice the full lifecycle on one container:

Create a container (without starting it) = (docker create --name my-nginx nginx)
Start the container =(docker start my-nginx0
Pause it and check status =(docker pause my-nginx),(docker ps -a)
Unpause it =(docker unpause my-nginx)
Stop it    =  (docker stop my-nginx)
Restart it  =  (docker restart my-nginx)
Kill it =  (docker kill my-nginx)
Remove it =  (docker rm my-nginx)

Check docker ps -a after each step — observe the state changes.

create → start → pause → unpause → stop → restart → kill → remove


Task 4: Working with Running Containers

Run an Nginx container in detached mode
docker run -d --name my-nginx -p 8080:80 nginx
https://localhost 8080

View its logs
docker logs my-nginx

View real-time logs (follow mode)
docker logs -f my-nginx

Exec into the container and look around the filesystem
docker exec -it my-nginx /bin/bash

 If bash not available:

docker exec -it my-nginx /bin/sh
 Explore filesystem:
ls
cd /usr/share/nginx/html
ls

Run a single command inside the container without entering it
docker exec my-nginx ls /

Inspect the container — find its IP address, port mappings, and mounts
docker inspect my-nginx
docker inspect my-nginx

Task 5: Cleanup

Stop all running containers in one command
Remove all stopped containers in one command
Remove unused images
Check how much disk space Docker is using
Stop all → docker stop $(docker ps -q)

Remove containers → docker container prune

Remove images → docker image prune -a

Check storage → docker system df


