Task 1: Key-Value Pairs
Create person.yaml that describes yourself with:

name
role
experience_years
learning (a boolean)
Verify: Run cat person.yaml — does it look clean? No tabs?
Here’s a clean person.yaml example you can create:

name: Priyanka Raut
role: Cloud Engineer (Fresher)
experience_years: 0
learning: true
 Tips to verify:

Run:

cat person.yaml



Task 2: Lists
Add to person.yaml:

tools — a list of 5 DevOps tools you know or are learning
hobbies — a list using the inline format [item1, item2]
Write in your notes: What are the two ways to write a list in YAML?
Here’s your updated person.yaml with lists:

name: Priyanka Raut
role: Cloud Engineer (Fresher)
experience_years: 0
learning: true

tools:
  - Docker
  - Kubernetes
  - Git
  - Jenkins
  - Linux

hobbies: [reading, coding, music]
 Your Notes (important for interview)

Two ways to write a list in YAML:

Block style (recommended, most common)
tools:
  - Docker
  - Kubernetes
  - Git
Inline style (compact, single line)
hobbies: [reading, coding, music]
 Interview Tip
Use block style in real DevOps configs (like Kubernetes YAML)
Inline is mostly for small/simple lists

If you want, next I’ll give yo


Task 3: Nested Objects
Create server.yaml that describes a server:

server with nested keys: name, ip, port
database with nested keys: host, name, credentials (nested further: user, password)
Verify: Try adding a tab instead of spaces — what happens when you validate it?
Here’s your server.yaml with proper nested objects:

server:
  name: app-server
  ip: 192.168.1.10
  port: 8080

database:
  host: db-server
  name: myapp_db
  credentials:
    user: admin
    password: secret123
 What you learned (important)
YAML supports nested structure using indentation (spaces only)
Each level is indented with 2 spaces (best practice)


Task 4: Multi-line Strings
In server.yaml, add a startup_script field using:

The | block style (preserves newlines)
The > fold style (folds into one line)
Write in your notes: When would you use | vs >?
Here’s your updated server.yaml with multi-line strings:

server:
  name: app-server
  ip: 192.168.1.10
  port: 8080
  startup_script: |
    #!/bin/bash
    echo "Starting application..."
    docker run -d -p 8080:80 myapp:latest
    echo "Application started"

database:
  host: db-server
  name: myapp_db
  credentials:
    user: admin
    password: secret123

startup_script_folded: >
  #!/bin/bash
  echo "Starting application..."
  docker run -d -p 8080:80 myapp:latest
  echo "Application started"
 Your Notes (Interview Ready)
🔹 | Literal Style
Preserves line breaks exactly as written
Output remains multi-line
Best for:
Shell scripts
Config files
Kubernetes init scripts

 Example use: startup scripts, CI/CD steps

🔹 > Folded Style
Converts multiple lines into a single line
Line breaks become spaces
Best for:
Long messages
Descriptions
Logs
 Simple Way to Remember
| → Keep format (same lines)
> → Compress into one line



Task 5: Validate Your YAML
Install yamllint or use an online validator
Validate both your YAML files
Intentionally break the indentation — what error do you get?
Fix it and validate again
Option 1: Install yamllint (recommended)
sudo apt update
sudo apt install yamllint -y
 Step 2: Validate your files
yamllint person.yaml
yamllint server.yaml
 If everything is correct:

You’ll see no output (means YAML is valid)

 Step 3: Intentionally break indentation

Example (wrong YAML):

server:
 name: app-server   # wrong indentation (1 space instead of 2)
  ip: 192.168.1.10
 Error you’ll get:
server.yaml
  3:2       error    wrong indentation: expected 2 but found 1

OR sometimes:

error: inconsistent indentation
 Step 4: Fix it

Correct version:

server:
  name: app-server
  ip: 192.168.1.10

Run again:

yamllint server.yaml



Task 6: Spot the Difference
Read both blocks and write what's wrong with the second one:

# Block 1 - correct
name: devops
tools:
  - docker
  - kubernetes
  


Happy Learning! TrainWithShubham
