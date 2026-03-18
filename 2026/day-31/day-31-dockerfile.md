
Challenge Tasks

Task 1: Your First Dockerfile

Create a folder called my-first-image
Inside it, create a Dockerfile that:
Uses ubuntu as the base image
Installs curl
Sets a default command to print "Hello from my custom image!"
Build the image and tag it my-ubuntu:v1
Run a container from your image
Verify: The message prints on docker run
mkdir my-first-image
cd
vim dockerfile
FROM:ubuntu
RUN apt-get update && apt-get install -y curl

CMD ["echo", "Hello from my custom image!"]
docker build -t my-ubuntu:v1
docker run my-ubuntu:v1
docker images
curl --version

Task 2: Dockerfile Instructions

Create a new Dockerfile that uses all of these instructions:

FROM — base image
RUN — execute commands during build
COPY — copy files from host to image
WORKDIR — set working directory
EXPOSE — document the port
CMD — default command
Build and run it. Understand what each line does.
 mkdir dockerfile-demo
cd dockerfile-demo
2. Create a Sample File (to COPY)
nano index.html

<h1>Hello from Dockerfile Demo </h1>
vim dockerfile
# 1. Base image
FROM nginx

# 2. Set working directory
WORKDIR /usr/share/nginx/html

# 3. Copy file from host to container
COPY index.html .

# 4. Run command during build
RUN echo "Custom Nginx Image Built"

# 5. Document port
EXPOSE 80

# 6. Default command
CMD ["nginx", "-g", "daemon off;"]

4. Build Image
docker build -t my-nginx:v2 .
 5. Run Container
docker run -d -p 8081:80 --name demo-container my-nginx:v2
http://localhost:8081

Task 3: CMD vs ENTRYPOINT

Create an image with CMD ["echo", "hello"] — run it, then run it with a custom command. What happens?
Create an image with ENTRYPOINT ["echo"] — run it, then run it with additional arguments. What happens?
Write in your notes: When would you use CMD vs ENTRYPOINT?
Part 1: Using CMD
Create Dockerfile
mkdir cmd-demo
cd cmd-demo
nano Dockerfile

Add:

FROM ubuntu
CMD ["echo", "hello"]
 Build Image
docker build -t cmd-image:v1 .
Run Container (default)
docker run cmd-image:v1

Output:

hello
 Run with Custom Command
docker run cmd-image:v1 echo "Hi Rohit"

 Output:

Hi Rohit
Observation (IMPORTANT)

CMD is overridden by new command

Part 2: Using ENTRYPOINT
Create Dockerfile
mkdir ../entrypoint-demo
cd ../entrypoint-demo
nano Dockerfile

FROM ubuntu
ENTRYPOINT ["echo"]
🔹 Build Image
docker build -t entry-image:v1 .
🔹 Run Container
docker run entry-image:v1 hello

hello
🔹 Run with Additional Argument
docker run entry-image:v1 "Hello Rohit"

CMD provides default command and can be overridden at runtime.

ENTRYPOINT

ENTRYPOINT defines a fixed command, and any additional arguments are appended.
 When to Use What?
Use CMD when:

You want flexibility

User can change command

Example: testing, scripts

Use ENTRYPOINT when:

You want container to behave like a tool
Command should always run
Example: CLI tools, apps


Task 4: Build a Simple Web App Image
Create a small static HTML file (index.html) with any content
Write a Dockerfile that:
Uses nginx:alpine as base
Copies your index.html to the Nginx web directory
Build and tag it my-website:v1
Run it with port mapping and access it in your browser
Task 4: Build a Simple Web App Image
🔹 1. Create Project Folder
mkdir my-website
cd my-website
🔹 2. Create HTML File
nano index.html

 Add this:

<h1> Welcome to My Docker Website</h1>
<p>This is my first Nginx container!</p>

Save and exit.

🔹 3. Create Dockerfile
nano Dockerfile

 Add:

# Use lightweight base image
FROM nginx:alpine

# Copy HTML file to nginx default directory
COPY index.html /usr/share/nginx/html/

# Expose port 80
EXPOSE 80
🔹 4. Build Image
docker build -t my-website:v1 .
🔹 5. Run Container
docker run -d -p 8082:80 --name my-website-container my-website:v1
🔹 6. Access in Browser

 Open:

http://localhost:8082

Welcome to My Docker Website
This is my first Nginx container!

Task 5: .dockerignore

Create a .dockerignore file in one of your project folders
Add entries for: node_modules, .git, *.md, .env
Build the image — verify that ignored files are not included
Open Dockerfile:

nano Dockerfile

 It should look like this:

FROM nginx:alpine

COPY index.html /usr/share/nginx/html/

EXPOSE 80

🔹 2. Create / Fix .dockerignore
nano .dockerignore

 Add:

node_modules
.git
*.md
.env

✔ Save

🔹 3. Build Again
docker build -t my-website:v2 .
docker run my-website:v2

Task 6: Build Optimization

Build an image, then change one line and rebuild — notice how Docker uses cache
Reorder your Dockerfile so that frequently changing lines come last
Write in your notes: Why does layer order matter for build speed?
Build Image First Time

Use your existing project (my-website):

docker build -t my-website:v1 .

🔹 2. Change One Line

Edit index.html:

nano index.html

Change content:

<h1>Updated Website </h1>
🔹 3. Build Again
docker build -t my-website:v2 .


 Only changed layers are rebuilt
 Other layers are reused (cached) ⚡

What is Docker Cache?

Docker stores each layer after build and reuses it if nothing has changed.


 Problem:

If index.html changes → everything rebuilds 

 Good Order
FROM nginx:alpine

RUN apt-get update

COPY index.html /usr/share/nginx/html/

 Now:

If index.html changes → only last layer rebuilds ⚡

 Why Layer Order Matters

Docker builds step-by-step
 If one step changes → all steps after it rebuild

