
Challenge Tasks

Task 1: Install & Verify

Check if Docker Compose is available on your machine
Verify the version
docker compose version

Task 2: Your First Compose File
Create a folder compose-basics
Write a docker-compose.yml that runs a single Nginx container with port mapping
Start it with docker compose up
Access it in your browser
Stop it with docker compose down
1. Create Project Folder
mkdir compose-basics
cd compose-basics
🔹 2. Create docker-compose.yml
nano docker-compose.yml

👉 Add this content:

version: "3.8"

services:
  nginx:
    image: nginx:alpine
    container_name: my-nginx-compose
    ports:
      - "8085:80"

Save and exit.

🔹 3. Start the Service
docker compose up -d

✔ This will:

Pull nginx image (if not present)

Create container

Start it in background

🔹 4. Verify in Browser

 Open:

http://localhost:8085

✔ You’ll see Nginx default page 

🔹 5. Check Running Containers
docker ps
🔹 6. Stop Everything
docker compose down

✔ This will:

Stop container

Remove container

Keep image


Task 3: Two-Container Setup
Write a docker-compose.yml that runs:

A WordPress container
A MySQL container
They should:

Be on the same network (Compose does this automatically)
MySQL should have a named volume for data persistence
WordPress should connect to MySQL using the service name
Start it, access WordPress in your browser, and set it up.

Verify: Stop and restart with docker compose down and docker compose up — is your WordPress data still there?
1. Create Folder
mkdir wordpress-mysql
cd wordpress-mysql
🔹 2. Create docker-compose.yml
nano docker-compose.yml

Paste this:

version: "3.8"

services:
  db:
    image: mysql:5.7
    container_name: mysql-db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppass
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wordpress-app
    depends_on:
      - db
    restart: always
    ports:
      - "8086:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppass
      WORDPRESS_DB_NAME: wordpress

volumes:
  db_data:

Save & exit.

🔹 3. Start Containers
docker compose up -d

✔ This will:

Start MySQL

Start WordPress

Create volume db_data

🔹 4. Open in Browser

 Go to:

http://localhost:8086

✔ You’ll see WordPress setup page 

🔹 5. Setup WordPress

Fill:

Site Name

Username

Password

✔ Done 

 Key Concept (Very Important)

 WordPress connects to MySQL using:

WORDPRESS_DB_HOST: db:3306

✔ db = service name
✔ Docker Compose provides DNS automatically

🔹 6. Verify Data Persistence
Stop everything:
docker compose down
Start again:
docker compose up -d

 Open again:

http://localhost:8086


Task 4: Compose Commands
Practice and document these:

Start services in detached mode
View running services
View logs of all services
View logs of a specific service
Stop services without removing
Remove everything (containers, networks)
Rebuild images if you make a change
1. Start Services in Detached Mode
docker compose up -d

✔ Runs containers in background
✔ Most commonly used command

🔹 2. View Running Services
docker compose ps

✔ Shows:

Service names

Container status

Ports

🔹 3. View Logs of All Services
docker compose logs

✔ Shows logs of all containers

🔹 4. View Logs (Real-Time / Follow)
docker compose logs -f

✔ Live logs (like tail -f)

🔹 5. View Logs of Specific Service
docker compose logs wordpress

or

docker compose logs db

✔ Useful for debugging specific container

🔹 6. Stop Services (Without Removing)
docker compose stop

✔ Containers stopped
✔ Data + containers still exist

🔹 7. Start Again After Stop
docker compose start

✔ Resumes stopped containers

🔹 8. Remove Everything (Containers + Network)
docker compose down

✔ Stops + removes:

Containers

Network

 Volume NOT removed by default

🔹 9. Remove Everything Including Volumes 
docker compose down -v

 Deletes database data also

🔹 10. Rebuild Images After Changes
docker compose up -d --build

✔ Rebuild + restart containers

Task 5: Environment Variables

Add environment variables directly in your docker-compose.yml
Create a .env file and reference variables from it in your compose file
Verify the variables are being picked up
1. Add Variables Directly in docker-compose.yml

Example:

services:
  app:
    image: nginx:alpine
    environment:
      MY_NAME: Rohit
      ENV: dev
🔹 2. Verify Variables Inside Container

Run:

docker compose up -d
docker exec -it <container_name> sh

Then:

env

 You’ll see:

MY_NAME=Rohit
ENV=dev

✔ Variables are working 

🔹 3. Use .env File (Best Practice )
Step 1: Create .env file
nano .env

 Add:

MY_NAME=Rohit
ENV=production
PORT=8087

Save & exit.

Step 2: Update docker-compose.yml
services:
  app:
    image: nginx:alpine
    ports:
      - "${PORT}:80"
    environment:
      MY_NAME: ${MY_NAME}
      ENV: ${ENV}
🔹 4. Start Services
docker compose up -d

✔ Compose automatically reads .env

🔹 5. Verify Again
docker exec -it <container_name> env

✔ Output:

MY_NAME=Rohit
ENV=production

