Mini Self-Check (write short answers in day-12-revision.md)

Which 3 commands save you the most time right now, and why?

1️⃣ history – My Shortcut Reminder

Why it saves time:

Shows all previously executed commands

I don’t need to remember long Docker / Git / Linux commands

I can reuse them quickly

2️2️⃣ ls -l – Quick File Check

Why it saves time:

Instantly shows file permissions

Shows owner and group

Helps in troubleshooting permission issues

3️⃣ grep – Find Anything Fast

Why it saves time:

Search inside files

Filter command output

Used in logs and debugging

How do you check if a service is healthy? List the exact 2–3 commands you’d run first.

1️⃣ systemctl status <service>

This is the first and most important command.

systemctl status nginx

What it tells me:

Is the service active or inactive

Is it running or failed

Recent error messages

PID and uptime

2️⃣ journalctl -u <service> -n 20

Check the recent logs of the service.

journalctl -u nginx -n 20

What it tells me:

Startup errors

Permission issues

 3️⃣ ss -tulnp | grep <port>

Check if service is actually listening on expected port.

ss -tulnp | grep 80

What it tells me:

Is the application really running?

Is the correct port open?

Which process is using the port?

Config errors



How do you safely change ownership and permissions without breaking access? Give one example command.
1️⃣ Check current permissions first
ls -l file.txt

Never change blindly. First see:

Current owner

Current group

Current permission bits

2️⃣ Confirm which user/service needs access

For example:
If the service runs as nginx user, files should belong to nginx or its group.

3️⃣ Change ownership carefully (minimal change)

Instead of giving 777 
I give only required access 


What will you focus on improving in the next 3 days?
1️⃣ Linux Troubleshooting Basics

Since most DevOps work happens on Linux servers, I want to get more confident with:

systemctl

journalctl

ps, top

netstat / ss

File permissions & ownership

2️⃣ Git Practice (Real Workflow)

Instead of just commands, I want to practice:

Branching properly

Merging & resolving conflicts

Pull requests workflow (locally simulate)

3️⃣ Docker Fundamentals

Since DevOps heavily uses containers, I want to improve:

Writing a simple Dockerfile

Running containers

Understanding port mapping

Checking container logs



