

Mini Task: Port Probe & Interpret

Identify one listening port from ss -tulpn (e.g., SSH on 22 or a local web app).


I found this listening port:

tcp   LISTEN  0  128  0.0.0.0:22   0.0.0.0:*   users:(("sshd",pid=890))

Port 22 is listening.
It is used for SSH (Secure Shell) service.
This means the system allows remote login using SSH.



From the same machine, test it: nc -zv localhost <port> (or curl -I http://localhost:<port>).
Test Listening Port from Same Machine

I tested the listening port using nc command.

$ nc -zv localhost 22
Connection to localhost 22 port [tcp/ssh] succeeded!

This shows that port 22 (SSH) is open and reachable on my local machine.

If testing a web service (like port 80), I can use curl:

$ curl -I http://localhost:80
HTTP/1.1 200 OK
Server: nginx

This confirms the web server is running and responding.

Conclusion:
The port is active and the service is accessible locally.



Write one line: is it reachable? If not, what’s the next check? (e.g., service status, firewall).
Yes, the port is reachable; if it was not reachable, I would check whether the service is running and verify firewall settings.







