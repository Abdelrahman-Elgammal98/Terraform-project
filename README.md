# Terraform-project

# 📁 Project Structure
project-folder/

│-- elb-m/                # Load Balancers configuration

│-- gateways-m/           # Internet Gateway & NAT Gateway

│-- instance-m/           # EC2 instances and provisioning

│-- project pics/         # Project-related images

│-- routes-m/             # Route Tables configuration

│-- s3-statefile/         # Terraform state storage (S3 backend)

│-- SecurityGroups-m/     # Security Groups for instances and ELBs

│-- subnet-m/             # Subnets definitions (Public & Private)

│-- vpc-m/                # VPC configuration

│-- main.tf               # Main Terraform configuration file

│-- outputs.tf            # Output values (ELB DNS, instance IPs, etc.)

│-- provider.tf           # AWS Provider setup

# 🧱 Project Modules

-vpc: Creates the main VPC.

-subnets: Provisions public and private subnets across two AZs.

-igw: Creates and attaches an Internet Gateway.

-route_table: Sets up a route table for public traffic.

-private_route_table: Configures the NAT Gateway and routes for private subnets.

-security_groups: Defines security groups for the EC2 instances and load balancers.

-data_source: Retrieves the latest public Amazon Linux 2023 AMI.

-ec2_instances: Deploys four EC2 instances:
  -Two public instances (configured with NGINX to proxy requests).
  -Two private instances (running Apache to serve web content).

-load_balancers: Provisions both public and private Application Load Balancers along with associated target groups and listeners.

-S3: Creates an S3 bucket for Terraform state.
