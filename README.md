# AWS-3-Tier-project

This project demonstrates the deployment of a 3-tier architecture on AWS using Terraform. It includes a frontend hosted on EC2, a backend application server, and a MySQL database on RDS, with proper networking using VPC, subnets, NAT gateway, and an Application Load Balancer.

## In this Project, I have used the following AWS services:

- VPC
- S3
- IAM
- EC2
- RDS
- Route 53

## Architecture Diagram

![3 Tier Architecture](https://github.com/user-attachments/assets/55b8ab4a-1835-4578-804e-02fc5c143382)

## Step by step demonstration

### ✅ Step 1: Create VPC 
Go to VPC and click on create VPC on AWS console 
<img width="1440" alt="Screenshot 2025-05-23 at 1 11 54 PM" src="https://github.com/user-attachments/assets/fd76ceb2-831e-4e1b-86bf-1b693d65bb1b" />

Then click on VPC only if you want to do the process manually and set up network infrastructure only and click on VPC and more it has production like setup Creates a full cloud environment and also connect to compute resources at the same time 
Used for real-world applications like 3-tier web apps, microservices

- give name to vpc 
- and also provide required CIDR block accordingly
- and no ipv6 cidr required
- go with the default Tenancy
<img width="1440" alt="Screenshot 2025-05-23 at 1 50 32 PM" src="https://github.com/user-attachments/assets/5695bf1a-0144-4dbe-b680-5066fe316c0f" />

- no of availability zones 2
- no of public subnets should also be 2
- private subnets = 4
- NAT gateways in 1 AZ
- no VPC endpoints required

<img width="1440" alt="Screenshot 2025-05-23 at 1 50 40 PM" src="https://github.com/user-attachments/assets/9b8c11cc-288b-42a6-b75d-563b25ffd0c8" />

click on Create VPC

<img width="1440" alt="Screenshot 2025-05-23 at 1 50 44 PM" src="https://github.com/user-attachments/assets/554326f9-6ebb-4515-86e6-140b50596f82" />

Now go to Subnets and change the name of subnets whch are just created
<img width="1440" alt="Screenshot 2025-05-23 at 2 10 27 PM" src="https://github.com/user-attachments/assets/0f4f3707-6fb8-4c50-991d-255448223829" />

- we will create 5 security groups for the following reasons

1) for the web server
2) for internet facing load balancer that is external load balancer
3) for app servers
4) for internal load balancers
5) another one for database

Now go to security groups and click on create security group 

<img width="1440" alt="Screenshot 2025-05-23 at 2 17 05 PM" src="https://github.com/user-attachments/assets/ae27d749-cf80-402c-a1bc-ec5fd7ef516e" />





