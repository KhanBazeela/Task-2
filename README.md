# Task-2
**HOW TO LAUNCH VM USING TERRAFORM AND AWS CLOUD**

**AWS DOCUMENT**

**EC2(Elastic compute cloud)**:\
Mainly consists of \
1-Renting VM\
2-Storing data on virtual drives(EBS)\
3-Distributing load across machine (ELB)\
4- Scaling the services using an auto-scaling group(ASG)

**EC2 sizing and configuration options**:\
Operating system\
Compute power unit\
RAM\
Storage space(Network attached(EBS and EFS)\
              (Hardware attached(EC2 instance and store)\
Network card: Speed of the card and Public ip address\
Firewall rules: Security group\
Bootstrap script.

**SSH keys are used to securely log in to your EC2 instance without passwords**.

They come in a pair:\
Private key (.pem) → stays on your laptop (never share this)\
Public key → stored on the EC2 instance\
AWS uses this key pair to authenticate you.

**Option 1**: During EC2 launch (most common)\
AWS asks you to create or select a key pair\
You download a .pem file\
AWS injects the public key into the instance\
AWS gives the .pem file only once



**Security groups**:\
SGs are fundamental of network security in AWS\
These act as firewall and regulate :\
Access to ports \
Authorised IP Ranges-IPV4 and IPV6.
Control of inbound network\
Control of outbound network.

Security groups:\
Can be attached to multiple instances\
locked down to a region/VPC combination\
does live "outside" the EC2 -if traffic is blocked ,the EC2 instance won't see it\
Its good to maintain one separate SG for SSH access\
If your application is not accessible ,then its security group issue.\
If your application gives a "connection refused" error,then its an application issue or its not launched\
All inbound traffic is bloced by default\
All outbound traffic is authorised by default

**VPC (Virtual Private Cloud) is your own private network inside AWS**.\\

A virtual data center network that only you control.
You decide:\
IP address range\
Subnets\
Routing\
Internet access\
Security rules

**----Public VPC-----**\\
A VPC where all subnets are public.
Features:\
Internet Gateway attached\
All instances can access the internet directly\\

Use case:\
Simple websites\
Non-sensitive workloads\
Not ideal for databases or secure apps

**----Private VPC----**
A VPC with only private subnets.
Features:\
No direct internet access\
Access via VPN / Direct Connect only\
Use case:\
Internal applications\

Highly secure environments\
A subnet is a smaller network inside a VPC.
It divides your VPC into logical sections.
Example:
VPC:      10.0.0.0/16\
Subnet-1: 10.0.1.0/24\
Subnet-2: 10.0.2.0/24\

A subnet exists in only ONE Availability Zone (AZ)\
VPC can span multiple AZs\
This helps with high availability

**---VPC with Public & Private Subnets (Most Common)---***

This is the real-world standard design.
Architecture:
Public subnet → Load balancer, Bastion\
Private subnet → App servers, Databases\
NAT Gateway for outbound internet\
Use case:
Web applications\
Enterprise systems


**Step 1**: Login to AWS Console/
Go to AWS Management Console → sign in
**Step 2**: Open EC2 Service\
Search EC2\
Click EC2 → Instances\
Click Launch instance
**Step 3**: Name your VM\
Example:
My-First-VM
**Step 4**: Choose an AMI (Operating System)\
Common choices:
Amazon Linux (best for beginners)\
Ubuntu\
Windows Server\
Select one and continue
**Step 5**: Choose Instance Type\
For free tier:
t2.micro or t3.micro\
(1 vCPU, 1 GB RAM)
**Step 6**: Create / Select Key Pair (VERY IMPORTANT)\
This is for SSH login\
Click Create new key pair\
Name it (e.g. mykey)\
Download .pem file\
Download only once — keep it safe
**Step 7**: Network Settings\
VPC: Default (for beginners)\
Subnet: Any\
Auto-assign Public IP: Enable\
Firewall (Security Group):
Allow SSH (22) → My IP\
Allow HTTP (80) if needed
**Step 8**: Storage\
Default is fine\
8 GB gp3
**Step 9**: Launch Instance\
Click Launch instance\
   VM is created
**Step 10**: Connect to the VM (Linux)\
On Linux / macOS / Git Bash:
chmod 400 mykey.pem\
ssh -i mykey.pem ec2-user@<public-ip>\
Ubuntu:
ssh -i mykey.pem ubuntu@<public-ip>\

<img width="1415" height="572" alt="image" src="https://github.com/user-attachments/assets/d172c7e2-66be-41b6-85b4-d63799a75242" />

<img width="1508" height="705" alt="image" src="https://github.com/user-attachments/assets/f1da8fed-df9e-4d97-a5cb-3aefde0a6b13" />

**TERRAFORM**
Folder structure
terraform-vm/
 ├── main.tf
 ├── variables.tf
 └── outputs.tf

Steps to Run Terraform
Step 1: Configure AWS credentials
aws configure
Step 2: Initialize Terraform
terraform init
Step 3: Preview resources
terraform plan
Step 4: Create VM
terraform apply
Type yes
Connect to the VM
ssh ec2-user@<public-ip>
(Private key is already in ~/.ssh/id_rsa)

