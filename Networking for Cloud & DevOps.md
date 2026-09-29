# Real Networking for Cloud & DevOps
## Practical Networking Guide with Definitions, Real Examples, Commands, and Troubleshooting

> **Goal:** Learn the networking that a Cloud / DevOps Engineer actually uses in AWS, Azure, Linux servers, Kubernetes, CI/CD, and production applications.
>
> **Learning approach:** Understand the concept first, then see how it appears in a real cloud environment.

---

# 1. What Is Networking?

## Definition

**Networking is the process of connecting computers, servers, applications, and other devices so they can communicate and exchange data.**

In simple words:

> Networking answers three questions:
>
> 1. **Who am I?** → IP address
> 2. **Where do I need to go?** → Routing
> 3. **Am I allowed to go there?** → Firewall / security rules

### Real-world example

Imagine an office:

```text
Employee PC
    |
    v
Office Switch
    |
    v
Router
    |
    v
Internet
```

The employee's computer needs:
- an address
- a path to the destination
- permission to communicate

Cloud networking works on the same basic ideas.

### Cloud example

```text
User
 |
 | HTTPS :443
 v
Internet
 |
 v
AWS Load Balancer
 |
 | HTTP :8080
 v
EC2 Application Server
 |
 | TCP :1433
 v
RDS SQL Server
```

Every connection above is networking.

---

# 2. Why Networking Is Critical in Cloud

Cloud services do not exist in isolation.

A typical application may contain:

```text
                    Internet
                       |
                       v
                      DNS
                       |
                       v
                 Load Balancer
                       |
             +---------+---------+
             |                   |
             v                   v
           EC2-1               EC2-2
             |                   |
             +---------+---------+
                       |
                       v
                    RDS DB
```

Networking determines:

- Which server can communicate with which server
- Which ports are open
- Whether a server can access the Internet
- Whether users can access the application
- Whether one VPC/VNet can communicate with another
- Whether an application can reach a database
- How traffic is balanced
- How private services communicate
- How hybrid networks connect to cloud

If you understand networking, services such as:

- AWS VPC
- AWS ALB/NLB
- AWS Route 53
- AWS NAT Gateway
- AWS Transit Gateway
- Azure VNet
- Azure Load Balancer
- Azure Application Gateway
- Azure Private Endpoint
- VPN

become much easier to understand.

---

# 3. Network vs Internet

## Network

A **network** is a group of connected devices that can communicate.

Example:

```text
PC1 ----+
        |
PC2 ----+---- Switch
        |
PC3 ----+
```

## Internet

The **Internet** is a huge interconnected network of networks.

```text
Home Network
     |
     v
ISP
     |
     v
Internet
     |
     +------ AWS
     |
     +------ Azure
     |
     +------ Google
```

---

# 4. IP Address

## Definition

An **IP address** identifies a device or network interface on an IP network.

Example:

```text
192.168.1.10
```

Think of an IP address like a **building address**.

If you want to send something to a building, you need its address.

Similarly:

```text
Client: 192.168.1.10
Server: 192.168.1.20
```

The client can send traffic to the server's IP.

---

# 5. IPv4

IPv4 addresses contain four numbers separated by dots.

Example:

```text
10.0.1.25
```

Each section is called an **octet**.

```text
10 . 0 . 1 . 25
```

Each octet can normally range from:

```text
0 - 255
```

Therefore:

```text
10.0.1.25
```

is a valid IPv4 address.

---

# 6. Private IP Address

Private IP addresses are used inside private networks.

Common private IPv4 ranges:

```text
10.0.0.0/8

172.16.0.0/12

192.168.0.0/16
```

Example:

```text
EC2
Private IP: 10.0.1.20
```

The address is reachable inside the appropriate private network, but it is not directly reachable from the public Internet.

### Real example

A database should normally have a private IP:

```text
Internet
   X
   |
   |
Private Network
   |
   v
RDS
10.0.20.10
```

Users should not directly connect to the database from the Internet.

---

# 7. Public IP Address

A public IP is an address that can be routed on the public Internet.

Example:

```text
203.0.113.10
```

A server with a public IP may be reachable from the Internet if:

- routing allows it
- firewall/security rules allow it
- the application is listening
- the required port is open

Important:

> Having a public IP does NOT automatically mean the service is reachable.

---

# 8. MAC Address

## Definition

A **MAC address** identifies a network interface at the data-link layer.

Example:

```text
52:54:00:12:34:56
```

Think:

```text
IP address = logical address
MAC address = network-interface hardware address
```

On local Ethernet networks, devices use MAC addresses for local delivery.

### Linux

```bash
ip link
```

Example:

```text
2: eth0:
    link/ether 52:54:00:12:34:56
```

---

# 9. Network Interface

A **network interface** is the interface through which a machine connects to a network.

Linux example:

```bash
ip addr
```

You may see:

```text
eth0
ens5
ens160
```

AWS EC2 uses an **Elastic Network Interface (ENI)**.

An ENI can have:

- private IP
- security groups
- MAC address
- public IP association
- additional private IPs

---

# 10. Port

## Definition

A **port** identifies a service/application endpoint on a machine.

An IP identifies the machine/interface.

A port identifies the service.

Example:

```text
10.0.1.20:80
```

means:

```text
IP   = 10.0.1.20
Port = 80
```

### Common ports

| Port | Protocol/Service | Common use |
|---:|---|---|
| 22 | SSH | Linux administration |
| 53 | DNS | Name resolution |
| 80 | HTTP | Web |
| 443 | HTTPS | Secure web |
| 25 | SMTP | Email |
| 3306 | MySQL | Database |
| 5432 | PostgreSQL | Database |
| 1433 | MS SQL | SQL Server |
| 6379 | Redis | Cache |
| 8080 | HTTP | Common application port |
| 9090 | HTTP | Prometheus |
| 3000 | HTTP | Node.js/Grafana/common dev apps |

---

# 11. Socket

A socket is commonly represented as:

```text
IP + Port + Protocol
```

Example:

```text
10.0.1.20:443/TCP
```

A client connection can look like:

```text
Client
192.168.1.10:51520
       |
       | TCP
       v
Server
10.0.1.20:443
```

The client uses a temporary source port such as `51520`.

---

# 12. TCP

## Definition

**TCP (Transmission Control Protocol)** is a connection-oriented transport protocol.

TCP provides mechanisms for:

- reliable delivery
- ordering
- retransmission
- connection management

### Real example

HTTPS normally uses TCP:

```text
Client
  |
  | TCP connection
  v
Server :443
```

### TCP 3-Way Handshake

Conceptually:

```text
Client                  Server
  |                       |
  |------ SYN ----------->|
  |<----- SYN/ACK --------|
  |------ ACK ----------->|
  |                       |
  |   Connection ready    |
```

### Linux troubleshooting

```bash
ss -tulnp
```

or:

```bash
ss -tan
```

---

# 13. UDP

## Definition

**UDP (User Datagram Protocol)** is a connectionless transport protocol.

It has less overhead than TCP.

Common examples:

- DNS
- DHCP
- streaming
- some real-time applications

Example:

```text
Client
  |
  | UDP :53
  v
DNS Server
```

---

# 14. TCP vs UDP

| TCP | UDP |
|---|---|
| Connection-oriented | Connectionless |
| Reliable delivery | No built-in delivery guarantee |
| Ordered data | No ordering guarantee |
| More overhead | Lower overhead |
| HTTP/HTTPS commonly use TCP | DNS commonly uses UDP |
| Database connections commonly use TCP | Real-time traffic may use UDP |

Do not memorize this only for interviews. Understand why an application chooses one.

---

# 15. Subnet

## Definition

A **subnet** is a logical subdivision of an IP network.

Example:

```text
VPC
10.0.0.0/16

        |
        +---- 10.0.1.0/24
        |
        +---- 10.0.2.0/24
        |
        +---- 10.0.10.0/24
        |
        +---- 10.0.20.0/24
```

Why create subnets?

To separate workloads.

Example:

```text
Public Subnet
    |
    +-- Load Balancer

Private App Subnet
    |
    +-- EC2

Private DB Subnet
    |
    +-- Database
```

---

# 16. CIDR

## Definition

CIDR stands for **Classless Inter-Domain Routing**.

Example:

```text
10.0.0.0/16
```

The `/16` tells us how many bits belong to the network portion.

### Common examples

```text
/16
/20
/24
/26
/27
/28
```

For a normal IPv4 network:

| CIDR | Total addresses |
|---|---:|
| /16 | 65,536 |
| /20 | 4,096 |
| /24 | 256 |
| /25 | 128 |
| /26 | 64 |
| /27 | 32 |
| /28 | 16 |

For cloud subnet design, you should be comfortable calculating these.

---

# 17. CIDR Real Cloud Example

Suppose we create:

```text
VPC
10.0.0.0/16
```

We divide it:

```text
10.0.0.0/16
       |
       +---- Public subnet
       |     10.0.1.0/24
       |
       +---- Public subnet
       |     10.0.2.0/24
       |
       +---- Private app
       |     10.0.10.0/24
       |
       +---- Private database
             10.0.20.0/24
```

This is a real pattern used in cloud architectures.

---

# 18. Default Gateway

## Definition

A **default gateway** is the router used when the destination is outside the local network.

Example:

```text
Client
192.168.1.10
     |
     v
Gateway
192.168.1.1
     |
     v
Internet
```

If the client wants to reach:

```text
8.8.8.8
```

it sends traffic toward the gateway because `8.8.8.8` is not in its local subnet.

---

# 19. Routing

## Definition

**Routing determines where network traffic should go.**

A router uses routing information to choose the next path.

Think of a routing table as a GPS for network traffic.

### Linux

```bash
ip route
```

Example:

```text
10.0.0.0/16 dev eth0
default via 10.0.1.1 dev eth0
```

Meaning:

```text
10.0.0.0/16
    |
    +--> local network

Everything else
    |
    +--> 10.0.1.1
```

---

# 20. Cloud Route Table

AWS route tables are a critical networking concept.

Example:

```text
Destination       Target
--------------------------------
10.0.0.0/16       local
0.0.0.0/0         igw-xxxxx
```

Meaning:

```text
10.0.0.0/16
   |
   +--> stay inside VPC

0.0.0.0/0
   |
   +--> Internet Gateway
```

A private subnet might instead use:

```text
Destination       Target
--------------------------------
10.0.0.0/16       local
0.0.0.0/0         NAT Gateway
```

---

# 21. 0.0.0.0/0

## Definition

`0.0.0.0/0` represents all IPv4 destinations.

Example:

```text
0.0.0.0/0 -> Internet Gateway
```

means:

> For destinations not covered by a more specific route, send traffic to the Internet Gateway.

This is called a **default route**.

---

# 22. Longest Prefix Match

If multiple routes match a destination, the most specific route wins.

Example:

```text
10.0.0.0/16 -> Route A
10.0.10.0/24 -> Route B
0.0.0.0/0 -> Route C
```

Destination:

```text
10.0.10.50
```

Matches all three, but:

```text
10.0.10.0/24
```

is the most specific.

Therefore Route B is selected.

This is important for troubleshooting cloud routes.

---

# 23. Internet Gateway

## AWS

An **Internet Gateway (IGW)** provides a path between a VPC and the Internet.

Typical architecture:

```text
Internet
    |
    v
Internet Gateway
    |
    v
Public Subnet
    |
    v
EC2
```

For an instance to be Internet reachable, multiple conditions must align:

```text
Public IP
+
Route to IGW
+
Security Group allows traffic
+
Network ACL allows traffic
+
Application listening
```

An Internet Gateway alone does not make an instance public.

---

# 24. NAT Gateway

## Definition

NAT allows private resources to communicate outward without directly exposing them to inbound Internet connections.

Typical AWS design:

```text
                    Internet
                       |
                       v
                Internet Gateway
                       |
                       v
                NAT Gateway
                       |
                       v
                Private Subnet
                       |
                       v
                     EC2
```

Example:

A private EC2 needs:

```text
apt update
```

It can use:

```text
EC2
 |
 v
NAT Gateway
 |
 v
Internet
```

But the Internet cannot normally initiate a new connection directly to that private EC2 through the NAT path.

---

# 25. NAT vs Internet Gateway

### Public subnet

```text
EC2
 |
 v
Route Table
 |
 v
Internet Gateway
 |
 v
Internet
```

### Private subnet

```text
EC2
 |
 v
Route Table
 |
 v
NAT Gateway
 |
 v
Internet Gateway
 |
 v
Internet
```

The NAT Gateway is placed in a public subnet.

---

# 26. DNS

## Definition

**DNS (Domain Name System) translates names into IP addresses and provides other DNS information.**

Instead of remembering:

```text
142.250.x.x
```

users use:

```text
example.com
```

Conceptually:

```text
example.com
      |
      v
DNS
      |
      v
IP address
```

---

# 27. Common DNS Records

| Record | Purpose |
|---|---|
| A | Name → IPv4 |
| AAAA | Name → IPv6 |
| CNAME | Alias to another hostname |
| MX | Mail server |
| TXT | Text/verification information |
| NS | Authoritative name servers |

### Example

```text
app.example.com
      |
      | A record
      v
203.0.113.20
```

---

# 28. DNS Troubleshooting

Useful commands:

```bash
dig example.com
```

```bash
nslookup example.com
```

```bash
dig +short example.com
```

Example troubleshooting:

```text
Browser
  |
  X
DNS resolution fails
```

Before checking the application, check DNS.

---

# 29. HTTP and HTTPS

## HTTP

HTTP is an application-layer protocol used by web applications.

Example:

```text
GET /users
POST /login
```

HTTP normally uses:

```text
TCP :80
```

## HTTPS

HTTPS is HTTP protected using TLS.

Normally:

```text
TCP :443
```

Example:

```text
User
 |
 | HTTPS :443
 v
ALB
 |
 | HTTP :8080
 v
Application
```

The ALB may terminate TLS and forward HTTP internally.

---

# 30. TLS / SSL

TLS provides encryption and authentication for network communication.

Typical flow:

```text
Browser
   |
   | HTTPS
   v
Load Balancer
   |
   | HTTPS or HTTP
   v
Application
```

A certificate proves the identity of the hostname and enables encrypted communication.

For DevOps, understand:

- certificates
- private/public keys
- certificate expiration
- TLS termination
- HTTPS
- port 443

---

# 31. Firewall

## Definition

A firewall controls network traffic according to rules.

Example:

```text
Internet
   |
   | TCP 443
   v
Firewall
   |
   +---- Allow
   |
   v
Web Server
```

If port 22 is not allowed:

```text
Internet
   |
   | TCP 22
   X
Firewall
```

SSH fails.

---

# 32. AWS Security Group

A **Security Group (SG)** acts as a virtual firewall for supported AWS resources.

Example:

```text
ALB SG
Allow TCP 443 from 0.0.0.0/0

EC2 SG
Allow TCP 8080 from ALB SG

RDS SG
Allow TCP 1433 from EC2 SG
```

Traffic:

```text
Internet
   |
   | 443
   v
ALB
   |
   | 8080
   v
EC2
   |
   | 1433
   v
RDS
```

This is much safer than:

```text
RDS
Allow 1433 from 0.0.0.0/0
```

---

# 33. AWS Network ACL

A **Network ACL (NACL)** controls traffic at the subnet level.

Think:

```text
Security Group
= resource/network-interface level

NACL
= subnet level
```

NACLs are stateless, so return traffic must be explicitly allowed by the rules.

For most everyday AWS application work, understand the distinction and troubleshooting behavior before trying to memorize every rule.

---

# 34. Security Group vs NACL

| Security Group | NACL |
|---|---|
| Resource/interface level | Subnet level |
| Stateful | Stateless |
| Return traffic automatically allowed when connection is allowed | Return traffic must be explicitly permitted |
| Supports allow rules | Supports allow and deny rules |
| Common first layer for EC2/RDS design | Additional subnet-level control |

---

# 35. Load Balancer

## Definition

A load balancer distributes traffic across multiple backend servers.

Without load balancing:

```text
Users
  |
  v
EC2-1
```

With load balancing:

```text
                 +--> EC2-1
Users -> ALB ----+
                 +--> EC2-2
                 |
                 +--> EC2-3
```

Benefits include:

- distributing traffic
- health checks
- high availability
- TLS termination
- removing unhealthy instances from service

---

# 36. Health Check

A load balancer checks whether backend servers are healthy.

Example:

```text
ALB
 |
 | GET /health
 v
EC2
```

If:

```text
HTTP 200
```

the target may be considered healthy.

If:

```text
HTTP 500
```

or connection fails repeatedly, it may be marked unhealthy.

### Real troubleshooting case

Application is running:

```bash
systemctl status myapp
```

shows:

```text
active (running)
```

But ALB says:

```text
Unhealthy
```

Possible causes:

- wrong target port
- wrong health-check path
- application only listening on `127.0.0.1`
- security group blocks ALB
- application returns 500
- NACL problem

---

# 37. Reverse Proxy

A reverse proxy receives requests and forwards them to backend services.

Example with Nginx:

```text
Internet
   |
   | :80 / :443
   v
Nginx
   |
   | :4042
   v
.NET Application
```

Nginx can provide:

- reverse proxy
- TLS termination
- routing
- static file serving
- basic load balancing

---

# 38. Real Nginx + .NET Example

Suppose your .NET application listens on:

```text
127.0.0.1:4042
```

Nginx listens on:

```text
0.0.0.0:80
```

Traffic:

```text
Client
 |
 | HTTP :80
 v
Nginx
 |
 | HTTP :4042
 v
.NET
```

Check listening ports:

```bash
ss -lntp
```

Test application directly:

```bash
curl http://127.0.0.1:4042
```

Test through Nginx:

```bash
curl http://localhost
```

This is a real DevOps networking scenario.

---

# 39. Network Interface Binding

This is a very important Linux/cloud troubleshooting concept.

An application can listen on:

```text
127.0.0.1:8080
```

or:

```text
0.0.0.0:8080
```

### 127.0.0.1

Only locally accessible:

```text
Server
 |
 +--> localhost
```

External machines cannot directly connect.

### 0.0.0.0

Listen on all IPv4 interfaces:

```text
eth0
ens5
localhost
...
```

Example:

```text
Application
0.0.0.0:8080
```

Now another host may connect if networking and firewall rules permit it.

---

# 40. Private vs Public Architecture

A common cloud architecture:

```text
                         Internet
                            |
                            v
                           ALB
                            |
              +-------------+-------------+
              |                           |
              v                           v
        Private EC2 #1             Private EC2 #2
              |                           |
              +-------------+-------------+
                            |
                            v
                          RDS
```

Notice:

- ALB is Internet-facing
- EC2 is private
- RDS is private

This reduces direct Internet exposure.

---

# 41. VPC

## Definition

An AWS **VPC (Virtual Private Cloud)** is an isolated virtual network where you can place AWS resources.

Example:

```text
VPC
10.0.0.0/16
 |
 +-- Public subnet
 |
 +-- Private application subnet
 |
 +-- Private database subnet
```

A VPC contains networking components such as:

- subnets
- route tables
- gateways
- security controls
- network interfaces

---

# 42. Azure VNet

Azure's equivalent is the **Virtual Network (VNet)**.

Conceptually:

```text
AWS                         Azure

VPC                         VNet
Subnet                      Subnet
Route Table                 Route Table
Security Group              NSG
Internet Gateway            Internet connectivity
NAT Gateway                 NAT Gateway
VPC Peering                 VNet Peering
```

The names differ, but the networking concepts are closely related.

---

# 43. AWS VPC vs Azure VNet

| Networking concept | AWS | Azure |
|---|---|---|
| Virtual network | VPC | VNet |
| Subnet | Subnet | Subnet |
| Instance/server | EC2 | VM |
| Firewall concept | Security Group | NSG |
| Managed NAT | NAT Gateway | NAT Gateway |
| Private connectivity | VPC Peering / Transit Gateway | VNet Peering / Virtual WAN |
| Private service access | PrivateLink | Private Endpoint |
| DNS | Route 53 | Azure DNS |

The goal is to understand the networking concept first, then learn the cloud provider's implementation.

---

# 44. VPC Peering / VNet Peering

## Definition

Peering allows two private networks to communicate directly.

Example:

```text
VPC-A
10.0.0.0/16
     |
     | Peering
     |
VPC-B
10.1.0.0/16
```

A route is still required.

For example:

```text
VPC-A route:
10.1.0.0/16 -> Peering

VPC-B route:
10.0.0.0/16 -> Peering
```

Important:

> Creating peering alone does not automatically create all required routes.

---

# 45. VPN

## Definition

A VPN creates an encrypted connection over an untrusted network.

Example:

```text
Company Data Center
       |
       | Encrypted VPN
       |
       v
     Internet
       |
       v
     AWS VPC
```

This is useful for hybrid cloud.

Example:

```text
On-Prem
192.168.0.0/16
      |
      | VPN
      v
AWS
10.0.0.0/16
```

Servers can communicate using private IP ranges when routing and security policies allow it.

---

# 46. Hybrid Cloud Networking

A company may have:

```text
On-Premises
      |
      | VPN / Direct Connect / ExpressRoute
      |
      v
Cloud
```

Example:

```text
Office
192.168.0.0/16
      |
      v
VPN
      |
      v
AWS VPC
10.0.0.0/16
      |
      v
Private EC2
```

This is common in enterprise environments.

---

# 47. Proxy

A proxy acts as an intermediary.

### Forward proxy

```text
Client
  |
  v
Proxy
  |
  v
Internet
```

The client uses the proxy to access external services.

### Reverse proxy

```text
Internet
   |
   v
Reverse Proxy
   |
   v
Backend
```

Nginx and many load balancers can perform reverse-proxy functions.

---

# 48. Network Address Translation

NAT changes source or destination addressing during traffic translation.

Common concepts:

```text
SNAT = Source NAT
DNAT = Destination NAT
```

Typical private server outbound flow:

```text
Private EC2
10.0.10.20
    |
    | SNAT
    v
NAT Gateway
    |
    v
Internet
```

The external service does not directly see the private `10.0.10.20` address.

---

# 49. DHCP

## Definition

DHCP automatically provides network configuration.

Typical information can include:

- IP address
- subnet information
- default gateway
- DNS server

Without DHCP, administrators would need to configure many network settings manually.

Cloud networks often provide managed DHCP/network configuration behavior.

---

# 50. ARP

## Definition

ARP helps IPv4 devices discover the MAC address associated with an IP address on a local network.

Example:

```text
Host A
192.168.1.10
    |
    | Who has 192.168.1.20?
    v
Host B
192.168.1.20
MAC: AA:BB:CC:DD:EE:FF
```

Linux:

```bash
ip neigh
```

ARP is more important for understanding how networking works than for day-to-day AWS configuration.

---

# 51. OSI Model

You should understand the OSI model conceptually.

```text
7  Application     HTTP, DNS
6  Presentation    TLS / encoding
5  Session
4  Transport       TCP / UDP
3  Network         IP / routing
2  Data Link       Ethernet / MAC
1  Physical        cables / signals
```

For Cloud/DevOps, spend the most practical time on:

```text
Layer 7 -> HTTP/DNS
Layer 4 -> TCP/UDP/ports
Layer 3 -> IP/routing
Layer 2 -> MAC/ARP
```

---

# 52. TCP/IP Model

The TCP/IP model is often easier for practical troubleshooting.

```text
Application
    |
Transport
    |
Internet
    |
Network Access
```

Example:

```text
HTTPS
 |
TCP :443
 |
IP
 |
Ethernet
```

---

# 53. Real Troubleshooting Method

When an application is not reachable, do not randomly change settings.

Use layers.

### Step 1: Is the process running?

```bash
systemctl status myapp
```

### Step 2: Is the port listening?

```bash
ss -lntp
```

### Step 3: Can the local machine reach it?

```bash
curl http://127.0.0.1:8080
```

### Step 4: Can another machine reach it?

```bash
curl http://10.0.1.20:8080
```

### Step 5: Is the route correct?

```bash
ip route
```

### Step 6: Is DNS working?

```bash
dig app.example.com
```

### Step 7: Is the firewall allowing traffic?

Check:

```text
Security Group
NACL
OS firewall
Application firewall
```

### Step 8: Is the load balancer healthy?

Check:

```text
Target group
Health check
Target port
Health path
```

---

# 54. Useful Linux Networking Commands

## Show IP addresses

```bash
ip addr
```

## Show interfaces

```bash
ip link
```

## Show routing table

```bash
ip route
```

## Show listening ports

```bash
ss -lntp
```

## Show TCP connections

```bash
ss -tan
```

## Test connectivity

```bash
ping 8.8.8.8
```

## Test a TCP port

```bash
nc -vz 10.0.1.20 443
```

## Test HTTP

```bash
curl -v https://example.com
```

## DNS lookup

```bash
dig example.com
```

## Alternative DNS tool

```bash
nslookup example.com
```

## Trace route

```bash
traceroute 8.8.8.8
```

## Show neighbor table

```bash
ip neigh
```

## Capture packets

```bash
sudo tcpdump -i eth0 port 443
```

---

# 55. Real Example: Website Not Working

Suppose users report:

```text
https://app.example.com
```

is not working.

Do this:

### 1. DNS

```bash
dig app.example.com
```

If DNS fails:

```text
DNS problem
```

### 2. Test the load balancer

```bash
curl -vk https://app.example.com
```

### 3. Check target health

```text
ALB
 |
 +-- EC2-1: unhealthy
 +-- EC2-2: unhealthy
```

### 4. Check application

On EC2:

```bash
systemctl status myapp
```

### 5. Check listening port

```bash
ss -lntp
```

Maybe the application is listening on:

```text
127.0.0.1:4042
```

while the load balancer is trying:

```text
10.0.10.20:4042
```

The application binding is wrong.

### 6. Check Security Group

```text
EC2 SG
Allow 4042 from ALB SG
```

### 7. Check NACL/routes

Only after checking the common causes.

---

# 56. Real Example: EC2 Cannot Access Internet

Suppose:

```bash
sudo apt update
```

fails.

Check:

### IP

```bash
ip addr
```

### Route

```bash
ip route
```

You should have something conceptually like:

```text
default via ...
```

### DNS

```bash
dig archive.ubuntu.com
```

### Route table

In AWS:

```text
Private subnet
0.0.0.0/0 -> NAT Gateway
```

### NAT

Verify the NAT Gateway exists and is available.

### Security Group

Outbound traffic must be permitted according to the security configuration.

### NACL

Check subnet-level rules if necessary.

---

# 57. Real Example: EC2 Can Ping but Application Fails

Do not assume that:

```text
ping works
```

means:

```text
application works
```

Ping uses ICMP.

Your application may use TCP.

Example:

```bash
ping 10.0.1.20
```

works.

But:

```bash
nc -vz 10.0.1.20 8080
```

fails.

Possible causes:

- application not listening
- port blocked
- wrong IP
- wrong security group
- wrong NACL
- application bound to localhost

---

# 58. Real Example: DNS Works but Website Fails

You run:

```bash
dig app.example.com
```

and receive:

```text
203.0.113.20
```

DNS is working.

But:

```bash
curl -v https://app.example.com
```

fails.

Now investigate:

```text
DNS
  |
  | OK
  v
IP
  |
  | ?
  v
Load Balancer
  |
  | ?
  v
Backend
```

This prevents wasting time changing DNS when DNS is already working.

---

# 59. Real Example: SSH Doesn't Work

You try:

```bash
ssh ubuntu@203.0.113.20
```

Troubleshoot:

```text
1. Does server have public IP?
2. Is route to Internet Gateway correct?
3. Does Security Group allow TCP 22?
4. Does NACL allow traffic?
5. Is sshd running?
6. Is port 22 listening?
7. Is the username correct?
8. Is the key correct?
9. Is the OS firewall blocking SSH?
```

On Linux:

```bash
sudo systemctl status ssh
```

```bash
sudo ss -lntp | grep :22
```

---

# 60. Network Troubleshooting Decision Tree

```text
Application unreachable
        |
        v
DNS resolves?
   /          \
 NO            YES
 |              |
Fix DNS         v
             IP reachable?
             /       \
           NO         YES
           |           |
       Check route     v
                    Port open?
                    /       \
                  NO         YES
                  |           |
             Check firewall   v
                            App working?
                            /       \
                          NO         YES
                          |           |
                     Check app       Done
                     logs/binding
```

This approach is much better than randomly changing cloud settings.

---

# 61. Kubernetes Networking — Why It Matters

Kubernetes adds another networking layer.

Conceptually:

```text
Internet
   |
   v
Load Balancer
   |
   v
Ingress
   |
   v
Service
   |
   v
Pod
```

You should understand:

- Pod IP
- Service IP
- ClusterIP
- NodePort
- LoadBalancer
- Ingress
- DNS inside cluster
- NetworkPolicy

The underlying networking fundamentals remain the same:

```text
IP
+
Port
+
Routing
+
DNS
+
Firewall
```

---

# 62. Container Networking

Docker also uses networking.

Example:

```text
Host
 |
 +---- Docker network
         |
         +---- Container A
         |
         +---- Container B
```

Commands:

```bash
docker network ls
```

```bash
docker network inspect bridge
```

A container may have its own private IP.

Example:

```text
Container
172.17.0.2:3000
```

Port publishing:

```bash
docker run -p 8080:3000 myapp
```

means:

```text
Host :8080
     |
     v
Container :3000
```

---

# 63. CI/CD Networking Example

A Jenkins pipeline may need to communicate with a Docker server.

```text
GitHub
   |
   v
Jenkins
   |
   | SSH / Docker API
   v
Docker Server
   |
   v
Container
```

If Jenkins cannot deploy:

Check:

```text
DNS
+
Route
+
TCP 22
+
SSH key
+
Security Group
+
Docker socket/daemon
```

This shows why networking is part of DevOps, not just network administration.

---

# 64. AWS Production-Style Architecture

A simplified architecture:

```text
                         Internet
                            |
                            v
                         Route 53
                            |
                            v
                      CloudFront
                            |
                            v
                    Application Load
                       Balancer
                            |
              +-------------+-------------+
              |                           |
              v                           v
        Private EC2 #1             Private EC2 #2
              |                           |
              +-------------+-------------+
                            |
                            v
                    Private Database
                           RDS
```

Supporting networking:

```text
VPC
 |
 +-- Public Subnets
 |      |
 |      +-- ALB
 |      +-- NAT Gateway
 |
 +-- Private App Subnets
 |      |
 |      +-- EC2
 |
 +-- Private DB Subnets
        |
        +-- RDS
```

Security:

```text
Internet
   |
   | 443
   v
ALB SG
   |
   | 8080
   v
EC2 SG
   |
   | 1433
   v
RDS SG
```

This is the type of architecture a Cloud/DevOps Engineer should be able to explain.

---

# 65. The Most Important Cloud Networking Concepts

If you are short on time, prioritize these:

```text
1. IP addresses
2. Private vs public IP
3. CIDR
4. Subnets
5. Routing
6. Route tables
7. Default gateway
8. TCP/UDP
9. Ports
10. DNS
11. NAT
12. Internet Gateway
13. Security Groups
14. NACL
15. Load Balancers
16. HTTP/HTTPS
17. TLS
18. VPC/VNet
19. VPN
20. Network troubleshooting
```

---

# 66. What You Don't Need to Master First

For a Cloud/DevOps-focused path, don't spend your first weeks going extremely deep into:

```text
STP
VLAN configuration
Cisco-specific CLI
Advanced BGP
IS-IS
OSPF internals
MPLS
Carrier networking
Physical cabling
```

You should understand their basic purpose eventually, especially if working with hybrid/enterprise networks, but they are not the first priority for cloud application infrastructure.

---

# 67. Practical Learning Path

## Phase 1 — Fundamentals

Learn:

```text
IP
MAC
Ports
TCP
UDP
DNS
HTTP
HTTPS
Subnet
CIDR
Gateway
```

Practice on Linux.

---

## Phase 2 — Routing

Learn:

```text
Route
Route table
Default route
NAT
SNAT
DNAT
Internet Gateway
NAT Gateway
```

Practice:

```bash
ip route
```

---

## Phase 3 — AWS Networking

Build:

```text
VPC
 |
 +-- Public Subnet
 |      |
 |      +-- EC2
 |
 +-- Private Subnet
        |
        +-- EC2
```

Then add:

```text
IGW
NAT Gateway
Route Tables
Security Groups
NACL
```

---

## Phase 4 — Application Networking

Build:

```text
DNS
 |
 v
ALB
 |
 v
EC2
 |
 v
RDS
```

Learn:

```text
DNS
TLS
HTTP
HTTPS
Health checks
Reverse proxy
Load balancing
```

---

## Phase 5 — Advanced Cloud Networking

Learn:

```text
VPC Peering
VNet Peering
Transit Gateway
VPN
PrivateLink
Private Endpoint
Hybrid Cloud
BGP basics
Cloud WAN concepts
```

---

# 68. Hands-On Lab Roadmap

## Lab 1 — Linux Network Basics

Run:

```bash
ip addr
ip link
ip route
ss -lntp
ip neigh
```

Goal:

> Understand your Linux machine's network configuration.

---

## Lab 2 — Web Server

Install Nginx:

```bash
sudo apt update
sudo apt install nginx -y
```

Check:

```bash
ss -lntp | grep :80
```

Test:

```bash
curl http://localhost
```

Goal:

> Understand application → port → network interface.

---

## Lab 3 — Two EC2 Instances

Create:

```text
EC2-A
10.0.1.10

EC2-B
10.0.2.10
```

Test:

```bash
ping 10.0.2.10
```

Then test a TCP port:

```bash
nc -vz 10.0.2.10 80
```

Goal:

> Understand private network communication.

---

## Lab 4 — Public and Private Subnets

Create:

```text
VPC
10.0.0.0/16

Public
10.0.1.0/24

Private
10.0.10.0/24
```

Add:

```text
IGW
NAT Gateway
Route Tables
```

Goal:

> Understand public vs private networking.

---

## Lab 5 — ALB

Build:

```text
Internet
   |
   v
ALB
   |
   +-- EC2-1
   |
   +-- EC2-2
```

Configure:

```text
Listener :80
Target :8080
Health check
```

Goal:

> Understand load balancing.

---

## Lab 6 — Private Database

Build:

```text
Internet
   |
   v
ALB
   |
   v
EC2
   |
   v
RDS
```

Allow:

```text
ALB -> EC2
EC2 -> RDS
```

Do NOT allow:

```text
Internet -> RDS
```

Goal:

> Understand layered security.

---

# 69. Interview Questions You Should Be Able to Answer

### Q1. What is an IP address?

An address used to identify a network interface on an IP network.

### Q2. What is a subnet?

A logical subdivision of an IP network.

### Q3. What is CIDR?

A notation used to represent IP network ranges, such as `10.0.0.0/16`.

### Q4. What is a route table?

A set of routing rules that determines where traffic should be sent.

### Q5. What is a default route?

A route used when no more specific route matches, commonly `0.0.0.0/0` for IPv4.

### Q6. What is NAT?

Network Address Translation changes address information as traffic passes through a NAT device.

### Q7. Why use a NAT Gateway?

To allow private resources to initiate outbound Internet connections without giving them direct public Internet exposure.

### Q8. What is a Security Group?

An AWS virtual firewall associated with supported resources/network interfaces.

### Q9. Security Group vs NACL?

Security Groups are stateful and operate at the resource/interface level; NACLs are stateless and operate at the subnet level.

### Q10. What happens when you type a website URL?

Simplified:

```text
URL
 |
 v
DNS resolution
 |
 v
IP address
 |
 v
TCP connection
 |
 v
TLS handshake for HTTPS
 |
 v
HTTP request
 |
 v
Load Balancer / Web Server
 |
 v
Application
```

### Q11. Why can an application work locally but not remotely?

Possible reasons:

```text
Application bound to 127.0.0.1
+
Firewall
+
Security Group
+
NACL
+
Wrong route
+
Wrong port
+
Wrong IP
```

### Q12. Why can ping work while HTTP fails?

Because ping uses ICMP while HTTP normally uses TCP port 80/443. They are different protocols and can be filtered independently.

---

# 70. Final Mental Model

Whenever you troubleshoot cloud networking, think:

```text
        WHO?
         |
      IP Address
         |
        WHERE?
         |
       Route
         |
       WHICH?
         |
        Port
         |
       ALLOWED?
         |
      Firewall
         |
       SERVICE?
         |
    Application
```

Or remember:

```text
IP
 ↓
Subnet
 ↓
Route
 ↓
Gateway
 ↓
Port
 ↓
Firewall
 ↓
Application
```

If you can explain these clearly, you have the foundation needed for AWS and Azure networking.

---

# 71. Cloud Networking Cheat Sheet

```text
IP Address
    = identifies a network interface

Subnet
    = smaller network inside a larger network

CIDR
    = notation for an IP network range

Port
    = identifies a service/application endpoint

TCP
    = reliable, connection-oriented transport

UDP
    = connectionless transport

DNS
    = translates names into network information

Route
    = tells traffic where to go

Route Table
    = collection of routing rules

Gateway
    = path to another network

Internet Gateway
    = VPC-to-Internet connectivity component

NAT Gateway
    = allows private resources to initiate outbound Internet traffic

Security Group
    = stateful resource/interface-level firewall in AWS

NACL
    = stateless subnet-level network access control in AWS

Load Balancer
    = distributes traffic across backend targets

VPC
    = AWS virtual network

VNet
    = Azure virtual network

VPN
    = encrypted network connection over an untrusted network

Reverse Proxy
    = receives client requests and forwards them to backend services
```

---

# 72. The Real Goal

Do not try to memorize networking commands.

Your real goal is to be able to look at this:

```text
Internet
   |
 Route 53
   |
 CloudFront
   |
 ALB :443
   |
 EC2 :8080
   |
 RDS :1433
```

and explain:

```text
1. How does the user find the application?
2. How does DNS work?
3. How does traffic reach the ALB?
4. Which port is used?
5. How does the ALB reach EC2?
6. Which route is used?
7. Which security group allows it?
8. Why is EC2 private?
9. How does EC2 reach RDS?
10. Why can't the Internet directly reach RDS?
11. How does EC2 access the Internet if it has no public IP?
12. What would you check if the ALB says "unhealthy"?
```

If you can answer these questions and prove the answers using Linux/AWS tools, you have **practical Cloud networking knowledge**, not just networking theory.

---

## Recommended Study Order

```text
Networking Fundamentals
        ↓
IP + CIDR + Subnets
        ↓
TCP/UDP + Ports
        ↓
Routing
        ↓
DNS
        ↓
NAT
        ↓
Firewalls
        ↓
AWS VPC
        ↓
AWS ALB
        ↓
AWS RDS Networking
        ↓
VPN / Peering
        ↓
Azure VNet
        ↓
Kubernetes Networking
        ↓
Cloud Troubleshooting
```

**Rule to remember:**

> **Cloud networking is the same fundamental networking you already know, implemented through software-defined cloud services.**
