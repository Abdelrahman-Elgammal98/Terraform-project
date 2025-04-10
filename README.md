# Terraform-project
Scalable Multi-Tier AWS Architecture with Web Servers using Terraform
This Terraform project provisions a complete AWS infrastructure for a web application. The architecture is designed to handle traffic securely and efficiently by leveraging both public and private components. When you access the public load balancer URL, traffic is directed to two public EC2 instances which act as reverse proxies by redirecting traffic to a private load balancer. The private load balancer then forwards requests to two private EC2 instances that host the actual web servers.

Table of Contents
Architecture Overview
Project Structure
Project Modules
How It Works
Prerequisites
Deployment Steps
Demo
Project Variables
Contributing
🧭 Architecture Overview
Screenshot 2025-04-07 230817

The AWS architecture is split into several components to enhance security and manageability:

VPC & Subnets
A dedicated VPC is created with separate public and private subnets across two Availability Zones (AZs).

Internet Gateway & Route Tables
An Internet Gateway is attached to the VPC with route tables that direct internet traffic to the public subnets.

NAT Gateway & Private Route Tables
A NAT Gateway is provisioned in the public subnet, with private route tables sending outbound traffic from private subnets through the NAT.

Security Groups
Security groups are used to control traffic:

Instances Security Group: Allows SSH (port 22) and HTTP (port 80).
Load Balancer Security Group: Permits HTTP traffic from the internet.
Data Source
Dynamically retrieves the latest Amazon Linux 2023 AMI for the EC2 instances.

EC2 Instances

Public EC2 Instances (2):
Deployed in public subnets and configured with NGINX to act as reverse proxies, forwarding traffic to the private load balancer.
Private EC2 Instances (2):
Located in private subnets, these instances run Apache HTTP server to serve the application content after receiving traffic from the private load balancer.
Load Balancers

Public Load Balancer:
Receives incoming internet traffic and distributes it to the public EC2 instances.
Private Load Balancer:
Receives the proxied traffic from public instances and balances it across the private EC2 instances.
Additional Resources

S3 Bucket: Stores the Terraform state.
DynamoDB Table: Provides state locking for consistent Terraform runs.
📁 Project Structure
project-directory/
├── modules
│   ├── vpc
│   ├── subnets
│   ├── igw
│   ├── route_table
│   ├── private_route_table
│   ├── security_groups
│   ├── data_source
│   ├── ec2_instances
│   ├── load_balancers
│   ├── S3
│   └── DynamoDB
├── web_server_1
├── web_server_2
├── main.tf
├── variables.tf
├── outputs.tf
├── provider.tf
└── backend.tf
🧱 Project Modules
The repository is organized into multiple modules that abstract the infrastructure components:

vpc: Creates the main VPC.
subnets: Provisions public and private subnets across two AZs.
igw: Creates and attaches an Internet Gateway.
route_table: Sets up a route table for public traffic.
private_route_table: Configures the NAT Gateway and routes for private subnets.
security_groups: Defines security groups for the EC2 instances and load balancers.
data_source: Retrieves the latest public Amazon Linux 2023 AMI.
ec2_instances: Deploys four EC2 instances:
Two public instances (configured with NGINX to proxy requests).
Two private instances (running Apache to serve web content).
load_balancers: Provisions both public and private Application Load Balancers along with associated target groups and listeners.
S3: Creates an S3 bucket for Terraform state.
DynamoDB: Sets up a DynamoDB table for Terraform state locking.
🔁 How It Works
User accesses the public ALB DNS URL.
Public ALB routes the request to public EC2 instances.
Public instances (NGINX) forward the request to the private ALB.
Private ALB distributes the traffic to private EC2 instances hosting the web server.
⚙️ Prerequisites
AWS Account
Terraform (v1.0+)
SSH key pair for EC2 access
🚀 Deployment Steps
Clone the repository:
git clone https://github.com/AbdallahMostafa02/Terraform_and_AWS_Project.git
cd Terraform_and_AWS_Project/
Initialize Terraform:
terraform init
Preview the plan:
terraform plan
Apply the configuration:
terraform apply
Access the app:
Copy the public load balancer DNS output and open it in a browser.
📸 Demo
Below are screenshots of the two web pages served by the private EC2 instances after accessing the public load balancer URL in the browser:

🔵 Web Server 1
Screenshot 2025-04-07 224655

🟢 Web Server 2
Screenshot 2025-04-07 212125

🔑 Project Variables
The following key variables must be set (or are provided with defaults):

instance_type: The type of EC2 instance (default: t2.micro).
key_name: Name of the SSH key pair (default: abdallah).
private_key_path: Path to your private SSH key (default: /home/abdallah/abdallah.pem).
Other variables (such as subnet CIDRs, VPC CIDR, etc.) are defined within the respective module files or passed from the root configuration.

🤝 Contributing
Contributions are welcome! Please fork this repository and submit pull requests for improvements or bug fixes.
