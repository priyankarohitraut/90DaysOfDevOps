
Challenge Tasks

Task 1: The Problem with Large Images

Write a simple Go, Java, or Node.js app (even a "Hello World" is fine)
Create a Dockerfile that builds and runs it in a single stage
Build the image and check its size
Note down the size — you'll compare it later.
1. Create Project
mkdir large-image-demo
cd large-image-demo
🔹 2. Create Simple App
nano app.js
console.log("Hello from Node.js 🚀");
🔹 3. Create package.json
nano package.json
{
  "name": "demo-app",
  "version": "1.0.0",
  "main": "app.js"
}
🔹 4. Create Dockerfile (Single Stage )
nano Dockerfile
FROM node:18

WORKDIR /app

COPY . .

CMD ["node", "app.js"]
🔹 5. Build Image
docker build -t node-demo:v1 .
🔹 6. Check Image Size
docker images

 Example output:

node-demo   v1   900MB+ 




Task 2: Multi-Stage Build
Rewrite the Dockerfile using multi-stage build:
Stage 1: Build the app (install dependencies, compile)
Stage 2: Copy only the built artifact into a minimal base image (alpine, distroless, or scratch)
Build the image and check its size again
Compare the two sizes
Write in your notes: Why is the multi-stage image so much smaller?
1. Rewrite Dockerfile (Multi-Stage)
nano Dockerfile

 Replace with this:

# 🔹 Stage 1: Build Stage
FROM node:18 AS builder

WORKDIR /app

COPY package.json .
RUN npm install

COPY . .

# 🔹 Stage 2: Runtime Stage (lightweight)
FROM node:18-alpine

WORKDIR /app

# Copy only required files from builder
COPY --from=builder /app /app

CMD ["node", "app.js"]
🔹 2. Build Image Again
docker build -t node-demo:v2 .
🔹 3. Check Image Size
docker images


Task 3: Push to Docker Hub
Create a free account on Docker Hub (if you don't have one)
Log in from your terminal
Tag your image properly: yourusername/image-name:tag
Push it to Docker Hub
Pull it on a different machine (or after removing locally) to verify
1. Rewrite Dockerfile (Multi-Stage)
nano Dockerfile

 Replace with this:

# 🔹 Stage 1: Build Stage
FROM node:18 AS builder

WORKDIR /app

COPY package.json .
RUN npm install

COPY . .

# 🔹 Stage 2: Runtime Stage (lightweight)
FROM node:18-alpine

WORKDIR /app

# Copy only required files from builder
COPY --from=builder /app /app

CMD ["node", "app.js"]
🔹 2. Build Image Again
docker build -t node-demo:v2 .
🔹 3. Check Image Size
docker images

Task 4: Docker Hub Repository
Go to Docker Hub and check your pushed image
Add a description to the repository
Explore the tags tab — understand how versioning works
Pull a specific tag vs latest — what happens?
1. Check Your Image on Docker Hub

👉 Go to:
https://hub.docker.com

👉 Click:

Repositories

Open your repo (e.g., rohit123/node-demo)

✔ You should see:

Image name

Tags (like v2, latest)

Pull command

🔹 2. Add Description

👉 Inside your repo:

Click Edit

Add description like:

This is a sample Node.js app built using Docker multi-stage build.
It demonstrates image optimization and deployment.

✔ Save ✅

🔹 3. Explore Tags Tab

👉 Click Tags

You’ll see something like:

Tag	Meaning
v1	Old version
v2	Updated version
latest	Default version
🧠 How Versioning Works

👉 Tags = versions of your image

Example:

rohit123/node-demo:v1
rohit123/node-demo:v2
rohit123/node-demo:latest
🔹 4. Pull Specific Tag
docker pull rohit123/node-demo:v2

✔ Pulls exact version

🔹 5. Pull Latest Tag
docker pull rohit123/node-demo:latest

✔ Pulls default/latest version


Task 5: Image Best Practices
Apply these to one of your images and rebuild:

Use a minimal base image (alpine vs ubuntu — compare sizes)
Don't run as root — add a non-root USER in your Dockerfile
Combine RUN commands to reduce layers
Use specific tags for base images (not latest)
Check the size before and aft

🔹 2. New Optimized Dockerfile (After )
FROM node:18-alpine

WORKDIR /app

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copy only required files first (better caching)
COPY package.json .

# Install dependencies
RUN npm install && npm cache clean --force

# Copy app code
COPY . .

# Change ownership
RUN chown -R appuser:appgroup /app

# Switch to non-root user
USER appuser

CMD ["node", "app.js"]
 What We Improved
1. Minimal Base Image
FROM node:18-alpine

 Much smaller than node:18

 2. Non-root User
USER appuser

 Improves security 

 3. Combined RUN Commands
RUN npm install && npm cache clean --force

 Fewer layers → smaller image

4. Specific Tag
node:18-alpine   
node:latest      

 Ensures stable builds

🔹 3. Build New Image
docker build -t node-demo:v3 .
🔹 4. Compare Sizes
docker images
