# 🌐 AWS Networking Basics for Programmers (Hands-On)

> Based on the YouTube video: **[AWS Networking Basics For Programmers | Hands-On](https://www.youtube.com/watch?v=2doSoMN2xvI)**

This video simplifies essential AWS networking concepts for developers, focusing on **hands-on, practical learning**. It covers fundamental building blocks like **VPCs**, **subnets**, **gateways**, **route tables**, and **security layers**, making it easier for those from a programming background to grasp cloud networking.

---

## 📦 1. Virtual Private Cloud (VPC)

A **VPC** is like a **private, isolated network within AWS**—your own secure slice of the cloud.

- Think of it as a **fence** that separates your AWS resources from others.
- You define the **IPv4 CIDR block** (e.g., `10.0.0.0/16`) to specify the address range for your network.
- The video breaks down **CIDR** with beginner-friendly examples:
  - `/24`: 256 IP addresses (e.g., `10.0.1.0 - 10.0.1.255`)
  - `/16`: 65,536 IP addresses
  - `/8`: Over 16 million IPs
- The number after the slash shows how many **bits are fixed**, determining how large the IP pool is.

---

## 🧩 2. Subnets

**Subnets** divide your VPC into smaller, logical networks.

- Help with **security, traffic routing, and resource separation**.
- Two main types:
  - **Public subnet**: Accessible from the internet.
  - **Private subnet**: Internal use only (no direct internet access).
- Example:
  - VPC CIDR: `10.0.0.0/16`
  - Public subnet: `10.0.0.0/24`
  - Private subnet: `10.0.1.0/24`
- **Each subnet must reside in a single Availability Zone (AZ)**.
- You need **at least one subnet** to launch resources like EC2.
- For **high availability**, deploy across **multiple AZs**.

---

## 🌐 3. Gateways

Gateways allow your VPC to **connect to other networks**, such as the internet.

### 🚪 Internet Gateway (IGW)

- Provides **internet access** to resources inside a VPC.
- Must be:
  - **Created**
  - **Attached** to your VPC
  - **Referenced** in the route table of public subnets

### 🔁 NAT Gateway (Network Address Translation)

- Allows **outbound traffic** from **private subnets** (e.g., downloading software updates).
- Prevents **inbound traffic** from the internet.
- Must be:
  - Deployed in a **public subnet**
  - Allocated an **Elastic IP**
  - Referenced in the **private subnet’s route table**

---

## 🗺️ 4. Route Tables

Route tables control **how traffic moves within and outside** your VPC.

- Every VPC includes a **default/main route table**.
- Best practice:
  - **Separate route tables** for public and private subnets
- Subnets are explicitly **associated with route tables**.

### Routing Examples:

| Subnet Type     | Route to `0.0.0.0/0` | Target               |
|-----------------|----------------------|----------------------|
| Public Subnet   | Yes                  | Internet Gateway     |
| Private Subnet  | Yes                  | NAT Gateway          |

> `0.0.0.0/0` represents **all internet IPs** (any traffic going outside the VPC).

---

## 🔐 5. Security Groups (SG)

A **Security Group** is a **virtual firewall** applied at the instance level (e.g., EC2).

- **Stateful**: If inbound traffic is allowed, the return outbound traffic is automatically allowed.
- Rules define:
  - Inbound access (e.g., allow SSH on port 22)
  - Outbound access (if needed)
- Applied directly to **EC2 instances**, **RDS**, and other services.

---

## 🚧 6. Network ACLs (NACLs)

NACLs operate at the **subnet level** and act as another layer of security.

- **Stateless**: You must define **both** inbound and outbound rules explicitly.
- Use cases:
  - Blocking specific IP addresses
  - Tight control at the subnet level
- Most users **leave default NACL settings**, relying on Security Groups.

| Feature             | Security Group | NACL         |
|---------------------|----------------|--------------|
| Applies to          | Instance level | Subnet level |
| Stateful?           | Yes            | No           |
| Rules evaluated     | Allow only     | Allow & deny |

---

## 💡 Summary for DevOps Engineers

This video provides a **solid foundation in AWS networking**, ideal for:

- Setting up **secure and functional networks**
- Deploying applications across public/private subnets
- Managing traffic and access rules
- Preparing for more advanced setups

However, the video **does not cover** advanced topics like:

- **VPC Peering**
- **Transit Gateways**
- **Hybrid networking**

These are critical for complex, enterprise-scale DevOps roles and require further learning.

---

## ✅ What You Should Know After Watching

| Concept        | You Should Understand |
|----------------|-----------------------|
| VPC            | What it is, why CIDR matters |
| Subnets        | Public vs Private, high availability setup |
| Internet/NAT Gateways | When and where to use them |
| Route Tables   | How to direct traffic for subnets |
| Security Groups| Instance-level firewalls |
| NACLs          | Subnet-level rules, when to use them |

