# AWS High Availability Lab

This lab demonstrates a simple **high-availability web architecture** in AWS across two Availability Zones (AZs) using public/private subnets, EC2 instances, NAT Gateway, and an Application Load Balancer (ALB).  

---

## **Lab Overview**

- **VPC**: 10.0.0.0/16  
- **Subnets**:  
  - Public Subnet A: 10.0.1.0/24 (AZ1)  
  - Public Subnet B: 10.0.2.0/24 (AZ2)  
  - Private Subnet A: 10.0.3.0/24 (AZ1)  
  - Private Subnet B: 10.0.4.0/24 (AZ2)  

- **EC2 Instances**:  
  - Frontend EC2 in each public subnet  
  - Backend EC2 in each private subnet  
  - Frontends use reverse proxy to access local backend (port 5000)
  - Bastion host between public and private EC2s

- **NAT Gateway**:  
  - Deployed in public subnet to allow private backend instances to access the internet  

- **ALB (Application Load Balancer)**:  
  - Public ALB in both public subnets  
  - Listens on HTTP port 80  
  - Routes traffic to frontend EC2s across AZs  
  - Provides high availability and automatic failover  

- **Security Groups**:  
  - **ALB SG**: inbound HTTP 80 from anywhere, outbound to frontend SG  
  - **Frontend SG**: inbound HTTP 80 from ALB SG, outbound all  
  - **Backend SG**: inbound TCP 5000 from frontend SG, outbound all  

---

## **Traffic Flow**

Browser
->
ALB (public)
->
Frontend EC2s (AZ1 or AZ2)
->
Backend EC2s (private) via reverse proxy
