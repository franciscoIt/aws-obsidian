#### Quick Comparison

||ALB|NLB|GWLB|CLB|
|---|---|---|---|---|
|**Layer**|7|4|3|4 & 7|
|**Protocol**|HTTP/HTTPS/gRPC|TCP/UDP/TLS|IP (GENEVE)|HTTP/TCP|
|**Use case**|Web apps, microservices|High perf, low latency|Security appliances|Legacy|
|**Static IP**|❌|✅|❌|❌|
|**WebSockets**|✅|✅|❌|❌|
|**Status**|Active|Active|Active|Legacy|
![[www.udemy.com_course_aws-certified-solutions-architect-associate-saa-c03_learn_lecture_13528090.png]]
#### 1. Application Load Balancer (ALB)

- Operates at **Layer 7** (HTTP/HTTPS)
- Routes traffic based on content — URL path, hostname, headers, query strings
- Supports **WebSockets**, HTTP/2, gRPC
- Ideal for microservices and container-based apps
- Integrates with AWS WAF, Cognito, Lambda

---

#### 2. Network Load Balancer (NLB)

- Operates at **Layer 4** (TCP/UDP/TLS)
- Handles **millions of requests per second** with ultra-low latency
- Preserves the client's source IP
- Supports static IPs and Elastic IPs per AZ
- Ideal for high-performance, latency-sensitive workloads
##### Targets:
- Ec2
- Private ip addresses
- ALB
- Health checks on **TCP,HTTP and HTTPS**
---

#### 3. Gateway Load Balancer (GWLB)

- Operates at **Layer 3** (IP packets) using the **GENEVE** protocol (port 6081)
- Designed to deploy, scale, and manage **third-party virtual appliances** (**firewalls, IDS/IPS, deep packet inspection**)
- Combines a transparent network gateway with load balancing
- Traffic is routed to appliances before reaching your app
![[www.udemy.com_course_aws-certified-solutions-architect-associate-saa-c03_learn_lecture_13528090 (2).png]]
Targets:
- EC2
- Private IP's

---

#### 4. Classic Load Balancer (CLB)

- Operates at **Layer 4 and Layer 7**
- The original AWS load balancer — now **legacy**
- Limited features compared to ALB/NLB
- AWS recommends migrating to ALB or NLB for all new workloads


Security groups:
![[www.udemy.com_course_aws-certified-solutions-architect-associate-saa-c03_learn_lecture_13528090 (1).png]]

# Sticky sessions
Are needed when an application stores **session state locally on the server** (e.g. in memory or local files), such as:

- Shopping cart contents
- User login state
- In-progress file uploads
- WebSocket connections
**Cookies**:
# - Custom 
- Application cookies > AWSAPP
- duration > AWSALB, AWSELB, 

# Cross zone load balancing
![[www.udemy.com_course_aws-certified-solutions-architect-associate-saa-c03_learn_lecture_18078093.png]]

![[www.udemy.com_course_aws-certified-solutions-architect-associate-saa-c03_learn_lecture_18078093 (1).png]]

 The load balancer uses an X.509 certificate (SSL/TLS server certificate)
- You can manage certificates using ACM (AWS Certificate Manager)
- You can create upload your own certificates alternatively
- HTTPS listener:
  - You must specify a default certificate
  - You can add an optional list of certs to support multiple domains
  - Clients can use SNI (Server Name Indication) to specify the hostname they reach
  - Ability to specify a security policy to support older versions of SSL / TLS (legacy clients)
# SNI 
SNI solves the problem of loading **multiple SSL certificates onto one web server** (to serve multiple websites)

It's a "newer" protocol, and requires the client to indicate the hostname of the target server in the initial SSL handshake

The server will then find the correct certificate, or return the default one

Note:

- Only works for ALB & NLB (newer generation), CloudFront
- Does not work for CLB (older gen)

# Connection draining 
When you deregister an instance from a load balancer (e.g., during a deployment, scale-in, or health check failure), any requests already in progress to that instance would be abruptly cut off without connection draining. This results in errors for those users.

### How It Works

1. You signal the load balancer to deregister a target (instance, container, Lambda, etc.)
2. The load balancer **stops sending new requests** to that target immediately
3. It **waits** for existing, in-flight connections to complete naturally
4. Once all connections finish (or the timeout is reached), the target is fully deregistered