🏗️ Networking Setup
VPC Design: A custom VPC (192.168.10.0/24) with DNS support enabled for hostname resolution.

Subnets:
Two public subnets (pub-a, pub-b) for internet-facing resources Load Balancer and NAT Gateway.
Two private subnets (pvt-a, pvt-b) for backend EC2 instances managed by Auto Scaling.

Routing:
Separate route tables for public and private subnets.
Public route table connected to an Internet Gateway for outbound traffic.
Private route table uses a NAT Gateway (in pub-a) for secure internet access from private subnets.

🌐 Internet Access
Internet Gateway: Attached to the VPC for public subnet connectivity.
Elastic IP & NAT Gateway: Enables outbound internet access for EC2s in private subnets without exposing them directly.

🔐 Security Groups
Load Balancer Security Group (SgLB): Allows inbound HTTP traffic (port 80) from anywhere.
EC2 SG (SgWebapp): Restricts access to HTTP traffic only from the Load Balancer Seurity Group, ensuring controlled communication.

⚖️ Load Balancing
Application Load Balancer (ALB):
Internet-facing with listeners on port 80.
Deployed across both public subnets for high availability.

Target Group:
Configured for HTTP on port 80.
Targets EC2 instances launched by the Auto Scaling Group.

🖥️ Compute & Scaling
Launch Template:
Uses Amazon Linux AMI (ami-0953476d60561c955) with t2.micro instance type.
Bootstraps a basic Apache web server serving a "Hello-World" page.
Includes key pair reference and tagging for traceability.

Auto Scaling Group (ASG):
Spans private subnets (pvt-a, pvt-b) for backend isolation.
Min size: 1, Max size: 4.
Integrated with the target group for seamless load balancing.
Metrics collection enabled at 1-minute granularity.

📊 Monitoring & Scaling Policies
CloudWatch Alarms:
HighCpu-alarm: Triggers scale-out when CPU > 70%.
LowCPU_Alarm: Triggers scale-in when CPU < 50%.

Scaling Policies:
ScaleOutPolicy: Adds 2 instances with a 2-minute cooldown.
ScaleInPolicy: Removes 1 instance with a 2-minute cooldown.
