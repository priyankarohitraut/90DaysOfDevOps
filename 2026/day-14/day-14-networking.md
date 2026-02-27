Identify one listening port from ss -tulpn (e.g., SSH on 22 or a local web app).
One Listening Port:
Port 22 – SSH (sshd service)
22 → Port number
sshd → Service name (SSH server)
LISTEN → Means system is waiting for incoming connections
Another example:
Copy code

tcp   LISTEN  0   511   127.0.0.1:8080   0.0.0.0:*   users:(("node",pid=5678,fd=21))


From the same machine, test it: nc -zv localhost <port> (or curl -I http://localhost:<port>).
2. 1️⃣ Test using nc (Netcat)
3. Bash
4. Copy code
5. nc -zv localhost 22
6. Example output:
7. Copy code
8. 
9. Connection to localhost 22 port [tcp/ssh] succeeded!
Meaning:
11. -z → Just scan, don’t send data
12. -v → Verbose (show result)
13. If “succeeded” → Port is OPEN and listening
14. For web app:
15. Bash
16. Copy code
17. nc -zv localhost 8080
2. 
18.  Test using curl (for web applications only)
19. Bash
20. Copy code
21. curl -I http://localhost:8080
22. Example output:
23. Copy code
24. 
25. HTTP/1.1 200 OK
26. Server: nginx

Write one line: is it reachable? If not, what’s the next check? (e.g., service status, firewall).
Yes, it is reachable if nc -zv localhost <port> shows “succeeded”; if not, next check the service status using systemctl status <service> and verify firewall rules using sudo firewall-cmd --list-all or sudo ufw status.






