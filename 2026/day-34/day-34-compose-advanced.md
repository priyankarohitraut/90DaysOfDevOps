
Task 1: Build Your Own App Stack
Create a docker-compose.yml for a 3-service stack:

A web app (use Python Flask, Node.js, or any language you know)
A database (Postgres or MySQL)
A cache (Redis)
Write a simple Dockerfile for the web app. The app doesn't need to be complex — even a "Hello World" that connects to the database is enough.
Create Folder
mkdir app-stack
cd app-stack
🔹 2. Create app.py
nano app.py
from flask import Flask
import psycopg2
import redis

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Flask + Postgres + Redis "

@app.route('/db')
def db():
    try:
        conn = psycopg2.connect(
            host="db",
            database="mydb",
            user="user",
            password="password"
        )
        return "Connected to Postgres "
    except Exception as e:
        return str(e)

@app.route('/cache')
def cache():
    try:
        r = redis.Redis(host='redis', port=6379)
        r.set('message', 'Hello from Redis!')
        return r.get('message').decode()
    except Exception as e:
        return str(e)

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
🔹 3. Create requirements.txt
nano requirements.txt
flask
psycopg2-binary
redis
🔹 4. Create Dockerfile
nano Dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
🔹 5. Create docker-compose.yml
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
    volumes:
      - pgdata:/var/lib/postgresql/data

  redis:
    image: redis:alpine

volumes:
  pgdata:
🔹 6. Build & Run
docker compose up -d --build
🔹 7. Test in Browser

 Open:

http://localhost:5000

✔ Output:

Hello from Flask + Postgres + Redis 
🔹 Test DB
http://localhost:5000/db

✔ Should show:

Connected to Postgres 
🔹 Test Redis
http://localhost:5000/cache

✔ Should show:

Hello from Redis!


Task 2: depends_on & Healthchecks
Add depends_on to your compose file so the app starts after the database
Add a healthcheck on the database service
Use depends_on with condition: service_healthy so the app waits for the database to be truly ready, not just started
Test: Bring everything down and up — does the app wait for the DB?
1. Update docker-compose.yml

Replace your file with this 

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

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - pgdata:/var/lib/postgresql/data

    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 5s
      timeout: 5s
      retries: 5

  redis:
    image: redis:alpine

volumes:
  pgdata:
🔹 2. What We Added
 depends_on with condition
depends_on:
  db:
    condition: service_healthy

 Means:

Web will wait until DB is healthy, not just started

Healthcheck for Postgres
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U user"]

 This command checks:

Is DB ready to accept connections?

🔹 3. Test It
Step 1: Bring Down Everything
docker compose down -v
Step 2: Start Again
docker compose up


Task 3: Restart Policies
Add restart: always to your database service
Manually kill the database container — does it come back?
Try restart: on-failure — how is it different?
Write in your notes: When would you use each restart policy?
1. Add Restart Policy (Database)

Update your docker-compose.yml 

services:
  db:
    image: postgres:13
    restart: always   #  add this
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - pgdata:/var/lib/postgresql/data
🔹 2. Start Services
docker compose up -d
🔹 3. Kill the Database Container

 First find container name:

docker compose ps

 Then kill:

docker kill app-stack-db-1
🔹 4. Observe Behavior
docker ps

✔ You’ll see:

Container automatically comes back UP 

 Why This Happens

 Because:

restart: always

✔ Docker automatically restarts container if it stops

🔹 5. Try on-failure

Update:

restart: on-failure
Repeat Test
docker compose up -d
docker kill app-stack-db-1


Task 4: Custom Dockerfiles in Compose
Instead of using a pre-built image for your app, use build: in your compose file to build from a Dockerfile
Make a code change in your app
Rebuild and restart with one command
1. Use build: in docker-compose.yml

Update your web service 

services:
  web:
    build: .        #  build from Dockerfile in current folder
    ports:
      - "5000:5000"
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started

  db:
    image: postgres:13
    restart: always
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - pgdata:/var/lib/postgresql/data

    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 5s
      timeout: 5s
      retries: 5

  redis:
    image: redis:alpine

volumes:
  pgdata:
🔹 2. Build & Run First Time
docker compose up -d --build

✔ This will:

Build your app image from Dockerfile

Start all services

🔹 3. Make a Code Change

Edit app.py:

nano app.py

 Change:

return "Hello from Flask + Postgres + Redis 🚀"

 To:

return "Updated App "
🔹 4. Rebuild & Restart (One Command)
docker compose up -d --build

✔ This will:

Detect change

Rebuild image

Restart container

🔹 5. Verify

Open:

http://localhost:5000

✔ You’ll see updated message

Task 5: Named Networks & Volumes
Define explicit networks in your compose file instead of relying on the default
Define named volumes for database data
Add labels to your services for better organization
1. Updated docker-compose.yml
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
    networks:
      - app-network
    labels:
      app: "flask-app"
      tier: "frontend"

  db:
    image: postgres:13
    restart: always
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - app-network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 5s
      timeout: 5s
      retries: 5
    labels:
      app: "flask-app"
      tier: "database"

  redis:
    image: redis:alpine
    networks:
      - app-network
    labels:
      app: "flask-app"
      tier: "cache"

# 🔹 Named Volumes
volumes:
  pgdata:

# 🔹 Custom Network
networks:
  app-network:
    driver: bridge
🔹 2. Start Services
docker compose up -d --build
 What You Added
 Named Network
networks:
  app-network:

 Benefits:

Better isolation

Clear architecture

Easy scaling

 Named Volume
volumes:
  pgdata:

 Used for:

Postgres data persistence

Survives container restart

 Labels
labels:
  app: "flask-app"
  tier: "database"

 Used for:

Organization

Filtering

Monitoring tools (Prometheus, ELK)

 Verify Everything
🔹 Check Networks
docker network ls

 You’ll see:

app-stack_app-network
🔹 Inspect Network
docker network inspect app-stack_app-network
🔹 Check Volumes
docker volume ls

 You’ll see:

app-stack_pgdata
🔹 Inspect Volume
docker volume inspect app-stack_pgdata
🔹 Check Labels
docker inspect app-stack-web-1 | grep Labels -A 10

Task 6: Scaling (Bonus)
Try scaling your web app to 3 replicas using docker compose up --scale
What happens? What breaks?
Write in your notes: Why doesn't simple scaling work with port mapping?
1. Scale Web Service
docker compose up -d --scale web=3
🔹 2. What Happens?

Run:

docker compose ps

 You’ll see:

web-1   running
web-2   running
web-3   running

✔ 3 containers created 

 But Here’s the Problem

 Only ONE container works on browser

http://localhost:5000
