
Challenge Tasks

Task 1: Pick Your App

Choose one of these (or use your own project):

A Python Flask/Django app with a database
A Node.js Express app with MongoDB
A static website served by Nginx with a backend API
Any app from your GitHub that doesn't have Docker yet
If you don't have an app, clone a simple open-source one and Dockerize it.
Step 1: Create Project Folder
mkdir devops-project
cd devops-project
🔹 Step 2: Create Simple Flask App
nano app.py
from flask import Flask
import psycopg2
import redis

app = Flask(__name__)

@app.route('/')
def home():
    return "DevOps Project 🚀"

@app.route('/db')
def db():
    try:
        conn = psycopg2.connect(
            host="db",
            database="mydb",
            user="user",
            password="password"
        )
        return "DB Connected ✅"
    except Exception as e:
        return str(e)

@app.route('/cache')
def cache():
    try:
        r = redis.Redis(host='redis', port=6379)
        r.set('msg', 'Hello Cache')
        return r.get('msg').decode()
    except:
        return "Redis Error"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
🔹 Step 3: Requirements
nano requirements.txt
flask
psycopg2-binary
redis
🔹 Step 4: Dockerfile
nano Dockerfile
FROM python:3.9-alpine

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
🔹 Step 5: Docker Compose
nano docker-compose.yml
services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db
      - redis

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password

  redis:
    image: redis:alpine
🔹 Step 6: Run Project
docker compose up -d --build
🔹 Step 7: Test

Open browser:

http://localhost:5000
http://localhos:5000db
http://localhost:5000cache


Task 2: Write the Dockerfile
Create a Dockerfile for your application
Use a multi-stage build if applicable
Use a non-root user
Keep the image small — use alpine or slim base images
Add a .dockerignore file
Build and test it locally.
1. Final Project Structure
devops-project/
├── app.py
├── requirements.txt
├── Dockerfile
└── .dockerignore
🔹 2. Optimized Dockerfile (Best Practice ✅)
# 🔹 Stage 1: Build dependencies
FROM python:3.9-alpine AS builder

WORKDIR /app

# Install build dependencies
RUN apk add --no-cache gcc musl-dev libffi-dev

COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

# 🔹 Stage 2: Final lightweight image
FROM python:3.9-alpine

WORKDIR /app

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copy installed packages from builder
COPY --from=builder /install /usr/local

# Copy app code
COPY . .

# Change ownership
RUN chown -R appuser:appgroup /app

# Switch user
USER appuser

# Run app
CMD ["python", "app.py"]
🔹 3. Create .dockerignore
nano .dockerignore
__pycache__
*.pyc
*.pyo
*.pyd
.env
.git
.gitignore
node_modules
🔹 4. Build Image
docker build -t flask-app:v1 .
🔹 5. Run Container
docker run -p 5000:5000 flask-app:v1
🔹 6. Test

Open browser:

http://localhost:5000



Task 3: Add Docker Compose
Write a docker-compose.yml that includes:

Your app service (built from Dockerfile)
A database service (Postgres, MySQL, MongoDB — whatever your app needs)
Volumes for database persistence
A custom network
Environment variables for configuration (use .env file)
Healthchecks on the database
Run docker compose up and verify everything works together.
1. Project Structure
devops-project/
├── app.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
└── .env
🔹 2. Create .env File
nano .env
POSTGRES_DB=mydb
POSTGRES_USER=user
POSTGRES_PASSWORD=password
🔹 3. docker-compose.yml
services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started
    environment:
      DB_HOST: db
      DB_NAME: ${POSTGRES_DB}
      DB_USER: ${POSTGRES_USER}
      DB_PASS: ${POSTGRES_PASSWORD}
      REDIS_HOST: redis
    networks:
      - app-network

  db:
    image: postgres:13
    restart: always
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - app-network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER}"]
      interval: 5s
      timeout: 5s
      retries: 5

  redis:
    image: redis:alpine
    networks:
      - app-network

# 🔹 Named Volume
volumes:
  pgdata:

# 🔹 Custom Network
networks:
  app-network:
    driver: bridge
🔹 4. Update app.py (Important)

Make sure it uses environment variables:

import os

conn = psycopg2.connect(
    host=os.getenv("DB_HOST"),
    database=os.getenv("DB_NAME"),
    user=os.getenv("DB_USER"),
    password=os.getenv("DB_PASS")
)
🔹 5. Run Everything
docker compose down
docker compose up -d --build
🔹 6. Verify
 Check containers
docker compose ps

 Should show:

web     Up   0.0.0.0:5000->5000/tcp
db      Up (healthy)
redis   Up
Test in browser
http://localhost:5000
http://localhost:5000/db
http://localhost:5000/cache
 Check logs
docker compose logs -f web




Task 4: Ship It
Tag your app image
Push it to Docker Hub
Share the Docker Hub link
Write a README.md in your project with:
What the app does
How to run it with Docker Compose
Any environment variables needed
1. Tag Your Image

 Replace yourusername with your Docker Hub username

docker tag devops-project-web yourusername/devops-flask-app:v1
🔹 2. Push to Docker Hub
docker push yourusername/devops-flask-app:v1

✔ After push, you’ll get repo like:

https://hub.docker.com/r/yourusername/devops-flask-app

 This is your shareable link

🔹 3. Create README.md
nano README.md
i will created 

🔹 4. Push Code to GitHub
git add .
git commit -m "final devops project"
git push



Task 5: Test the Whole Flow
Remove all local images and containers

Pull from Docker Hub and run using only your compose file

Does it work fresh? If not — fix it until it does
Task 5: Test the Whole Flow
Remove all local images and containers

Pull from Docker Hub and run using only your compose file

Does it work fresh? If not — fix it until it does
1. Clean Everything (Simulate Fresh Machine)

 This removes all containers & images

docker compose down -v
docker system prune -a

 Type y to confirm

🔹 2. Verify Clean State
docker ps -a
docker images

 Should be almost empty 

🔹 3. Pull Image from Docker Hub
docker pull yourusername/devops-flask-app:v1

✔ Image downloaded

🔹 4. Update docker-compose.yml (IMPORTANT)

 Replace build: . with image: 

web:
  image: yourusername/devops-flask-app:v1
  ports:
    - "5000:5000"
  depends_on:
    db:
      condition: service_healthy
    redis:
      condition: service_started
  environment:
    DB_HOST: db
    DB_NAME: ${POSTGRES_DB}
    DB_USER: ${POSTGRES_USER}
    DB_PASS: ${POSTGRES_PASSWORD}
    REDIS_HOST: redis
🔹 5. Ensure .env Exists
nano .env
POSTGRES_DB=mydb
POSTGRES_USER=user
POSTGRES_PASSWORD=password
🔹 6. Run Full Stack
docker compose up -d
🔹 7. Verify Everything
 Check containers
docker compose ps

 Should show:

web     Up
db      Up (healthy)
redis   Up
 Test in browser
http://localhost:5000
http://localhost:5000/db
http://localhost:5000/cache






