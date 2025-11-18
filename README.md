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
