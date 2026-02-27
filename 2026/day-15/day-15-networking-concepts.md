
## Task 1: DNS – How Names Become IPs

### What happens when you type google.com in a browser?

When I type `google.com` in a browser, my system first checks the local DNS cache.
If not found, it sends a request to a DNS server.
The DNS server returns the IP address of google.com.
Then the browser connects to that IP address and loads the website.

---

### DNS Record Types

* **A** – Maps a domain name to an IPv4 address
* **AAAA** – Maps a domain name to an IPv6 address
* **CNAME** – Points one domain name to another domain name
* **MX** – Defines mail server for receiving emails
* **NS** – Defines the authoritative name servers for a domain

---

### Output of `dig google.com`

```bash
$ dig google.com

;; ANSWER SECTION:
google.com.     120     IN      A       142.250.183.14
```

A Record: **142.250.183.14**
TTL: **120 seconds**

---

## Task 2: IP Addressing

### What is an IPv4 address?

An IPv4 address is a 32-bit number used to identify devices on a network.
It has 4 parts separated by dots.
Example: `192.168.1.10`
Each part ranges from 0 to 255.

---

### Public vs Private IP

* **Public IP** – Used on the internet and globally unique
  Example: `8.8.8.8`

* **Private IP** – Used inside local networks
  Example: `192.168.1.10`

---

### Private IP Ranges

* `10.0.0.0 – 10.255.255.255`
* `172.16.0.0 – 172.31.255.255`
* `192.168.0.0 – 192.168.255.255`

---

### Output of `ip addr show`

```bash
$ ip addr show

2: enp0s3: <BROADCAST,MULTICAST,UP>
    inet 192.168.1.5/24 brd 192.168.1.255 scope global dynamic
```

My private IP is: **192.168.1.5**

---

## Task 3: CIDR & Subnetting

### What does /24 mean in 192.168.1.0/24?

It means the first 24 bits are used for the network part.
Remaining bits are used for host addresses.

---

### Usable Hosts

* /24 → 254 usable hosts
* /16 → 65,534 usable hosts
* /28 → 14 usable hosts

---

### Why do we subnet?

We subnet to divide a large network into smaller networks.
It improves security and reduces network traffic.
It also helps better IP address management.

---

### CIDR Table

| CIDR | Subnet Mask     | Total IPs | Usable Hosts |
| ---- | --------------- | --------- | ------------ |
| /24  | 255.255.255.0   | 256       | 254          |
| /16  | 255.255.0.0     | 65,536    | 65,534       |
| /28  | 255.255.255.240 | 16        | 14           |

---

## Task 4: Ports – The Doors to Services

### What is a port?

A port is a logical communication endpoint.
It helps identify which service or application should receive the traffic.

---

### Common Ports

| Port  | Service |
| ----- | ------- |
| 22    | SSH     |
| 80    | HTTP    |
| 443   | HTTPS   |
| 53    | DNS     |
| 3306  | MySQL   |
| 6379  | Redis   |
| 27017 | MongoDB |

---

### Output of `ss -tulpn`

```bash
$ ss -tulpn

tcp   LISTEN  0  128  0.0.0.0:22      0.0.0.0:*   users:(("sshd",pid=890))
tcp   LISTEN  0  80   0.0.0.0:80      0.0.0.0:*   users:(("nginx",pid=1100))
```

Port 22 → SSH
Port 80 → Nginx (HTTP service)

---

## Task 5: Putting It Together

### curl [http://myapp.com:8080](http://myapp.com:8080) – what concepts are involved?

DNS resolves myapp.com to an IP.
Port 8080 is used for application service.
The request uses TCP/IP to connect and get response.

---

### App can't reach database at 10.0.1.50:3306 – what to check?

First check if IP is reachable (ping).
Then check if port 3306 is open.
Also check firewall rules and database service status.

---

## What I Learned (Key Points)

1. DNS converts domain names into IP addresses.
2. CIDR helps divide networks efficiently.
3. Ports allow multiple services to run on same IP.

---
