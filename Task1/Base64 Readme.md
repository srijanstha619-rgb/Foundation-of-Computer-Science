#  Task 1 – Base64 Encoding and File Transfer Using Docker

##  Module: Foundation of Computer Science  
##  Topic: Enhancing Secure Data Exchange with Encoding Formats and Protocol Integration  

---


## Overview

This task demonstrates secure data exchange using Docker containerisation and bridge networking within a client–server architecture, where Base64 encoding is 
applied before transmission over the HTTP protocol and decoded after reception between two isolated containers communicating through IP-based networking.

---

##  System Architecture

- Docker Network: blue
- Server Container: Hosts encoded file using Python HTTP server
- Client Container: Downloads and decodes file
- Communication Protocol: HTTP
- Encoding Method: Base64

---

#  Step 1 – Create Docker Network

bash
docker network create srijan
docker network ls


---

#  Step 2 – Start Server Container

bash
docker run -it --name hello --network srijan python:3.12-slim bash


---

#  Step 3 – Install Required Packages (Server)

bash
apt update
apt install -y wget iputils-ping


---

#  Step 4 – Create Original File (Server)

bash
echo "Founfation of computer science" > fou.txt
ls


---

#  Step 5 – Encode File Using Base64 (Server)

bash
base64 fou.txt > encoded.txt
cat encoded.txt


Example encoded output:


VGhpcyBpcyBzZWNyZXQgZGF0YSBmb3IgQmFzZTY0IERlbW8K


---

#  Step 6 – Check Server IP Address (Windows CMD)

bash
docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" hello


Example output:


172.19.0.2


---

#  Step 7 – Start HTTP Server (Server)

bash
python -m http.server 8086


---

#  Step 8 – Start Client Container

(Open a new CMD window)

bash
docker run -it --name  --network srijan python:3.12-slim bash


---

#  Step 9 – Install Required Packages (Client)

bash
apt update
apt install -y wget iputils-ping


---

#  Step 10 – Test Network Connectivity (Client)

Using container name:

bash
ping hello


Using IP address:

bash
ping 172.19.0.2


---

# Step 11 – Download Encoded File (Client)

Using container name:

bash
wget http://server:8086/encoded.txt


Using IP address:

bash
wget http://172.19.0.2:8086/encoded.txt


---

#  Step 12 – Decode File (Client)

bash
base64 -d encoded.txt > decoded.txt
cat decoded.txt


Expected output:


This is secret data for Base64 Demo


---

#  Common Errors

### Connection refused
Cause: HTTP server not running.  
Fix: Run python -m http.server 8082 in server container.

### Temporary failure in name resolution
Cause: Containers not on same network.  
Fix: Ensure both are connected to blue.

### SSL error when using HTTPS
Cause: Python HTTP server does not support TLS.  
HTTP is unencrypted. HTTPS requires TLS configuration.

---

#  Learning Outcomes

- Understanding Docker networking  
- Implementing client-server communication  
- Demonstrating Base64 encoding before transmission  
- Transferring files using HTTP protocol  
- Verifying communication using container names and IP addresses  
- Understanding difference between HTTP and HTTPS  

---


