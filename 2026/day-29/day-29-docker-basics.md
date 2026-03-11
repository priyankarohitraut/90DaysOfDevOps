
Challenge Tasks
Task 1: What is Docker?
Research and write short notes on:

What is a container and why do we need them?
Containers vs Virtual Machines — what's the real difference?
What is the Docker architecture? (daemon, client, images, containers, registry)
Draw or describe the Docker architecture in your own words.
1. What is a Container and Why Do We Need Them?

A container is a lightweight package that contains an application and all the things it needs to run, such as libraries, dependencies, and configuration files. Containers ensure that the application runs the same way on any system, whether it is a developer’s laptop, a test server, or a production environment. This helps solve the common problem of “it works on my machine but not on the server.” Containers are fast, portable, and use fewer resources than virtual machines.

2. Containers vs Virtual Machines

Containers and Virtual Machines both help run applications in isolated environments, but they work differently.

Virtual Machines run a full operating system on top of a hypervisor. Each VM has its own OS, which makes them heavier and slower to start.

Containers share the host operating system kernel and only package the application and its dependencies. Because of this, containers are lightweight, start quickly, and use fewer system resources.

In simple terms:

VM = Hardware-level virtualization with full OS

Container = OS-level virtualization sharing the host kernel

3. Docker Architecture

Docker follows a client-server architecture. The main components are:

Docker Client
The Docker client is the command-line interface where users run commands like docker build, docker pull, and docker run.

Docker Daemon
The Docker daemon (dockerd) runs in the background and manages Docker objects such as images, containers, networks, and volumes.

Docker Images
Images are read-only templates used to create containers. They contain the application and all required dependencies.

Docker Containers
A container is a running instance of a Docker image. It runs the application in an isolated environment.

Docker Registry
A registry is a place where Docker images are stored. The most common public registry is Docker Hub.

4. Docker Architecture (Simple Explanation)

When a user runs a Docker command, the Docker Client sends the request to the Docker Daemon. The daemon processes the request and performs actions such as building images or running containers. If the required image is not available locally, Docker pulls it from a registry like Docker Hub. After that, Docker creates and runs a container from the image.

Flow Example:

User → Docker Client → Docker Daemon → Docker Image → Docker Container
↓
Docker Registry


Task 2: Install Docker
Install Docker on your machine (or use a cloud instance)
Verify the installation
Run the hello-world container
Read the output carefully — it explains what just happened
1️⃣ Update the System
sudo apt update
2️⃣ Install Required Packages
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
3️⃣ Add Docker GPG Key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
4️⃣ Add Docker Repository
sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable"
5️⃣ Install Docker
sudo apt update
sudo apt install docker-ce -y
6️⃣ Check Docker Status
sudo systemctl status docker

You should see:

active (running)
7️⃣ Verify Docker Installation
docker --version

Example output:

Docker version 24.x.x
8️⃣ Run the Hello World Container
sudo docker run hello-world
9️⃣ Expected Output Explanation

When you run this command, Docker does the following:

1️⃣ Docker checks if the hello-world image exists locally
2️⃣ If not, it pulls the image from Docker Hub
3️⃣ Docker creates a container from the image
4️⃣ The container runs and prints a welcome message
5️⃣ The container exits after finishing

Example message:

Hello from Docker!
This message shows that your installation appears to be working correctly.
🔟 Optional (Run Docker Without sudo)

Add your user to the Docker group:

sudo usermod -aG docker $USER

Then log out and log in again.

Now you can run:

docker run hello-world

without sudo.

Example Flow (What Happened Internally)
docker run hello-world
        │
        ▼
Docker Client
        │
        ▼
Docker Daemon
        │
        ▼
Pull Image from Docker Hub
        │
        ▼
Create Container
        │
        ▼
Run Program → Print Message




Task 3: Run Real Containers
Run an Nginx container and access it in your browser
Run an Ubuntu container in interactive mode — explore it like a mini Linux machine
List all running containers
List all containers (including stopped ones)
Stop and remove a container
1️⃣ Run an Nginx Container

Pull and run the Nginx web server container:

docker run -d -p 8080:80 --name mynginx nginx

Explanation:

-d → run container in background (detached mode)

-p 8080:80 → map host port 8080 to container port 80

--name mynginx → give the container a name

nginx → Docker image name

2️⃣ Access Nginx in Browser

Open your browser and go to:

http://localhost:8080

You should see the Welcome to Nginx page.
This means the container is running successfully.

3️⃣ Run an Ubuntu Container (Interactive Mode)

Run Ubuntu like a mini Linux system:

docker run -it ubuntu bash

Explanation:

-i → interactive mode

-t → terminal access

ubuntu → image

bash → shell inside container

Now try Linux commands inside the container:

ls
pwd
whoami
apt update

Exit the container:

exit
4️⃣ List Running Containers
docker ps

Shows only currently running containers.

Example output:

CONTAINER ID   IMAGE   COMMAND   STATUS   PORTS
5️⃣ List All Containers (Including Stopped)
docker ps -a

Shows:

running containers

stopped containers

6️⃣ Stop a Container

Stop the Nginx container:

docker stop mynginx

or using container ID:

docker stop <container-id>
7️⃣ Remove a Container

Delete the container:

docker rm mynginx

If container is running, stop it first.

Quick Command Summary
Task	Command
Run Nginx container	docker run -d -p 8080:80 nginx
Run Ubuntu interactively	docker run -it ubuntu bash
List running containers	docker ps
List all containers	docker ps -a
Stop container	docker stop <name>
Remove container	docker rm <name>


Task 4: Explore

Run a container in detached mode — what's different?
Give a container a custom name
Map a port from the container to your host
Check logs of a running container
Run a command inside a running container
1️⃣ Run a Container in Detached Mode

Detached mode means the container runs in the background and your terminal is free.

Example:

docker run -d nginx

Explanation:

-d → detached mode (background)

Without -d, the container runs in the foreground and the terminal gets attached to it.

Check running containers:

docker ps
2️⃣ Give a Container a Custom Name

You can assign a name using --name.

Example:

docker run -d --name webserver nginx

Now instead of container ID you can use the name:

docker stop webserver
3️⃣ Map a Port from Container to Host

Port mapping allows you to access container services from your computer.

Example:

docker run -d -p 8080:80 --name mynginx nginx

Meaning:

Host Port     Container Port
8080    →     80

Open browser:

http://localhost:8080

You will see the Nginx welcome page.

4️⃣ Check Logs of a Running Container

Logs show the output produced by a container.

Example:

docker logs mynginx

To follow logs in real time:

docker logs -f mynginx
5️⃣ Run a Command Inside a Running Container

Use docker exec.

Example:

docker exec -it mynginx bash

Now you are inside the container shell.

Try commands:

ls
pwd
ps aux

Exit container shell:

exit
Quick Summary
Task	Command
Run container in background	docker run -d nginx
Give custom name	--name mycontainer
Map ports	-p 8080:80
Check logs	docker logs container_name
Run command in container	docker exec -it container bash



