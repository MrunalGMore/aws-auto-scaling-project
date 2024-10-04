# AWS Auto Scaling and Monitoring for a Web Application.

## Project Overview

This project demonstrates the deployment of a web application on an AWS EC2 instance, with Auto Scaling and CloudWatch monitoring enabled. The project ensures that the application is scalable based on traffic demand, optimizes resource usage, and monitors system performance.

---

## Table of Contents

1. [Project Setup](#project-setup)
2. [Auto Scaling Configuration](#auto-scaling-configuration)
3. [Monitoring Setup](#monitoring-setup)
4. [Resource Optimization](#resource-optimization)
5. [Scaling Policies](#scaling-policies)
6. [Best Practices](#best-practices)
7. [Contact](#contact)

---

## Project Setup

### 1. **Launch an EC2 Instance**

- Log in to your [AWS Console](https://aws.amazon.com/console/).
- Navigate to **Services > EC2 > Launch Instance**.
- Choose **Amazon Linux 2 AMI** and select the **t2.micro** instance (free-tier eligible).
- Configure the instance:
  - Network: Default VPC
  - Storage: 8 GB (default is sufficient)
- Configure Security Group:
  - Allow **SSH (port 22)** and **HTTP (port 80)**.
- Generate or use an existing **key pair** for SSH access.
- **Launch** the instance.

### 2. **Install Web Server (Apache)**

SSH into the EC2 instance using the key pair:

```bash
ssh -i "your-key.pem" ec2-user@<your-ec2-public-ip>

#### Install and start Apache:

sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd

Verify the web server is running by accessing http://<your-ec2-public-ip>.

### 3. **Deploy the Web Application**
Navigate to the Apache root directory:
cd /var/www/html

Create a simple index.html file with content for your web app:
Save and exit. Your web app should now be accessible.

Auto Scaling Configuration
1. Create a Launch Template
Go to EC2 > Launch Templates > Create Launch Template.
Use the same configuration as your existing EC2 instance.
This template will serve as a blueprint for future instances created by the Auto Scaling group.

2. Create an Auto Scaling Group
Go to EC2 > Auto Scaling Groups > Create Auto Scaling Group.
Select the launch template you just created.
Configure the group:
Minimum size: 1
Desired capacity: 1
Maximum size: 3
Attach the Auto Scaling Group to a Load Balancer (optional for larger deployments).
Define scaling policies to automatically adjust capacity based on traffic.

Monitoring Setup
1. Enable CloudWatch Monitoring
Go to CloudWatch > Alarms > Create Alarm.
Select EC2 CPU Utilization as the metric.
Set up thresholds for your alarm, e.g., alert when CPU utilization is over 70%.
Set up email notifications using Amazon SNS (Simple Notification Service).

2. View Performance Metrics
Use CloudWatch to monitor CPU utilization, memory usage, disk I/O, and network traffic.
Customize dashboards for a real-time view of system health.

Resource Optimization
1. Detailed Monitoring
Enable Detailed Monitoring on your EC2 instances to get per-minute metrics rather than the default five-minute metrics.
This will give you better insights into performance but may incur additional costs.

2. Use Spot Instances (Optional)
Spot Instances are cheaper but may be terminated if AWS needs the capacity back.
You can modify your Auto Scaling Group to use a mix of Spot and On-Demand instances for cost-effective scaling.

Scaling Policies
1. CPU-Based Auto Scaling
Create scaling policies that trigger when CPU utilization crosses certain thresholds:
Scale-out (add instances): When CPU utilization > 70% for 5 minutes.
Scale-in (remove instances): When CPU utilization < 30% for 5 minutes.

2. Custom Metrics (Optional)
You can also scale based on custom CloudWatch metrics like Memory Utilization or Network Traffic.
Use CloudWatch Agent to collect and push custom metrics to CloudWatch.
