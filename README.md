Project Description:

•	Custom VPC with public and private subnets across two Availability Zones

•	Internet Gateway & NAT Gateway for network access

•	Route tables for public and private routing

•	Security Groups for ALB and web app instances

•	Application Load Balancer (ALB) in public subnets

•	EC2 Launch Template for web server configuration

•	Auto Scaling Group for dynamic instance management

•	CloudWatch Alarms and Scaling Policies based on CPU utilization
    

Network Design

VPC-CIDR: 192.168.10.0/24

Public Subnets: 	pub-a & pub-b

Private Subnets:	pvt-a & pvt-b

Internet Access:	Internet Gateway + public subnet

NAT Gateway:	Allows private subnet egress

    

Load Balancer

•	Internet-facing Application Load Balancer in public subnets

•	Connected to EC2 targets in private subnets via Target Group

•	HTTP listener forwarding traffic to private instances

     

Auto Scaling Group

•	Launches EC2 instances using t2.micro and AMI ID ami-0953476d60561c955

•	Handles lifecycle with min/max size and CPU alarms

•	Launch Template includes user data for HTTP setup and launch a simple website.

                

CloudWatch Alarms & Scaling Policies

•	Scale out if average CPU > 70%

•	Scale in if average CPU < 50%

•	Uses change in capacity to adjust instance count


Security Configuration

•	Security group for ALB: allows HTTP from 0.0.0.0/0

•	Security group for EC2: allows HTTP from ALB SG only
