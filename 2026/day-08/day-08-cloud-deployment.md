Launch a cloud instance (AWS EC2 or Utho)
1. sing in to aws
2. luanch instance
3. launch
4. connect to instance


Connect via SSH
Instance is Running

 You have the .pem key file

 You know the Public IPv4 address

 Port 22 (SSH) is allowed in Security Group

EC2 → Instance → Security → Security Group → Inbound rules



Install Nginx

update system
sudo apt update

install nginx
sudo apt install nginx

Configure security groups for web access (port 80 by default for nginx)
Extract and save logs to a file
Verify your webpage is accessible from the internet
This is real DevOps work - exactly what you'll do in production.

Expected Output
By the end of today, you should have:

A markdown file named: day-08-cloud-deployment.md
Screenshots showing:
SSH connection to your server
Nginx welcome page accessible from browser
Log file contents
The log file: nginx-logs.txt
Prerequisites
AWS account (Free Tier) OR Utho account
Basic understanding of Linux commands (Days 1-7)
SSH client (Terminal on Mac/Linux, PuTTY on Windows)
Guidelines
Part 1: Launch Cloud Instance & SSH Access (15 minutes)
Step 1: Create a Cloud Instance

Step 2: Connect via SSH

Part 2: Install Docker & Nginx (20 minutes)
Step 1: Update System

Step 3: Install Nginx

Verify Nginx is running:

Part 3: Security Group Configuration (10 minutes)
Test Web Access: Open browser and visit: http://<your-instance-ip>

You should see the Nginx welcome page!

📸 Screenshot this page - you'll need it for submission

Part 4: Extract Nginx Logs (15 minutes)
Step 1: View Nginx Logs
       log cheak

Step 2: Save Logs to File
        
Step 3: Download Log File to Your Local Machine

