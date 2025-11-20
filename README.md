# aws-learning
## AWS Networking Assignment: VPC and Network Setup (1)


## Objective:
Create a custom VPC with both public and private subnets, configure internet access, and deploy EC2 instances with secure segmentation.
### 1. VPC and Subnets
Create a VPC.

Create 4 subnets:

- 2 Public Subnets (one in each availability zone: Public-A, Public-B)

- 2 Private Subnets (one in each availability zone: Private-A, Private-B)

<img width="1264" height="630" alt="Screenshot 2025-11-16 at 20 37 30" src="https://github.com/user-attachments/assets/5e246a44-3d69-4d35-afcd-fd27faa8918c" />


### 2. Security Groups

Create a security group for instances:

Inbound rules:

SSH (port 22) from anywhere (0.0.0.0/0)

HTTP (port 80) from anywhere (0.0.0.0/0)

Outbound rules:

Allow all traffic (0.0.0.0/0)

### 3. EC2 Instances

Public instance (Bastion host):

Launch in Public Subnet A

Assign the security group created above

Allows direct SSH/HTTP access from the internet

Private instance:

Launch in Private Subnet A

Assign the same security group as the public instance

No direct internet access
<img width="1261" height="635" alt="Screenshot 2025-11-16 at 23 33 44" src="https://github.com/user-attachments/assets/f81109cf-8e42-4ea7-8ae9-3dc7a5089c49" />



### 4. Route Tables

Create 2 route tables:

- Public route table → attach public subnets

- Private route table → attach private subnets

<img width="1274" height="631" alt="Screenshot 2025-11-16 at 20 37 46" src="https://github.com/user-attachments/assets/78b34c44-23db-402e-9cfe-edc50b7ae7f5" />

Create an Internet Gateway (IGW) and attach it to the VPC

Add a route in the public route table:

Destination: 0.0.0.0/0

Target: Internet Gateway


### 5. NAT Gateway

Create a NAT Gateway in Public Subnet A

Add a route in the private route table via NAT Gateway (for internet access)

Attach the NAT Gateway to the private route table

<img width="1260" height="630" alt="Screenshot 2025-11-16 at 20 31 05" src="https://github.com/user-attachments/assets/2a854de6-43f6-49e9-a4ef-6db15d33ab06" />
<img width="1261" height="585" alt="Screenshot 2025-11-16 at 20 35 02" src="https://github.com/user-attachments/assets/0617f5d7-e7e0-4962-90c9-58c68877f9de" />

Allows private instances to access the internet through the Bastion host / NAT Gateway


### 6. Summary

Public instance acts as a Bastion host for SSH access.

Private instance uses the NAT Gateway to access the internet.

Security group ensures controlled inbound and open outbound traffic.

Route tables and subnets ensure proper public/private routing.



<img width="691" height="524" alt="Screenshot 2025-11-18 at 18 24 48" src="https://github.com/user-attachments/assets/4cb139e7-1728-4950-90d0-29e03c6173ff" />


## AWS Networking Assignment: Application Load Balancer (2)

## Objective: 

Deploy an Application Load Balancer (ALB) with two EC2 instances behind it. Ensure security group (SG) configurations allow only ALB access to EC2 instances while preventing direct access. 

### 1. Launched Two EC2 Instances

Used Amazon Linux 2 as AMI

Placed instances in the same VPC but in different Availability Zones to ensure redundancy.

Used a user data script to install and configure a simple web server (Apache).

```
#!/bin/bash

yum update -y
yum install -y httpd

systemctl start httpd
systemctl enable httpd

echo "<h1>Hello CoderCo from $(hostname -f)</h1>" > /var/www/html/index.html
```
Each instance serves a page displaying instance-specific information (e.g., hostname, ID).
<img width="1268" height="436" alt="Screenshot 2025-11-19 at 23 34 34" src="https://github.com/user-attachments/assets/6a69d6dd-4dcd-488b-a978-ede528e2a393" />


### 2. Created an Application Load Balancer (ALB)

Deployed an internet-facing ALB.

Configured listener on HTTP (port 80).

Created a Target Group and registered both EC2 instances.

Enabled and tested health checks to verify that both instances respond correctly.

Verified that the ALB distributes traffic evenly across the instances.
<img width="1261" height="570" alt="Screenshot 2025-11-19 at 23 34 28" src="https://github.com/user-attachments/assets/112294d2-3ca8-4ac3-b25e-cd3a91a5d069" />
<img width="1265" height="563" alt="Screenshot 2025-11-19 at 23 34 17" src="https://github.com/user-attachments/assets/15676cfd-572f-4b04-8147-fb7341717e98" />


### 3. Configured Security Groups
ALB Security Group

Allowed inbound HTTP (80) traffic from the internet.

Allowed outbound traffic required for load balancer → target communication.

EC2 Security Group

Allowed inbound HTTP/HTTPS traffic only from the ALB's Security Group.

Denied direct access from the internet (no 0.0.0.0/0 allowed).

Allowed outbound traffic for updates and package installations.

### 4. Tested and Validated

Confirmed that both instances are healthy in the ALB target group.

Accessed the ALB’s DNS name and verified load balancing between both EC2 instances.

Ensured EC2 instances cannot be accessed directly via their public IPs
<img width="1280" height="800" alt="Screenshot 2025-11-19 at 23 34 08" src="https://github.com/user-attachments/assets/3cca7e1e-b1c1-42dd-9762-f45b30792ab6" />


### Result

The final architecture provides:

High availability across multiple AZs

Load balancing using an ALB

Secure traffic flow is restricted through proper SG rules

Simple dynamic web content rendered from each EC2 instance

<img width="924" height="541" alt="Screenshot 2025-11-20 at 20 38 42" src="https://github.com/user-attachments/assets/38c60701-d83a-4efe-83ce-487ab6e59701" />



This setup forms the foundation for scalable, production-grade workloads on AWS.
