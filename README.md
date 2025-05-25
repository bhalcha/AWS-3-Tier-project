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

- we will create 5 security groups for the following reasons and allow the required traffic which is given below

1) for the web server -> name:- Web-SG ,inbound rule HTTP but make it custom and add Web-ALB-SG to it and also add another HTTP and add our VPC CIDR block to it :- 
2) for internet facing load balancer that is external load balancer -> name :- Web-ALB-SG ,inbound rule :- HTTP (port no 80) anywhre 0000/0
3) for app servers :- name - App-SG :- allow port no 4000 because our application is react base application 
4) for internal load balancers :- name:- Internal-ALB-SG :- allow port no 80 HTTP but give our VPC cidr block to its IP range dont allow for everyone
5) another one for database:- name:- RDS-SG  - port 3306 - allow traffic from VPC only so give IP range 

Now go to security groups and click on create security group 

<img width="1440" alt="Screenshot 2025-05-23 at 2 17 05 PM" src="https://github.com/user-attachments/assets/ae27d749-cf80-402c-a1bc-ec5fd7ef516e" />

now we will give name and description accordingly and allow the required traffic ( open the ports ) which is given in our previous points  opt for our own created VPC 

<img width="1440" alt="Screenshot 2025-05-23 at 2 35 38 PM" src="https://github.com/user-attachments/assets/201f646b-cbd9-489d-ae0f-a27b05149cf0" />


and then click on create security group and do it likewise for all 5 security groups allowing the required ports 

<img width="1440" alt="Screenshot 2025-05-23 at 2 49 24 PM" src="https://github.com/user-attachments/assets/b903b927-8686-4a8a-9d20-82f0623f8ed4" />

<img width="1440" alt="Screenshot 2025-05-23 at 2 56 48 PM" src="https://github.com/user-attachments/assets/52f57614-8c1c-463a-b3a3-736c78abf3c5" />

<img width="1440" alt="Screenshot 2025-05-23 at 3 03 26 PM" src="https://github.com/user-attachments/assets/9072f9d0-e0da-4c8a-ab26-68a092495760" />

<img width="1440" alt="Screenshot 2025-05-23 at 3 13 44 PM" src="https://github.com/user-attachments/assets/f80ca217-94ec-4a4d-bd6f-a11bc928db47" />

### ✅ Step 2: S3 Bucket and IAM Role Setup

Now go to S3 and click on create bucket and just name the bucket and everything keep by default
<img width="1440" alt="Screenshot 2025-05-23 at 6 02 23 PM" src="https://github.com/user-attachments/assets/1d46ba5b-8160-4222-8263-b85f76a51e7a" />
click on create bucket

<img width="1440" alt="Screenshot 2025-05-23 at 6 02 29 PM" src="https://github.com/user-attachments/assets/3839db4d-43b3-4a7a-9879-48049ee77a41" />

Now drag and drop application code files to S3 bucket

<img width="1440" alt="Screenshot 2025-05-23 at 6 06 32 PM" src="https://github.com/user-attachments/assets/7f7dd887-3dc8-4940-8057-11ad91d61394" />


Now go to IAM and create a IAM role 
- change the USE case to EC2 

<img width="1440" alt="Screenshot 2025-05-23 at 6 30 53 PM" src="https://github.com/user-attachments/assets/1dc6ea0d-c86f-436a-9691-b56bcf3cd456" />

click on next 
- and add the following permissiom
- AmazonEC2RoleforSSM
- also give a name to the role
- then click on create role

### ✅ Step 3: Database Configuration
Launch and configure an RDS instance to serve as the backend database

- now in subnet groups we will add custom VPC and also add the subnets DB subnets to this subnet groups and this will be attached to the RDS instance 

- select az ap-south-1a and ap-south-1b
- now select DB1 Subnet and DB2 subnet

<img width="1440" alt="Screenshot 2025-05-23 at 7 02 06 PM" src="https://github.com/user-attachments/assets/2ec53f9e-0934-4b61-bc2e-54febd8856c8" />

## now lets create the database 

- click on create database
- click on standard create
- select engine option as MySQL
- select engine version
- select the free tier template
- set the name for database
- set the password
- instance type db.t4g.micro
- and then select to our custom VPC
- then select our just created subnet group
- change security group to RDS-SG

<img width="1440" alt="Screenshot 2025-05-23 at 7 42 18 PM" src="https://github.com/user-attachments/assets/9c72bbf4-6438-4085-b04b-a4225e68f7ed" />

<img width="1440" alt="Screenshot 2025-05-23 at 7 42 25 PM" src="https://github.com/user-attachments/assets/03c6e688-6630-4c4e-84bb-6ef7505d561a" />

<img width="1440" alt="Screenshot 2025-05-23 at 7 42 38 PM" src="https://github.com/user-attachments/assets/3a4562a8-18f1-4fd2-999d-c1df3f327b01" />


<img width="1440" alt="Screenshot 2025-05-23 at 7 40 41 PM" src="https://github.com/user-attachments/assets/b9476b07-ff6f-45c9-a392-8a3a196c789f" />



### ✅ Step 4: Application Tier Setup
in this step we will create EC2 instance for app tier 
- give name to instance
- select amazon Linux AMI
  <img width="1440" alt="Screenshot 2025-05-23 at 7 46 34 PM" src="https://github.com/user-attachments/assets/ac7b1e60-b3d5-43a1-986a-1104847eaeee" />

- instance Type t2.Micro
- proceed without key pair if you dont want to connect EC2 by any third party app

<img width="1440" alt="Screenshot 2025-05-23 at 7 49 28 PM" src="https://github.com/user-attachments/assets/dc587a76-d691-40bb-a58a-3cad535859b7" />

- EDIT Network settings
- select Demo VPC
- select subnet as app1 subnet
- no need of public IP
- Select existing security group that we have created (App-SG)
  <img width="1440" alt="Screenshot 2025-05-23 at 7 54 11 PM" src="https://github.com/user-attachments/assets/d0b6b714-6f39-4054-b2d9-51e1299cf9d1" />

- go to advance details and attach IAM role to instance (role which we have already created
- Then launch the instance
  <img width="1440" alt="Screenshot 2025-05-23 at 7 57 54 PM" src="https://github.com/user-attachments/assets/283adb9f-ef9e-42a6-a63c-574c3b27cfd6" />

#### connect to instance and switch to root user

- connect using session manager in aws

- now install mysql client on our instance
- now configue mysql database using DB endpoints
- create database named webappdb using sql commands
- and then we will create a table called tansactions
- now make any entry to database

- change the dbconfig.js file in and add databse endpoints in it

<img width="1440" alt="Screenshot 2025-05-25 at 1 07 06 PM" src="https://github.com/user-attachments/assets/2523c92e-a921-4298-a3f1-1bb345d34a88" />

<img width="1440" alt="Screenshot 2025-05-25 at 1 07 12 PM" src="https://github.com/user-attachments/assets/3037e2b2-71fb-40f5-b7bb-c394817e3641" />

- now installing and configuring nodejs

- also install nvm (node version manager)
- install pm2
- now download the app tier folder from s3 bucket to the instance using s3 cp command
- then install npm (node port manager) 
<img width="1440" alt="Screenshot 2025-05-25 at 1 27 25 PM" src="https://github.com/user-attachments/assets/c0238005-fd66-417d-b51a-e81d526e8629" />


### creating target groups for internal load balancer 

- and we will opt for our vpc and target for app tier instance
- <img width="1440" alt="Screenshot 2025-05-25 at 2 15 00 PM" src="https://github.com/user-attachments/assets/0c6343f1-8dcc-4809-bf9d-bd4b23e0f654" />

- now creating the load balancer opt for application load balancer 
- this is an internal load balancer so we are optiong for internal option
- select our custom VPC
- and opt for both availability zones
- select app1 and app 2 subnet in it because the load balancer is for app tier
- select security group as internal ALB sg thet we have created for internal sg
- now select our target group (App-internal-TG)
- now our intrnal load balancer is created

- make changes in nginx.conf file provide it internal lb DNS name


### now lets set up WEb tier resources 

- in  this step i will launch a instance for web-tier
- give it a name called Web-Tier-Instance
- select amazon linux ami
- select a key pair
- edit network settings and select vpc as our new custom vpc
- and select public subnets
- select web-SG existing SG
- add our EC2 role in advanced settings
- launch the instance
- connect to instance
- copy web tier code file from s3 using s3 cp command
- now npm install into web tier folder
- now npm run build
- install nginx to our instance
- update the nginx configuration
- after configuration restart the nginx
- and allow all traffic in web tier instance SG

## now creating the external load balancer 

- name the load balancer as external load balancer
- opt for our custom VPC
- select both availablility zones
- make internet facing
- aftersetting this load balancer and target group
- you can HIT this load balancer and you will see the output


# Following are some screenshots of the output


