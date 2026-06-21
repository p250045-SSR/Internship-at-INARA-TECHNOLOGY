_19-June-2026 - Abdul Ghafar_
# OSI and TCP/IP Models

## 1. The Layered Model

A layered model divides networking into different layers. Each layer has a specific job and works with the layer above and below it.

### OSI Model (7 Layers)

1. Physical – Sends bits through cables/wireless.
2. Data Link – Uses MAC addresses and handles frames.
3. Network – Uses IP addresses and routing.
4. Transport – Reliable communication (TCP/UDP).
5. Session – Manages connections.
6. Presentation – Data formatting and encryption.
7. Application – User services (HTTP, FTP, SSH).

### TCP/IP Model (4 Layers)

1. Network Access
2. Internet
3. Transport
4. Application

---

## 2. The Role of Each Layer

|Layer|Function|Example Protocols|
|---|---|---|
|Application|User services|HTTP, HTTPS, FTP, DNS, SSH|
|Transport|End-to-end communication|TCP, UDP|
|Network|Routing packets|IP, ICMP|
|Data Link|Local network communication|Ethernet, Wi-Fi|
|Physical|Transmits bits|Cable, Fiber, Radio|

---

## 3. Encapsulation

Encapsulation means each layer adds its own header to the data before sending it.

Example:

```
Data ↓TCP Header + Data = Segment ↓IP Header + Segment = Packet ↓Ethernet Header + Packet = Frame ↓Bits sent on network
```

At the receiver side, headers are removed (Decapsulation).

---

## 4. Mapping Protocols to Layers

|Protocol|Layer|
|---|---|
|HTTP/HTTPS|Application|
|FTP|Application|
|DNS|Application|
|SSH|Application|
|TCP|Transport|
|UDP|Transport|
|IP|Network|
|ICMP|Network|
|Ethernet|Data Link|
|Wi-Fi|Data Link|
# 2. IP Addressing and Subnetting

## 1. IPv4 Address Structure

An IPv4 address is a **32-bit number** divided into **4 octets** (8 bits each).

Example:

```
192.168.1.10
```

- 192 = 1st octet
- 168 = 2nd octet
- 1 = 3rd octet
- 10 = 4th octet

An IP address has:

- **Network Part** → Identifies the network
- **Host Part** → Identifies the device

Example:

```
192.168.1.10/24
```

Network = `192.168.1.0`  
Host = `10`

---

## 2. Subnet Masks and CIDR Notation

### Subnet Mask

Used to separate the network part from the host part.

Common masks:

|CIDR|Subnet Mask|
|---|---|
|/8|255.0.0.0|
|/16|255.255.0.0|
|/24|255.255.255.0|
|/25|255.255.255.128|
|/26|255.255.255.192|
|/27|255.255.255.224|
|/28|255.255.255.240|

Example:

```
IP: 192.168.1.10Mask: 255.255.255.0
```

### CIDR Notation

CIDR shows how many bits belong to the network.

Example:

```
192.168.1.10/24
```

`/24` means first 24 bits are network bits.

---

## 3. Calculating Networks and Host Counts

### Formula

**Hosts = 2^(Host Bits) - 2**

(-2 because network address and broadcast address cannot be assigned)

### Examples

| CIDR | Host Bits | Usable Hosts |
| ---- | --------- | ------------ |
| /24  | 8         | 254          |
| /25  | 7         | 126          |
| /26  | 6         | 62           |
| /27  | 5         | 30           |
| /28  | 4         | 14           |
| /29  | 3         | 6            |

Example:

```
192.168.1.0/27
```

Host Bits = 5

```
2^5 - 2 = 30 hosts
```

---

## 4. Public and Private Address Ranges

### Private IP Addresses

Used inside homes, offices, and companies.

| Range                         |
| ----------------------------- |
| 10.0.0.0 – 10.255.255.255     |
| 172.16.0.0 – 172.31.255.255   |
| 192.168.0.0 – 192.168.255.255 |

Example:

```
192.168.1.100
```

### Public IP Addresses

Used on the Internet and are globally unique.

Example:

```
8.8.8.8
```

---

# Practical Subnetting Examples

### Problem 1

Network:

```
192.168.1.0/24
```

Find usable hosts.

```
32 - 24 = 8 host bits2^8 - 2 = 254 hosts
```

Answer:

```
254 usable hosts
```

---

### Problem 2

Network:

```
192.168.1.0/26
```

Find:

- Network Address
- Broadcast Address
- Host Range

Block size:

```
256 - 192 = 64
```

Subnet ranges:

```
192.168.1.0 - 63192.168.1.64 - 127192.168.1.128 - 191192.168.1.192 - 255
```

First subnet:

```
Network   = 192.168.1.0Broadcast = 192.168.1.63Hosts     = 192.168.1.1 - 192.168.1.62
```

---

### Problem 3

CIDR:

```
/28
```

Hosts:

```
32 - 28 = 4 host bits2^4 - 2 = 14 hosts
```

Answer:

```
14 usable hosts
```

# 3. Routing and Switching Concepts

## 1. Switching at Layer 2

- A **switch** works at **Layer 2 (Data Link Layer)**.
- It forwards data using **MAC addresses**.
- Used for communication **inside the same network/LAN**.

**Example:**  
PC1 → Switch → PC2 (same network)

### ARP in Switching (Layer 2)

**ARP (Address Resolution Protocol)** is used to find the **MAC address** of a device when its **IP address** is known.

**Example:**

- PC-A wants to send data to `192.168.1.20`.
- PC-A sends an ARP Request: "Who has 192.168.1.20?"
- The target device replies with its MAC address.
- The switch then forwards frames using MAC addresses.

**In 2 lines:**  
ARP converts an IP address into a MAC address.  
Switches use MAC addresses to deliver frames inside a local network.

---

## 2. Routing at Layer 3

- A **router** works at **Layer 3 (Network Layer)**.
- It forwards data using **IP addresses**.
- Used for communication **between different networks**.

**Example:**  
192.168.1.10 → Router → 10.0.0.5

### OSPF in Router (Layer 3)

**OSPF (Open Shortest Path First)** is a **dynamic routing protocol** used by routers.

**How it works:**

- Routers share network information with each other.
- OSPF automatically finds the **shortest/best path** to a destination.

**In 2 lines:**  
OSPF lets routers automatically learn routes.  
It chooses the shortest path using a metric called **cost**.

### Static Routing

A route is manually configured by the network administrator.

**Example:**

```
ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

**Advantages:**

- Simple for small networks.
- No routing protocol overhead.

**Disadvantages:**

- Must be updated manually.
- Not suitable for large networks.

**In 2 lines:**  
Static routing is configured manually by an administrator.  
It does not change automatically if the network changes.

---

### Dynamic Routing

Routes are learned automatically using routing protocols like:

- OSPF
- RIP
- EIGRP
- BGP

**Advantages:**

- Automatically updates routes.
- Good for large networks.

**Disadvantages:**

- Uses more CPU and bandwidth.

**In 2 lines:**  
Dynamic routing automatically learns and updates routes.  
It uses routing protocols such as OSPF and RIP.

---

## 3. Default Gateway

- The **default gateway** is the router's IP address in your network.
- Devices send traffic to the gateway when the destination is **outside their local network**.

**Example:**

- PC IP: 192.168.1.10
- Gateway: 192.168.1.1

To access Google, the PC sends packets to **192.168.1.1**.

---

## 4. Routing Table

- A **routing table** is a list of routes stored on a device.
- It tells the device **where to send packets**.
- The router checks the table and chooses the best path.

**Simple Example:**

| Network        | Next Hop        |
| -------------- | --------------- |
| 192.168.1.0/24 | Local           |
| 10.0.0.0/24    | 192.168.1.254   |
| 0.0.0.0/0      | Default Gateway |

`0.0.0.0/0` means **send all unknown traffic to the default gateway**.

---

# Easy Difference

| Switching        | Routing            |
| ---------------- | ------------------ |
| Layer 2          | Layer 3            |
| Uses MAC Address | Uses IP Address    |
| Same Network     | Different Networks |
| Device: Switch   | Device: Router     |

# 4. Static Routes and Gateways

A **route** tells a device where to send network traffic.

A **gateway** is the next device (usually a router) that forwards traffic to other networks.

### Example Network

```
PC:      192.168.1.10Mask:    255.255.255.0Gateway: 192.168.1.1Router:  192.168.1.1
```

If the PC wants to reach:

- `192.168.1.50` → Same network → Send directly.
- `8.8.8.8` → Different network → Send to gateway `192.168.1.1`.

---

# 2. Static Route

A **static route** is a route manually added by an administrator.

### Syntax

```
Destination Network → Next Hop (Gateway)
```

### Example

```
Network: 10.0.0.0/24Gateway: 192.168.1.1
```

Meaning:

> To reach the network `10.0.0.0/24`, send packets to router `192.168.1.1`.

Think of it like:

> "If you want to go to city A, first go to bus station B."

---

# 3. Adding Routes

## Linux

### Add a route

```
sudo ip route add 10.0.0.0/24 via 192.168.1.1
```

Meaning:

- Destination network = `10.0.0.0/24`
- Next hop = `192.168.1.1`

---

## Windows

```
route add 10.0.0.0 mask 255.255.255.0 192.168.1.1
```

---

# 4. Removing Routes

## Linux

```
sudo ip route del 10.0.0.0/24
```

---

## Windows

```
route delete 10.0.0.0
```

---

# 5. Default Gateway

A **default gateway** is used when no specific route exists.

It is called the **route of last resort**.

### Example

PC routing table:

```
192.168.1.0/24  Direct0.0.0.0/0       192.168.1.1
```

When sending traffic to:

```
8.8.8.8
```

No matching route exists, so the PC uses:

```
0.0.0.0/0 → 192.168.1.1
```

This is the default gateway.

---

# 6. Configuring a Default Gateway

## Linux

```
sudo ip route add default via 192.168.1.1
```

or

```
sudo ip route replace default via 192.168.1.1
```

---

## Windows

```
route add 0.0.0.0 mask 0.0.0.0 192.168.1.1
```

---

# 7. Inspecting Routes (Viewing Routing Table)

A routing table stores all routes known by the system.

## Linux

```
ip route
```

Example output:

```
default via 192.168.1.1192.168.1.0/24 dev eth010.0.0.0/24 via 192.168.1.1
```

Meaning:

- Default traffic → Router `192.168.1.1`
- Local network → Directly connected
- 10.0.0.0 network → Through router

---

## Windows

```
route print
```

or

```
netstat -r
```

---

# 8. How a Device Chooses a Route

Routing Table:

```
10.0.0.0/24      via 192.168.1.2172.16.0.0/16    via 192.168.1.30.0.0.0/0        via 192.168.1.1
```

If destination is:

- `10.0.0.50` → Use `192.168.1.2`
- `172.16.5.10` → Use `192.168.1.3`
- `8.8.8.8` → Use default gateway `192.168.1.1`

# 05. DNS (Domain Name System)

DNS is like the **phonebook of the Internet**.

Humans remember names like:

```
google.comfacebook.comopenai.com
```

Computers use IP addresses like:

```
142.250.190.78157.240.22.35
```

DNS converts a **domain name → IP address**.

---

# 1. The Resolution Process (How DNS Works)

Suppose you open:

```
www.google.com
```

Your computer does not know Google's IP address.

### Step-by-Step Flow

```
User  |  vPC (DNS Resolver)  |  vDNS Server  |  vRoot Server  |  v.com Server  |  vgoogle.com DNS Server  |  vReturns IP Address
```

### Easy Example

You type:

```
www.google.com
```

1. Browser asks PC:  
    "Do you know google.com's IP?"
2. PC checks cache.
    - Found → Use it.
    - Not found → Ask DNS server.
3. DNS server asks:
    - Root server
    - .com server
    - Google DNS server
4. Google DNS server replies:

```
142.250.x.x
```

5. Browser connects to that IP.

### In One Line

```
Domain Name → DNS Lookup → IP Address → Website Opens
```

---

# 2. Common DNS Record Types

DNS stores different kinds of records.

## A Record

Maps domain → IPv4 address.

Example:

```
google.com → 142.250.190.78
```

```
Type: A
```

---

## AAAA Record

Maps domain → IPv6 address.

Example:

```
google.com → 2404:6800:4009::200e
```

```
Type: AAAA
```

---

## CNAME Record

Alias of another domain.

Example:

```
www.example.com        |        vexample.com
```

```
Type: CNAME
```

Think:

```
Nickname → Real Name
```

---

## MX Record

Mail server record.

Example:

```
gmail.com
```

Uses MX records to receive emails.

```
Type: MX
```

---

## NS Record

Nameserver record.

Tells who manages DNS for a domain.

Example:

```
google.com
```

may use:

```
ns1.google.comns2.google.com
```

```
Type: NS
```

---

## TXT Record

Stores text information.

Used for:

- SPF
- DKIM
- Domain verification

Example:

```
google-site-verification=abc123
```

```
Type: TXT
```

---

# Summary Table

| Record | Purpose          |
| ------ | ---------------- |
| A      | Domain → IPv4    |
| AAAA   | Domain → IPv6    |
| CNAME  | Alias            |
| MX     | Mail server      |
| NS     | Name server      |
| TXT    | Text information |

---

# 3. Querying with dig

`dig` = Domain Information Groper

Used on Linux/macOS.

## Basic Query

```
dig google.com
```

Output:

```
google.com. 300 IN A 142.250.190.78
```

Meaning:

```
google.com → 142.250.190.78
```

---

## Query Only A Record

```
dig google.com A
```

---

## Query AAAA Record

```
dig google.com AAAA
```

---

## Query MX Record

```
dig gmail.com MX
```

Example output:

```
gmail.com. IN MX 5 gmail-smtp-in.l.google.com.
```

---

## Query NS Record

```
dig google.com NS
```

---

## Short Output

```
dig +short google.com
```

Output:

```
142.250.190.78
```

Very useful.

---

## Reverse Lookup

IP → Domain

```
dig -x 8.8.8.8
```

Output:

```
dns.google
```

---

# 4. Querying with nslookup

Older DNS tool but still common.

---

## Basic Lookup

```
nslookup google.com
```

Output:

```
Name: google.comAddress: 142.250.190.78
```

---

## Query MX Record

```
nslookup -type=MX gmail.com
```

---

## Query NS Record

```
nslookup -type=NS google.com
```

---

## Query TXT Record

```
nslookup -type=TXT google.com
```

---

## Reverse Lookup

```
nslookup 8.8.8.8
```

Output:

```
dns.google
```

---

# dig vs nslookup

| Feature             | dig | nslookup |
| ------------------- | --- | -------- |
| Detailed output     | ✅   | ❌        |
| Modern tool         | ✅   | ❌        |
| Easy to read        | ❌   | ✅        |
| Most used by admins | ✅   | ❌        |

For Linux administration:

```
Use dig whenever possible.
```

---

# 5. Local Resolver Configuration

A resolver is the DNS server your PC uses.

---

## Linux

View DNS servers:

```
cat /etc/resolv.conf
```

Example:

```
nameserver 8.8.8.8nameserver 1.1.1.1
```

Meaning:

```
Primary DNS   = 8.8.8.8Secondary DNS = 1.1.1.1
```

---

## Show DNS Configuration

Ubuntu:

```
resolvectl status
```

or

```
systemd-resolve --status
```

---

## Windows

Show DNS settings:

```
ipconfig /all
```

Example:

```
DNS Servers . . . . . : 8.8.8.8                         1.1.1.1
```

---

# DNS Resolution Example (Complete Flow)

Suppose:

```
PC IP = 192.168.1.10DNS Server = 8.8.8.8Website = google.com
```

Flow:

```
1. User enters google.com2. PC asks DNS server (8.8.8.8)3. DNS server finds:   google.com = 142.250.190.784. DNS server sends IP back5. PC connects to:   142.250.190.786. Website loads
```

### Simple Formula

```
google.com     ↓DNS Lookup     ↓142.250.190.78     ↓Connection Established     ↓Website Opens
```


# 06. Diagnostic Tools: `ping`, `traceroute`, `ss`, and `netstat`

These tools help you **troubleshoot network problems** and check whether devices and services are reachable.

---

# 1. ping

## What is ping?

`ping` checks whether a remote device is reachable on the network.

It sends **ICMP Echo Request** packets and waits for **ICMP Echo Reply** packets.

### Simple Example

```
ping google.com
```

Output:

```
64 bytes from 142.250.190.14: icmp_seq=1 ttl=117 time=20 ms64 bytes from 142.250.190.14: icmp_seq=2 ttl=117 time=21 ms
```

### Meaning

- **64 bytes** → packet size
- **icmp_seq=1** → packet number
- **ttl=117** → Time To Live
- **time=20 ms** → response time

---

## Purpose of ping

- Check internet connectivity
- Check if a host is online
- Measure latency (delay)
- Troubleshoot network issues

---

## Examples

### Ping Google

```
ping google.com
```

### Ping by IP

```
ping 8.8.8.8
```

### Send only 4 packets (Linux)

```
ping -c 4 google.com
```

### Continuous ping (Windows)

```
ping -t google.com
```

### Ping localhost

```
ping 127.0.0.1
```

Tests your own machine's network stack.

---

## Interpreting Results

### Success

```
Reply from 8.8.8.8
```

Meaning:

- Host is reachable.

### Failure

```
Request timed out
```

Meaning:

- Host is unreachable.
- Firewall may block ICMP.
- Network problem exists.

---

# 2. traceroute

## What is traceroute?

`traceroute` shows the path packets take from your computer to the destination.

Think of it as showing every "hop" between you and the destination.

---

## Example Network

```
Your PC   ↓Router A   ↓Router B   ↓ISP Router   ↓Google Server
```

Traceroute displays each router along the path.

---

## Commands

### Linux

```
traceroute google.com
```

### Windows

```
tracert google.com
```

---

## Example Output

```
1  192.168.1.1      1 ms2  10.10.0.1        5 ms3  172.16.0.1      12 ms4  142.250.190.14  20 ms
```

---

## Meaning

### Hop 1

```
192.168.1.1
```

Your local router (default gateway)

### Hop 2

```
10.10.0.1
```

ISP router

### Hop 3

```
172.16.0.1
```

Another ISP router

### Hop 4

```
142.250.190.14
```

Destination server

---

## Why Use traceroute?

- Find where connection fails
- Identify slow routers
- View network path
- Troubleshoot routing problems

---

## Useful Commands

### Numeric output only

```
traceroute -n google.com
```

### Specify max hops

```
traceroute -m 20 google.com
```

---

# 3. ss

## What is ss?

`ss` (Socket Statistics) shows:

- Active network connections
- Listening ports
- TCP connections
- UDP connections

It is the modern replacement for `netstat`.

---

## Why Use ss?

To answer:

- Which ports are open?
- Which services are listening?
- Which hosts are connected?

---

## Show All Connections

```
ss
```

---

## Show TCP Connections

```
ss -t
```

Output:

```
State      Recv-Q Send-Q Local Address:PortESTAB      0      0      192.168.1.10:22
```

---

## Show UDP Connections

```
ss -u
```

---

## Show Listening Ports

```
ss -l
```

Example:

```
LISTEN 0 128 *:22
```

Meaning:

```
Port 22 is listening
```

SSH server is waiting for connections.

---

## Show TCP Listening Ports

```
ss -lt
```

---

## Show UDP Listening Ports

```
ss -lu
```

---

## Show Process Names

```
ss -tulpn
```

Example:

```
tcp LISTEN 0 128 *:22 users:(("sshd",pid=600))
```

Meaning:

```
Port 22 → SSH service
```

---

## Common States

### LISTEN

```
Waiting for incoming connection
```

### ESTAB (ESTABLISHED)

```
Connection is active
```

### CLOSE-WAIT

```
Connection closing
```

### TIME-WAIT

```
Recently closed connection
```

---

# 4. netstat

## What is netstat?

`netstat` shows:

- Network connections
- Routing table
- Listening ports
- Interface statistics

Older tool but still widely used.

---

## Show All Connections

```
netstat -a
```

---

## Show Listening Ports

```
netstat -l
```

---

## Show TCP Connections

```
netstat -t
```

---

## Show UDP Connections

```
netstat -u
```

---

## Show Process IDs

```
netstat -tulpn
```

Example:

```
tcp 0 0 0.0.0.0:22 LISTEN 600/sshd
```

Meaning:

```
SSH service listening on port 22
```

---

## View Routing Table

```
netstat -r
```

Example:

```
Destination    Gateway0.0.0.0        192.168.1.1
```

Meaning:

```
Default route uses gateway 192.168.1.1
```

---

# Testing Reachability

Reachability means:

```
Can my device communicate with another device?
```

### Test using ping

```
ping 8.8.8.8
```

Success:

```
Reply received
```

Failure:

```
Timeout
```

---

# Tracing a Network Path

Use:

```
traceroute google.com
```

or

```
tracert google.com
```

Shows:

```
PC → Router → ISP → Destination
```

Every step is called a **hop**.

---

# Viewing Sockets and Listening Ports

## Show Listening Services

```
ss -tulpn
```

Example:

```
22   SSH80   HTTP443  HTTPS
```

---

## Check Specific Port

```
ss -tulpn | grep 22
```

Output:

```
LISTEN *:22
```

Meaning:

```
SSH server is running.
```

---

# Quick Comparison

|Tool|Purpose|
|---|---|
|ping|Check if host is reachable|
|traceroute|Show route/hops to destination|
|ss|Show sockets and open ports|
|netstat|Show connections, ports, routes|


# 07. What is HTTP?

**HTTP (HyperText Transfer Protocol)** is the protocol used by web browsers and web servers to communicate.

When you open a website:

```
Browser  ---> HTTP Request ---> ServerBrowser <--- HTTP Response --- Server
```

### Example

You type:

```
http://example.com
```

Browser sends:

```
GET / HTTP/1.1Host: example.com
```

Server replies:

```
HTTP/1.1 200 OKContent-Type: text/html<html>...</html>
```

---

# 2. What is HTTPS?

**HTTPS (HyperText Transfer Protocol Secure)** is HTTP + TLS encryption.

Without HTTPS:

```
Browser ---- HTTP ---- Server
```

Anyone on the network can read the data.

With HTTPS:

```
Browser ==== Encrypted ==== Server
```

No one can easily read:

- Passwords
- Credit card numbers
- Login information

---

# 3. What is TLS?

**TLS (Transport Layer Security)** encrypts communication between client and server.

TLS provides:

### 1. Encryption

Converts readable data into unreadable form.

Example:

```
Password: admin123
```

becomes

```
jshd873hd83h83h
```

---

### 2. Integrity

Ensures data is not modified during transfer.

---

### 3. Authentication

Confirms you are talking to the real website.

Example:

```
https://google.com
```

TLS certificate proves it is actually Google.

---

# 4. HTTP Request and Response Cycle

This is the complete flow when opening a website.

### Step 1: User enters URL

```
https://google.com
```

---

### Step 2: DNS Lookup

Browser asks DNS:

```
What is the IP of google.com?
```

DNS replies:

```
142.250.x.x
```

---

### Step 3: TCP Connection

Browser creates connection with server.

```
Client ------ TCP ------ Server
```

---

### Step 4: TLS Handshake (HTTPS only)

Browser and server agree on encryption.

---

### Step 5: HTTP Request

Browser sends:

```
GET / HTTP/1.1Host: google.com
```

---

### Step 6: Server Processes Request

Server finds requested page.

---

### Step 7: HTTP Response

Server sends:

```
HTTP/1.1 200 OKContent-Type: text/html
```

with webpage content.

---

### Step 8: Browser Displays Page

User sees website.

---

## Easy Diagram

```
User ↓DNS Lookup ↓TCP Connection ↓TLS Handshake ↓HTTP Request ↓Server Processing ↓HTTP Response ↓Web Page Display
```

---

# 5. Common HTTP Methods

| Method | Purpose       |
| ------ | ------------- |
| GET    | Retrieve data |
| POST   | Send data     |
| PUT    | Update data   |
| DELETE | Delete data   |

### Example

View webpage:

```
GET /index.html
```

Login form:

```
POST /login
```

---

# 6. Common HTTP Status Codes

Status codes tell what happened to the request.

## 200 OK

Success.

```
HTTP/1.1 200 OK
```

Website loaded successfully.

---

## 201 Created

Resource created successfully.

Example:

```
HTTP/1.1 201 Created
```

After creating a user account.

---

## 301 Moved Permanently

Page moved to another URL.

Example:

```
http://site.com
```

redirects to:

```
https://site.com
```

---

## 302 Found

Temporary redirect.

---

## 400 Bad Request

Browser sent invalid request.

---

## 401 Unauthorized

Login required.

---

## 403 Forbidden

Access denied.

Example:

```
You are logged in but not allowed.
```

---

## 404 Not Found

Page does not exist.

```
HTTP/1.1 404 Not Found
```

---

## 500 Internal Server Error

Problem on server.

---

## 502 Bad Gateway

Server received invalid response from another server.

---

## 503 Service Unavailable

Server is overloaded or under maintenance.

---

# Easy Memory Trick

| Range | Meaning      |
| ----- | ------------ |
| 1xx   | Information  |
| 2xx   | Success      |
| 3xx   | Redirect     |
| 4xx   | Client Error |
| 5xx   | Server Error |

---

# 7. Role of TLS

TLS secures communication.

Without TLS:

```
PC -------- Data -------- Server
```

Attacker can read packets.

---

With TLS:

```
PC ==== Encrypted Data ==== Server
```

Attacker sees only encrypted data.

---

TLS protects:

- Passwords
- Banking information
- Emails
- Personal data

---

# 8. TLS Handshake (High Level)

Before encrypted communication starts, browser and server perform a handshake.

### Step 1: Client Hello

Browser says:

```
Hello ServerI support TLS 1.3
```

---

### Step 2: Server Hello

Server replies:

```
Hello ClientLet's use TLS 1.3
```

and sends certificate.

---

### Step 3: Certificate Verification

Browser checks:

- Is certificate valid?
- Is it expired?
- Is it trusted?

If valid:

```
Continue
```

Otherwise:

```
Certificate Warning
```

---

### Step 4: Key Exchange

Both sides create shared encryption keys.

---

### Step 5: Secure Communication Starts

Now data is encrypted.

```
HTTPS Traffic
```

flows safely.

---

## Easy TLS Handshake Diagram

```
Browser                 ServerClient Hello  -------->                <------ Server HelloCertificate    <------Verify CertificateKey Exchange   <----->Encrypted Communication Starts
```

---

# Useful Commands

## 1. Test HTTP Headers

```
curl -I http://example.com
```

Example output:

```
HTTP/1.1 301 Moved Permanently
```

---

## 2. Test HTTPS Headers

```
curl -I https://google.com
```

---

## 3. View Full HTTP Response

```
curl -v https://google.com
```

`-v` = verbose mode.

Shows:

- TLS handshake
- Request headers
- Response headers

---

## 4. Download a Web Page

```
curl https://example.com
```

---

## 5. Show TLS Certificate

```
openssl s_client -connect google.com:443
```

Shows:

- Certificate
- TLS version
- Cipher suite

---

## 6. Show Certificate Details

```
echo | openssl s_client -connect google.com:443 2>/dev/null | openssl x509 -text
```

---

## 7. Check Website Headers

```
wget --server-response https://google.com
```

---

## 8. Test HTTPS Connection

```
openssl s_client -connect google.com:443
```

# 08. SSL Certificate Basics

An **SSL/TLS certificate** is a digital identity card for a website or server.

When you visit `https://google.com`, the server sends its certificate to your browser. The browser checks:

1. Is this certificate valid?
2. Is it issued by a trusted authority?
3. Has it expired?
4. Does it belong to this website?

If everything is correct, an encrypted TLS connection is established.

---

# 1. What a Certificate Contains

Think of a certificate as a digital ID card.

A certificate contains:

| Field                           | Purpose                              |
| ------------------------------- | ------------------------------------ |
| Subject                         | Website/owner name                   |
| Common Name (CN)                | Domain name                          |
| Subject Alternative Names (SAN) | Additional domains                   |
| Issuer                          | Certificate Authority that issued it |
| Public Key                      | Used for encryption                  |
| Serial Number                   | Unique certificate ID                |
| Valid From                      | Start date                           |
| Valid To                        | Expiry date                          |
| Signature                       | CA's digital signature               |

---

## Example

For google.com:

```
Subject: CN=google.comIssuer: Google Trust ServicesValid From:Jan 1 2026Valid To:Apr 1 2026Public Key:RSA 2048-bit
```

Meaning:

- Certificate belongs to Google
- Issued by Google Trust Services
- Valid for specified dates
- Contains Google's public key

---

## View Certificate Contents

### OpenSSL

```
openssl x509 -in cert.pem -text -noout
```

Example output:

```
Subject: CN=example.comIssuer: CN=Let's EncryptNot Before:Jun 1 2026Not After:Sep 1 2026
```

---

# 2. Certificate Authorities (CA) and Trust Chains

## What is a Certificate Authority?

A CA is a trusted organization that verifies identities and issues certificates.

Examples:

- DigiCert
- Let's Encrypt
- GlobalSign
- Sectigo

---

## Why Do We Need a CA?

Without a CA, anyone could create a fake certificate for:

```
google.com
```

and pretend to be Google.

The CA verifies ownership before issuing the certificate.

---

## Trust Chain

A browser does not trust every certificate directly.

Instead it trusts a chain:

```
Root CA   ↓Intermediate CA   ↓Website Certificate
```

Example:

```
Root CA    ↓DigiCert Intermediate    ↓google.com Certificate
```

---

## Easy Analogy

Think of:

```
Government   ↓Passport Office   ↓Your Passport
```

Browser trusts:

```
Root CA   ↓Intermediate CA   ↓Website Certificate
```

---

## View Certificate Chain

```
openssl s_client -connect google.com:443
```

Look for:

```
Certificate chain
```

# 3. Validity Periods and Expiry

Every SSL/TLS certificate has a **start date** and an **end date**.

- **Valid From** → Date when the certificate becomes usable.
- **Valid To (Expiry Date)** → Date when the certificate expires.

Example:

```
Valid From: Jun 1 2026Valid To:   Jun 1 2027
```

This certificate works for **1 year**.

### Why Expiry Exists

- Improves security.
- Forces certificate renewal.
- Prevents using old compromised certificates.

### What Happens After Expiry?

If a certificate expires:

- Browser shows warning.
- HTTPS connection becomes untrusted.
- Users may see:

```
Your connection is not privateNET::ERR_CERT_DATE_INVALID
```

---

# 4. Viewing a Certificate

You can view certificate details such as:

- Subject (owner)
- Issuer (Certificate Authority)
- Validity dates
- Public key
- SANs (domains covered)

---

# OpenSSL Commands

## View a Local Certificate

```
openssl x509 -in cert.pem -text -noout
```

Shows full certificate information.

---

## View Only Expiry Date

```
openssl x509 -in cert.pem -noout -enddate
```

Example output:

```
notAfter=Jun 1 12:00:00 2027 GMT
```

---

## View Start Date

```
openssl x509 -in cert.pem -noout -startdate
```

Example:

```
notBefore=Jun 1 12:00:00 2026 GMT
```

---

## View Subject (Owner)

```
openssl x509 -in cert.pem -noout -subject
```

Example:

```
subject=CN=example.com
```

---

## View Issuer (CA)

```
openssl x509 -in cert.pem -noout -issuer
```

Example:

```
issuer=CN=Let's Encrypt
```

---

## View Certificate from a Website

```
openssl s_client -connect google.com:443
```


# 09. VLANs (Virtual LANs)

## 1. Purpose of VLANs

A VLAN divides one physical switch into multiple logical networks.

### Why use VLANs?

- Separate different departments (HR, IT, Finance)
- Improve security
- Reduce broadcast traffic
- Better network management

### Example

Without VLANs:

```
PC1 ----|PC2 ----|---- Switch ---- All devices in same networkPC3 ----|
```

With VLANs:

```
VLAN 10 (HR)      VLAN 20 (IT)PC1               PC3PC2               PC4
```

Even if connected to the same switch, VLAN 10 and VLAN 20 cannot communicate directly.

---

## 2. Access Ports

An access port belongs to **one VLAN only**.

Used for:

- PCs
- Printers
- Servers

### Example

```
PC ---- Access Port ---- Switch            VLAN 10
```

The PC does not know about VLAN tags.

### Cisco Commands

Create VLAN:

```
Switch(config)# vlan 10Switch(config-vlan)# name HR
```

Assign port to VLAN 10:

```
Switch(config)# interface fa0/1Switch(config-if)# switchport mode accessSwitch(config-if)# switchport access vlan 10
```

Verify:

```
show vlan brief
```

---

## 3. Trunk Ports and 802.1Q Tagging

A trunk port carries traffic for **multiple VLANs**.

Used between:

- Switch ↔ Switch
- Switch ↔ Router
- Switch ↔ Hypervisor

### Example

```
Switch A ===== Trunk ===== Switch BVLAN 10VLAN 20VLAN 30
```

### 802.1Q Tagging

802.1Q adds a VLAN ID tag to Ethernet frames.

Example:

```
Frame + VLAN 10 TagFrame + VLAN 20 TagFrame + VLAN 30 Tag
```

This allows one cable to carry multiple VLANs.

### Cisco Commands

Configure trunk:

```
Switch(config)# interface fa0/24Switch(config-if)# switchport mode trunk
```

Allow specific VLANs:

```
Switch(config-if)# switchport trunk allowed vlan 10,20,30
```

Verify:

```
show interfaces trunk
```

---

## 4. Native VLAN

The Native VLAN is the VLAN whose traffic is sent **without an 802.1Q tag** on a trunk link.

Default Native VLAN:

```
VLAN 1
```

### Example

```
Trunk LinkVLAN 10 → TaggedVLAN 20 → TaggedVLAN 1  → Untagged (Native VLAN)
```

### Change Native VLAN

```
Switch(config)# interface fa0/24Switch(config-if)# switchport trunk native vlan 99
```

Verify:

```
show interfaces trunk
```

---

# Important Commands

### View VLANs

```
show vlan brief
```

### Create VLAN

```
vlan 10name HR
```

### Configure Access Port

```
interface fa0/1switchport mode accessswitchport access vlan 10
```

### Configure Trunk Port

```
interface fa0/24switchport mode trunk
```

### Allow VLANs on Trunk

```
switchport trunk allowed vlan 10,20,30
```

### Set Native VLAN

```
switchport trunk native vlan 99
```

### Verify Trunk

```
show interfaces trunk
```


# 10. NAT (Network Address Translation)

NAT is a technique used by routers/firewalls to change IP addresses in packets as they pass through a network.

---

# 1. Why NAT is Used

### Purpose

NAT allows many devices with **private IP addresses** to access the Internet using **one public IP address**.

### Example

Private Network:

- PC1 → 192.168.1.10
- PC2 → 192.168.1.20
- Router Public IP → 203.0.113.5

When PCs access the Internet, the router replaces their private IPs with **203.0.113.5**.

### Benefits

✅ Saves public IP addresses

✅ Hides internal network structure

✅ Provides basic security

---

# 2. Source NAT (SNAT)

### What it does

Changes the **source IP address** of outgoing packets.

### Example

Before NAT:

```
Source: 192.168.1.10Destination: 8.8.8.8
```

After NAT:

```
Source: 203.0.113.5Destination: 8.8.8.8
```

The router changes the source IP from private to public.

### Use Case

- Internal users accessing the Internet.

---

# 3. Destination NAT (DNAT)

### What it does

Changes the **destination IP address** of incoming packets.

### Example

A web server inside LAN:

```
192.168.1.100
```

Public IP:

```
203.0.113.5
```

Internet user accesses:

```
203.0.113.5:80
```

Router changes destination:

```
203.0.113.5 → 192.168.1.100
```

### Use Case

- Hosting websites
- Port forwarding
- Remote access

---

# 4. Port Address Translation (PAT)

Also called:

- NAT Overload
- Many-to-One NAT

### What it does

Allows many devices to share one public IP using different port numbers.

### Example

|Device|Private IP|Public IP|
|---|---|---|
|PC1|192.168.1.10|203.0.113.5:1001|
|PC2|192.168.1.20|203.0.113.5:1002|
|PC3|192.168.1.30|203.0.113.5:1003|

Router tracks connections using port numbers.

### Benefit

One public IP can serve hundreds of devices.

---

# 5. NAT and Connectivity

### Outbound Connection

Internal users → Internet

```
PC → Router(NAT) → Internet
```

Works easily using SNAT/PAT.

### Inbound Connection

Internet → Internal Server

Requires:

- DNAT
- Port Forwarding

Example:

```
Public IP:203.0.113.5:80       ↓Private IP:192.168.1.100:80
```

Without port forwarding, outside users cannot directly reach internal devices.

---

# Quick Summary

|NAT Type|Changes|
|---|---|
|SNAT|Source IP|
|DNAT|Destination IP|
|PAT|Source IP + Port|
|Port Forwarding|Public IP → Internal Server|

---

# Linux iptables Commands

### View NAT Rules

```
sudo iptables -t nat -L -n -v
```

---

### Source NAT (SNAT)

```
sudo iptables -t nat -A POSTROUTING -o eth0 \-j SNAT --to-source 203.0.113.5
```

---

### Masquerading (Dynamic SNAT)

Used when public IP changes.

```
sudo iptables -t nat -A POSTROUTING -o eth0 \-j MASQUERADE
```

---

### Destination NAT (DNAT)

Forward traffic to internal server:

```
sudo iptables -t nat -A PREROUTING -p tcp \--dport 80 -j DNAT \--to-destination 192.168.1.100:80
```

---

### Show Current Connections

```
conntrack -L
```

---

### View Routing Table

```
ip route
```


# 11. iptables: Chains, Tables, Filter and NAT

`iptables` is a Linux firewall tool used to allow, block, or modify network traffic.

---

# 1. Tables and Chains

## Table

A **table** is a group of rules for a specific purpose.

Common tables:

| Table  | Purpose                   |
| ------ | ------------------------- |
| filter | Allow or block traffic    |
| nat    | Change IP addresses (NAT) |
| mangle | Modify packets            |
| raw    | Special packet processing |

---

## Chain

A **chain** is a place where packets are checked against rules.

Common chains:

| Chain   | Purpose                                      |
| ------- | -------------------------------------------- |
| INPUT   | Traffic coming to this device                |
| OUTPUT  | Traffic leaving this device                  |
| FORWARD | Traffic passing through this device (router) |

### Example

PC → Server

- Packet enters Linux server → **INPUT**
- Packet leaves Linux server → **OUTPUT**
- Packet passes through router → **FORWARD**

---

# 2. Rule Structure and Targets

A rule tells iptables what traffic to match and what action to take.

### Structure

```
iptables -A CHAIN conditions -j TARGET
```

### Example

```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

Meaning:

- `-A` = Add rule
- `INPUT` = Check incoming traffic
- `-p tcp` = TCP protocol
- `--dport 22` = Port 22 (SSH)
- `-j ACCEPT` = Allow traffic

---

## Common Targets

| Target | Action                |
| ------ | --------------------- |
| ACCEPT | Allow packet          |
| DROP   | Silently block packet |
| REJECT | Block and send error  |
| LOG    | Log packet            |
| DNAT   | Destination NAT       |
| SNAT   | Source NAT            |

### Examples

Allow:

```
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

Block:

```
iptables -A INPUT -p tcp --dport 23 -j DROP
```

Reject:

```
iptables -A INPUT -p tcp --dport 23 -j REJECT
```

---

# 3. The Filter Table

The **filter table** is the default table.

Used for:

- Allowing traffic
- Blocking traffic
- Firewall rules

Chains:

- INPUT
- OUTPUT
- FORWARD

### View Rules

```
iptables -L
```

### Allow SSH

```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

### Allow HTTP

```
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

### Block Telnet

```
iptables -A INPUT -p tcp --dport 23 -j DROP
```

### Set Default Policy

Block all incoming traffic:

```
iptables -P INPUT DROP
```

Allow all incoming traffic:

```
iptables -P INPUT ACCEPT
```

---

# 4. The NAT Table

**NAT (Network Address Translation)** changes source or destination IP addresses.

Used when:

- Sharing Internet
- Port Forwarding
- Hiding private IPs

Chains:

| Chain       | Purpose                   |
| ----------- | ------------------------- |
| PREROUTING  | Before routing decision   |
| POSTROUTING | After routing decision    |
| OUTPUT      | Locally generated packets |

---

## Source NAT (SNAT)

Changes source IP address.

Example:

```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

Meaning:

- `-t nat` = Use NAT table
- `POSTROUTING` = After routing
- `eth0` = Internet interface
- `MASQUERADE` = Replace private IP with public IP

---

## Destination NAT (DNAT)

Changes destination IP address.

Example:

Forward port 80 traffic to internal server:

```
iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.1.100
```

Meaning:

Internet User → Router Public IP → Internal Server

---

# Important Commands

## List Rules

```
iptables -L
```

## List NAT Rules

```
iptables -t nat -L
```

## Show Rules with Numbers

```
iptables -L --line-numbers
```

## Delete Rule

```
iptables -D INPUT 1
```

(Delete rule number 1)

## Flush All Rules

```
iptables -F
```

## Flush NAT Rules

```
iptables -t nat -F
```

## Save Rules (Ubuntu/Debian)

```
iptables-save
```

## Restore Rules

```
iptables-restore < rules.txt
```

---

# Quick Summary

- **Table** = Collection of rules (`filter`, `nat`)
- **Chain** = Path where packets are checked (`INPUT`, `OUTPUT`, `FORWARD`)
- **Rule** = Condition + Action
- **Target** = Action (`ACCEPT`, `DROP`, `DNAT`, `SNAT`)
- **Filter Table** = Firewall (allow/block traffic)
- **NAT Table** = Change IP addresses (SNAT/DNAT)



# Packet Analysis with Wireshark and tcpdump

Packet analysis means **capturing and inspecting network packets** to understand what devices are sending and receiving.

Think of packets as **letters traveling through a network**. Wireshark and tcpdump help you read those letters.

---

# 1. Capturing Traffic

## What is Traffic Capture?

Traffic capture means recording packets that pass through a network interface.

Examples of captured traffic:

- Ping packets (ICMP)
- Web traffic (HTTP/HTTPS)
- DNS queries
- SSH connections

---

## Wireshark Capture

### Start Capture

1. Open Wireshark
2. Select a network interface
    - Ethernet
    - Wi-Fi
3. Click **Start Capturing** (blue shark fin)

Wireshark immediately starts collecting packets.

---

## Linux Interface Check

```
ip a
```

or

```
ifconfig
```

Example output:

```
eth0wlan0lo
```

Where:

- eth0 = Ethernet
- wlan0 = Wi-Fi
- lo = Loopback

---

# 2. Applying Capture and Display Filters

Wireshark has two types of filters:

|Filter Type|Applied Before Capture?|Applied After Capture?|
|---|---|---|
|Capture Filter|Yes|No|
|Display Filter|No|Yes|

---

# A. Capture Filters

Capture only selected packets.

Example:

Capture only ICMP packets:

```
icmp
```

Capture only SSH:

```
port 22
```

Capture only HTTP:

```
port 80
```

Capture traffic from a host:

```
host 192.168.1.10
```

Capture DNS:

```
port 53
```

---

## Common Capture Filters

### ICMP

```
icmp
```

### SSH

```
tcp port 22
```

### HTTP

```
tcp port 80
```

### HTTPS

```
tcp port 443
```

### DNS

```
udp port 53
```

### Specific IP

```
host 8.8.8.8
```

---

# B. Display Filters

Display filters are applied after packets are captured.

---

## Show Only ICMP

```
icmp
```

---

## Show Only DNS

```
dns
```

---

## Show Only TCP

```
tcp
```

---

## Show Only HTTP

```
http
```

---

## Show Specific Source IP

```
ip.src == 192.168.1.10
```

---

## Show Specific Destination IP

```
ip.dst == 8.8.8.8
```

---

## Show Port 80

```
tcp.port == 80
```

---

## Show Port 22

```
tcp.port == 22
```

---

## Show Failed TCP Retransmissions

```
tcp.analysis.retransmission
```

---

# 3. Reading a Capture

When you click a packet in Wireshark, you see three sections.

---

## Section 1: Packet List

Shows all packets.

Example:

```
No  Source        Destination   Protocol1   192.168.1.5   8.8.8.8       DNS2   8.8.8.8       192.168.1.5   DNS3   192.168.1.5   142.250.x.x   TCP
```

---

## Section 2: Packet Details

Shows protocol layers.

Example:

```
FrameEthernetIPTCPHTTP
```

---

## Section 3: Packet Bytes

Raw packet data in hexadecimal.

Example:

```
45 00 00 54 ...
```

Usually used by advanced analysts.

---

# Understanding Common Protocols

## ARP

Maps IP address to MAC address.

Example:

```
Who has 192.168.1.1?
```

---

## ICMP

Used by ping.

Example:

```
ping google.com
```

Wireshark shows:

```
Echo RequestEcho Reply
```

---

## DNS

Domain name lookup.

Example:

```
google.com
```

DNS converts:

```
google.com↓142.250.x.x
```

---

## TCP

Reliable communication.

Look for:

```
SYNSYN-ACKACK
```

This is called the TCP Three-Way Handshake.

---

## HTTP

Web requests.

Example:

```
GET /index.html
```

---

# TCP Three-Way Handshake

When a connection starts:

Client:

```
SYN
```

Server:

```
SYN ACK
```

Client:

```
ACK
```

Connection established.

Wireshark filter:

```
tcp.flags.syn == 1
```

---

# Follow a TCP Stream

Very useful feature.

Right-click packet:

```
Follow → TCP Stream
```

Shows complete conversation between client and server.

---

# 4. Command Line Capture with tcpdump

tcpdump is the command-line version of Wireshark.

Very common on Linux servers.

---

## Install

Ubuntu:

```
sudo apt install tcpdump
```

---

## Show Interfaces

```
tcpdump -D
```

Example:

```
1.eth02.wlan03.lo
```

---

## Capture All Packets

```
sudo tcpdump
```

---

## Capture on Specific Interface

```
sudo tcpdump -i eth0
```

---

## Capture ICMP

```
sudo tcpdump -i eth0 icmp
```

---

## Capture DNS

```
sudo tcpdump -i eth0 port 53
```

---

## Capture SSH

```
sudo tcpdump -i eth0 port 22
```

---

## Capture HTTPS

```
sudo tcpdump -i eth0 port 443
```

---

## Capture From Specific Host

```
sudo tcpdump host 8.8.8.8
```

---

## Save Capture to File

```
sudo tcpdump -i eth0 -w capture.pcap
```

---

## Read Saved File

```
tcpdump -r capture.pcap
```

---

## Show Packet Details

```
sudo tcpdump -i eth0 -nn
```

Options:

```
-n  = No DNS lookup-nn = No DNS and port resolution
```

---

## Capture First 20 Packets

```
sudo tcpdump -c 20
```

---

# Practical Task: Capture and Analyse Traffic

## Step 1: Start Wireshark

Select interface:

```
eth0 or wlan0
```

Click Start Capture.

---

## Step 2: Generate Traffic

Open terminal:

```
ping google.com
```

or

```
ping 8.8.8.8
```

---

## Step 3: Apply Filter

In Wireshark:

```
icmp
```

You should see:

```
Echo RequestEcho Reply
```

---

## Step 4: Analyze Packets

Check:

Source:

```
Your PC
```

Destination:

```
8.8.8.8
```

Protocol:

```
ICMP
```

Round-trip communication:

```
Request → Reply
```

---

## Step 5: DNS Analysis

Open terminal:

```
nslookup google.com
```

Apply filter:

```
dns
```

Observe:

```
DNS QueryDNS Response
```

---

## Step 6: Save Capture

Wireshark:

```
File → Save As
```

Save:

```
traffic.pcapng
```

---

# Most Important Commands for Exam/Interview

```
ip aifconfig
```

```
tcpdump -D
```

```
sudo tcpdump
```

```
sudo tcpdump -i eth0
```

```
sudo tcpdump -i eth0 icmp
```

```
sudo tcpdump -i eth0 port 53
```

```
sudo tcpdump host 8.8.8.8
```

```
sudo tcpdump -c 20
```

```
sudo tcpdump -w capture.pcap
```

```
tcpdump -r capture.pcap
```

### Quick Summary

- **Wireshark** = GUI packet analyzer.
- **tcpdump** = Command-line packet analyzer.
- **Capture Filter** = Filters traffic before capturing.
- **Display Filter** = Filters packets after capturing.
- **ICMP** = Ping traffic.
- **DNS** = Domain name lookup traffic.
- **TCP Handshake** = SYN → SYN-ACK → ACK.
- **PCAP/PCAPNG** = Packet capture file formats.
- **Follow TCP Stream** = View complete conversation between two hosts.



# Firewall with UFW, Firewalld, and AppArmor

## 1. Simplified Rules with UFW (Uncomplicated Firewall)

**UFW** is a simple firewall tool used mainly on Ubuntu. It makes firewall management easy.

### Common Commands

```
# Enable UFWsudo ufw enable# Disable UFWsudo ufw disable# Check statussudo ufw status# Allow SSHsudo ufw allow 22# Allow HTTPsudo ufw allow 80# Deny a portsudo ufw deny 23# Delete a rulesudo ufw delete allow 80
```

### Example

```
sudo ufw allow 80
```

Allows web traffic on port 80.

---

## 2. Zone-Based Control with Firewalld

**Firewalld** is a dynamic firewall used in Red Hat, CentOS, Fedora, and Rocky Linux.

### What are Zones?

A **zone** is a set of firewall rules based on trust level.

Examples:

- **public** → Untrusted networks (Internet)
- **home** → Trusted home network
- **work** → Office network

### Common Commands

```
# Check firewall statussudo firewall-cmd --state# View active zonessudo firewall-cmd --get-active-zones# Allow HTTP servicesudo firewall-cmd --permanent --add-service=http# Allow port 8080sudo firewall-cmd --permanent --add-port=8080/tcp# Reload rulessudo firewall-cmd --reload# List all rulessudo firewall-cmd --list-all
```

### Example

```
sudo firewall-cmd --permanent --add-service=httpssudo firewall-cmd --reload
```

Allows HTTPS traffic permanently.

---

## 3. Application Confinement with AppArmor

**AppArmor** is not a firewall. It is a security tool that restricts what applications can access.

### What it does

It limits:

- Files an application can read/write
- Network access
- System resources

If an application is hacked, AppArmor can reduce the damage.

### Common Commands

```
# Check AppArmor statussudo aa-status# Put profile in enforce modesudo aa-enforce /etc/apparmor.d/profile_name# Put profile in complain modesudo aa-complain /etc/apparmor.d/profile_name# Disable profilesudo aa-disable profile_name# Enable profilesudo aa-enable profile_name
```

### Example

```
sudo aa-status
```

Shows all active AppArmor profiles.

---

## 4. Choosing the Right Tool

| Tool          | Purpose                       | Best For                       |
| ------------- | ----------------------------- | ------------------------------ |
| **UFW**       | Simple firewall management    | Beginners, Ubuntu users        |
| **Firewalld** | Advanced zone-based firewall  | Enterprise and Red Hat systems |
| **AppArmor**  | Restrict application behavior | Application security           |

### Quick Summary

- **UFW** → Easy firewall with simple rules.
- **Firewalld** → Advanced firewall using zones.
- **AppArmor** → Protects the system by restricting applications.
- **UFW and Firewalld** control **network traffic**.
- **AppArmor** controls what **applications can do** after they start running.