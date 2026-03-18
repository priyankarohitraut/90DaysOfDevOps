
Challenge Tasks

Task 1: The Problem
Run a Postgres or MySQL container
Create some data inside it (a table, a few rows — anything)
Stop and remove the container
Run a new one — is your data still there?
Write what happened and why.
Task 1: The Problem (Data Loss in Containers)
🔹 1. Run a Postgres Container
docker run -d \
--name my-postgres \
-e POSTGRES_PASSWORD=pass123 \
-p 5432:5432 \
postgres
🔹 2. Exec into Container
docker exec -it my-postgres psql -U postgres
🔹 3. Create Data

Inside PostgreSQL shell:

CREATE TABLE test (id INT, name TEXT);

INSERT INTO test VALUES (1, 'Rohit'), (2, 'DevOps');

SELECT * FROM test;

✔ You will see:

1 | Rohit
2 | DevOps

 Exit:

\q
🔹 4. Stop and Remove Container
docker stop my-postgres
docker rm my-postgres
🔹 5. Run New Container Again
docker run -d \
--name my-postgres \
-e POSTGRES_PASSWORD=pass123 \
-p 5432:5432 \
postgres
🔹 6. Check Data Again
docker exec -it my-postgres psql -U postgres
SELECT * FROM test;

Task 2: Named Volumes

Create a named volume
Run the same database container, but this time attach the volume to it
Add some data, stop and remove the container
Run a brand new container with the same volume
Is the data still there?
Verify: docker volume ls, docker volume inspect
🔹 1. Create a Named Volume
docker volume create my-postgres-data
🔹 2. Verify Volume
docker volume ls

✔ You should see:

my-postgres-data
🔹 3. Run Postgres with Volume
docker run -d \
--name my-postgres \
-e POSTGRES_PASSWORD=pass123 \
-p 5432:5432 \
-v my-postgres-data:/var/lib/postgresql/data \
postgres

👉 Important:

/var/lib/postgresql/data = default Postgres data directory

🔹 4. Add Data
docker exec -it my-postgres psql -U postgres

Inside:

CREATE TABLE test (id INT, name TEXT);
INSERT INTO test VALUES (1, 'Rohit'), (2, 'DevOps');
SELECT * FROM test;

✔ Data created

Exit:

\q
🔹 5. Stop & Remove Container
docker stop my-postgres
docker rm my-postgres
🔹 6. Run NEW Container (Same Volume)
docker run -d \
--name my-postgres \
-e POSTGRES_PASSWORD=pass123 \
-p 5432:5432 \
-v my-postgres-data:/var/lib/postgresql/data \
postgres
🔹 7. Check Data Again
docker exec -it my-postgres psql -U postgres
SELECT * FROM test;
Result

✔ Data is STILL THERE 

 id |  name
----+---------
  1 | Rohit
  2 | DevOps
 Why This Works

 Because:

Data is stored in Docker Volume

Not inside container filesystem

Volume remains even if container is deleted

🔍 Verify Volume Details
🔹 List volumes
docker volume ls
🔹 Inspect volume
docker volume inspect my-postgres-data


Task 3: Bind Mounts
Create a folder on your host machine with an index.html file
Run an Nginx container and bind mount your folder to the Nginx web directory
Access the page in your browser
Edit the index.html on your host — refresh the browser
Write in your notes: What is the difference between a named volume and a bind mount?
1. Create Folder on Host
mkdir my-bind-demo
cd my-bind-demo
🔹 2. Create index.html
nano index.html

Add:

<h1>Bind Mount Demo </h1>
<p>This is from host machine</p>

Save and exit.

🔹 3. Run Nginx with Bind Mount
docker run -d \
--name bind-nginx \
-p 8083:80 \
-v $(pwd):/usr/share/nginx/html \
nginx:alpine
🔹 4. Open in Browser

 Go to:

http://localhost:8083

✔ You will see your HTML page 

🔹 5. Edit File on Host
nano index.html

 Change content:

<h1>Updated Live </h1>
<p>Changes reflect instantly!</p>
🔹 6. Refresh Browser

 Reload:

http://localhost:8083


Task 4: Docker Networking Basics
List all Docker networks on your machine
Inspect the default bridge network
Run two containers on the default bridge — can they ping each other by name?
Run two containers on the default bridge — can they ping each other by IP?
1. List All Docker Networks
docker network ls

 You’ll see something like:

NETWORK ID     NAME      DRIVER    SCOPE
abc123         bridge    bridge    local
def456         host      host      local
ghi789         none      null      local
🔹 2. Inspect Default Bridge Network
docker network inspect bridge

 You’ll see:

Subnet (like 172.17.0.0/16)

Gateway

Connected containers

 Example:

"Subnet": "172.17.0.0/16"
🔹 3. Run Two Containers (Default Bridge)
docker run -dit --name container1 alpine sh
docker run -dit --name container2 alpine sh
🔹 4. Test Ping by Name 

Enter container1:

docker exec -it container1 sh

Now try:

ping container2

 Result:
 Will NOT work

🔹 5. Test Ping by IP 

First get IP of container2:

docker inspect container2 | grep IPAddress

Example:

172.17.0.3

Now inside container1:

ping 172.17.0.3

Task 5: Custom Networks
Create a custom bridge network called my-app-net
Run two containers on my-app-net
Can they ping each other by name now?
Write in your notes: Why does custom networking allow name-based communication but the default bridge doesn't?
1. Create Custom Network
docker network create my-app-net

✔ Network created

🔹 2. Run Two Containers on This Network
docker run -dit --name app1 --network my-app-net alpine sh
docker run -dit --name app2 --network my-app-net alpine sh
🔹 3. Test Communication by Name 

Enter app1:

docker exec -it app1 sh

Now ping:

ping app2

1. Create Custom Network
docker network create my-app-net

✔ Network created

🔹 2. Run Two Containers on This Network
docker run -dit --name app1 --network my-app-net alpine sh
docker run -dit --name app2 --network my-app-net alpine sh
🔹 3. Test Communication by Name ✅

Enter app1:

docker exec -it app1 sh

Now ping:

ping app2

